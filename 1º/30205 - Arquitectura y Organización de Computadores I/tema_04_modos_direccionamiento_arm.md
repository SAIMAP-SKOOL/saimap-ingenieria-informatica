# Tema 4: Modos de Direccionamiento en ARM

Los modos de direccionamiento definen cómo el procesador calcula la ubicación del operando que va a utilizar en una instrucción. ARM destaca por ofrecer una gran flexibilidad en el direccionamiento de datos en registros (gracias a su Barrel Shifter integrado) y en el direccionamiento a memoria RAM (a través de opciones avanzadas de indexación).

---

## 1. Direccionamiento a Registros y Escalado (Barrel Shifter)

En las operaciones de procesamiento de datos, el operando secundario (`Operand2`) puede ser un registro modificado de forma transparente por el hardware del **Barrel Shifter** antes de pasar a la ALU, todo en un único ciclo de reloj.

*   **LSL (Logical Shift Left)**: Desplazamiento lógico a la izquierda (multiplica por potencias de 2). Rellena con ceros.
*   **LSR (Logical Shift Right)**: Desplazamiento lógico a la derecha (divide enteros sin signo entre potencias de 2).
*   **ASR (Arithmetic Shift Right)**: Desplazamiento aritmético a la derecha. Preserva el bit de signo (bit 31), ideal para división de enteros con signo.
*   **ROR (Rotate Right)**: Rotación de bits a la derecha (los bits que salen por la derecha vuelven a entrar por la izquierda).

### Ejemplo de sintaxis:
```assembly
ADD R0, R1, R2, LSL #3   ; R0 = R1 + (R2 * 8)
```

---

## 2. Modos de Direccionamiento a Memoria (LDR / STR)

Debido a la arquitectura Load/Store de ARM, el acceso a memoria se restringe a las instrucciones de carga (`LDR`) y almacenamiento (`STR`). Existen tres métodos principales para calcular la dirección de memoria efectiva a partir de un registro base (`Rn`) y un desplazamiento (*offset*):

### 1. Indexado Directo (Offset Addressing)
Se calcula la dirección sumando o restando el desplazamiento al registro base, pero el registro base **no se modifica** al final de la instrucción.
*   *Sintaxis*: `LDR R0, [R1, #4]`
*   *Operación*: Carga en `R0` el dato situado en la dirección `R1 + 4`. El registro `R1` permanece inalterado.

### 2. Pre-indexado (Pre-indexed Addressing)
Se calcula la dirección de acceso sumando el desplazamiento al registro base y, tras acceder a la memoria, el registro base **se actualiza** con la dirección calculada (se escribe el sufijo de *writeback* `!`).
*   *Sintaxis*: `LDR R0, [R1, #4]!`
*   *Operación*: Carga en `R0` el dato de la dirección `R1 + 4`, y actualiza `R1 = R1 + 4`.

### 3. Post-indexado (Post-indexed Addressing)
Se accede a la memoria utilizando **directamente** el valor inicial del registro base. Tras realizar el acceso, el registro base **se actualiza** de forma automática sumándole el desplazamiento.
*   *Sintaxis*: `LDR R0, [R1], #4`
*   *Operación*: Carga en `R0` el dato situado en la dirección `R1`, y posteriormente actualiza `R1 = R1 + 4`.

---

## 3. El Toque Informático

### Simulador de Modos de Direccionamiento y Memoria
El siguiente script en Python simula el comportamiento de una memoria RAM virtual y demuestra cómo la CPU procesa los diferentes modos de indexación (directo, pre-indexado con writeback y post-indexado):

