# Tema 7: Elementos de Memoria Básicos: Biestables

Los sistemas combinacionales carecen de la capacidad de almacenar estados históricos: sus salidas responden de forma ciega e instantánea a las entradas presentes. Para construir computadores que sigan secuencias de instrucciones (programas), necesitamos circuitos capaces de memorizar información. Esto se logra mediante la **realimentación**, donde la salida de una compuerta lógica se conecta de vuelta a su entrada, creando elementos estables de memoria llamados **Latches** y **Biestables (Flip-Flops)**.

---

## 1. Realimentación y Latches Asíncronos

Un circuito asíncrono no utiliza señal de reloj. Su estado cambia en el instante en que cambian sus entradas.

### Latch RS (Reset-Set) con puertas NOR
Es el elemento de memoria elemental. Consta de dos compuertas NOR realimentadas de forma cruzada:

```
        R -----+-----\   NOR
               |      )o------ Q
            +--|-+---/
            |  | |
            |  | +------------+
            |  |              |
            |  +-----------+  |
            |              |  |
            +------------+ |  |
                         | |  |
        S -----+-----/   NOR  |
               |      )o------+
               +-----/--------- \bar{Q}
```

*   **Comportamiento**:
    *   $S=1, R=0 \implies$ **Set**: La salida pasa a $Q=1$ (almacena un "1").
    *   $S=0, R=1 \implies$ **Reset**: La salida pasa a $Q=0$ (almacena un "0").
    *   $S=0, R=0 \implies$ **Memoria**: Conserva el estado anterior ($Q_{next} = Q$).
    *   $S=1, R=1 \implies$ **Estado Prohibido**: Ambas salidas $Q$ y $\bar{Q}$ intentan ponerse a 0, rompiendo la complementariedad. Si las entradas vuelven a 0 a la vez, el circuito entra en una **condición de carrera (race condition)** oscilatoria indeterminada.

---

## 2. Biestables Síncronos (Flip-Flops)

Para coordinar millones de celdas de memoria de forma armoniosa y evitar condiciones de carrera, se introduce una señal periódica común de sincronización llamada **Reloj (CLK - Clock)**.
*   **Latch**: Es sensible al **nivel** de la señal de habilitación (conduce mientras la señal esté alta).
*   **Flip-Flop**: Es sensible únicamente al **flanco (transición rápida)** del reloj (flanco de subida $\uparrow$ o de bajada $\downarrow$). Durante los periodos lógicos estables del reloj, el circuito ignora cualquier cambio en sus entradas.

---

## 3. Tipos de Flip-Flops y sus Ecuaciones Características

| Tipo | Ecuación Característica | Comportamiento en Flanco Activo | Uso Principal |
| :--- | :--- | :--- | :--- |
| **D (Data)** | $Q_{n+1} = D$ | La salida toma directamente el valor de la entrada $D$. | Registros de almacenamiento, buses. |
| **JK** | $Q_{n+1} = J\bar{Q}_n + \bar{K}Q_n$ | Resuelve el estado prohibido de RS. Si $J=K=1$, la salida conmuta (inversión). | Contadores, divisores de frecuencia. |
| **T (Toggle)** | $Q_{n+1} = T \oplus Q_n$ | Si $T=1$, la salida cambia al estado opuesto; si $T=0$, se mantiene. | Contadores síncronos, acumuladores. |

---

## 4. El Toque Informático

### SRAM vs. DRAM: La batalla de la Caché y la Memoria Principal
La memoria de un computador se diseña con tecnologías de almacenamiento radicalmente distintas:
1.  **SRAM (Static RAM)**:
    Cada celda de memoria consiste en un **latch realimentado (usando de 4 a 6 transistores)**.
    *   *Ventaja*: Es extremadamente rápida (latencias de menos de $1 \, \text{ns}$) porque el estado se mantiene de forma estática y estable mientras reciba corriente.
    *   *Uso*: Se emplea en la memoria caché integrada de la CPU ($L1, L2, L3$). Su coste de fabricación y área física es muy alto.
2.  **DRAM (Dynamic RAM)**:
    Cada celda consta de **un solo transistor y un condensador minúsculo**.
    *   *Ventaja*: Altísima densidad de integración y coste bajísimo (permite fabricar módulos de 16 GB de RAM de forma barata).
    *   *Inconveniente*: El condensador pierde su carga eléctrica en milisegundos. La CPU o el controlador de memoria deben realizar **ciclos de refresco constantes** para leer la celda y recargar el condensador, lo que hace que la DRAM sea mucho más lenta y consuma más energía en reposo.

A continuación, implementamos en Python una simulación orientada a objetos de un Flip-Flop D sensible a flanco de subida, mostrando cómo almacena la información sólo en las transiciones de reloj.

