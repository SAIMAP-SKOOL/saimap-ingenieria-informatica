# Tema 13: Introducción a la Programación en Lenguaje Ensamblador

El lenguaje ensamblador es la representación más baja y cercana al hardware de la programación de software. Cada instrucción en ensamblador se corresponde de forma directa (mapeo uno a uno) con una instrucción binaria ejecutable por la CPU (código de máquina). Aprender a programar en ensamblador permite entender cómo se manipulan físicamente los registros, cómo se gestiona la memoria RAM y cómo se implementan a nivel de hardware las estructuras lógicas de control (bucles e `if-else`) de los lenguajes de alto nivel.

---

## 1. Anatomía de una Instrucción de Máquina

Una instrucción de máquina es una palabra binaria dividida en campos específicos:

```
  6 bits       5 bits   5 bits   5 bits      11 bits
 +-----------+--------+--------+--------+-----------------+
 |  OPCode   |   Rs   |   Rt   |   Rd   |    Desplazamiento / Inmediato
 +-----------+--------+--------+--------+-----------------+
```

*   **OPCode (Código de Operación)**: Bits que indican a la Unidad de Control la operación a realizar (ej. Suma, Carga, Salto).
*   **Campos de Operandos**: Identifican qué registros del procesador o qué direcciones de memoria contienen los datos sobre los que se opera.
    *   `Rs` (Source), `Rt` (Target): Registros fuente de operandos.
    *   `Rd` (Destination): Registro de destino donde se guardará el resultado.

---

## 2. Modos de Direccionamiento

Definen la forma en que la instrucción calcula la dirección física del dato (operando) al que desea acceder:

1.  **Direccionamiento Inmediato**: El propio dato está dentro de la instrucción. Es extremadamente rápido al no requerir accesos a registros o memoria.
    *   *Ejemplo*: `ADD R1, R2, #5` (Suma el valor constante $5$ a $R2$ y guarda en $R1$).
2.  **Direccionamiento Directo a Registro**: El dato reside en un registro del procesador.
    *   *Ejemplo*: `ADD R1, R2, R3` (Suma el contenido de $R2$ y $R3$ y guarda en $R1$).
3.  **Direccionamiento Directo (o Absoluto)**: La instrucción contiene la dirección exacta de memoria RAM del dato.
    *   *Ejemplo*: `LD R1, [0x2000]` (Carga en $R1$ el dato de la celda de memoria `0x2000`).
4.  **Direccionamiento Indirecto a Registro (o Indexado)**: La dirección de memoria del dato se calcula sumando una constante de desplazamiento al contenido de un registro base. Es el modo utilizado para recorrer vectores (arrays) y estructuras en memoria.
    *   *Ejemplo*: `LW R1, 4(R2)` (La dirección de lectura es $R2 + 4$).

---

## 3. Repertorio de Instrucciones Básico (Estilo MIPS / RISC-V)

Para construir algoritmos, empleamos las instrucciones básicas del procesador:
*   **Carga y Almacenamiento**: `LW` (Load Word - lee de memoria a registro), `SW` (Store Word - escribe de registro a memoria).
*   **Aritmético-Lógicas**: `ADD` (Suma), `SUB` (Resta), `AND`, `OR`.
*   **Bifurcaciones (Saltos Condicionales)**:
    *   `BEQ R1, R2, Etiqueta` (Branch if Equal: salta a `Etiqueta` si $R1 = R2$).
    *   `BNE R1, R2, Etiqueta` (Branch if Not Equal: salta a `Etiqueta` si $R1 \neq R2$).
*   **Saltos Incondicionales**: `J Etiqueta` (Jump: salta a `Etiqueta` sin comprobar condiciones).

---

## 4. El Toque Informático

### El Ciclo de Traducción de Software: Compilador, Ensamblador y Enlazador
Cuando escribes código en un lenguaje de alto nivel como C++, el programa no se ejecuta de forma directa en el procesador. Pasa por las siguientes fases de traducción:

```
  [Código C++] -> (Compilador) -> [Código Ensamblador] -> (Ensamblador) -> [Código Objeto (.o)] -> (Enlazador) -> [Ejecutable (.exe)]
```

1.  **Compilador**: Traduce el código de alto nivel a un archivo de texto en lenguaje ensamblador específico de la arquitectura (ej. x86 o ARM).
2.  **Ensamblador (Assembler)**: Traduce los nemotécnicos de ensamblador a código binario binario de máquina ejecutable por el procesador, generando un archivo objeto (`.o` o `.obj`).
3.  **Enlazador (Linker)**: Junta varios archivos objeto y resuelve las llamadas a funciones externas de librerías del sistema, unificándolos en el archivo ejecutable final (`.exe` en Windows o ELF en Linux).

