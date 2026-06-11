# Tema 8: Resolución Numérica de Ecuaciones

Encontrar las soluciones exactas a ecuaciones de la forma $f(x) = 0$ suele ser imposible de forma analítica cuando se trata de funciones no lineales complejas. El cálculo numérico proporciona algoritmos iterativos para aproximar estas raíces con la precisión deseada. Estos métodos sustentan el funcionamiento de motores de física en videojuegos, la resolución de trayectorias espaciales y la optimización de sistemas en ingeniería.

---

## 1. Planteamiento del Problema

Buscamos aproximar un número real $\alpha \in \mathbb{R}$ tal que:
$$f(\alpha) = 0$$
donde $f$ es una función continua en un intervalo que contiene a la raíz $\alpha$.

Los métodos numéricos son **iterativos**: parten de una aproximación inicial (o un intervalo) y generan una sucesión de aproximaciones $\{x_k\}_{k=0}^{\infty}$ que esperamos que converja a la raíz:
$$\lim_{k\to\infty} x_k = \alpha$$

---

## 2. Método de la Bisección (Revisión y Análisis Numérico)

Como vimos en el Tema 4, el método de la Bisección se basa en el Teorema de Bolzano.

### Análisis del Error y Convergencia
Si $[a_0, b_0]$ es el intervalo inicial y denotamos por $x_k$ el punto medio calculado en la iteración $k$:
*   La raíz real $\alpha$ y la aproximación $x_k$ están contenidas en el intervalo de la etapa $k$, cuyo tamaño es $\frac{b_0 - a_0}{2^k}$.
*   Por tanto, el **error absoluto cometido en el paso $k$**, denotado por $e_k = |x_k - \alpha|$, está estrictamente acotado por:
    $$e_k \le \frac{b_0 - a_0}{2^{k+1}}$$

Este método garantiza una **convergencia lineal** con un factor de reducción de error constante de $0.5$ en cada paso. Es muy robusto y siempre converge si se cumple la hipótesis de Bolzano, pero su velocidad es relativamente lenta.

---

## 3. Método de Newton-Raphson

El **Método de Newton-Raphson** es uno de los algoritmos de resolución numérica más potentes. En lugar de dividir intervalos, utiliza la información de la derivada de la función para acelerar la convergencia.

### Derivación mediante Polinomio de Taylor
Si disponemos de una aproximación $x_k$ cercana a la raíz $\alpha$, aproximamos la función $f(x)$ mediante su polinomio de Taylor de primer grado (recta tangente) en el punto $x_k$:
$$f(x) \approx f(x_k) + f'(x_k)(x - x_k)$$

Buscamos el punto de corte con el eje X de esta recta de aproximación, imponiendo $f(x) = 0$:
$$0 \approx f(x_k) + f'(x_k)(x - x_k) \implies x - x_k \approx -\frac{f(x_k)}{f'(x_k)}$$

Esto da origen a la **fórmula iterativa de Newton-Raphson**:
$$x_{k+1} = x_k - \frac{f(x_k)}{f'(x_k)} \quad (\text{para } f'(x_k) \ne 0)$$

### Interpretación Geométrica
En cada paso $k$, trazamos la recta tangente a la curva $y = f(x)$ en el punto $(x_k, f(x_k))$. La intersección de esta recta tangente con el eje horizontal define la siguiente aproximación $x_{k+1}$.

```
    y ^
      |           * curva f(x)
      |          /|
      |         / | (x_k, f(x_k))
      |        /  |
      |       / \ |
  ----+------*---+-----+-----> x
      0    x_k+1 |     x_k
                 v recta tangente
```

---

## 4. Convergencia del Método de Newton-Raphson

El método de Newton-Raphson presenta propiedades de convergencia excelentes, pero también limitaciones críticas.

