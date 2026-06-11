# Tema 12: El Ciclo de Instrucción Paso a Paso

El ciclo de instrucción es el proceso secuencial repetitivo mediante el cual un computador extrae una instrucción de la memoria, determina qué acción requiere y la ejecuta. Para comprender a bajo nivel cómo opera el hardware, debemos modelar este ciclo utilizando el **Lenguaje de Transferencia entre Registros (RTL - Register Transfer Language)**, un formalismo matemático que describe el flujo detallado de datos entre los registros de la CPU en cada ciclo de reloj.

---

## 1. Fases del Ciclo de Instrucción

Cualquier instrucción de máquina, desde una suma básica hasta un salto condicional, recorre secuencialmente las siguientes fases:

```
    +-------------------------------------------------------+
    |                     1. FETCH (Búsqueda)               |
    |                   IR <- M[PC]; PC <- PC + 4           |
    +-------------------------------------------------------+
                                |
                                v
    +-------------------------------------------------------+
    |                 2. DECODE (Decodificación)            |
    |                Identificación del OPCode              |
    +-------------------------------------------------------+
                                |
                                v
    +-------------------------------------------------------+
    |              3. FETCH OPERANDS (Búsqueda Operandos)   |
    |               MAR <- Dirección del dato; MDR <- M     |
    +-------------------------------------------------------+
                                |
                                v
    +-------------------------------------------------------+
    |                     4. EXECUTE (Ejecución)            |
    |                   Resultado ALU <- Operando           |
    +-------------------------------------------------------+
                                |
                                v
    +-------------------------------------------------------+
    |                5. WRITE-BACK (Escritura / Guardar)    |
    |                  Registro destino <- Resultado        |
    +-------------------------------------------------------+
```

1.  **Búsqueda (Fetch)**: Se lee la instrucción apuntada por el PC desde la memoria RAM y se guarda en el Registro de Instrucción (IR).
2.  **Decodificación (Decode)**: La Unidad de Control analiza el código binario en el IR para identificar la operación (OPCode) y los modos de direccionamiento de los operandos.
3.  **Búsqueda de Operandos**: Si la instrucción requiere datos que residen en memoria, se calculan sus direcciones efectivas y se leen de la RAM.
4.  **Ejecución (Execute)**: La ALU realiza la operación indicada (aritmética o lógica).
5.  **Escritura (Write-Back)**: El resultado obtenido se escribe en el registro destino del banco de registros o en la memoria RAM.

---

## 2. Lenguaje de Transferencia entre Registros (RTL)

El RTL modela las microoperaciones temporales que ocurren dentro del procesador. Se utiliza la notación $M[\text{dirección}]$ para referirse a una lectura en la memoria física.

### Ejemplo: El Ciclo de Búsqueda (Fetch) en RTL
El ciclo Fetch es universal para todas las instrucciones y requiere tres ciclos de reloj (estados de tiempo $T_1, T_2, T_3$):
*   $T_1: MAR \leftarrow PC$ (Dirección al bus de direcciones).
*   $T_2: MDR \leftarrow M[MAR], \quad PC \leftarrow PC + 4$ (Lectura de memoria e incremento simultáneo del PC).
*   $T_3: IR \leftarrow MDR$ (Carga de la instrucción en el registro de decodificación).

---

## 3. Flujo en RTL para Instrucciones Típicas

Una vez terminada la fase de Fetch ($T_1 \text{ a } T_3$), la Unidad de Control ejecuta las fases restantes en los siguientes ciclos de reloj según la instrucción:

### 3.1 Instrucción de Carga (Load Word - `LW R1, [1000]`)
Lee el dato de la dirección de memoria `1000` y lo almacena en el registro `R1`:
*   $T_4: MAR \leftarrow \text{Dirección}(1000)$ (Se envía la dirección del dato).
*   $T_5: MDR \leftarrow M[MAR]$ (Se lee el dato de la memoria).
*   $T_6: R1 \leftarrow MDR$ (El dato se guarda en el banco de registros).

### 3.2 Instrucción de Suma Aritmética (`ADD R1, R2, R3`)
Suma el contenido de `R2` y `R3` y guarda el resultado en `R1` (operación puramente interna de la CPU):
*   $T_4: \text{Entrada\_A\_ALU} \leftarrow R2, \quad \text{Entrada\_B\_ALU} \leftarrow R3$ (Operandos a la ALU).
*   $T_5: R1 \leftarrow \text{Salida\_ALU}$ (Escritura del resultado en el registro de destino).

---

## 4. El Toque Informático

### El Reloj del Sistema y el Camino Crítico (Critical Path)
Cada microoperación descrita en RTL debe completarse de forma segura en exactamente un ciclo del reloj del procesador.
*   **El Camino Crítico (Critical Path)** es la ruta física de compuertas que tarda más tiempo en propagar sus señales electrónicas (típicamente, el camino que realiza una suma compleja en la ALU y escribe el resultado en memoria).
*   **Consecuencia**: La frecuencia de reloj máxima del procesador está limitada por el inverso del retardo del camino crítico:
    $$f_{\text{máx}} = \frac{1}{\text{Retardo del camino crítico}}$$
    Si intentamos overclockear (subir la frecuencia de reloj) más allá de este límite físico, las señales no llegarán a estabilizarse en los registros antes del siguiente flanco de reloj, provocando corrupción de datos y el temido pantallazo azul del sistema.

