# Tema 10: Integración Numérica

La integración numérica (o cuadratura numérica) consiste en calcular una aproximación del valor de una integral definida $\int_{a}^{b} f(x) \, dx$ utilizando un conjunto de valores discretos de la función. En la ingeniería informática, se aplica en motores de física 2D/3D (integración de leyes de movimiento de Newton para actualizar velocidad y posición), simulación de fluidos, procesamiento de señales de audio/vídeo y cálculo probabilístico en aprendizaje automático.

---

## 1. Motivación y Fórmulas de Newton-Cotes

Existen dos razones principales para recurrir a la integración numérica:
1.  **La función $f(x)$ carece de primitiva elemental**: Por ejemplo, $\int_{0}^{1} e^{-x^2} \, dx$ o $\int_{a}^{b} \frac{\sin x}{x} \, dx$.
2.  **Solo disponemos de datos muestreados**: En lugar de una expresión analítica, tenemos una tabla de valores proveniente de sensores o experimentos en intervalos discretos.

Las **fórmulas de Newton-Cotes** aproximan la integral sustituyendo la función original $f(x)$ por su polinomio interpolador $P_n(x)$ en el intervalo $[a, b]$, y calculando la integral exacta del polinomio:
$$\int_{a}^{b} f(x) \, dx \approx \int_{a}^{b} P_n(x) \, dx$$

---

## 2. La Regla del Trapecio

La regla del Trapecio aproxima la función $f(x)$ por una línea recta (polinómico de grado 1) que conecta los extremos del intervalo $(a, f(a))$ y $(b, f(b))$.

### 2.1 Regla del Trapecio Simple
El área bajo la línea recta es el área de un trapecio geométrico:
$$\int_{a}^{b} f(x) \, dx \approx \frac{b - a}{2} \Big( f(a) + f(b) \Big)$$

El **error de truncamiento** cometido viene dado por:
$$E_T = -\frac{(b-a)^3}{12} f''(c) \quad \text{donde } c \in (a, b)$$

### 2.2 Regla del Trapecio Compuesta
Para mejorar la precisión, dividimos el intervalo $[a, b]$ en $n$ subintervalos de igual ancho $h = \frac{b-a}{n}$ definidos por los nodos $x_i = a + ih$. Aplicamos la regla simple en cada subintervalo y sumamos:

$$\int_{a}^{b} f(x) \, dx \approx \frac{h}{2} \left[ f(x_0) + 2\sum_{i=1}^{n-1} f(x_i) + f(x_n) \right]$$

El **error compuesto** está acotado por:
$$E_{T,comp} = -\frac{(b-a)h^2}{12} f''(c) \quad \implies O(h^2)$$
La precisión aumenta cuadráticamente al reducir el tamaño de paso $h$.

---

## 3. La Regla de Simpson (Regla de Simpson 1/3)

La regla de Simpson aproxima $f(x)$ mediante un polinomio cuadrático (parábola de grado 2) que pasa por tres puntos: los extremos del intervalo y el punto medio $m = \frac{a+b}{2}$.

### 3.1 Regla de Simpson Simple
$$\int_{a}^{b} f(x) \, dx \approx \frac{b - a}{6} \left[ f(a) + 4f\left(\frac{a+b}{2}\right) + f(b) \right]$$

El **error de truncamiento** es:
$$E_S = -\frac{(b-a)^5}{2880} f^{(4)}(c) \quad \text{donde } c \in (a, b)$$

> [!TIP]
> Dado que el error depende de la cuarta derivada $f^{(4)}(c)$, la regla de Simpson es **exacta para cualquier polinomio de grado 3 o menor**, a pesar de estar basada en una parábola cuadrática.

### 3.2 Regla de Simpson Compuesta
Dividimos el intervalo $[a, b]$ en un **número par $n$** de subintervalos de ancho $h = \frac{b-a}{n}$. Aplicando la regla simple a cada par consecutivo de intervalos resulta:

$$\int_{a}^{b} f(x) \, dx \approx \frac{h}{3} \left[ f(x_0) + 4\sum_{i=1,3,\dots}^{n-1} f(x_i) + 2\sum_{j=2,4,\dots}^{n-2} f(x_j) + f(x_n) \right]$$

El **error compuesto** de Simpson está acotado por:
$$E_{S,comp} = -\frac{(b-a)h^4}{180} f^{(4)}(c) \quad \implies O(h^4)$$
Esta tasa de convergencia de orden 4 la hace extremadamente eficiente y precisa para funciones suaves.

---

## 4. El Toque Informático

### Integración Numérica en Motores de Física de Videojuegos
En el desarrollo de un motor físico (como los usados en motores de videojuegos o simuladores), debemos integrar las ecuaciones diferenciales de movimiento de Newton para actualizar el estado del juego en cada fotograma (frame rate):
$$\frac{dx}{dt} = v(t), \quad \frac{dv}{dt} = a(t) = \frac{F(t)}{m}$$

