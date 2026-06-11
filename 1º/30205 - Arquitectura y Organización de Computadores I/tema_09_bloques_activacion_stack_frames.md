# Tema 9: Bloques de Activación (Stack Frames) y Gestión de Variables Locales

Cuando una subrutina está en ejecución, requiere espacio en memoria para almacenar variables de ámbito local, argumentos de llamada que exceden la capacidad de los registros físicos y el contexto de retorno. El **Bloque de Activación** (o *Stack Frame*) es el registro dinámico de memoria reservado en la pila para dar soporte a una invocación particular de una subrutina.

---

## 1. El Rol del Frame Pointer (FP) frente al Stack Pointer (SP)

*   **Stack Pointer (SP / R13)**: Puntero de cima de pila. Se modifica continuamente durante la ejecución de la subrutina debido a operaciones de apilado temporal (`PUSH` y `POP`). Esto hace que referenciar una variable local utilizando un desplazamiento fijo respecto a `SP` (ej: `[SP, #8]`) sea inestable y propenso a errores del compilador.
*   **Frame Pointer (FP / R11)**: Puntero de marco de pila. Permanece **estático e invariable** a lo largo de toda la vida de la subrutina. Proporciona una base estable para direccionar datos:
    *   **Variables locales**: Direccionadas en desplazamientos **negativos** respecto al `FP` (ej: `[FP, #-16]`).
    *   **Parámetros excedentes**: Parámetros que no caben en los registros R0-R3 y se pasan en la pila, direccionados en desplazamientos **positivos** respecto al `FP` (ej: `[FP, #8]`).

---

## 2. Anatomía de un Bloque de Activación (Pila FD)

La estructura típica de un bloque de activación en ARM tras ejecutar el prólogo de la función sigue este diseño espacial en la pila:

```
 Dirección Alta   | Parámetro 5 (Excedente) | <-- [FP, #8]
                  | Parámetro 6 (Excedente) |
                  |-------------------------|
                  | LR (Retorno original)   | <-- [FP, #4]
                  | FP (FP anterior)        | <-- FP Actual (R11) apunta aquí
                  | R4 (Contexto salvado)   | <-- [FP, #-4]
                  |-------------------------|
                  | Variable Local 1        | <-- [FP, #-8]
                  | Variable Local 2        | <-- [FP, #-12] (Alineada)
                  |-------------------------|
 Dirección Baja   | Cima de Pila Actual     | <-- SP (Modificable)
```

---

## 3. Prólogo y Epílogo de una Función con Stack Frame

Para inicializar y destruir correctamente este bloque, la subrutina ejecuta rutinas fijas de entrada y salida:

### El Prólogo (Creación del Bloque)
```assembly
MiFuncion:
    PUSH {R4, FP, LR}      ; 1. Salvar registros y el FP del llamador
    ADD R11, SP, #4        ; 2. Establecer el nuevo FP (apunta a la posición de FP salvado)
    SUB SP, SP, #8         ; 3. Reservar 8 bytes para variables locales (2 enteros)
```

### El Epílogo (Destrucción del Bloque y Retorno)
```assembly
    SUB SP, R11, #4        ; 1. Desasignar variables locales (restaurar SP desde FP)
    POP {R4, FP, PC}       ; 2. Restaurar registros del llamador, su FP y retornar
```

---

## 4. El Toque Informático

### Visualizador del Layout de Memoria de un Stack Frame
El siguiente script en Python genera y visualiza en la consola el mapa de direcciones físicas de un bloque de activación dinámico a partir de la declaración de variables locales y parámetros del programador:

```python
def mapear_stack_frame(num_variables_locales, num_parametros):
    fp = 0x7FC0
    print("=== MAPA ESPACIAL DEL STACK FRAME ===")
    print(f"Dirección | {'Contenido':25s} | Desplazamiento FP")
    print("-" * 50)
    
    # 1. Parámetros excedentes (pasados por el llamador)
    for p in range(num_parametros, 4, -1):
        offset = (p - 5) * 4 + 8
        print(f"0x{fp + offset:04X} | Parámetro Excedente {p:02d}  | [FP, #{offset}]")
        
    print(f"0x{fp + 4:04X} | Link Register (LR)        | [FP, #4]")
    print(f"0x{fp:04X} | Frame Pointer (FP) anterior | [FP, #0] <-- FP Actual")
    
    # 2. Contexto y variables locales (direcciones inferiores)
    for v in range(1, num_variables_locales + 1):
        offset = -v * 4
        print(f"0x{fp + offset:04X} | Variable Local {v:02d}      | [FP, #{offset}]")
        
    sp_final = fp - (num_variables_locales * 4)
    print(f"0x{sp_final:04X} | Cima de Pila Actual       | <-- SP")
    print("=" * 50)

# Ejemplo: Función con 3 variables locales de 32 bits y 6 parámetros (2 excedentes)
mapear_stack_frame(num_variables_locales=3, num_parametros=6)
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Dada una función en C:
```c
void MiFuncion(int a, int b, int c, int d, int e, int f) {
    int local1;
    local1 = e + f;
}
```
Escribir el código en ensamblador ARM para acceder a las variables y calcular la suma en `local1`, asumiendo que ya se ha creado el Bloque de Activación.

**Solución:**
1.  **Identificar la ubicación de los argumentos**:
    *   Los parámetros `a`, `b`, `c` y `d` se pasan en los registros `R0`, `R1`, `R2` y `R3` respectivamente.
    *   Los parámetros `e` y `f` son excedentes (parámetros 5 y 6), por lo que se pasan en la pila. Se encuentran en `[FP, #8]` (`e`) y `[FP, #12]` (`f`).
2.  **Identificar la ubicación de la variable local**:
    *   La variable `local1` se almacena en el primer espacio de variables locales en la pila, direccionado en `[FP, #-8]`.
3.  **Escribir las instrucciones**:
    ```assembly
    LDR R0, [R11, #8]   ; Cargar parámetro 'e' en R0
    LDR R1, [R11, #12]  ; Cargar parámetro 'f' en R1
    ADD R2, R0, R1      ; R2 = e + f
    STR R2, [R11, #-8]  ; Guardar el resultado en 'local1'
    ```

---

## 6. Ejercicios Propuestos

1.  Explica razonadamente por qué el estándar de llamadas AAPCS obliga a que la pila del procesador ARM mantenga una **alineación de 8 bytes** de doble palabra al realizar llamadas a subrutinas externas.
2.  Un programador decide no usar el registro `FP` (R11) y direccionar todas las variables locales mediante desplazamientos relativos al puntero `SP`. Discute las ventajas e inconvenientes de esta práctica (conocida como *omitir el puntero de marco* o *frame pointer omission*).
3.  Dibuena un diagrama en papel de la disposición de memoria de un Bloque de Activación para una función que recibe 5 parámetros y declara 4 variables locales tipo entero de 32 bits.