A continuación, implementamos en Python una simulación que traza paso a paso los valores de los registros internos (PC, MAR, MDR, IR y Banco de Registros) durante la ejecución de una instrucción de carga de memoria (`LW`).

```python
class SimuladorCicloInstruccion:
    def __init__(self):
        # RAM simulada: instrucciones en direcciones bajas, datos en direcciones altas
        self.ram = {
            0x1000: "LW R1, [0x2000]", # Instrucción en la dirección de PC
            0x2000: 99                 # Dato almacenado en memoria
        }
        self.PC = 0x1000
        self.MAR = 0
        self.MDR = 0
        self.IR = ""
        self.registros = {"R1": 0}
        
    def trazar_ejecucion(self):
        print(f"Estado Inicial: PC=0x{self.PC:04X}, R1={self.registros['R1']}\n")
        
        # T1: MAR <- PC
        self.MAR = self.PC
        print(f"T1: MAR <- PC                 | MAR = 0x{self.MAR:04X}")
        
        # T2: MDR <- M[MAR] e incrementamos PC
        self.MDR = self.ram[self.MAR]
        self.PC += 4
        print(f"T2: MDR <- M[MAR], PC <- PC+4 | MDR = '{self.MDR}', PC = 0x{self.PC:04X}")
        
        # T3: IR <- MDR
        self.IR = self.MDR
        print(f"T3: IR <- MDR                 | IR = '{self.IR}'\n--- Fin de Fetch ---\n")
        
        # T4 (Decode y Execute de LW R1, [0x2000]): MAR <- Dirección del dato
        direccion_dato = 0x2000
        self.MAR = direccion_dato
        print(f"T4: MAR <- Dirección del dato | MAR = 0x{self.MAR:04X}")
        
        # T5: MDR <- M[MAR]
        self.MDR = self.ram[self.MAR]
        print(f"T5: MDR <- M[MAR]             | MDR = {self.MDR}")
        
        # T6: R1 <- MDR
        self.registros["R1"] = self.MDR
        print(f"T6: R1 <- MDR                 | R1 = {self.registros['R1']}")
        
        print(f"\nEstado Final: PC=0x{self.PC:04X}, R1={self.registros['R1']} (Correcto)")

# Correr simulación
sim = SimuladorCicloInstruccion()
sim.trazar_ejecucion()
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Escribir las microoperaciones detalladas en lenguaje RTL para la ejecución completa de una instrucción de escritura en memoria: `SW R1, [2000]` (Store Word: guarda el contenido del registro `R1` en la dirección de memoria `2000`). Asumir que la fase Fetch ($T_1\text{-}T_3$) ya ha concluido.

**Solución:**
Una vez cargada la instrucción en el IR, las fases de la instrucción `SW` en los siguientes ciclos de reloj son:
*   $T_4: MAR \leftarrow \text{Dirección}(2000)$ (Se coloca la dirección destino en el registro MAR conectado al bus de direcciones).
*   $T_5: MDR \leftarrow R1$ (Se copia el contenido del registro de datos `R1` al MDR conectado al bus de datos).
*   $T_6: M[MAR] \leftarrow MDR$ (La Unidad de Control activa la señal de escritura `Write` en el bus de control, ordenando a la memoria RAM capturar el dato del bus de datos e introducirlo en la celda direccionada).

### Ejercicio 2
Un procesador funciona a una frecuencia de reloj de $2 \, \text{GHz}$ (duración del ciclo de reloj $T = 0.5 \, \text{ns}$). Los retardos de conmutación física de sus componentes son: acceso a memoria = $0.4 \, \text{ns}$, cálculo en ALU = $0.25 \, \text{ns}$, acceso al banco de registros = $0.1 \, \text{ns}$. Determinar cuál de estos componentes limita la frecuencia máxima del reloj si quisiéramos acelerar el procesador.

**Solución:**
El componente que limita la frecuencia es aquel que tiene el mayor retardo de propagación, ya que define el camino crítico de la instrucción:
1.  El retardo del componente más lento es el de **Acceso a Memoria ($0.4 \, \text{ns}$)**.
2.  Para que una microoperación de acceso a memoria se complete de forma estable, el ciclo de reloj $T$ debe ser mayor que el retardo de dicho componente:
    $$T \ge 0.4 \, \text{ns}$$
3.  La frecuencia máxima teórica permitida es:
    $$f_{\text{máx}} = \frac{1}{0.4 \times 10^{-9} \, \text{s}} = 2.5 \times 10^9 \, \text{Hz} = 2.5 \, \text{GHz}$$

Por lo tanto, la velocidad de reloj no puede superar los $2.5 \, \text{GHz}$ a menos que se optimice el tiempo de acceso a la memoria (por ejemplo, introduciendo memorias caché SRAM más rápidas).

---

## 6. Ejercicios Propuestos

1.  Escribe las microoperaciones en lenguaje RTL para el ciclo Fetch de una CPU cuyo tamaño de instrucción en memoria es de 2 bytes (16 bits) en lugar de 4 bytes.
2.  Deduce el flujo en RTL para una instrucción de bifurcación incondicional `JUMP 0x1500` (salto de instrucción, que modifica directamente el flujo de ejecución copiando el valor de destino al PC).
3.  Explica cómo implementa la Unidad de Control la fase de Decodificación (Decode) utilizando un decodificador binario conectado a los bits superiores del Registro de Instrucción (IR).
