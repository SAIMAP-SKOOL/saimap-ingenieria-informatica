# Tema 5: Procesamiento de Datos y Control de Secuenciamiento en ARM

El núcleo funcional de cualquier CPU es la capacidad de procesar datos (operaciones aritméticas y lógicas) y alterar el flujo de ejecución del programa en función de las condiciones resultantes. La arquitectura ARM integra un sistema de ejecución condicional de instrucciones único en la industria.

---

## 1. Instrucciones de Procesamiento de Datos

Las instrucciones de procesamiento de datos operan exclusivamente en registros internos. Se pueden clasificar en:

### Operaciones Aritméticas
*   **ADD (Addition)**: Suma básica de dos operandos: `ADD R0, R1, R2` ($R0 = R1 + R2$).
*   **SUB (Subtraction)**: Resta básica: `SUB R0, R1, R2` ($R0 = R1 - R2$).
*   **RSB (Reverse Subtraction)**: Resta inversa: `RSB R0, R1, R2` ($R0 = R2 - R1$). Útil para restar registros de valores inmediatos.
*   **ADC / SBC (Addition/Subtraction with Carry)**: Suma o resta incorporando el bit de acarreo (`C`) del registro de estado CPSR. Permite realizar operaciones de precisión múltiple (ej. operar números de 64 bits).

### Operaciones Lógicas y de Manipulación de Bits
*   **AND**: Operación Y lógica bit a bit: `AND R0, R1, R2` ($R0 = R1 \land R2$).
*   **ORR**: Operación O lógica inclusiva: `ORR R0, R1, R2` ($R0 = R1 \lor R2$).
*   **EOR**: Operación O lógica exclusiva (XOR): `EOR R0, R1, R2` ($R0 = R1 \oplus R2$).
*   **BIC (Bit Clear)**: Limpia los bits del primer operando que están a 1 en el segundo operando: `BIC R0, R1, R2` ($R0 = R1 \land \neg R2$).

### Operaciones de Comparación (no guardan resultado en Rd, solo actualizan flags)
*   **CMP (Compare)**: Resta implícita ($Rn - \text{Operand2}$) que actualiza las banderas del CPSR.
*   **TST (Test)**: Y lógica implícita ($Rn \land \text{Operand2}$) que actualiza las banderas del CPSR.

---

## 2. Ejecución Condicional en ARM

En la mayoría de las arquitecturas clásicas, la ejecución condicional se limita únicamente a las instrucciones de salto. En ARM, **casi todas las instrucciones de procesamiento de datos y transferencia pueden ser condicionales** mediante la adición de un sufijo de dos letras que se corresponde con el estado de los flags del CPSR.

| Sufijo | Nombre | Flags Evaluados | Significado Aritmético |
|:---:|:---:|:---:|:---|
| **EQ** | Equal | $Z = 1$ | Igualdad / Resultado cero |
| **NE** | Not Equal | $Z = 0$ | Desigualdad / Resultado no cero |
| **CS / HS** | Carry Set / Higher or Same | $C = 1$ | Mayor o igual (sin signo) |
| **CC / LO** | Carry Clear / Lower | $C = 0$ | Menor (sin signo) |
| **MI** | Minus | $N = 1$ | Negativo |
| **PL** | Plus | $N = 0$ | Positivo o cero |
| **GT** | Greater Than | $Z = 0$ y $N = V$ | Mayor que (con signo) |
| **LT** | Less Than | $N \neq V$ | Menor que (con signo) |
| **GE** | Greater or Equal | $N = V$ | Mayor o igual que (con signo) |
| **LE** | Less or Equal | $Z = 1$ o $N \neq V$ | Menor o igual que (con signo) |

### Ejemplo práctico (evitando saltos):
*   En C:
    ```c
    if (R1 == R2) R0 = R0 + 1;
    else R0 = R0 - 1;
    ```
*   En Ensamblador ARM sin ejecución condicional en datos (usando saltos):
    ```assembly
        CMP R1, R2
        BNE sino
        ADD R0, R0, #1
        B fin
    sino:
        SUB R0, R0, #1
    fin:
    ```
*   En Ensamblador ARM optimizado (aprovechando la ejecución condicional):
    ```assembly
        CMP R1, R2
        ADDEQ R0, R0, #1
        SUBNE R0, R0, #1
    ```
Esta optimización elimina las penalizaciones por desalineación de cauce (*pipeline stalls*) provocadas por saltos condicionales en el procesador.

---

## 3. Instrucciones de Salto (Control de Secuenciamiento)

*   **B (Branch)**: Salto condicional o incondicional a una etiqueta relativa: `B etiqueta` o `BNE bucle`.
*   **BX (Branch and Exchange)**: Salto a una dirección contenida en un registro, permitiendo opcionalmente cambiar de estado de ejecución (ARM/Thumb): `BX LR`.

