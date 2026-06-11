# Tema 8: Sistemas Secuenciales: Análisis y Diseño de Máquinas de Estados

Un sistema secuencial síncrono basa su comportamiento en una **Máquina de Estados Finitos (FSM - Finite State Machine)**. A diferencia de los circuitos combinacionales, las salidas de una FSM dependen de sus entradas y del **estado interno** acumulado en sus biestables. Modelar sistemas mediante FSMs es la metodología estándar para diseñar controladores complejos en hardware, incluyendo la Unidad de Control de los propios procesadores.

---

## 1. Modelos de Máquinas de Estados Finitos

Existen dos modelos clásicos de FSM que difieren en la forma en que generan sus salidas:

### 1.1 Modelo de Moore
Las salidas dependen **únicamente del estado actual** del sistema.
*   **Fórmula**: $Y(t) = f(Q(t))$
*   **Representación gráfica**: El valor de la salida se dibuja dentro del propio círculo que representa al estado (ej. `Estado A / Salida 0`).
*   **Ventaja**: Las salidas cambian de forma síncrona y limpia en los flancos de reloj, libre de ruidos transitorios (glitches) de entrada.

### 1.2 Modelo de Mealy
Las salidas dependen del **estado actual y de las entradas presentes** en ese instante.
*   **Fórmula**: $Y(t) = f(Q(t), X(t))$
*   **Representación gráfica**: El valor de la salida se dibuja en la flecha de transición de estado junto con el valor de la entrada que la provoca (ej. `Entrada / Salida`).
*   **Ventaja**: Suele requerir menos estados para realizar la misma función lógica que una máquina de Moore.

```
       Modelo de Moore                   Modelo de Mealy
         +-----------+                     +-----------+
  X ---->|   Lógica  |              X --+->|   Lógica  |
         | Transición|                  |  | Transición|
      +->|           |                  |  |           |
      |  +-----------+                  |  +-----------+
      |        |                        |        |
      |   Biestables (Q)                |   Biestables (Q)
      |   (Memoria)                     |   (Memoria)
      +--------+                        +--------+
               |                                 |
         +-----------+                     +-----------+
   Q --->|  Lógica   |----> Y        Q --->|  Lógica   |----> Y
         |  Salida   |                     |  Salida   |
         +-----------+                  X -+-----------+
```

---

## 2. Metodología de Diseño de una FSM Síncrona

El proceso para diseñar físicamente un circuito secuencial consta de los siguientes pasos ordenados:

1.  **Diagrama de Estados**: Dibujar los estados (círculos) y las transiciones (flechas) que describen el comportamiento del controlador ante las entradas.
2.  **Tabla de Transición de Estados**: Traducir el diagrama a una tabla que asocie el Estado Actual ($Q_n$) y las Entradas ($X$) con el Estado Siguiente ($Q_{n+1}$) y las Salidas ($Y$).
3.  **Codificación de Estados**: Asignar combinaciones binarias a cada estado. Si la máquina tiene $M$ estados, necesitaremos $k$ flip-flops de forma que:
    $$2^k \ge M$$
4.  **Selección de Flip-Flops**: Usar la **tabla de excitación** del biestable elegido (generalmente D o JK) para determinar qué entradas del flip-flop se necesitan para forzar el salto de $Q_n$ a $Q_{n+1}$.
5.  **Simplificación por Karnaugh**: Obtener las expresiones booleanas simplificadas para las entradas de excitación de los flip-flops y las salidas.
6.  **Esquema Lógico**: Dibujar el circuito interconectando compuertas combinacionales y los flip-flops.

---

## 3. El Toque Informático

### La Unidad de Control de la CPU como una FSM Gigante
En la arquitectura de un procesador, la **Unidad de Control (UC)** actúa como el director de orquesta del computador.
*   La UC es conceptualmente una gran FSM síncrona.
*   Su **estado** representa la fase actual del ciclo de instrucción (Fetch, Decode, Execute, etc.).
*   Su **entrada** principal es el código de operación (OPCode) de la instrucción cargada en el Registro de Instrucción (IR).
*   Sus **salidas** son decenas de señales lógicas de control que abren o cierran puertas lógicas en los buses del procesador, habilitan la escritura en registros específicos o le dicen a la ALU si debe sumar o restar.

A continuación, implementamos en Python una simulación lógica de una FSM de Moore diseñada para detectar la secuencia binaria "101" en una transmisión de datos serie.

