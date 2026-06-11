# Tema 10: Estructura Interna del Computador: El Modelo de Von Neumann

Hasta mediados de la década de 1940, los primeros computadores eran máquinas de propósito único: para cambiar de programa (por ejemplo, pasar de calcular trayectorias balísticas a resolver sistemas de ecuaciones), los operadores debían reconfigurar físicamente los cables e interruptores de la máquina (programación por hardware). El matemático **John von Neumann** introdujo la revolucionaria idea del **Programa Almacenado**, donde las instrucciones del programa se codifican numéricamente y se almacenan en la misma memoria física que los datos, dando origen a la arquitectura universal de los computadores modernos.

---

## 1. Los Subsistemas Principales del Modelo de Von Neumann

La arquitectura de Von Neumann divide el computador en tres bloques funcionales bien diferenciados interconectados entre sí:

```
                  +-----------------------------------+
                  |               C P U               |
                  |  +-----------------------------+  |
                  |  |     Unidad de Control       |  |
                  |  +-----------------------------+  |
                  |                 ^                 |
                  |                 |                 |
                  |  +-----------------------------+  |
                  |  |            A L U            |  |
                  |  +-----------------------------+  |
                  +-----------------------------------+
                     ^              ^              ^
                     |              |              |
                     v              v              v
                  ===================================== BUSES DEL SISTEMA
                     ^              ^              ^
                     |              |              |
                     v              v              v
            +-----------------+           +-----------------+
            |     Memoria     |           |   Entrada /     |
            |    Principal    |           |    Salida       |
            +-----------------+           +-----------------+
```

1.  **Unidad Central de Procesamiento (CPU)**:
    *   **Unidad de Control (UC)**: Interpreta y secuencia las instrucciones del programa almacenado.
    *   **Unidad Aritmético-Lógica (ALU)**: Realiza los cálculos lógicos y aritméticos.
    *   **Banco de Registros**: Celdas de memoria internas ultrarrápidas para almacenar operandos inmediatos.
2.  **Memoria Principal**: Un array de celdas direccionables linealmente en el que conviven de forma indistinta los datos y las instrucciones de los programas.
3.  **Subsistema de Entrada/Salida (E/S)**: Interfaz para comunicar el computador con periféricos externos (teclados, monitores, discos).

---

## 2. Los Buses del Sistema

Los subsistemas se comunican mediante tres buses (conjuntos de pistas de cobre paralelas):
*   **Bus de Datos (Bidireccional)**: Transporta los datos de las operaciones y los códigos de instrucción leídos de la memoria. Su ancho (ej. 32 o 64 bits) define la longitud de palabra nativa del procesador.
*   **Bus de Direcciones (Unidireccional)**: La CPU coloca en este bus la dirección de memoria física exacta a la que desea acceder. Solo la CPU escribe en este bus. Su ancho determina el espacio de direccionamiento máximo del procesador (por ejemplo, con 32 líneas de dirección, el procesador puede direccionar hasta $2^{32} = 4 \, \text{GB}$ de RAM).
*   **Bus de Control (Bidireccional)**: Transporta señales de temporización y sincronización (línea de lectura/escritura, señales de interrupción, reloj del sistema).

---

## 3. Arquitectura Von Neumann frente a Harvard

*   **Arquitectura Von Neumann**: Datos e instrucciones comparten el **mismo bus físico y la misma memoria**.
*   **Arquitectura Harvard**: Dispone de **dos memorias físicas separadas** (Memoria de Datos y Memoria de Instrucciones), cada una con su respectivo bus independiente.

---

## 4. El Toque Informático

### El Cuello de Botella de Von Neumann y la Solución Caché L1
En el modelo Von Neumann puro, la CPU no puede leer una instrucción (operación Fetch) y escribir o leer un dato en memoria (operación Execute) simultáneamente, ya que comparten el mismo bus físico. Este retardo se conoce como el **Cuello de Botella de Von Neumann (Von Neumann Bottleneck)** y limita gravemente el rendimiento en procesadores rápidos.

*   **La Solución en Microchips Modernos**:
    Los procesadores actuales (Intel Core, AMD Ryzen, Apple M1) emplean un diseño híbrido:
    *   Hacia el **interior de la CPU (en el núcleo)**: Se utiliza una arquitectura **tipo Harvard** integrando dos memorias caché independientes de Nivel 1: la **Caché L1 de Instrucciones (L1I)** y la **Caché L1 de Datos (L1D)**, permitiendo accesos concurrentes de máxima velocidad.
    *   Hacia el **exterior de la CPU (placa base)**: Se utiliza una arquitectura **tipo Von Neumann** unificada en la memoria RAM principal por simplicidad física de cableado.