### Convergencia Cuadrática Local
Si la función $f(x)$ es dos veces continuamente derivable, la raíz $\alpha$ es simple ($f'(\alpha) \ne 0$) y el valor inicial $x_0$ se elige suficientemente cerca de $\alpha$, entonces el método converge de forma **cuadrática**:
$$\lim_{k\to\infty} \frac{|x_{k+1} - \alpha|}{|x_k - \alpha|^2} = \left| \frac{f''(\alpha)}{2f'(\alpha)} \right|$$

*Significado práctico*: El número de dígitos decimales de precisión correctos se **duplica** en cada iteración cuando nos encontramos cerca de la raíz.

### 4.2 Limitaciones e Inestabilidades
1.  **Dependencia del punto de partida**: Si $x_0$ no está cerca de la raíz, el método puede oscilar infinitamente o divergir.
2.  **Derivada nula**: Si en algún paso $f'(x_k) \approx 0$, la división por la derivada causa un error aritmético o proyecta la aproximación al infinito.
3.  **Raíces múltiples**: Si la raíz tiene multiplicidad $m > 1$ (es decir, $f(\alpha) = 0$ y $f'(\alpha) = 0$), la convergencia del método de Newton se degrada de cuadrática a lineal.

---

## 5. El Toque Informático

A continuación, implementamos en Python ambos métodos (Bisección y Newton-Raphson) para resolver la ecuación:
$$f(x) = \cos(x) - x = 0$$
Su derivada es $f'(x) = -\sin(x) - 1$.
Comparamos la velocidad de convergencia imprimiendo la evolución del error absoluto estimado paso a paso.

```python
import numpy as np
import matplotlib.pyplot as plt

def f(x):
    return np.cos(x) - x

def df(x):
    return -np.sin(x) - 1.0

# 1. Implementación de Bisección
def biseccion(func, a, b, tol=1e-12, max_iter=50):
    historial = []
    if func(a) * func(b) >= 0:
        return None, historial
    for k in range(max_iter):
        x_k = a + (b - a) / 2.0
        fx = func(x_k)
        historial.append(x_k)
        if abs(fx) < tol or (b - a) / 2.0 < tol:
            break
        if func(a) * fx < 0:
            b = x_k
        else:
            a = x_k
    return x_k, historial

# 2. Implementación de Newton-Raphson
def newton_raphson(func, dfunc, x0, tol=1e-12, max_iter=50):
    historial = [x0]
    x = x0
    for k in range(max_iter):
        derivada = dfunc(x)
        if abs(derivada) < 1e-15:
            print("Advertencia: Derivada cercana a cero.")
            break
        x_siguiente = x - func(x) / derivada
        historial.append(x_siguiente)
        if abs(x_siguiente - x) < tol:
            break
        x = x_siguiente
    return x, historial

# Ejecución
raiz_exacta = 0.7390851332151606 # Número de Dottie
raiz_bis, hist_bis = biseccion(f, 0.0, 1.0)
raiz_new, hist_new = newton_raphson(f, df, 0.0)

# Comparar velocidad de convergencia (errores acumulados)
err_bis = [abs(x - raiz_exacta) for x in hist_bis]
err_new = [abs(x - raiz_exacta) for x in hist_new]

print(f"Raíz Bisección: {raiz_bis:.12f} (Iteraciones: {len(hist_bis)})")
print(f"Raíz Newton:    {raiz_new:.12f} (Iteraciones: {len(hist_new)-1})")

# Gráfico de convergencia del error
plt.figure(figsize=(9, 5))
plt.plot(err_bis, label='Bisección (Convergencia Lineal)', marker='o', color='blue')
plt.plot(err_new, label='Newton-Raphson (Convergencia Cuadrática)', marker='s', color='red')
plt.yscale('log')
plt.xlabel('Iteración')
plt.ylabel('Error absoluto $|x_k - \\alpha|$')
plt.title('Comparación de Velocidad de Convergencia')
plt.legend()
plt.grid(True, which="both", ls="--")
plt.show()
```

La gráfica muestra cómo Newton-Raphson necesita apenas 5 iteraciones para alcanzar la precisión del límite de coma flotante de doble precisión de 64 bits, mientras que la Bisección requiere cerca de 40 iteraciones para lograr la misma precisión.

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Aplicar el método de Newton-Raphson para aproximar el valor de $\sqrt{2}$ mediante la resolución de la ecuación $f(x) = x^2 - 2 = 0$, comenzando con $x_0 = 1.5$. Realizar dos iteraciones de forma manual y evaluar el error cometido.

**Solución:**
1.  **Definir la función y su derivada**:
    $$f(x) = x^2 - 2 \implies f'(x) = 2x$$
2.  **Fórmula de recurrencia**:
    $$x_{k+1} = x_k - \frac{x_k^2 - 2}{2x_k} = x_k - \frac{x_k}{2} + \frac{1}{x_k} = \frac{x_k}{2} + \frac{1}{x_k}$$
    *(Esta es la conocida fórmula de Herón para la raíz cuadrada).*
3.  **Iteración 1 ($k = 0$)**:
    $$x_1 = \frac{1.5}{2} + \frac{1}{1.5} = 0.75 + \frac{2}{3} = 0.75 + 0.66666667 = 1.41666667$$
    Error absoluto exacto ($|\sqrt{2} - x_1|$):
    $$|1.41421356 - 1.41666667| = 0.00245311 \approx 2.45 \cdot 10^{-3}$$
4.  **Iteración 2 ($k = 1$)**:
    $$x_2 = \frac{1.41666667}{2} + \frac{1}{1.41666667} = 0.70833333 + \frac{12}{17} = 0.70833333 + 0.70588235 = 1.41421569$$
    Error absoluto exacto ($|\sqrt{2} - x_2|$):
    $$|1.41421356 - 1.41421569| = 0.00000213 \approx 2.13 \cdot 10^{-6}$$

Notamos que el error disminuye drásticamente de $10^{-3}$ a $10^{-6}$ en una sola iteración, evidenciando la convergencia cuadrática del método.

---

## 7. Ejercicios Propuestos

1.  Dada la ecuación $x^3 - x - 1 = 0$, aplicar el método de Newton-Raphson para realizar tres iteraciones partiendo de $x_0 = 1.5$.
2.  Estudiar analíticamente el comportamiento del método de Newton-Raphson al intentar resolver la ecuación $x^{1/3} = 0$, comenzando con cualquier punto de partida $x_0 \ne 0$. ¿Qué ocurre con la convergencia? Explicar la causa.
3.  Diseñar la fórmula iterativa del método de Newton-Raphson para calcular la función recíproca $\frac{1}{R}$ sin emplear operaciones de división, es decir, resolviendo $f(x) = \frac{1}{x} - R = 0$.
    *(Esta aproximación se utiliza en hardware informático a nivel de ALU para implementar la división rápida).*
