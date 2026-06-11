# Tema 9: Interpolación Polinómica

El problema de la interpolación consiste en encontrar una función sencilla (normalmente un polinomio) que pase exactamente por un conjunto dado de puntos de datos discretos. En la ingeniería informática, la interpolación es fundamental para el redimensionado de imágenes (reescalado de píxeles), la reconstrucción de señales analógicas digitalizadas, la compresión de datos y la animación 3D (curvas clave de movimiento o Splines).

---

## 1. Formulación del Problema

Disponemos de un conjunto de $n+1$ puntos (nodos) de datos discretos:
$$(x_0, y_0), (x_1, y_1), (x_2, y_2), \dots, (x_n, y_n)$$
donde los nodos $x_i$ son todos distintos entre sí ($x_i \ne x_j$ para todo $i \ne j$).

Buscamos un **polinomio interpolador** $P_n(x)$ de grado menor o igual a $n$ tal que satisfaga las condiciones de interpolación:
$$P_n(x_i) = y_i \quad \text{para todo } i = 0, 1, \dots, n$$

> [!NOTE]
> **Teorema de Existencia y Unicidad:**
> Dados $n+1$ puntos con abscisas distintas, existe un único polinomio $P_n(x)$ de grado a lo sumo $n$ que pasa exactamente por todos ellos. Aunque usemos diferentes métodos de construcción (Lagrange, Newton, sistemas lineales), obtendremos algebraicamente el mismo polinomio.

---

## 2. Polinomio Interpolador de Lagrange

El método de Lagrange construye el polinomio como una combinación lineal de las ordenadas $y_i$ ponderadas por **polinomios de base de Lagrange**, denotados por $L_i(x)$.

### Polinomios de la Base
Para cada nodo $i$, definimos $L_i(x)$ como un polinomio de grado $n$ que vale $1$ en el nodo $x_i$ y $0$ en todos los demás nodos $x_j$ ($j \ne i$):
$$L_i(x) = \prod_{j=0, j\ne i}^{n} \frac{x - x_j}{x_i - x_j} = \frac{(x-x_0)(x-x_1)\dots(x-x_{i-1})(x-x_{i+1})\dots(x-x_n)}{(x_i-x_0)(x_i-x_1)\dots(x_i-x_{i-1})(x_i-x_{i+1})\dots(x_i-x_n)}$$

### Construcción del Polinomio
El polinomio interpolador final es:
$$P_n(x) = \sum_{i=0}^{n} y_i L_i(x)$$

*Limitación*: Si se añade un nuevo punto de datos $(x_{n+1}, y_{n+1})$, todos los polinomios de la base $L_i(x)$ deben recalcularse por completo.

---

## 3. Interpolación de Newton y Diferencias Divididas

El método de Newton resuelve la rigidez de Lagrange permitiendo añadir nuevos puntos de manera incremental sin rehacer el trabajo previo.

El polinomio se escribe en la **forma de Newton**:
$$P_n(x) = a_0 + a_1(x-x_0) + a_2(x-x_0)(x-x_1) + \dots + a_n(x-x_0)(x-x_1)\dots(x-x_{n-1})$$

Los coeficientes $a_i$ se calculan sistemáticamente mediante **diferencias divididas**, denotadas por $f[x_0, x_1, \dots, x_k]$.

### Definición de Diferencias Divididas
*   Orden 0: $f[x_i] = y_i$
*   Orden 1: $f[x_i, x_{i+1}] = \frac{f[x_{i+1}] - f[x_i]}{x_{i+1} - x_i}$
*   Orden general $k$:
    $$f[x_0, x_1, \dots, x_k] = \frac{f[x_1, \dots, x_k] - f[x_0, \dots, x_{k-1}]}{x_k - x_0}$$

Los coeficientes del polinomio de Newton son precisamente las diferencias divididas de la diagonal superior:
$$a_k = f[x_0, x_1, \dots, x_k]$$

---

## 4. Análisis del Error y Fenómeno de Runge

Si los puntos provienen de una función continua $f(x)$, el error de interpolación en cualquier punto $x$ es:
$$E(x) = f(x) - P_n(x) = \frac{f^{(n+1)}(c)}{(n+1)!}\prod_{i=0}^{n} (x - x_i) \quad \text{donde } c \in (x_0, x_n)$$

### El Fenómeno de Runge
A finales del siglo XIX, Carl Runge descubrió que al interpolar funciones continuas usando nodos equiespaciados, aumentar el grado del polinomio $n$ puede hacer que el error aumente de forma ilimitada, produciendo **fuertes oscilaciones en los extremos del intervalo**.
La función clásica de Runge es:
$$f(x) = \frac{1}{1 + 25x^2} \quad \text{en } [-1, 1]$$

### Solución: Nodos de Chebyshev
Para minimizar la oscilación del término de error $\prod (x-x_i)$, se utilizan nodos no equiespaciados distribuidos más densamente cerca de los extremos. Estos se conocen como **nodos de Chebyshev**:
$$x_k = \cos\left(\frac{2k + 1}{2n + 2} \pi\right) \quad \text{para } k = 0, 1, \dots, n$$

---

## 5. El Toque Informático

A continuación, implementamos en Python el cálculo del polinomio interpolador y visualizamos el **Fenómeno de Runge** comparando la interpolación de la función de Runge con $11$ nodos equiespaciados frente a $11$ nodos de Chebyshev.

