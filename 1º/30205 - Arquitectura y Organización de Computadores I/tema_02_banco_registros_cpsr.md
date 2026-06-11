# Tema 2: Banco de Registros y Registro de Estado (CPSR) en ARM

El diseño interno de un procesador se expone al programador a través de su **Modelo de Programación**, constituido por el banco de registros internos y el registro de estado. En la arquitectura ARM de 32 bits, el programador tiene acceso directo a 16 registros principales y a un registro especial de control.

---

## 1. El Banco de Registros de ARM (Modo Usuario)

En el modo de ejecución normal (Modo Usuario), el procesador ARM dispone de **16 registros visibles** de 32 bits, denominados de `R0` a `R15`, más el registro de estado `CPSR`.

| Registro | Alias | Función Principal |
|:---:|:---:|:---|
| **R0 - R3** | - | Registros de propósito general. Se usan para pasar parámetros a funciones y retornar valores. |
| **R4 - R10** | - | Registros de propósito general. Conservan su valor tras llamadas a subrutinas (AAPCS). |
| **R11** | **FP** | *Frame Pointer* (Puntero de Marco). Opcional, marca el inicio del bloque de activación en la pila. |
| **R12** | **IP** | *Intra-Procedure-call scratch register*. Registro temporal usado por el enlazador dinámico. |
| **R13** | **SP** | *Stack Pointer* (Puntero de Pila). Almacena la dirección de memoria del elemento en la cima de la pila. |
| **R14** | **LR** | *Link Register* (Registro de Enlace). Guarda la dirección de retorno al llamar a una subrutina. |
| **R15** | **PC** | *Program Counter* (Contador de Programa). Apunta a la dirección de la instrucción en ejecución (con pipeline, apunta a $PC+8$). |

---

## 2. El Registro de Estado CPSR (Current Program Status Register)

El `CPSR` es un registro dedicado de 32 bits que contiene información sobre el estado actual del procesador. Se divide en campos de flags y de control:

```
 Bit  31  30  29  28  ...  7   6   5   4   3   2   1   0
     [ N | Z | C | V ] ... [ I | F | T | M4| M3| M2| M1| M0]
     \______Flags_____/     \___________Control___________/
```

### Los Flags de Condición (Bits 28 a 31)
Son modificados de forma automática por instrucciones aritméticas/lógicas específicas (si añadimos el sufijo `S`, ej: `ADDS`, `SUBS`) o por instrucciones de comparación (`CMP`, `TST`).
*   **N (Negative - Bit 31)**: Se activa ($N=1$) si el resultado de la última operación fue negativo (el bit 31 del resultado es 1).
*   **Z (Zero - Bit 30)**: Se activa ($Z=1$) si el resultado de la última operación fue exactamente cero.
*   **C (Carry - Bit 29)**: Se activa ($C=1$) si hubo acarreo en una suma sin signo (el resultado supera $2^{32}-1$) o si no hubo necesidad de préstamo (*borrow*) en una resta (es decir, el minuendo fue mayor o igual que el sustraendo).
*   **V (oVerflow - Bit 28)**: Se activa ($V=1$) si se produce un desbordamiento aritmético al operar con números con signo (ej. sumar dos números positivos y obtener un resultado negativo).

### Bits de Control (Bits 0 a 7)
*   **I y F (Interrupt Masks)**: Deshabilitan las interrupciones externas normales (IRQ) e interrupciones rápidas (FIQ) cuando valen 1.
*   **T (State bit)**: Define si el procesador está ejecutando en modo ARM (32 bits, $T=0$) o modo Thumb (16 bits, $T=1$).
*   **M[4:0] (Mode bits)**: Indican el modo de operación actual del procesador (Usuario, System, FIQ, IRQ, Supervisor, Abort, Undefined). El cambio de modo permite el aislamiento de recursos entre el software de usuario y el núcleo del sistema operativo.

---

## 3. El Toque Informático

### Simulador del Estado de Registros y Flags
Para comprender cómo las instrucciones máquina modifican los flags de la CPU, implementamos este simulador interactivo en Python que modela la ALU del procesador ARM actualizando el banco de registros y el CPSR:

