# Tema 11: Organización del Procesador: Ruta de Datos y Unidad de Control

La Unidad Central de Procesamiento (CPU) es el cerebro del computador encargado de buscar, decodificar y ejecutar las instrucciones de los programas. Internamente, la CPU se organiza dividiendo sus funciones en dos grandes estructuras: la **Ruta de Datos (Datapath)**, que actúa como el cuerpo físico (realiza las transferencias y operaciones lógicas sobre los operandos), y la **Unidad de Control (UC)**, que actúa como el cerebro director (secuencia y activa los caminos correctos de la ruta de datos).

---

## 1. El Camino de Datos (Datapath) y sus Registros

La Ruta de Datos contiene todos los elementos que almacenan y procesan los operandos numéricos:

```
                      DATAPATH
                     +-----------------------------------+
                     |          Banco Registros          |
                     |           (R0, R1, ...)           |
                     +-----------------------------------+
                        |                             |
                        v                             v
                     +-----------------------------------+
                     |               A L U               |
                     +-----------------------------------+
                        |                             |
                        v                             v
                     ===================================== BUS INTERNO CPU
```

*   **La ALU**: Ejecuta las operaciones sobre los operandos de entrada.
*   **Banco de Registros (Register File)**: Conjunto de registros de alta velocidad direccionables por el programador (por ejemplo, registros `$s0`, `$t0` en MIPS) para retener operandos temporales.
*   **Registro de Flags**: Almacena las señales de estado ($Z, S, V, C$) producidas por la última operación de la ALU.

---

## 2. Los Registros de Control e Interfaz de Bus

Son registros internos especiales de la CPU, invisibles para el programador de alto nivel, indispensables para orquestar la comunicación con la memoria principal a través del bus del sistema:

1.  **Contador de Programa (PC - Program Counter)**: Almacena la dirección de memoria de la **siguiente** instrucción que debe leerse de memoria. Se incrementa automáticamente tras cada lectura.
2.  **Registro de Instrucción (IR - Instruction Register)**: Retiene el código binario de la instrucción que la CPU está ejecutando en ese momento.
3.  **Registro de Direcciones de Memoria (MAR - Memory Address Register)**: Conectado físicamente al bus de direcciones. Almacena la dirección exacta a la que la CPU desea acceder en memoria para leer o escribir.
4.  **Registro de Datos de Memoria (MDR - Memory Data Register)**: Conectado físicamente al bus de datos. Actúa como buffer bidireccional; retiene el dato leído de memoria o el dato listo para escribirse en ella.

---

## 3. La Unidad de Control (UC)

La Unidad de Control interpreta la instrucción guardada en el IR y genera las señales lógicas que controlan el flujo del dato. Existen dos filosofías de diseño de la UC:

### 3.1 Unidad de Control Cableada
Se implementa utilizando circuitos combinacionales puros (decodificadores, puertas lógicas AND/OR, flip-flops de estado).
*   **Ventaja**: Es extremadamente rápida. Minimiza el retardo de conmutación.
*   **Inconveniente**: Es muy rígida. Si queremos añadir una nueva instrucción a la CPU, debemos rediseñar físicamente todo el circuito integrado. Típica en arquitecturas RISC (como ARM, MIPS).

### 3.2 Unidad de Control Microprogramada
Las señales lógicas no se cablean. En su lugar, se almacenan en una memoria ROM de control interna en forma de "microprogramas". Cada instrucción del procesador se traduce en una secuencia de microinstrucciones leídas de la ROM interna.
*   **Ventaja**: Es extremadamente flexible. Permite corregir fallos o añadir instrucciones actualizando la microprogramación de la ROM interna.
*   **Inconveniente**: Es más lenta por requerir accesos de memoria internos. Típica en arquitecturas complejas CISC (como x86).

---

## 4. El Toque Informático

### Segmentación del Cauce (Pipelining) en CPUs Modernas
En los microprocesadores comerciales, las instrucciones no se procesan de forma estrictamente secuencial de principio a fin.
*   Se utiliza la técnica de **Segmentación (Pipelining)**, similar a una cadena de montaje de automóviles.
*   La CPU se divide en etapas independientes (ej. 5 etapas: Fetch, Decode, Execute, Memory, Write-back).
*   Mientras una instrucción está ejecutándose en la ALU (etapa Execute), la siguiente instrucción se está decodificando en la etapa Decode, y una tercera se está leyendo de memoria en la etapa Fetch.

Para que esta segmentación funcione sin colisiones, la CPU integra **Registros de Segmentación (Pipeline Registers)** intermedios entre cada etapa. Estos registros retienen de forma síncrona el estado parcial de la instrucción para entregárselo a la siguiente etapa en el flanco de reloj, permitiendo procesar una instrucción por ciclo de reloj de forma efectiva.

