# Tema 6: Técnicas para el Análisis de Circuitos Resistivos

Para circuitos complejos con múltiples fuentes y mallas, la aplicación directa de las leyes de Kirchhoff resulta caótica. Este tema introduce metodologías sistemáticas para reducir cualquier red a un sistema ordenado de ecuaciones lineales y analiza los teoremas de Thévenin y Norton, indispensables para simplificar etapas de hardware en diseño de sistemas.

---

## 1. Métodos Sistemáticos: Nudos y Mallas

### 1.1 Método de Análisis por Nudos (Tensiones de Nudo)
Toma como variables del circuito las tensiones de los nudos respecto a un **nudo de referencia** (masa o GND, cuyo potencial es 0V).
1.  Identificar los nudos esenciales del circuito. Elegir uno como referencia.
2.  Aplicar la Primera Ley de Kirchhoff (LCK) a cada nudo restante, expresando las corrientes de rama en función de las tensiones de nudo mediante la Ley de Ohm:
    $$I = \frac{V_{\text{origen}} - V_{\text{destino}}}{R}$$
3.  Resolver el sistema de ecuaciones para hallar las tensiones de nudo.

### 1.2 Método de Análisis por Mallas (Corrientes de Malla)
Toma como variables corrientes ficticias que circulan en lazos cerrados independientes (mallas).
1.  Identificar las mallas del circuito y asignar una corriente de malla (normalmente en sentido horario).
2.  Aplicar la Segunda Ley de Kirchhoff (LKT) a cada malla, expresando las caídas de tensión en función de las corrientes de malla. (Si una resistencia es compartida por dos mallas, la corriente neta es la diferencia de ambas corrientes).
3.  Resolver el sistema de ecuaciones.

---

## 2. Teoremas de Simplificación de Redes: Thévenin y Norton

Cualquier red lineal activa de dos terminales (A y B) puede simplificarse por un circuito equivalente muy sencillo.

```
       Circuito Lineal Activo                 Circuito Equivalente
      +------------------------+ A          +-----[ Rth ]-----+ A
      |                        |            |                 |
      |                        |          (Vth)               |
      |                        |            |                 |
      +------------------------+ B          +-----------------+ B
```

### 2.1 Teorema de Thévenin
Establece que el circuito puede sustituirse por una **fuente de tensión equivalente $V_{\text{th}}$ en serie con una resistencia equivalente $R_{\text{th}}$**:
*   $V_{\text{th}}$ (Tensión de Thévenin): Es la diferencia de potencial entre las terminales A y B en circuito abierto (sin conectar carga).
*   $R_{\text{th}}$ (Resistencia de Thévenin): Es la resistencia equivalente vista desde A y B tras apagar todas las fuentes independientes (fuentes de tensión cortocircuitadas, fuentes de corriente abiertas).

### 2.2 Teorema de Norton
Establece que el circuito puede sustituirse por una **fuente de corriente equivalente $I_{\text{N}}$ en paralelo con la misma resistencia equivalente $R_{\text{th}}$**:
*   $I_{\text{N}}$ (Corriente de Norton): Es la corriente que circula entre A y B al cortocircuitar las terminales.

Relación entre equivalentes (Transformación de fuentes):
$$V_{\text{th}} = I_{\text{N}} \cdot R_{\text{th}}$$

---

## 3. El Toque Informático

### Algoritmo SPICE de Resolución de Circuitos
Los simuladores de circuitos electrónicos (como LTspice o Multisim) se basan en el algoritmo **SPICE** (Simulation Program with Integrated Circuit Emphasis).
SPICE automatiza el análisis por nudos planteando las ecuaciones en una matriz conocida como **Análisis Modificado por Nudos (MNA)**:
$$G \cdot V = I$$
donde $G$ es la matriz de conductancias, $V$ es el vector de tensiones de nudo incógnitas e $I$ es el vector de corrientes inyectadas por las fuentes.

A continuación, implementamos en Python el planteamiento y resolución matricial de un circuito de tres nudos para simular el comportamiento de un resolvedor SPICE.