```python
class MemoriaCPU:
    def __init__(self):
        # Memoria RAM vacía estructurada como diccionario
        self.ram = {0x1000: 99, 0x1004: 150, 0x1008: 200}
        self.r0 = 0
        self.r1 = 0x1000  # Registro base
        
    def reset_r1(self):
        self.r1 = 0x1000
        self.r0 = 0

    def simular_indexado_directo(self, offset):
        direccion_efectiva = self.r1 + offset
        self.r0 = self.ram.get(direccion_efectiva, 0)
        print("LDR R0, [R1, #offset]")
        print(f"  - Dirección de acceso: 0x{direccion_efectiva:04X}")
        print(f"  - Valor cargado en R0: {self.r0}")
        print(f"  - Estado final R1:     0x{self.r1:04X} (SIN MODIFICAR)")
        print("-" * 55)

    def simular_pre_indexado(self, offset):
        direccion_efectiva = self.r1 + offset
        self.r0 = self.ram.get(direccion_efectiva, 0)
        self.r1 = direccion_efectiva  # Writeback activo (!)
        print("LDR R0, [R1, #offset]!")
        print(f"  - Dirección de acceso: 0x{direccion_efectiva:04X}")
        print(f"  - Valor cargado en R0: {self.r0}")
        print(f"  - Estado final R1:     0x{self.r1:04X} (ACTUALIZADO)")
        print("-" * 55)

    def simular_post_indexado(self, offset):
        direccion_efectiva = self.r1
        self.r0 = self.ram.get(direccion_efectiva, 0)
        self.r1 = self.r1 + offset  # Actualización posterior
        print("LDR R0, [R1], #offset")
        print(f"  - Dirección de acceso: 0x{direccion_efectiva:04X}")
        print(f"  - Valor cargado en R0: {self.r0}")
        print(f"  - Estado final R1:     0x{self.r1:04X} (ACTUALIZADO POST)")
        print("-" * 55)

# Ejecución de simulación
sim = MemoriaCPU()
sim.simular_indexado_directo(4)

sim.reset_r1()
sim.simular_pre_indexado(4)

sim.reset_r1()
sim.simular_post_indexado(4)
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Dado el estado de registros `R0 = 0x00000000`, `R1 = 0x00002000` y sabiendo que la posición de memoria `0x00002004` contiene el valor `0x12345678`, determina el estado final de `R0` y `R1` tras ejecutar la instrucción:
`LDR R0, [R1, #4]!`

**Solución:**
1.  **Identificar el modo**: Contiene los corchetes que engloban al desplazamiento e incluye el signo de exclamación al final (`!`), por lo que es un modo **Pre-indexado con actualización (Writeback)**.
2.  **Calcular dirección de acceso**:
    $$\text{Dirección} = R1 + 4 = 0x2000 + 4 = 0x2004$$
3.  **Realizar el acceso y cargar**: Se lee el valor de `0x2004` (`0x12345678`) y se escribe en el registro destino `R0`.
    $$R0 = 0x12345678$$
4.  **Actualizar base**: Al ser pre-indexado con `!`, el registro base `R1` se actualiza con la dirección calculada:
    $$R1 = 0x00002004$$

Resultados finales: `R0 = 0x12345678`, `R1 = 0x00002004`.

### Ejercicio 2
Explicar qué utilidad práctica tiene el modo post-indexado (`LDR R0, [R1], #4`) a la hora de procesar elementos consecutivos de un vector.

**Solución:**
En un bucle que procesa un vector de enteros (donde cada entero ocupa 4 bytes), el modo post-indexado realiza dos tareas en una sola instrucción de máquina:
1.  Carga el elemento actual apuntado por el puntero `R1` en `R0` para procesarlo.
2.  Desplaza de forma automática el puntero `R1` sumándole 4 para que apunte directamente al siguiente entero del vector.

Esto reduce el número de instrucciones del bucle (evitando tener que añadir un `ADD R1, R1, #4` posterior), optimizando la velocidad del bucle y el uso de la memoria de programa.

---

## 5. Ejercicios Propuestos

1.  Dada la instrucción `STR R4, [R5], #-8`, describe qué operación se realiza y cómo queda el valor de `R5` tras el almacenamiento.
2.  Escribe la instrucción equivalente para calcular: $R0 = R1 - (R2 \cdot 16)$ utilizando el Barrel Shifter en una única instrucción.
3.  Si ejecutas `LDR R0, [R1, R2, LSL #2]`, explica paso a paso cómo calcula el procesador la dirección de acceso a memoria. ¿Cómo se denomina este modo de direccionamiento?
