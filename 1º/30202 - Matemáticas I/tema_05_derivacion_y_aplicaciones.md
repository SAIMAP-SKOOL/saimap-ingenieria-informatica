# Tema 5: Derivación y Aplicaciones

La derivación mide la tasa de variación instantánea de una función. En la ingeniería informática, las derivadas son el núcleo de los algoritmos de optimización (Machine Learning y Redes Neuronales), la simulación física en videojuegos, el procesamiento digital de imágenes (detección de bordes) y el análisis de algoritmos de control numérico.

---

## 1. Concepto e Interpretación de la Derivada

Dada una función $f(x)$ definida en un intervalo abierto que contiene a $a$, definimos la **derivada** de $f$ en el punto $a$, denotada por $f'(a)$, como el límite:
$$f'(a) = \lim_{h\to 0} \frac{f(a+h) - f(a)}{h}$$
si este límite existe y es finito.

### Interpretación Geométrica
La derivada $f'(a)$ representa la **pendiente de la recta tangente** a la curva $y = f(x)$ en el punto $(a, f(a))$.

La ecuación de la recta tangente en dicho punto viene dada por:
$$y - f(a) = f'(a)(x - a)$$

```
     y ^
       |             * curva f(x)
       |            /|
       |     *-----/------+ recta tangente (pendiente f'(a))
       |    /     * (a, f(a))
       |   /     /|
  -----+--+-----+-------------> x
       0        a
```

---

## 2. Reglas de Derivación Clásicas

Para derivar funciones complejas de forma sistemática sin recurrir a la definición por límite, empleamos las reglas algebraicas básicas y la **Regla de la Cadena** para la composición de funciones:

*   **Derivada de una constante**: $(c)' = 0$
*   **Regla de la potencia**: $(x^n)' = n x^{n-1}$
*   **Derivada de la suma/resta**: $(u \pm v)' = u' \pm v'$
*   **Regla del producto**: $(u \cdot v)' = u'v + uv'$
*   **Regla del cociente**: $\left(\frac{u}{v}\right)' = \frac{u'v - uv'}{v^2}$
*   **Regla de la Cadena (composición de funciones)**: Si $y = (f \circ g)(x) = f(g(x))$, entonces:
    $$(f(g(x)))' = f'(g(x)) \cdot g'(x)$$

---

## 3. Teoremas Fundamentales del Valor Medio

Estos teoremas establecen conexiones entre los valores de una función y los valores de sus derivadas en intervalos específicos.

### 3.1 Teorema de Rolle
Sea $f(x)$ una función continua en $[a, b]$ y derivable en $(a, b)$. Si $f(a) = f(b)$, entonces existe al menos un punto $c \in (a, b)$ tal que:
$$f'(c) = 0$$

*Interpretación*: Si una función tiene la misma altura al inicio y al final de un intervalo, debe haber un punto donde su tangente sea completamente horizontal.

### 3.2 Teorema del Valor Medio de Lagrange
Sea $f(x)$ continua en $[a, b]$ y derivable en $(a, b)$. Entonces existe al menos un punto $c \in (a, b)$ tal que:
$$f'(c) = \frac{f(b) - f(a)}{b - a}$$

*Interpretación*: Existe al menos un punto donde la tasa de cambio instantánea (pendiente de la tangente) es igual a la tasa de cambio promedio a lo largo de todo el intervalo (pendiente de la secante que une los extremos).

### 3.3 Teorema del Valor Medio de Cauchy
Sean $f(x)$ y $g(x)$ continuas en $[a, b]$ y derivables en $(a, b)$. Si $g'(x) \ne 0$ para todo $x \in (a, b)$, entonces existe al menos un punto $c \in (a, b)$ tal que:
$$\frac{f'(c)}{g'(c)} = \frac{f(b) - f(a)}{g(b) - g(a)}$$

### 3.4 Regla de L'Hôpital
Consecuencia directa del Teorema de Cauchy, se emplea para resolver límites indeterminados de tipo $\frac{0}{0}$ o $\frac{\infty}{\infty}$.
Si $\lim_{x\to a} \frac{f(x)}{g(x)}$ presenta una indeterminación de tipo $\frac{0}{0}$ o $\frac{\infty}{\infty}$, y $f, g$ son derivables en las cercanías de $a$ con $g'(x) \ne 0$:
$$\lim_{x\to a} \frac{f(x)}{g(x)} = \lim_{x\to a} \frac{f'(x)}{g'(x)}$$
siempre que este último límite exista o sea infinito.

---

## 4. Extremos Locales y Optimización

El análisis de derivadas permite localizar los valores máximos y mínimos de una función real.

### Puntos Críticos
Un punto $x = c$ en el dominio de $f$ es un **punto crítico** si:
$$f'(c) = 0 \quad \text{o} \quad f'(c) \text{ no existe}$$

### Criterio de la Segunda Derivada para Extremos Locales
Sea $f(x)$ una función dos veces derivable en un punto crítico $c$ donde $f'(c) = 0$:
*   Si $f''(c) > 0 \implies x = c$ es un **mínimo local**.
*   Si $f''(c) < 0 \implies x = c$ es un **máximo local**.
*   Si $f''(c) = 0$, el criterio no decide (se deben evaluar derivadas de orden superior).

---

## 5. El Toque Informático

