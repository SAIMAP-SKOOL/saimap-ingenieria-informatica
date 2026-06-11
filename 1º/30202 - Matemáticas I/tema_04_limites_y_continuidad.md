# Tema 4: Cálculo de Límites y Continuidad

El cálculo infinitesimal se sustenta sobre el concepto de límite. En la ingeniería informática, estos conceptos son esenciales para comprender la continuidad de señales físicas digitalizadas, el modelado de variables continuas en simulación física y el análisis de algoritmos de optimización o búsqueda continua de raíces.

---

## 1. Límites de Funciones Reales

Consideramos una función real de variable real $f: D \subset \mathbb{R} \to \mathbb{R}$.

### 1.1 Definición Formal de Límite (Límite de Weierstrass o definición $\varepsilon-\delta$)
Decimos que el límite de $f(x)$ cuando $x$ tiende a $a$ es $L \in \mathbb{R}$, y lo escribimos como:
$$\lim_{x\to a} f(x) = L$$
si para cada número real $\varepsilon > 0$, existe un número real $\delta > 0$ tal que para todo $x \in D$, si la distancia entre $x$ y $a$ (sin ser $x=a$) es menor que $\delta$, entonces la distancia entre $f(x)$ y $L$ es menor que $\varepsilon$:

$$\lim_{x\to a} f(x) = L \iff \forall \varepsilon > 0, \exists \delta > 0 \text{ tal que } \forall x \in D, 0 < |x - a| < \delta \implies |f(x) - L| < \varepsilon$$

```
     f(x) ^
          |
    L+eps |-------------------+
          |                  /|
      L   |                 / |
          |                *  |
    L-eps |---------------+|  |
          |               ||  |
          +---------------++--+----> x
          0              a-del a a+del
```

### 1.2 Límites Laterales
*   **Límite por la izquierda**: $x$ se aproxima a $a$ desde valores menores que $a$ ($x < a$):
    $$\lim_{x\to a^-} f(x) = L_1$$
*   **Límite por la derecha**: $x$ se aproxima a $a$ desde valores mayores que $a$ ($x > a$):
    $$\lim_{x\to a^+} f(x) = L_2$$

> [!NOTE]
> El límite $\lim_{x\to a} f(x)$ existe si y solo si existen ambos límites laterales y son iguales:
> $$\lim_{x\to a} f(x) = L \iff \lim_{x\to a^-} f(x) = \lim_{x\to a^+} f(x) = L$$

---

## 2. Continuidad de Funciones

Una función $f(x)$ es **continua en un punto** $a \in D$ si satisface las tres condiciones siguientes:
1.  Existe $f(a)$ (la función está definida en $a$).
2.  Existe el límite $\lim_{x\to a} f(x)$.
3.  El valor del límite coincide con el valor de la función:
    $$\lim_{x\to a} f(x) = f(a)$$

Una función es **continua en un intervalo abierto** $(a, b)$ si es continua en cada uno de sus puntos. Es **continua en un intervalo cerrado** $[a, b]$ si es continua en $(a, b)$, continua por la derecha en $a$ ($\lim_{x\to a^+} f(x) = f(a)$) y continua por la izquierda en $b$ ($\lim_{x\to b^-} f(x) = f(b)$).

### Tipos de Discontinuidad
Si no se cumple la definición de continuidad, el punto $a$ se cataloga como una **discontinuidad**:
1.  **Evitable**: Existe $\lim_{x\to a} f(x) = L$, pero $f(a) \ne L$ o $f(a)$ no está definido.
2.  **Inevitables o Esenciales**:
    *   **De salto finito**: Los límites laterales existen pero son distintos:
        $$|\lim_{x\to a^-} f(x) - \lim_{x\to a^+} f(x)| = K < \infty$$
    *   **De salto infinito**: Al menos uno de los límites laterales es $+\infty$ o $-\infty$.

---

## 3. Teoremas Fundamentales de Continuidad

Estos teoremas proporcionan garantías analíticas cruciales para la resolución de ecuaciones y problemas de optimización.

### 3.1 Teorema de Bolzano
Sea $f(x)$ una función continua en el intervalo cerrado $[a, b]$. Si la función toma valores de signos opuestos en los extremos del intervalo (es decir, $f(a) \cdot f(b) < 0$), entonces existe al menos un punto $c \in (a, b)$ tal que:
$$f(c) = 0$$

*Interpretación geométrica*: Si una curva continua empieza por debajo del eje X y termina por encima (o viceversa), necesariamente tiene que cruzar el eje X en algún punto intermedio.

```
      y ^
        |            * f(b) > 0
        |           /
  ------+----------c----------> x
        |         /
        |        /
        |       * f(a) < 0
```

### 3.2 Teorema del Valor Intermedio (Darboux)
Si $f(x)$ es continua en $[a, b]$, entonces toma todos los valores intermedios entre $f(a)$ y $f(b)$.

### 3.3 Teorema de Weierstrass
Si $f(x)$ es una función continua en un intervalo cerrado y acotado $[a, b]$, entonces $f(x)$ alcanza sus valores máximo y mínimo absolutos en ese intervalo. Es decir, existen $x_1, x_2 \in [a, b]$ tales que:
$$f(x_1) \le f(x) \le f(x_2) \quad \forall x \in [a, b]$$