Si bien se utilizan integradores simplificados en tiempo real (como la **Integración de Euler** o **Verlet**), las simulaciones científicas de precisión y el análisis estadístico offline recurren a las reglas compuestas de Trapecio y Simpson para procesar grandes flujos de datos.

A continuación, implementamos en Python ambas reglas compuestas para resolver la integral de una función no elemental común en física y estadística:
$$I = \int_{0}^{1} e^{-x^2} \, dx$$
Comparamos el error absoluto de ambos métodos frente a la solución exacta de alta precisión.

```python
import numpy as np
import scipy.integrate as integrate

def f(x):
    return np.exp(-x**2)

# 1. Regla del Trapecio Compuesta
def trapecio_compuesto(func, a, b, n):
    h = (b - a) / n
    x = np.linspace(a, b, n + 1)
    y = func(x)
    
    suma = y[0] + 2.0 * np.sum(y[1:-1]) + y[-1]
    return (h / 2.0) * suma

# 2. Regla de Simpson Compuesta
def simpson_compuesto(func, a, b, n):
    if n % 2 != 0:
        raise ValueError("El número de subintervalos n debe ser par para la regla de Simpson.")
    h = (b - a) / n
    x = np.linspace(a, b, n + 1)
    y = func(x)
    
    suma = y[0] + 4.0 * np.sum(y[1:-1:2]) + 2.0 * np.sum(y[2:-2:2]) + y[-1]
    return (h / 3.0) * suma

# Parámetros
a, b = 0.0, 1.0
subintervalos = 20 # Debe ser par

# Integral de referencia de alta precisión usando scipy (método adaptativo QUAD)
valor_referencia, _ = integrate.quad(f, a, b)

res_trap = trapecio_compuesto(f, a, b, subintervalos)
res_simp = simpson_compuesto(f, a, b, subintervalos)

err_trap = abs(valor_referencia - res_trap)
err_simp = abs(valor_referencia - res_simp)

print(f"Valor de referencia exacto: {valor_referencia:.16f}\n")
print(f"Trapecio Compuesto (n={subintervalos}): {res_trap:.16f} | Error: {err_trap:.2e}")
print(f"Simpson Compuesto  (n={subintervalos}): {res_simp:.16f} | Error: {err_simp:.2e}")
```

La regla de Simpson aprovecha la suavidad de la campana de Gauss para dar un error en el orden de $10^{-8}$ con solo $20$ intervalos, mientras que la regla del Trapecio arroja un error mucho mayor de $10^{-4}$ con el mismo número de muestras.

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Aproximar el valor de la integral $\int_{0}^{2} x^3 \, dx$ usando la Regla de Simpson Simple ($n=2$). Comparar con el valor analítico real obtenido por la regla de Barrow.

**Solución:**
1.  **Cálculo analítico real (Barrow)**:
    $$\int_{0}^{2} x^3 \, dx = \left[ \frac{x^4}{4} \right]_0^2 = \frac{2^4}{4} - \frac{0^4}{4} = \frac{16}{4} - 0 = 4$$
2.  **Aproximación por la Regla de Simpson Simple**:
    El intervalo es $[a, b] = [0, 2]$.
    El punto medio es $m = \frac{a+b}{2} = \frac{0+2}{2} = 1$.
    Evaluamos la función $f(x) = x^3$ en los tres nodos:
    *   $f(a) = f(0) = 0^3 = 0$
    *   $f(m) = f(1) = 1^3 = 1$
    *   $f(b) = f(2) = 2^3 = 8$
    Sustituyendo en la fórmula de Simpson Simple:
    $$\int_{0}^{2} x^3 \, dx \approx \frac{b-a}{6} \Big[ f(a) + 4f(m) + f(b) \Big] = \frac{2-0}{6} \Big[ 0 + 4(1) + 8 \Big] = \frac{2}{6} \cdot 12 = \frac{1}{3} \cdot 12 = 4$$
3.  **Comparación de resultados**:
    El valor aproximado es exactamente $4$, igual al valor real. Esto se debe a que la regla de Simpson tiene un error proporcional a la cuarta derivada de la función. Dado que la función $f(x) = x^3$ tiene derivadas de orden 4 nulas ($f^{(4)}(x) = 0$), el error de truncamiento de la regla de Simpson es exactamente cero.

---

## 6. Ejercicios Propuestos

1.  Estimar el valor de $\int_{1}^{3} \frac{1}{x} \, dx$ utilizando la Regla del Trapecio Compuesta con $n = 4$ subintervalos. Comparar el resultado aproximado con el valor exacto de la integral ($\ln 3 \approx 1.098612$).
2.  Calcular una aproximación de $\int_{0}^{\pi} \sin(x) \, dx$ empleando la Regla de Simpson Compuesta con $n = 4$ intervalos y calcular el error absoluto cometido.
3.  Determinar analíticamente el número mínimo de subintervalos $n$ requeridos para aproximar la integral $\int_{0}^{1} e^x \, dx$ mediante la Regla del Trapecio Compuesta con un error absoluto garantizado menor que $10^{-4}$.