```python
import numpy as np
import matplotlib.pyplot as plt

def funcion_runge(x):
    return 1.0 / (1.0 + 25.0 * x**2)

# Algoritmo de Interpolación de Lagrange
def interpolar_lagrange(x_nodos, y_nodos, x_eval):
    n = len(x_nodos)
    resultado = np.zeros_like(x_eval)
    
    for i in range(n):
        # Calculamos el polinomio base L_i(x)
        L_i = np.ones_like(x_eval)
        for j in range(n):
            if i != j:
                L_i *= (x_eval - x_nodos[j]) / (x_nodos[i] - x_nodos[j])
        resultado += y_nodos[i] * L_i
        
    return resultado

# Definimos los nodos de interpolación en [-1, 1]
N = 10  # Grado del polinomio n = 10 (11 nodos)

# 1. Nodos equiespaciados
nodos_eq = np.linspace(-1, 1, N + 1)
y_eq = funcion_runge(nodos_eq)

# 2. Nodos de Chebyshev
nodos_ch = np.cos((2 * np.arange(N + 1) + 1) / (2 * N + 2) * np.pi)
y_ch = funcion_runge(nodos_ch)

# Evaluación en malla densa para graficar
x_grid = np.linspace(-1, 1, 300)
y_real = funcion_runge(x_grid)
y_interp_eq = interpolar_lagrange(nodos_eq, y_eq, x_grid)
y_interp_ch = interpolar_lagrange(nodos_ch, y_ch, x_grid)

# Gráfico
plt.figure(figsize=(10, 6))
plt.plot(x_grid, y_real, label="Función de Runge real", color="black", linewidth=2)
plt.plot(x_grid, y_interp_eq, label="Interp. Equiespaciada (Runge)", linestyle="--", color="red")
plt.plot(x_grid, y_interp_ch, label="Interp. Chebyshev (Sin oscilación)", linestyle="-.", color="green")
plt.scatter(nodos_eq, y_eq, color="red", zorder=3, label="Nodos Equiespaciados")
plt.scatter(nodos_ch, y_ch, color="green", zorder=3, label="Nodos Chebyshev")

plt.ylim(-0.3, 1.3)
plt.title(f"Fenómeno de Runge vs Nodos de Chebyshev (Grado {N})")
plt.xlabel("x")
plt.ylabel("y")
plt.legend()
plt.grid(True)
plt.show()
```

La gráfica ilustra claramente cómo el uso de nodos equiespaciados introduce desviaciones masivas cerca de $-1$ y $1$ (fenómeno de Runge), mientras que la distribución óptima de Chebyshev estabiliza por completo el polinomio.

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Obtener el polinomio interpolador en forma de Newton para los siguientes puntos: $(0, 1)$, $(1, 3)$ y $(2, 7)$.

**Solución usando la Tabla de Diferencias Divididas:**
1.  **Establecer los nodos**:
    *   $x_0 = 0, y_0 = 1$
    *   $x_1 = 1, y_1 = 3$
    *   $x_2 = 2, y_2 = 7$
2.  **Construir la tabla de diferencias divididas**:
    *   **Orden 0 (Valores de $y$):**
        $$f[x_0] = 1, \quad f[x_1] = 3, \quad f[x_2] = 7$$
    *   **Orden 1:**
        $$f[x_0, x_1] = \frac{f[x_1] - f[x_0]}{x_1 - x_0} = \frac{3 - 1}{1 - 0} = 2$$
        $$f[x_1, x_2] = \frac{f[x_2] - f[x_1]}{x_2 - x_1} = \frac{7 - 3}{2 - 1} = 4$$
    *   **Orden 2:**
        $$f[x_0, x_1, x_2] = \frac{f[x_1, x_2] - f[x_0, x_1]}{x_2 - x_0} = \frac{4 - 2}{2 - 0} = \frac{2}{2} = 1$$
3.  **Construir el polinomio en la forma de Newton**:
    Tomando los valores de la diagonal superior ($a_0 = 1$, $a_1 = 2$, $a_2 = 1$):
    $$P_2(x) = a_0 + a_1(x-x_0) + a_2(x-x_0)(x-x_1)$$
    $$P_2(x) = 1 + 2(x-0) + 1(x-0)(x-1) = 1 + 2x + x(x-1) = x^2 + x + 1$$
4.  **Verificación**:
    *   $P_2(0) = 0^2 + 0 + 1 = 1 \quad (\text{Correcto})$
    *   $P_2(1) = 1^2 + 1 + 1 = 3 \quad (\text{Correcto})$
    *   $P_2(2) = 2^2 + 2 + 1 = 7 \quad (\text{Correcto})$

El polinomio interpolador es $P_2(x) = x^2 + x + 1$.

---

## 7. Ejercicios Propuestos

1.  Determinar el polinomio interpolador de Lagrange para los puntos:
    $$(1, -1), \quad (2, 4), \quad (3, 11)$$
2.  Dada la tabla de datos de un sensor de temperatura:
    
    | Tiempo $t$ | Temperatura $T(t)$ |
    | :--- | :--- |
    | 0 | 20 |
    | 2 | 22 |
    | 5 | 28 |
    
    Utilizar la interpolación de Newton para estimar la temperatura en el instante $t = 3$.
3.  Demostrar que la suma de los polinomios de la base de Lagrange para cualquier conjunto de nodos es idénticamente igual a 1, es decir:
    $$\sum_{i=0}^{n} L_i(x) = 1 \quad \forall x \in \mathbb{R}$$
    *(Pista: Considera qué ocurre si interpolas la función constante $f(x) = 1$).*