---

## 4. El Toque Informático

### Simulador de Ejecución Condicional de la ALU
El siguiente script en Python simula cómo la Unidad de Control de la CPU evalúa los sufijos condicionales leyendo el CPSR antes de permitir que la ALU ejecute una instrucción:

```python
class ALUConCondicionales:
    def __init__(self):
        # Flags CPSR simulados
        self.flags = {"N": 0, "Z": 0, "C": 0, "V": 0}
        self.r0 = 100
        
    def evaluar_condicion(self, sufijo):
        z, n, c, v = self.flags["Z"], self.flags["N"], self.flags["C"], self.flags["V"]
        if sufijo == "AL":  # Always
            return True
        elif sufijo == "EQ":  # Equal
            return z == 1
        elif sufijo == "NE":  # Not Equal
            return z == 0
        elif sufijo == "GT":  # Greater Than (signed)
            return z == 0 and n == v
        elif sufijo == "LT":  # Less Than (signed)
            return n != v
        return False

    def ejecutar_instruccion(self, cond_sufijo, op_name, valor):
        # Primero evaluamos si se cumple la condición
        debe_ejecutar = self.evaluar_condicion(cond_sufijo)
        
        if debe_ejecutar:
            if op_name == "ADD":
                self.r0 += valor
            elif op_name == "SUB":
                self.r0 -= valor
            print(f"Ejecutando: {op_name}{cond_sufijo} #{valor} -> R0 = {self.r0}")
        else:
            print(f"Omitiendo:  {op_name}{cond_sufijo} #{valor} (Condición no cumplida)")

# Simulación
alu = ALUConCondicionales()
# Simulamos un resultado previo donde R1 == R2 (Z = 1)
alu.flags["Z"] = 1

# Intentamos ejecutar dos instrucciones consecutivas
alu.ejecutar_instruccion("EQ", "ADD", 10)  # Debería ejecutarse
alu.ejecutar_instruccion("NE", "SUB", 5)   # Debería omitirse
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Dadas las instrucciones:
```assembly
CMP R0, R1
SUBGT R2, R2, #10
```
Si `R0 = 5` y `R1 = 8`, explica paso a paso qué ocurre durante la ejecución de la instrucción `SUBGT`.

**Solución:**
1.  **CMP R0, R1**: Realiza la operación $R0 - R1 \implies 5 - 8 = -3$.
    *   Como el resultado es diferente de cero, $Z=0$.
    *   Como el resultado es negativo y no hay desbordamiento de signo, $N=1$ y $V=0$.
2.  **SUBGT R2, R2, #10**: La instrucción tiene el sufijo `GT` (Greater Than - Mayor que con signo).
    *   La CPU evalúa la condición `GT`, que requiere que: $Z = 0$ y $N = V$.
    *   Comprobamos los flags: $Z = 0$ (se cumple) pero $N = 1$ y $V = 0 \implies N \neq V$ (no se cumple).
    *   Como la condición no es verdadera, la CPU **omite** la resta y actúa como un `NOP` (No Operación). El registro `R2` mantiene su valor original.

### Ejercicio 2
Explicar el funcionamiento de la instrucción `BIC R0, R1, #0xFF` e indicar cómo altera los bits de `R1`.

**Solución:**
La instrucción `BIC` (*Bit Clear*) borra los bits del primer registro fuente (`R1`) correspondientes a las posiciones donde el segundo operando (máscara) tiene un valor de `1`.
*   El operando `#0xFF` en binario de 32 bits es: `0x000000FF` (los 8 bits menos significativos están a 1, el resto a 0).
*   La operación lógica es: $R0 = R1 \land \neg(0x000000FF)$.
*   Por lo tanto, `BIC` pone a cero los 8 bits menos significativos de `R1` y mantiene intactos los 24 bits más significativos. El resultado final se guarda en `R0`.

---

## 6. Ejercicios Propuestos

1.  Dada la siguiente sección de código en C:
    ```c
    if (a < b) {
        c = c + 5;
    } else {
        c = c - 5;
    }
    ```
    Escribe su traducción equivalente en ensamblador ARM optimizando el código para evitar instrucciones de salto, asumiendo que `a` está en `R0`, `b` en `R1` y `c` en `R2`.
2.  Investiga el comportamiento de la instrucción `RSBS R0, R1, #0`. ¿Qué operación aritmética está realizando sobre `R1` y cuál es su efecto sobre las banderas del CPSR?
3.  Explica la diferencia entre las condiciones de comparación **sin signo** (ej: `HI`, `LO`) y **con signo** (ej: `GT`, `LT`). ¿Por qué la condición `GE` requiere evaluar el flag `V` (Overflow) además del flag `N`?