```python
class ARMCPU:
    def __init__(self):
        # 16 registros de 32 bits (R0-R15)
        self.registers = [0] * 16
        # Flags CPSR: N, Z, C, V (inicializados en 0)
        self.flags = {"N": 0, "Z": 0, "C": 0, "V": 0}
        
    def print_state(self):
        print("=== BANCO DE REGISTROS ===")
        for r in range(0, 16, 4):
            print(f"R{r:02d}: 0x{self.registers[r]:08X} | R{r+1:02d}: 0x{self.registers[r+1]:08X} | "
                  f"R{r+2:02d}: 0x{self.registers[r+2]:08X} | R{r+3:02d}: 0x{self.registers[r+3]:08X}")
        print("FLAGS CPSR:")
        print(f"  N (Negativo): {self.flags['N']} | Z (Cero): {self.flags['Z']} | "
              f"C (Acarreo):  {self.flags['C']} | V (Desbordamiento): {self.flags['V']}")
        print("=" * 55)

    def ejecutar_subs(self, rd, rn, rm_val):
        # Operación: Rd = Rn - Rm_val
        rn_val = self.registers[rn]
        resultado = (rn_val - rm_val) & 0xFFFFFFFF
        self.registers[rd] = resultado
        
        # Actualizar flags condicionales (simulación de SUBS)
        # 1. Flag Z
        self.flags["Z"] = 1 if resultado == 0 else 0
        # 2. Flag N
        self.flags["N"] = 1 if (resultado & 0x80000000) != 0 else 0
        # 3. Flag C (En restas en ARM, C=1 si no hay préstamo, es decir, Rn >= Rm)
        self.flags["C"] = 1 if rn_val >= rm_val else 0
        # 4. Flag V (Desbordamiento de signo)
        # Ocurre si restamos signos opuestos y el resultado tiene signo incorrecto
        signo_rn = (rn_val & 0x80000000) != 0
        signo_rm = (rm_val & 0x80000000) != 0
        signo_res = (resultado & 0x80000000) != 0
        if signo_rn != signo_rm and signo_res != signo_rn:
            self.flags["V"] = 1
        else:
            self.flags["V"] = 0

# Inicialización y simulación de la instrucción: SUBS R0, R1, R2
cpu = ARMCPU()
cpu.registers[1] = 10  # R1 = 10
cpu.registers[2] = 10  # R2 = 10
print("Antes de ejecutar SUBS R0, R1, R2:")
cpu.print_state()

cpu.ejecutar_subs(rd=0, rn=1, rm_val=cpu.registers[2])
print("Después de ejecutar SUBS R0, R1, R2 (resultado 0):")
cpu.print_state()
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Indicar la función específica del registro `R14 (LR)` y explicar qué ocurre cuando una función secundaria llama a una tercera (llamadas anidadas) sin salvar este registro.

**Solución:**
El `R14` o `LR` (Link Register) almacena de forma automática la dirección de memoria a la que la CPU debe retornar tras terminar de ejecutar la subrutina actual. Al ejecutar la instrucción `BL subrutina`, el hardware guarda la dirección de la siguiente instrucción en `LR`.

Si hay **llamadas anidadas**:
1.  La función `Principal` ejecuta `BL FuncionA`. El hardware guarda la dirección de retorno a `Principal` en `LR`.
2.  `FuncionA` ejecuta `BL FuncionB`. El hardware sobrescribe `LR` con la dirección de retorno a `FuncionA`.
3.  Al finalizar `FuncionB`, retorna haciendo `MOV PC, LR` (volviendo con éxito a `FuncionA`).
4.  Cuando `FuncionA` termina e intenta retornar haciendo `MOV PC, LR`, se produce un **bucle infinito** porque `LR` todavía contiene la dirección de retorno a `FuncionA`, no a `Principal`. Para evitar esto, `FuncionA` debe guardar `LR` en la pila (Stack) al inicio y recuperarlo al final.

### Ejercicio 2
Si ejecutamos una resta que da como resultado un número negativo, ¿cómo cambian las banderas `N` y `Z` en el registro de estado `CPSR`?

**Solución:**
*   **Z (Zero)**: Se pondrá en `0`, ya que el resultado es diferente de cero.
*   **N (Negative)**: Se pondrá en `1`, porque el bit más significativo (bit 31) del resultado en binario de 32 bits estará a 1, indicando que representa un valor negativo bajo complemento a 2.

---

## 5. Ejercicios Propuestos

1.  Dada la instrucción `CMP R0, R1`, explica detalladamente qué operación matemática realiza la CPU internamente y si modifica el registro de destino `R0`.
2.  Analiza la diferencia en la modificación de banderas al ejecutar `ADD R0, R1, R2` frente a `ADDS R0, R1, R2`. ¿Por qué por defecto ARM no actualiza las banderas del CPSR en todas sus instrucciones de datos?
3.  Investiga el papel de los modos de ejecución del procesador ARM. ¿Por qué el código del sistema operativo se ejecuta en modo Supervisor (SVC) y el código de las aplicaciones en modo Usuario (USR)?