```python
class FlipFlopD:
    def __init__(self):
        self.Q = 0
        self.clk_prev = 0
        
    def procesar(self, D, CLK):
        # Detectamos flanco de subida: CLK cambia de 0 a 1
        if self.clk_prev == 0 and CLK == 1:
            print(f"[FLANCO SUBIDA] Actualizando Q: {self.Q} -> {D}")
            self.Q = D
        else:
            # En cualquier otro estado de reloj, la salida se mantiene constante
            pass
        self.clk_prev = CLK
        return self.Q

# Simulación de un cronograma
ff = FlipFlopD()
entradas_cronograma = [
    # (D, CLK)
    (1, 0), # CLK=0, D=1 -> No cambia
    (1, 1), # CLK=1 (Flanco de subida), D=1 -> Q pasa a 1
    (0, 1), # CLK=1 (Nivel alto), D cambia a 0 -> Ignorado (no hay flanco)
    (0, 0), # CLK=0 (Flanco de bajada) -> Ignorado
    (0, 1), # CLK=1 (Flanco de subida), D=0 -> Q pasa a 0
]

print("Simulación de cronograma de Flip-Flop D:")
for idx, (d, clk) in enumerate(entradas_cronograma):
    q = ff.procesar(d, clk)
    print(f"  Paso {idx}: D={d}, CLK={clk} -> Salida Q={q}")
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Deducir la ecuación característica de un Flip-Flop JK a partir de su tabla de excitación.

**Solución:**
1.  **Construimos la tabla de transición de estados**:

    | $J$ | $K$ | $Q_n$ | $Q_{n+1}$ | Minterm asociado |
    | :---: | :---: | :---: | :---: | :--- |
    | 0 | 0 | 0 | 0 | - |
    | 0 | 0 | 1 | 1 | $m_1 = \bar{J}\bar{K}Q_n$ |
    | 0 | 1 | 0 | 0 | - |
    | 0 | 1 | 1 | 0 | - |
    | 1 | 0 | 0 | 1 | $m_4 = J\bar{K}\bar{Q}_n$ |
    | 1 | 0 | 1 | 1 | $m_5 = J\bar{K}Q_n$ |
    | 1 | 1 | 0 | 1 | $m_6 = JK\bar{Q}_n$ |
    | 1 | 1 | 1 | 0 | - |

2.  **Escribimos la ecuación para $Q_{n+1}$ sumando los minterms**:
    $$Q_{n+1} = \bar{J}\bar{K}Q_n + J\bar{K}\bar{Q}_n + J\bar{K}Q_n + JK\bar{Q}_n$$
3.  **Simplificamos algebraicamente**:
    *   Agrupamos los términos con factor común $J\bar{Q}_n$:
        $$J\bar{Q}_n(\bar{K} + K) = J\bar{Q}_n$$
    *   Agrupamos los términos con factor común $\bar{K}Q_n$:
        $$\bar{K}Q_n(\bar{J} + J) = \bar{K}Q_n$$
    *   Sumamos los resultados obtenidos:
        $$Q_{n+1} = J\bar{Q}_n + \bar{K}Q_n$$

Queda demostrada la ecuación característica del Flip-Flop JK.

### Ejercicio 2
Dibujar a mano el cronograma de salida de un Flip-Flop D disparado por flanco de subida, dados las siguientes señales de reloj y datos:
*   CLK: pulsos regulares en periodos $t=1$ (subida), $t=2$ (bajada), $t=3$ (subida), $t=4$ (bajada).
*   D: vale $1$ en el intervalo $[0, 2]$, y pasa a $0$ en el intervalo $[2, 5]$.
*   Asumir estado inicial $Q = 0$.

**Solución:**
Analizamos el estado únicamente en los instantes de **flanco de subida** ($t=1$ y $t=3$):
1.  En $t = 1$ ($\uparrow$ CLK): La entrada $D$ vale $1$. La salida $Q$ se actualiza al valor de $D$, pasando a $Q = 1$.
2.  En el intervalo $t \in (1, 3)$: El reloj baja en $t=2$ e ignoramos el cambio de $D$ a $0$ en ese mismo instante. La salida mantiene su valor estable $Q = 1$.
3.  En $t = 3$ ($\uparrow$ CLK): La entrada $D$ vale $0$. La salida $Q$ se actualiza pasando a $Q = 0$.
4.  En el intervalo $t > 3$: La salida mantiene su valor $Q = 0$.

El cronograma final de $Q(t)$ es un pulso que vale $0$ para $t \in [0, 1]$, pasa a $1$ para $t \in [1, 3]$ y vuelve a $0$ para $t > 3$.

---

## 6. Ejercicios Propuestos

1.  Dibuja el esquema lógico de un Latch RS utilizando compuertas lógicas NAND elementales y deduce su tabla de verdad (indicando cuál es el estado prohibido).
2.  Dibuja un diagrama de bloques mostrando cómo construir un Flip-Flop de tipo T utilizando únicamente un Flip-Flop de tipo JK.
3.  Explica conceptualmente qué es el **tiempo de establecimiento (setup time)** y el **tiempo de mantenimiento (hold time)** en un biestable síncrono, y analiza por qué violar estos límites físicos provoca inestabilidad (metaestabilidad) en el circuito.
