# Tema 6: Traducción de Estructuras de Control a Ensamblador ARM

En la programación ordinaria en lenguajes de alto nivel, el programador emplea condicionales (`if-else`) y bucles (`for`, `while`) para guiar la ejecución. Sin embargo, a nivel de máquina no existen tales construcciones complejas. La CPU ejecuta instrucciones de manera secuencial a menos que se fuerce una alteración explícita de la secuencia mediante saltos y comparaciones.

---

## 1. Traducción de Condicionales (if-else)

La estructura condicional `if-else` se traduce utilizando una combinación de comparación (`CMP`), saltos condicionales y etiquetas de salto.

### Estructura General
*   **En C**:
    ```c
    if (condicion) {
        // Bloque IF
    } else {
        // Bloque ELSE
    }
    ```
*   **En Ensamblador ARM (Estructura clásica)**:
    ```assembly
        CMP R0, R1          ; Comparación de variables
        B_condicion_falsa else_label
        ; --- Bloque IF ---
        ; código si es verdadero
        B fin_if
    else_label:
        ; --- Bloque ELSE ---
        ; código si es falso
    fin_if:
    ```

---

## 2. Traducción de Bucles (while y for)

Los bucles se implementan ejecutando repetidamente un bloque de código y realizando una comparación condicional al final o al inicio de cada iteración.

### El Bucle `while` (Pre-test)
*   **En C**:
    ```c
    while (R0 < 10) {
        R0++;
    }
    ```
*   **En Ensamblador ARM**:
    ```assembly
    bucle_while:
        CMP R0, #10         ; Evaluar condición
        BGE fin_while       ; Si R0 >= 10, salimos del bucle
        ADD R0, R0, #1      ; Cuerpo del bucle: R0 = R0 + 1
        B bucle_while       ; Volver a evaluar
    fin_while:
    ```

### El Bucle `for` (Contador)
*   **En C**:
    ```c
    for (int i = 0; i < 100; i++) {
        suma += i;
    }
    ```
*   **En Ensamblador ARM (Asumiendo i en R0, suma en R1)**:
        `MOV R0, #0` (Inicialización de `i = 0`).
    ```assembly
    bucle_for:
        CMP R0, #100        ; Comparar i con 100
        BGE fin_for         ; Si i >= 100, salir
        ADD R1, R1, R0      ; suma = suma + i
        ADD R0, R0, #1      ; i++
        B bucle_for         ; Siguiente iteración
    fin_for:
    ```

---

## 3. Vectores, Tablas y Direccionamiento de Arrays

Para manipular un array en ensamblador, debemos calcular la dirección de memoria exacta de cada elemento en función de su índice y del tamaño físico del tipo de datos (en bytes):
$$\text{Dirección del elemento } i = \text{Dirección Base del Array} + (i \cdot \text{Tamaño en bytes del elemento})$$

*   **Tipos de datos de tamaño fijo**:
    *   `char` (1 byte): Desplazamiento por índice directo.
    *   `short` (2 bytes): Desplazamiento por `índice * 2`.
    *   `int` o `float` (4 bytes): Desplazamiento por `índice * 4`.

### Bucle de recorrido de un vector en ARM (con post-indexado)
*   **En C**:
    ```c
    int vector[5] = {10, 20, 30, 40, 50};
    int suma = 0;
    for (int i = 0; i < 5; i++) {
        suma += vector[i];
    }
    ```
*   **En Ensamblador ARM (Asumiendo R1 = Puntero a vector, R2 = suma, R3 = contador i)**:
        `MOV R2, #0` (suma = 0), `MOV R3, #5` (contador de iteraciones = 5).
    ```assembly
    bucle_vector:
        SUBS R3, R3, #1     ; Decrementar contador y actualizar flags
        BMI fin_vector      ; Si R3 < 0, salir del bucle
        LDR R0, [R1], #4    ; Cargar elemento actual y avanzar puntero (Post-indexado)
        ADD R2, R2, R0      ; suma += elemento
        B bucle_vector
    fin_vector:
    ```