A continuación, implementamos en Python una simulación lógica del estado de los registros internos de la CPU (PC, IR, MAR, MDR) durante la carga de un registro.

```python
class CPUState:
    def __init__(self):
        # Registros accesibles al programador
        self.registros = {"R0": 0, "R1": 0, "R2": 0}
        
        # Registros de control internos
        self.PC = 0x1000 # Dirección inicial del programa
        self.IR = None
        self.MAR = 0x0
        self.MDR = 0x0
        
    def imprimir_estado(self):
        print(f"Estado Registros CPU: PC=0x{self.PC:04X}, IR={self.IR}, MAR=0x{self.MAR:04X}, MDR=0x{self.MDR:04X}")
        print(f"  Banco de Registros: R0={self.registros['R0']}, R1={self.registros['R1']}, R2={self.registros['R2']}\n")

# Simulación de CPU
cpu = CPUState()
cpu.imprimir_estado()

# Paso 1: Carga de dirección de instrucción a MAR
cpu.MAR = cpu.PC
print("[CICLO FETCH] Cargando PC a MAR...")
cpu.imprimir_estado()

# Paso 2: Bus de datos simula lectura de la instrucción de memoria (dirección 0x1000)
# La instrucción es cargar el valor 42 en R0
cpu.MDR = "LD R0, #42"
cpu.IR = cpu.MDR
cpu.PC += 4 # Incrementamos PC (instrucciones de 4 bytes)
print("[CICLO FETCH] Lectura de memoria terminada. Instrucción cargada en IR.")
cpu.imprimir_estado()

# Paso 3: Ejecución (Decode y Execute)
# La CPU ejecuta la instrucción almacenada en el IR
cpu.registros["R0"] = 42
print("[CICLO EXECUTE] Ejecutando: R0 <- 42")
cpu.imprimir_estado()
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Describir detalladamente el flujo de registros de control internos involucrados cuando la CPU realiza la fase de búsqueda (Fetch) de una nueva instrucción de memoria.

**Solución:**
El flujo de registros durante el ciclo Fetch se realiza siguiendo esta secuencia de pasos:
1.  **MAR $\leftarrow$ PC**: La dirección contenida en el Contador de Programa (PC) se transfiere al Registro de Direcciones de Memoria (MAR). Esta dirección se coloca en el bus de direcciones.
2.  **Lectura de Memoria**: La Unidad de Control activa la señal de lectura (Read) en el bus de control. La memoria responde localizando la dirección e introduciendo el código binario de la instrucción en el bus de datos.
3.  **MDR $\leftarrow$ Bus de Datos**: El Registro de Datos de Memoria (MDR) captura la instrucción del bus de datos.
4.  **IR $\leftarrow$ MDR**: El código de la instrucción se transfiere desde el MDR al Registro de Instrucción (IR), donde queda retenido para su decodificación.
5.  **PC $\leftarrow$ PC + valor**: El Contador de Programa (PC) se incrementa para apuntar a la dirección física de la siguiente instrucción en memoria (típicamente se le suma 1, 2 o 4 según el tamaño de la instrucción en bytes).

### Ejercicio 2
Comparar la Unidad de Control Cableada y la Microprogramada en base a su velocidad de reloj máxima y la facilidad para actualizar el repertorio de instrucciones (ISA).

**Solución:**
1.  **Velocidad de reloj máxima**:
    *   *Cableada*: **Superior**. Al estar implementada con compuertas combinacionales optimizadas a nivel de silicio, el retardo de conmutación de señales es mínimo, permitiendo frecuencias de reloj muy elevadas.
    *   *Microprogramada*: **Inferior**. Cada instrucción de máquina exige realizar accesos de lectura adicionales a una memoria ROM de control interna (para extraer las microinstrucciones), lo que añade ciclos de reloj y limita la frecuencia máxima.
2.  **Facilidad de actualización**:
    *   *Cableada*: **Nula**. Cualquier modificación o añadido en el repertorio de instrucciones exige cambiar físicamente el diseño del silicio y fabricar un chip nuevo.
    *   *Microprogramada*: **Alta**. El repertorio se puede modificar o ampliar simplemente reescribiendo la ROM de control interna (firmware del microprocesador), sin alterar los transistores del datapath.

---

## 6. Ejercicios Propuestos

1.  Dibuja un diagrama esquemático mostrando las conexiones de los buses externos de direcciones y datos con los registros MAR y MDR de la CPU.
2.  Explica la diferencia entre un registro de propósito general accesible por el programador (como los del Register File) y un registro de control (como el IR o MAR) detallando sus funciones en la CPU.
3.  Investiga el término **Riesgos de Control (Control Hazards)** y **Riesgos de Datos (Data Hazards)** en la arquitectura segmentada (pipeline) y cómo afectan al rendimiento de la CPU.
