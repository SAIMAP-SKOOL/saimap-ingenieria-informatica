# Tema 8: La Pila (Stack) y la Preservación del Contexto en ARM

La pila (*Stack*) es una estructura de datos de tipo LIFO (Last-In, First-Out: el último elemento en entrar es el primero en salir) gestionada directamente por el hardware del procesador mediante el registro **Stack Pointer (SP / R13)**. La pila es vital para implementar subrutinas, almacenar variables locales y preservar el estado interno de los registros de la CPU.

---

## 1. Clasificación de Pilas y el Estándar de ARM

Una pila en memoria se puede clasificar según dos criterios independientes:

### Según la dirección de crecimiento de la memoria:
*   **Ascendente (Ascending)**: La pila crece hacia direcciones de memoria más altas (se suma al SP al apilar).
*   **Descendente (Descending)**: La pila crece hacia direcciones de memoria más bajas (se resta al SP al apilar).

### Según a dónde apunta el puntero SP:
*   **Vacía (Empty)**: `SP` apunta a la primera posición vacía (libre) de la pila.
*   **Llena (Full)**: `SP` apunta al último elemento realmente almacenado en la pila.

### El Estándar ARM: Pila Llena Descendente (Full Descending - FD)
Por convención, el estándar de llamadas de la arquitectura ARM (AAPCS) establece que la pila es **Full Descending (FD)**:
*   Al realizar un **PUSH (apilar)**: El puntero `SP` se decrementa primero en 4 bytes y luego se escribe el dato en la dirección resultante.
*   Al realizar un **POP (desapilar)**: Se lee el dato de la dirección actual apuntada por `SP` y posteriormente el puntero `SP` se incrementa en 4 bytes.

```
 Dirección Alta   |                  |
                  |------------------|
                  | Dato Existente   | 
                  |------------------|
                  | Último Dato      | <-- SP Inicial (Pila Llena)
                  |------------------|
                  | [Siguiente PUSH] | <-- SP se decrementará a esta posición
 Dirección Baja   |                  |
```

---

## 2. Instrucciones PUSH y POP en ARM

ARM no dispone de instrucciones de apilado unitario elementales. En su lugar, utiliza instrucciones de transferencia múltiple extremadamente potentes, que admiten alias nemotécnicos:

*   **PUSH {R4, R5, LR}**: Apila el conjunto de registros especificado. Su instrucción máquina equivalente en pila FD es:
    `STMDB SP!, {R4, R5, LR}` (*Store Multiple Decrement Before* - Decrementa el puntero antes de almacenar).
*   **POP {R4, R5, PC}**: Desapila los registros y transfiere el retorno directamente al contador de programa. Su equivalente físico es:
    `LDMIA SP!, {R4, R5, PC}` (*Load Multiple Increment After* - Lee la memoria e incrementa el puntero después).

*Nota: El signo de exclamación `!` indica actualización obligatoria (writeback) del registro base `SP` tras la operación.*

---

## 3. Preservación del Contexto

Para que las subrutinas no interfieran entre sí, se define un acuerdo de preservación de registros (contexto del procesador):
*   **Registros no volátiles (callee-saved)**: Los registros `R4-R11` y `SP` deben conservar su valor al retornar de la función. Si la subrutina necesita utilizarlos, debe apilarlos en el prólogo y restaurarlos en el epílogo.
*   **Registros volátiles (caller-saved)**: `R0-R3`, `R12` y `LR` pueden ser destruidos libremente por la subrutina.

```assembly
MiSubrutina:
    PUSH {R4, R5, R11, LR}  ; Prólogo: Guardar contexto y dirección de retorno
    ; ... cuerpo de la subrutina utilizando R4 y R5 ...
    POP {R4, R5, R11, PC}   ; Epílogo: Restaurar contexto y retornar
```

---

## 4. El Toque Informático

### Simulador de Pila Llena Descendente (Full Descending Stack)
El siguiente script en Python simula el comportamiento detallado del mapa de memoria RAM de la pila y del puntero `SP` durante operaciones consecutivas de PUSH y POP:

```python
class StackSimulator:
    def __init__(self):
        self.ram = {}
        self.sp = 0x8000  # Dirección de inicio alta de la pila
        
    def push(self, valor):
        # En pila FD: Primero decrementar SP, luego guardar
        self.sp -= 4
        self.ram[self.sp] = valor
        print(f"PUSH {valor} -> SP se decrementa a 0x{self.sp:04X} | Guardado [{valor}]")
        self.print_stack()

    def pop(self):
        # En pila FD: Primero leer de SP, luego incrementar SP
        if self.sp >= 0x8000:
            print("[ERROR] Stack Underflow: La pila está vacía.")
            return None
        valor = self.ram[self.sp]
        del self.ram[self.sp]  # Limpiar simulación
        self.sp += 0x0004
        print(f"POP -> Leído [{valor}] desde SP | SP se incrementa a 0x{self.sp:04X}")
        self.print_stack()
        return valor
        
    def print_stack(self):
        print("ESTADO ACTUAL DE LA PILA:")
        for addr in sorted(self.ram.keys(), reverse=True):
            marker = " <-- SP" if addr == self.sp else ""
            print(f"  [0x{addr:04X}]: {self.ram[addr]}{marker}")
        if self.sp == 0x8000:
            print("  [0x8000]: (CIMA DE PILA VACÍA) <-- SP")
        print("-" * 55)

# Ejecución
pila = StackSimulator()
pila.push(42)
pila.push(99)
pila.pop()
pila.pop()
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Dado un estado inicial de la pila con `SP = 0x00007FC0`. Si ejecutamos la instrucción `PUSH {R0, R1, R2}`, determina:
1.  El valor final del puntero de pila `SP`.
2.  Las direcciones de memoria exactas ocupadas por cada uno de los registros en una pila Full Descending.

**Solución:**
1.  **Cálculo del SP final**:
    *   La instrucción apila 3 registros (`R0`, `R1` y `R2`). Cada registro ocupa 4 bytes.
    *   El tamaño total es $3 \times 4 = 12$ bytes.
    *   Al ser descendente:
        $$\text{SP final} = 0x7FC0 - 12 = 0x7FC0 - 0x000C = 0x00007FB4$$

2.  **Distribución de registros en memoria**:
    En ARM, las instrucciones de almacenamiento múltiple (`STM`) guardan los registros en memoria de forma que **el registro con menor índice se almacena en la dirección más baja** y el de mayor índice en la más alta:
    *   Dirección `0x00007FB4`: Guarda `R0` (Dirección final de `SP`).
    *   Dirección `0x00007FB8`: Guarda `R1`.
    *   Dirección `0x00007FBC`: Guarda `R2`.

### Ejercicio 2
Explicar qué ocurre si dentro de una subrutina realizamos un `PUSH {R4, LR}` pero al finalizar ejecutamos un `POP {R4, LR}` seguido de `BX LR`. ¿Es correcto? ¿Existe alguna alternativa más eficiente?

**Solución:**
La secuencia es funcionalmente correcta: se guarda el contexto y retorno, se restaura y se salta a `LR`. Sin embargo, es **ineficiente** porque requiere dos instrucciones de control de flujo al final.
La alternativa estándar de la arquitectura ARM consiste en hacer:
`POP {R4, PC}`
Esta instrucción desapila el contenido original de `R4` de vuelta a `R4` y, al mismo tiempo, extrae la dirección de retorno que estaba en la pila y la carga directamente en el Program Counter (`PC`). Esto realiza la restauración y el salto de retorno en un único ciclo de instrucción máquina.

---

## 6. Ejercicios Propuestos

1.  Diferencia razonadamente el comportamiento de una pila **Full Descending (FD)** frente a una pila **Empty Descending (ED)** al realizar una operación de apilado.
2.  Dado un programa que realiza un bucle infinito de PUSH sin realizar nunca un POP, describe qué tipo de fallo de memoria ocurrirá y por qué los sistemas operativos limitan el tamaño de la pila de los programas.
3.  Escribe el prólogo y epílogo en ensamblador ARM para una subrutina que utiliza los registros de trabajo `R4`, `R5` y `R6`, asegurando la correcta preservación del contexto y retorno seguro.