---

## 4. El Toque Informático

### Traductor de Bucles de Alto Nivel a Ensamblador ARM
El siguiente script en Python simula un mini-compilador que recibe las propiedades de un bucle `for` y genera de forma automática el bloque correspondiente en código ensamblador ARMv7:

```python
def generar_codigo_bucle_arm(var_ind, limite, paso, registro_indice, registro_suma):
    # Genera la estructura de código ensamblador para un bucle for clásico
    print(f"; --- Bucle generado automáticamente para variable '{var_ind}' ---")
    print(f"        MOV {registro_indice}, #0          ; Inicializar {var_ind} = 0")
    print("bucle_for:")
    print(f"        CMP {registro_indice}, #{limite}        ; Comparar con límite")
    print("        BGE fin_for                 ; Si es mayor o igual, salir")
    print(f"        ADD {registro_suma}, {registro_suma}, {registro_indice} ; Sumar valor")
    print(f"        ADD {registro_indice}, {registro_indice}, #{paso}   ; Incremento")
    print("        B bucle_for                 ; Siguiente ciclo")
    print("fin_for:")
    print("; -------------------------------------------------------------")

# Generar bucle: for (int i = 0; i < 10; i++)
generar_codigo_bucle_arm("i", 10, 1, "R0", "R1")
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Traducir a ensamblador ARM la siguiente estructura condicional en alto nivel:
```c
if (R0 > 25) {
    R1 = R1 + 10;
} else {
    R1 = R1 - 10;
}
```

**Solución:**
Podemos hacerlo mediante dos enfoques:
1.  **Enfoque de Salto Clásico (Ideal para bloques de código largos)**:
    ```assembly
        CMP R0, #25         ; Comparar R0 con 25
        BLE else_block      ; Si R0 <= 25, saltar al bloque else
        ADD R1, R1, #10     ; Bloque if: R1 = R1 + 10
        B end_if
    else_block:
        SUB R1, R1, #10     ; Bloque else: R1 = R1 - 10
    end_if:
    ```

2.  **Enfoque de Ejecución Condicional (Ideal para optimización de pocas líneas)**:
    ```assembly
        CMP R0, #25         ; Comparar R0 con 25
        ADDGT R1, R1, #10   ; R1 = R1 + 10 si R0 > 25
        SUBLE R1, R1, #10   ; R1 = R1 - 10 si R0 <= 25
    ```

### Ejercicio 2
Un programador tiene la base de un vector de enteros en `R0` y quiere acceder al elemento `vector[3]`. Describe la instrucción ensamblador optimizada para cargar este elemento en `R1` en una única instrucción aprovechando los modos de direccionamiento.

**Solución:**
*   Cada elemento `int` del vector ocupa 4 bytes en memoria.
*   El elemento `vector[3]` se sitúa en un desplazamiento de $3 \times 4 = 12$ bytes desde la dirección base en `R0`.
*   La instrucción óptima es:
    `LDR R1, [R0, #12]`
Esta instrucción suma 12 a `R0`, lee la posición resultante en la memoria RAM y almacena el entero de 32 bits cargado en `R1`. El puntero base `R0` no se modifica.

---

## 6. Ejercicios Propuestos

1.  Escribe el código ensamblador ARM equivalente al siguiente bucle `while`:
    ```c
    while (R1 != 0) {
        R2 = R2 + 2;
        R1 = R1 - 1;
    }
    ```
2.  Dado un vector de bytes (`char`) cuya dirección base está en `R1`, escribe las instrucciones necesarias para recorrer e inicializar los primeros 10 elementos a cero.
3.  Explica qué problema de rendimiento ocurre en los microprocesadores modernos con cauce segmentado (pipelined CPUs) al procesar instrucciones de salto (`B`) y por qué la ejecución condicional de datos ayuda a solventarlo.