A continuación, implementamos en Python una simulación lógica del comportamiento de los buses de direcciones y datos al acceder a una memoria unificada (tipo Von Neumann).

```python
class MemoriaVonNeumann:
    def __init__(self, tamano=16):
        # Memoria de 16 posiciones. Almacena de forma indistinta datos e instrucciones.
        # Las instrucciones se representan como diccionarios, los datos como enteros
        self.celdas = [0] * tamano
        
    def escribir(self, bus_direcciones, bus_datos):
        # Simulación de escritura controlada por bus de control
        self.celdas[bus_direcciones] = bus_datos
        print(f"[ESCRITURA] Celda {bus_direcciones:02d} <- Guardado: {bus_datos}")
        
    def leer(self, bus_direcciones):
        # Simulación de lectura colocando el dato de la celda en el bus de datos
        bus_datos = self.celdas[bus_direcciones]
        print(f"[LECTURA]  Bus Direcciones colocó: {bus_direcciones:02d} -> Bus Datos lee: {bus_datos}")
        return bus_datos

# Simulación de carga y ejecución de un programa
mem = MemoriaVonNeumann(16)

# Cargamos el programa en las posiciones 0 y 1 (Instrucciones)
mem.escribir(0, {"op": "ADD", "addr": 5}) # Instrucción: Suma el dato de la dirección 5
mem.escribir(1, {"op": "SUB", "addr": 6}) # Instrucción: Resta el dato de la dirección 6

# Cargamos los datos en las direcciones 5 y 6
mem.escribir(5, 120) # Dato 1 = 120
mem.escribir(6, 45)  # Dato 2 = 45

print("\n--- Ejecución del Procesador (Ciclo Fetch-Execute) ---")
# 1. Fetch de la primera instrucción (en dirección 0)
instruccion1 = mem.leer(0)
# 2. Execute (requiere leer el dato de la dirección de memoria indicada por la instrucción)
dato = mem.leer(instruccion1["addr"])
print(f"  CPU ejecuta: {instruccion1['op']} con el valor {dato}")
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Un microcontrolador tiene un bus de direcciones de 16 líneas y un bus de datos de 8 líneas. Calcular:
1. El tamaño máximo de memoria física direccionable en bytes.
2. El tamaño de la palabra del computador.

**Solución:**
1.  **Cálculo de la capacidad de direccionamiento**:
    El bus de direcciones consta de 16 bits. El número de combinaciones de direcciones físicas únicas es:
    $$\text{Direcciones} = 2^{16} = 65.536 \text{ posiciones}$$
2.  **Cálculo de la memoria direccionable**:
    Dado que el bus de datos tiene 8 líneas, cada palabra direccionable almacena exactamente 8 bits (1 byte):
    $$\text{Capacidad} = 65.536 \times 1 \text{ Byte} = 64 \, \text{KB}$$

El procesador puede direccionar un mapa de memoria máximo de $64 \, \text{KB}$ (65.536 bytes).

### Ejercicio 2
Explicar por qué la arquitectura Harvard permite un mayor rendimiento y menor latencia en comparación con el modelo Von Neumann a coste de un diseño físico más complejo.

**Solución:**
En la arquitectura Harvard, al estar la memoria de instrucciones físicamente separada de la memoria de datos y conectadas mediante buses independientes, la CPU puede leer una nueva instrucción de la memoria (fase Fetch) y acceder simultáneamente a la memoria de datos (para leer o escribir operandos en la fase Execute).
*   **Rendimiento**: Se dobla el ancho de banda del bus efectivo del sistema, eliminando el "cuello de botella de Von Neumann".
*   **Complejidad**: Exige duplicar el número de pines de direcciones y datos de la CPU (aumentando el tamaño del circuito integrado y el número de pistas de cobre en la placa base) y duplica el número de controladores de bus de memoria, aumentando drásticamente el coste físico de fabricación.

---

## 6. Ejercicios Propuestos

1.  Determina el rango de direccionamiento máximo en bytes para un procesador de 32 bits (cuyo bus de direcciones tiene 32 líneas) y para uno de 64 bits.
2.  Dibuja un diagrama conceptual de bloques que ilustre la diferencia en los buses de conexión en una configuración tipo Von Neumann frente a una configuración Harvard.
3.  ¿Qué es la memoria de Entrada/Salida mapeada en memoria (Memory-Mapped I/O) y en qué se diferencia del direccionamiento de Entrada/Salida aislada (Port-Mapped I/O) desde el punto de vista del bus de direcciones?