```python
class DetectorSecuencia101:
    def __init__(self):
        # Estados: S0 (reposo), S1 (detectado '1'), S2 (detectado '10'), S3 (detectado '101')
        self.estado = "S0"
        
    def transicionar(self, bit_entrada):
        # FSM de Moore: la salida Y depende únicamente del estado actual
        # S0 -> Y=0, S1 -> Y=0, S2 -> Y=0, S3 -> Y=1
        
        # 1. Lógica de transición de estados
        if self.estado == "S0":
            self.estado = "S1" if bit_entrada == 1 else "S0"
        elif self.estado == "S1":
            self.estado = "S2" if bit_entrada == 0 else "S1"
        elif self.estado == "S2":
            self.estado = "S3" if bit_entrada == 1 else "S0"
        elif self.estado == "S3":
            # Si recibimos un 0 tras detectar '101', volvemos a S2 (pues el '1' final de '101' es el '1' inicial del siguiente)
            self.estado = "S2" if bit_entrada == 0 else "S1"
            
        # 2. Lógica de salida de Moore
        salida = 1 if self.estado == "S3" else 0
        return self.estado, salida

# Simulación con un flujo de bits
secuencia_entrada = [1, 0, 1, 0, 1, 1, 0, 1]
fsm = DetectorSecuencia101()

print("Simulación de FSM detectora de '101':")
for bit in secuencia_entrada:
    est, y = fsm.transicionar(bit)
    print(f"  Entrada: {bit} -> Transiciona a Estado: {est} -> Salida Y: {y}")
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Diseñar una máquina de Moore síncrona utilizando Flip-Flops D que detecte la secuencia "11" en un flujo de bits serie y active una salida $Y=1$.

**Solución:**
1.  **Definir estados**:
    *   $A$: Estado de reposo (no se han recibido '1's). Salida $Y = 0$.
    *   $B$: Se ha recibido un único '1'. Salida $Y = 0$.
    *   $C$: Se han recibido dos o más '1's consecutivos (secuencia detectada). Salida $Y = 1$.
2.  **Codificación de estados (3 estados $\implies 2$ bits, Flip-Flops $Q_1, Q_0$)**:
    *   $A = 00_2$
    *   $B = 01_2$
    *   $C = 11_2$
3.  **Construir la Tabla de Transición y Excitación**:
    Como usamos Flip-Flops D, la entrada $D_i$ es exactamente igual al estado siguiente deseado $Q_{i\text{,next}}$:

    | $Q_1Q_0$ | Entrada $X$ | $Q_{1\text{,next}}Q_{0\text{,next}}$ | Entradas $D_1D_0$ | Salida $Y$ |
    | :---: | :---: | :---: | :---: | :---: |
    | 00 ($A$) | 0 | 00 ($A$) | 00 | 0 |
    | 00 ($A$) | 1 | 01 ($B$) | 01 | 0 |
    | 01 ($B$) | 0 | 00 ($A$) | 00 | 0 |
    | 01 ($B$) | 1 | 11 ($C$) | 11 | 0 |
    | 11 ($C$) | 0 | 00 ($A$) | 00 | 1 |
    | 11 ($C$) | 1 | 11 ($C$) | 11 | 1 |

4.  **Simplificar ecuaciones por Karnaugh**:
    *   **Ecuación de Salida $Y$**: Depende solo del estado actual. Inspeccionando la tabla:
        $$Y = Q_1 Q_0$$
    *   **Ecuación para $D_0$**:
        $$D_0 = \bar{Q}_1\bar{Q}_0X + \bar{Q}_1Q_0X + Q_1Q_0X = X(\bar{Q}_1\bar{Q}_0 + \bar{Q}_1Q_0 + Q_1Q_0) = X(Q_0 + \bar{Q}_1)$$
    *   **Ecuación para $D_1$**:
        $$D_1 = \bar{Q}_1Q_0X + Q_1Q_0X = Q_0 X ( \bar{Q}_1 + Q_1 ) = Q_0 X$$

El circuito final utiliza 2 Flip-Flops D con entradas $D_0 = X(Q_0 + \bar{Q}_1)$ y $D_1 = Q_0X$, y una puerta AND para la salida $Y = Q_1Q_0$.

---

## 5. Ejercicios Propuestos

1.  Dibuja el diagrama de transición de estados de una máquina de Mealy que reciba un bit serie de entrada y active su salida a $1$ únicamente en el instante en que detecte un flanco de cambio de bits (secuencia "01" o "10").
2.  Explica conceptualmente la diferencia física y de cronograma entre una salida en el modelo de Mealy y una salida en el modelo de Moore. ¿A qué se refiere el término **riesgo de transición (hazards / glitches)** en Mealy?
3.  Diseña la tabla de transición de estados de un contador síncrono Gray de 2 bits utilizando Flip-Flops D (secuencia de estados: $00 \to 01 \to 11 \to 10 \to 00 \dots$).