A continuación, presentamos un programa completo escrito en Ensamblador MIPS que implementa un bucle iterativo para calcular la suma de los primeros 10 números enteros positivos (equivalente a `for (int i=1; i<=10; i++) sum += i;`).

```mips
# PROGRAMA EN ENSAMBLADOR MIPS: Suma de los primeros 10 enteros
# Registros utilizados:
#   $t0: Contador del bucle (i)
#   $t1: Acumulador de la suma (sum)
#   $t2: Límite del bucle (10)

.text
.globl main

main:
    # 1. Inicialización
    li $t0, 1          # Carga inmediata: contador i = 1
    li $t1, 0          # Carga inmediata: sum = 0
    li $t2, 10         # Carga inmediata: límite = 10

bucle:
    # 2. Condición de parada del bucle
    # Si i > 10 (es decir, si no se cumple i <= 10), salimos del bucle
    # Para ello, si i es mayor que 10, salimos.
    # En MIPS simplificado: si i (t0) es igual a 11 (t2 + 1), salimos
    # O más sencillo: si i es mayor que 10, salimos.
    # Usamos BEQ para comprobar si el contador superó al límite
    
    # Suma: sum = sum + i
    add $t1, $t1, $t0  # t1 = t1 + t0
    
    # Incremento del contador: i = i + 1
    addi $t0, $t0, 1   # t0 = t0 + 1 (direccionamiento inmediato)
    
    # ¿Hemos terminado? Comprobamos si i <= 10
    # Si t0 <= t2, volvemos a la etiqueta 'bucle'
    # MIPS no tiene salto menor directo, evaluamos la condición contraria:
    # Si t0 (i) es menor o igual a t2 (10), saltamos de nuevo a bucle
    # Usamos un truco lógico o instrucción de comparación:
    ble $t0, $t2, bucle # Branch if Less or Equal: si t0 <= t2, salta a bucle

fin:
    # El resultado final queda almacenado en el registro $t1
    # Terminar ejecución del programa (llamada al sistema en MIPS)
    li $v0, 10
    syscall
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Traducir la siguiente estructura condicional de C++ a lenguaje Ensamblador MIPS simplificado:
```cpp
if (a == b) {
    c = a + 5;
} else {
    c = b - 2;
}
```
*Asumir que las variables `a`, `b` y `c` están almacenadas en los registros `$t0`, `$t1` y `$t2` respectivamente.*

**Solución:**
Traducimos utilizando instrucciones de salto condicional e incondicional:

```mips
    # Comparamos a ($t0) y b ($t1).
    # Si NO son iguales (Branch if Not Equal), saltamos a la rama del 'else'
    bne $t0, $t1, rama_else

rama_if:
    # Si eran iguales, se ejecuta esto: c = a + 5
    addi $t2, $t0, 5    # t2 = t0 + 5
    j fin_condicional   # Salto incondicional para evitar ejecutar el 'else'

rama_else:
    # Si no eran iguales, se ejecuta esto: c = b - 2
    addi $t2, $t1, -2   # t2 = t1 - 2 (la resta se hace sumando un número negativo)

fin_condicional:
    # Continuación del programa...
```

### Ejercicio 2
Identificar el modo de direccionamiento utilizado en cada una de las siguientes instrucciones del procesador:
1. `LW R1, [0x1024]`
2. `ADD R1, R2, #100`
3. `LW R1, 8(R3)`

**Solución:**
1.  `LW R1, [0x1024]`: **Direccionamiento Directo (o Absoluto)**. La instrucción contiene directamente la dirección física de memoria (`0x1024`) a la que se desea acceder.
2.  `ADD R1, R2, #100`: **Direccionamiento Inmediato** (para el operando `#100`). El valor constante $100$ está contenido directamente dentro de la palabra de la instrucción. Para los registros $R1$ y $R2$ se utiliza **Direccionamiento Directo a Registro**.
3.  `LW R1, 8(R3)`: **Direccionamiento Indirecto a Registro (o Indexado)**. La dirección efectiva del dato en memoria se calcula sumando el desplazamiento constante $8$ al contenido almacenado en el registro base $R3$.

---

## 6. Ejercicios Propuestos

1.  Escribe el código en lenguaje ensamblador MIPS correspondiente a un bucle `while` que reste de 5 en 5 un número guardado en el registro `$t0` hasta que su valor sea menor o igual a cero.
2.  Traduce a código ensamblador MIPS la siguiente operación aritmética: `c = (a + b) - (d + 8)` asumiendo correspondencia a registros `$t0` a `$t3`.
3.  Explica la diferencia entre un **Repertorio de Instrucciones CISC (Complex Instruction Set Computer)** y uno **RISC (Reduced Instruction Set Computer)** en términos de la duración de ciclo de instrucción y del formato de codificación en binario de las instrucciones.