```python
import numpy as np

# Planteamos las ecuaciones del circuito mediante el método de nudos (MNA)
# Circuito con 3 nudos esenciales (más referencia GND):
# Nudo 1: Conectado a fuente de 10V respecto a GND
# Nudo 2: Conectado a nudo 1 por R=2 ohm, a nudo 3 por R=3 ohm, y a GND por R=5 ohm
# Nudo 3: Conectado a nudo 2 por R=3 ohm, y a GND por R=4 ohm y fuente de corriente de 2A (entrante)

# El sistema de ecuaciones lineal G * V = I se define como:
# Fila 1 (Nudo 1): V1 = 10
# Fila 2 (Nudo 2): (V2 - V1)/2 + V2/5 + (V2 - V3)/3 = 0  ===> -0.5*V1 + (0.5 + 0.2 + 0.333)*V2 - 0.333*V3 = 0
# Fila 3 (Nudo 3): (V3 - V2)/3 + V3/4 = 2               ===> -0.333*V2 + (0.333 + 0.25)*V3 = 2

G = np.array([
    [1.0, 0.0, 0.0],
    [-0.5, (0.5 + 0.2 + 1.0/3.0), -1.0/3.0],
    [0.0, -1.0/3.0, (1.0/3.0 + 0.25)]
])

I = np.array([10.0, 0.0, 2.0])

# Resolvemos el sistema de ecuaciones usando álgebra lineal de numpy
V_nodos = np.linalg.solve(G, I)

print("Tensiones de nudo calculadas matricialmente (estilo SPICE):")
for i, v in enumerate(V_nodos):
    print(f"Voltaje Nudo {i+1} (V{i+1}): {v:.4f} V")
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Calcular el circuito equivalente de Thévenin visto desde las terminales A y B para el circuito resistivo de la figura, que consta de una fuente de tensión independiente de $15 \, \text{V}$ en serie con una resistencia de $6 \, \Omega$, conectadas a una resistencia en paralelo de $3 \, \Omega$ entre las terminales.

```
       +---[ 6 ohm ]---+-----+ A
       |               |     |
     (15V)           [3 ohm] [ Carga ]
       |               |     |
       +---------------+-----+ B
```

**Solución:**
1.  **Calcular la tensión de Thévenin ($V_{\text{th}}$)**:
    $V_{\text{th}}$ es la tensión en circuito abierto (sin carga) entre A y B.
    El circuito se reduce a un **divisor de tensión** formado por la fuente de $15 \, \text{V}$ y las resistencias de $6 \, \Omega$ y $3 \, \Omega$:
    $$V_{\text{th}} = V_{\text{AB}} = V_{15\text{V}} \frac{R_{\text{paralelo}}}{R_{\text{serie}} + R_{\text{paralelo}}} = 15 \frac{3}{6 + 3} = 15 \frac{3}{9} = 5 \, \text{V}$$
2.  **Calcular la resistencia de Thévenin ($R_{\text{th}}$)**:
    Apagamos la fuente de tensión independiente cortocircuitándola (cable).
    Visto desde las terminales A y B, las resistencias de $6 \, \Omega$ y $3 \, \Omega$ quedan conectadas en **paralelo**:
    $$R_{\text{th}} = R_{6\Omega} \parallel R_{3\Omega} = \frac{6 \cdot 3}{6 + 3} = \frac{18}{9} = 2 \, \Omega$$
3.  **Circuito Equivalente de Thévenin**:
    Una fuente de tensión de $5 \, \text{V}$ en serie con una resistencia de $2 \, \Omega$.

---

## 5. Ejercicios Propuestos

1.  Resolver el circuito de dos mallas de la figura mediante el método de corrientes de malla y calcular la corriente que circula por la resistencia compartida de $10 \, \Omega$ si las mallas tienen fuentes de $12 \, \text{V}$ y $6 \, \text{V}$.
2.  Para el ejercicio resuelto 1, calcular cuál sería el equivalente de Norton (fuente de corriente $I_{\text{N}}$ y resistencia $R_{\text{N}}$).
3.  ¿Qué establece el Teorema de Máxima Transferencia de Potencia respecto al valor de la resistencia de carga $R_{\text{L}}$ conectada a un circuito con resistencia de Thévenin $R_{\text{th}}$? Explica su importancia en el diseño de circuitos amplificadores de audio o antenas.