### 5.1 Descenso de Gradiente (Gradient Descent)
En inteligencia artificial y aprendizaje automático, los problemas de entrenamiento (como entrenar redes neuronales o ajustar una regresión lineal) consisten en minimizar una función de coste $J(\theta)$ que mide el error del modelo.

El **Descenso de Gradiente** es un algoritmo de optimización iterativo basado en derivadas. Para una función de una sola variable $f(x)$, si queremos encontrar su mínimo local, empezamos en una estimación inicial $x_0$ y nos desplazamos en la **dirección opuesta a la derivada** (la derivada apunta en la dirección de máximo crecimiento; por tanto, restar la derivada nos hace descender hacia el mínimo):

$$x_{k+1} = x_k - \alpha \cdot f'(x_k)$$

donde $\alpha > 0$ es el **hiperparámetro de tasa de aprendizaje** (learning rate).
*   Si $\alpha$ es muy pequeño, la convergencia será extremadamente lenta.
*   Si $\alpha$ es muy grande, el algoritmo puede oscilar y divergir del mínimo.

A continuación, implementamos el algoritmo de descenso de gradiente en Python para encontrar el mínimo de la función cuadrática:
$$f(x) = x^2 - 4x + 5$$
Su derivada analítica es $f'(x) = 2x - 4$. Su mínimo exacto está en $x = 2$ (donde $f'(2) = 0$).

```python
import numpy as np
import matplotlib.pyplot as plt

def f(x):
    return x**2 - 4*x + 5

def df(x):
    return 2*x - 4

def descenso_gradiente(x_inicial, learning_rate=0.1, epochs=20, tol=1e-5):
    historial_x = [x_inicial]
    historial_y = [f(x_inicial)]
    
    x = x_inicial
    for epoch in range(epochs):
        gradiente = df(x)
        
        # Condición de parada si el gradiente es casi cero
        if abs(gradiente) < tol:
            break
            
        # Paso de actualización del descenso de gradiente
        x = x - learning_rate * gradiente
        
        historial_x.append(x)
        historial_y.append(f(x))
        
    return x, historial_x, historial_y

# Ejecución
x_opt, hist_x, hist_y = descenso_gradiente(x_inicial=0.0, learning_rate=0.25)

print(f"Mínimo aproximado encontrado en x = {x_opt:.6f}")
print(f"Valor mínimo de la función f(x) = {f(x_opt):.6f}")

# Graficar la trayectoria del descenso
x_val = np.linspace(-1, 5, 200)
plt.figure(figsize=(9, 5))
plt.plot(x_val, f(x_val), label='f(x) = x^2 - 4x + 5', color='blue')
plt.scatter(hist_x, hist_y, color='red', zorder=3, label='Iteraciones')
plt.plot(hist_x, hist_y, color='red', linestyle='--', alpha=0.6)
plt.xlabel('x')
plt.ylabel('f(x)')
plt.title('Optimización mediante Descenso de Gradiente')
plt.legend()
plt.grid(True)
plt.show()
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Encontrar las dimensiones de una caja rectangular con base cuadrada abierta por arriba que tenga un volumen de $4 \, m^3$ y que minimice la cantidad de material empleado para su construcción (área superficial).

**Solución utilizando Optimización por Derivadas:**
1.  **Plantear las variables**:
    *   Lado de la base cuadrada: $x$ ($x > 0$).
    *   Altura de la caja: $h$ ($h > 0$).
2.  **Relación de volumen**:
    $$V = x^2 \cdot h = 4 \implies h = \frac{4}{x^2}$$
3.  **Función a optimizar (Área superficial)**:
    La caja no tiene tapa superior, por tanto:
    $$A(x, h) = \text{Área base} + 4 \cdot \text{Área caras laterales} = x^2 + 4xh$$
4.  **Sustituir $h$ para obtener una función en una sola variable $x$**:
    $$A(x) = x^2 + 4x\left(\frac{4}{x^2}\right) = x^2 + \frac{16}{x}$$
5.  **Calcular la primera derivada y buscar puntos críticos**:
    $$A'(x) = 2x - \frac{16}{x^2}$$
    Para encontrar los puntos críticos, igualamos a cero:
    $$2x - \frac{16}{x^2} = 0 \implies 2x^3 = 16 \implies x^3 = 8 \implies x = 2$$
6.  **Validar si es un mínimo mediante la segunda derivada**:
    $$A''(x) = 2 + \frac{32}{x^3}$$
    Evaluando en $x = 2$:
    $$A''(2) = 2 + \frac{32}{8} = 2 + 4 = 6 > 0$$
    Puesto que $A''(2) > 0$, la dimensión $x = 2 \, m$ es un mínimo local.
7.  **Calcular la altura $h$**:
    $$h = \frac{4}{2^2} = 1 \, m$$
8.  **Dimensiones óptimas**: La base debe medir $2 \times 2 \, m$ y la altura debe ser de $1 \, m$.

---

## 7. Ejercicios Propuestos

1.  Calcular aplicando la regla de L'Hôpital el siguiente límite indeterminado:
    $$\lim_{x\to 0} \frac{x - \sin x}{x^3}$$
2.  Estudiar los intervalos de crecimiento, decrecimiento y extremos locales de la función:
    $$f(x) = x e^{-x}$$
3.  Demostrar que la ecuación polinómica $3x^5 + 15x - 8 = 0$ tiene exactamente una raíz real.
    *(Pista: Usa primero Bolzano para asegurar la existencia, y luego Rolle para probar por contradicción la unicidad de la raíz).*