---

## 4. El Toque Informático

### 4.1 Búsqueda de Raíces por Bipartición (Bisección)
El Teorema de Bolzano garantiza la existencia de una raíz en un intervalo $[a, b]$ si $f(a) \cdot f(b) < 0$. Este hecho analítico da origen al **Algoritmo de Bisección**, un método numérico robusto para resolver ecuaciones de la forma $f(x) = 0$.

El algoritmo divide repetidamente a la mitad el intervalo de búsqueda:
1.  Calcular el punto medio $m = \frac{a+b}{2}$.
2.  Si $f(m) \approx 0$ o la longitud del intervalo $(b-a)$ es menor que una tolerancia predefinida, detener la búsqueda y retornar $m$.
3.  Evaluar el signo de $f(m)$:
    *   Si $f(a) \cdot f(m) < 0$, la raíz está en $[a, m] \implies b = m$.
    *   Si $f(m) \cdot f(b) < 0$, la raíz está en $[m, b] \implies a = m$.
4.  Repetir.

### Complejidad y Robustez
*   **Complejidad temporal**: En cada paso el intervalo de búsqueda se reduce a la mitad. Tras $k$ iteraciones, la amplitud del intervalo es $\frac{b-a}{2^k}$. Para lograr una precisión $\varepsilon$, requerimos un número máximo de iteraciones:
    $$k = \left\lceil \log_2\left(\frac{b-a}{\varepsilon}\right) \right\rceil$$
    Esto confiere al algoritmo una **complejidad temporal de $O(\log(1/\varepsilon))$**, análoga a la búsqueda binaria en arrays.
*   **Parada robusta**: Para evitar bucles infinitos causados por imprecisiones de coma flotante en funciones muy empinadas o planas, la condición de parada debe comprobar la amplitud del intervalo $|b-a| < \varepsilon$ y un límite de iteraciones.

A continuación, implementamos el algoritmo de bisección en Python para hallar la raíz de $f(x) = x^3 - x - 2$ en el intervalo $[1, 2]$.

```python
import numpy as np

def f(x):
    return x**3 - x - 2

def biseccion(func, a, b, tol=1e-7, max_iter=100):
    # Validamos Bolzano
    if func(a) * func(b) >= 0:
        raise ValueError("El teorema de Bolzano no garantiza la raíz en este intervalo.")
        
    for iteracion in range(1, max_iter + 1):
        m = a + (b - a) / 2.0  # Evitamos cancelaciones catastróficas frente a (a+b)/2
        fm = func(m)
        
        # Criterios de parada
        if abs(fm) < tol or (b - a) / 2.0 < tol:
            return m, iteracion
            
        if func(a) * fm < 0:
            b = m
        else:
            a = m
            
    return m, max_iter

# Ejecución
raiz, iters = biseccion(f, 1.0, 2.0)
print(f"Raíz aproximada: {raiz:.8f}")
print(f"Número de iteraciones: {iters}")
print(f"Validación f(raiz): {f(raiz):.2e}")
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Demostrar que la ecuación $x^5 - 3x - 1 = 0$ tiene al menos una solución en el intervalo $[1, 2]$.

**Solución utilizando el Teorema de Bolzano:**
1.  Definimos la función $f(x) = x^5 - 3x - 1$.
2.  $f(x)$ es una función polinómica. Por tanto, es continua en todo su dominio $\mathbb{R}$ y, por consiguiente, continua en el intervalo cerrado $[1, 2]$.
3.  Evaluamos los valores de la función en los extremos del intervalo:
    *   $f(1) = 1^5 - 3(1) - 1 = 1 - 3 - 1 = -3 \quad (< 0)$.
    *   $f(2) = 2^5 - 3(2) - 1 = 32 - 6 - 1 = 25 \quad (> 0)$.
4.  Puesto que $f(1) < 0$ y $f(2) > 0$, el producto $f(1) \cdot f(2) < 0$.
5.  Al cumplirse las hipótesis del Teorema de Bolzano, existe al menos un punto $c \in (1, 2)$ tal que $f(c) = 0$.
6.  Esto demuestra formalmente que existe al menos una solución de la ecuación en el intervalo $[1, 2]$.

---

## 6. Ejercicios Propuestos

1.  Estudiar la continuidad de la función dada por tramos y clasificar sus discontinuidades:
    $$f(x) = \begin{cases} 
    \frac{x^2 - 1}{x - 1} & \text{si } x < 1 \\ 
    3 & \text{si } x = 1 \\ 
    x^2 + 1 & \text{si } x > 1 
    \end{cases}$$
2.  Demostrar que las gráficas de las funciones $g(x) = e^x$ y $h(x) = x^2 + 2$ se cortan en al menos un punto del intervalo $[-2, 0]$.
3.  Determinar la cantidad mínima de iteraciones del algoritmo de bisección requeridas para aproximar la raíz de una función en el intervalo $[0, 4]$ con un error de precisión menor que $10^{-6}$.
