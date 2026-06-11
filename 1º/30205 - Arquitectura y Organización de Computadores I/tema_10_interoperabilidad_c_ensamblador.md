# Tema 10: Interoperabilidad de Lenguajes: Código C y Ensamblador ARM

En el desarrollo de software profesional de sistemas, sistemas empotrados y motores de alto rendimiento, es habitual combinar código escrito en C/C++ con rutinas optimizadas en ensamblador. Para que esta comunicación bidireccional funcione sin corromper el sistema, el procesador sigue una norma estricta: el **Estándar de Llamadas para la Arquitectura ARM (AAPCS / EABI)**.

---

## 1. El Estándar de Llamadas de ARM (AAPCS)

El estándar **AAPCS (Procedure Call Standard for the ARM Architecture)** define los convenios que regulan el uso de los registros y de la pila para llamadas a funciones:

### Paso de Parámetros y Retorno de Valores
*   **Primeros 4 parámetros**: Se colocan obligatoriamente de izquierda a derecha en los registros `R0`, `R1`, `R2` y `R3`.
*   **Parámetros excedentes (a partir del 5º)**: Se apilan en el Stack (`R13`), en orden inverso (de derecha a izquierda, de modo que el 5º parámetro quede en la cima de la pila).
*   **Retorno de valores**: El resultado devuelto por la función se deposita en el registro `R0` (si el dato es de 32 bits o menor) o en la pareja `R0-R1` (si es un entero de 64 bits tipo `long long`).

### Clasificación y Conservación de Registros
*   **Callee-saved (Preservados por el receptor - R4 a R11, y R13/SP)**: Si la función secundaria necesita usar cualquiera de estos registros, debe guardar su valor en la pila al inicio y restaurarlo antes de retornar. El llamador asume que estos registros mantendrán sus valores intactos tras la llamada.
*   **Caller-saved (Preservados por el emisor - R0 a R3, R12, y R14/LR)**: Son volátiles. La función secundaria puede sobrescribirlos libremente sin salvarlos. Si el llamador necesita conservar los datos que tenía en ellos, debe apilarlos *antes* de ejecutar la instrucción `BL`.

---

## 2. Declaración e Integración de Código

Para enlazar rutinas de C y ensamblador, se emplean directivas de visibilidad global:

### Desde el Fichero C (`main.c`)
Declaramos la función ensamblador externa mediante el modificador `extern`:
```c
#include <stdio.h>

// Declaración de función externa implementada en ensamblador
extern int sumar_tres_enteros(int a, int b, int c);

int main() {
    int resultado = sumar_tres_enteros(10, 20, 30);
    printf("Resultado: %d\\n", resultado);
    return 0;
}
```

### Desde el Fichero Ensamblador (`sumar.s`)
Publicamos el símbolo del punto de entrada usando la directiva `.global`:
```assembly
        .text
        .align 2
        .global sumar_tres_enteros

sumar_tres_enteros:
        ; Parámetros de entrada en R0 (a), R1 (b) y R2 (c) según AAPCS
        ADD R0, R0, R1      ; R0 = a + b
        ADD R0, R0, R2      ; R0 = (a + b) + c (El resultado queda en R0 para el retorno)
        BX LR               ; Retornar al llamador C
```

---

## 3. El Toque Informático

### Auditor Automático de Cumplimiento AAPCS
El siguiente script en Python actúa como un linter de código estático básico que comprueba si un código ensamblador ARMv7 de muestra cumple con las normas de preservación de registros de la EABI/AAPCS al llamar a una subrutina:

```python
def auditar_registro_aapcs(instrucciones_funcion):
    registros_modificados = set()
    registros_guardados = set()
    errores = []
    
    # Registros que obligatoriamente deben conservarse (callee-saved)
    callee_saved = {"R4", "R5", "R6", "R7", "R8", "R9", "R10", "R11"}
    
    for inst in instrucciones_funcion:
        partes = inst.replace(",", "").replace("{", "").replace("}", "").split()
        if not partes:
            continue
        op = partes[0]
        
        if op == "PUSH":
            # Registrar qué registros se guardaron
            for reg in partes[1:]:
                registros_guardados.add(reg)
        elif op == "POP":
            pass  # En epílogo se asume restauración
        else:
            # Identificar registros destino (primer argumento de la instrucción)
            if len(partes) > 1 and partes[1].startswith("R"):
                reg_destino = partes[1]
                registros_modificados.add(reg_destino)
                
    # Verificar violación de preservación
    for reg in registros_modificados:
        if reg in callee_saved and reg not in registros_guardados:
            errores.append(f"[ERROR] El registro callee-saved '{reg}' fue modificado pero no se guardó en PUSH.")
            
    print("=== INFORME DE AUDITORÍA AAPCS ===")
    if not errores:
        print("  - [OK] La subrutina cumple con la convención de preservación de registros.")
    else:
        for err in errores:
            print(f"  - {err}")
    print("=================================")

# Prueba con subrutina que modifica R4 sin apilarlo
codigo_incorrecto = [
    "PUSH {R5, LR}",
    "MOV R0, #10",
    "ADD R4, R0, R1",  # R4 modificado pero no apilado
    "POP {R5, PC}"
]
auditar_registro_aapcs(codigo_incorrecto)
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Una función en C llama a una rutina en ensamblador con la siguiente signatura:
`extern int procesar_datos(int x1, int x2, int x3, int x4, int x5);`
Indica detalladamente en qué registros y posiciones de memoria se sitúa cada uno de los parámetros recibidos por la función ensamblador al iniciarse.

**Solución:**
Siguiendo las especificaciones de la convención de llamadas AAPCS de ARM:
*   `x1`: Registro `R0`.
*   `x2`: Registro `R1`.
*   `x3`: Registro `R2`.
*   `x4`: Registro `R3`.
*   `x5`: Es el 5º parámetro (excedente). Se recibe en la cima de la pila, es decir, direccionado en la dirección apuntada por el Stack Pointer: `[SP]`.

### Ejercicio 2
Explicar por qué es necesario usar la directiva `.global` en el código ensamblador y qué error se producirá en la fase de construcción del software si se omite.

**Solución:**
La directiva `.global` le indica al ensamblador que exponga el símbolo de la función en la tabla de símbolos pública del archivo objeto (`.o`). Esto hace que el símbolo sea visible para otros módulos externos.
Si se omite `.global`, el compilador de C compilará el archivo `main.c` asumiendo que la función existe, pero en la fase de **enlace (Linking)**, el Linker no podrá encontrar la dirección de memoria de la función en la tabla de símbolos del archivo ensamblador, provocando un error de enlace del tipo:
`"Undefined reference to 'sumar_tres_enteros'"`.

---

## 5. Ejercicios Propuestos

1.  Dada la función en C: `int calcular(int a, int b);`. Si la implementamos en ensamblador, ¿estamos autorizados a modificar los registros `R0`, `R1`, `R2` y `R3` sin guardarlos en la pila? Justifica según la clasificación caller-saved/callee-saved.
2.  Escribe una rutina en ensamblador ARM que sea llamada desde C para retornar el valor absoluto de un entero pasado como único parámetro (`extern int valor_absoluto(int num);`).
3.  Investiga el papel del registro `R12 (IP)` en el estándar AAPCS. ¿Qué significa que sea un registro *intra-procedure-call scratch register* y en qué casos lo modifica el enlazador dinámico del sistema?
