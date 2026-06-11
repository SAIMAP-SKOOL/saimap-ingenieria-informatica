# Tema 2: Sucesiones Numéricas

Una sucesión es una función cuyo dominio es el conjunto de los números naturales ($\mathbb{N}$) y cuyo codominio es el de los números reales ($\mathbb{C}$ o $\mathbb{R}$). En informática, las sucesiones describen el comportamiento paso a paso de los algoritmos iterativos, la asignación dinámica de memoria y la complejidad temporal asintótica de los programas.

---

## 1. Concepto de Límite y Convergencia

Una **sucesión real** es una lista ordenada de números reales denotada por $\{a_n\}_{n=1}^{\infty}$ o simplemente por $(a_n)$:
$$a_1, a_2, a_3, \dots, a_n, \dots$$
donde $a_n$ representa el término general o $n$-ésimo término de la sucesión.

### Definición Formal de Límite
Se dice que la sucesión $\{a_n\}$ converge a un límite $L \in \mathbb{R}$ si, para cualquier valor de tolerancia $\varepsilon > 0$, existe un número natural $N \in \mathbb{N}$ tal que para todo término posterior a $N$ ($n > N$), la distancia entre el término de la sucesión y el límite es menor que $\varepsilon$:

$$\lim_{n\to\infty} a_n = L \iff \forall\varepsilon > 0, \exists N \in \mathbb{N} \text{ tal que } \forall n > N, |a_n - L| < \varepsilon$$

```
   a_n ^
       |
  L+eps|------------------------------------ (Límite superior)
       |      o       o
    L  |* - - - - - - - - - - - - - o - - - - (Línea del Límite L)
       |  o        o     *     o  *
  L-eps|------------------------------------ (Límite inferior)
       |                       
       |
       +------+------+------+------+------+----> n
       0      1      2     ...     N     N+1
```

*   **Sucesiones Convergentes**: Tienen un límite finito $L \in \mathbb{R}$.
*   **Sucesiones Divergentes**: Su límite es $+\infty$ o $-\infty$.
*   **Sucesiones Oscilantes**: No tienden a ningún valor fijo ni divergen al infinito (por ejemplo, $a_n = (-1)^n$).

---

## 2. Cálculo de Límites y Resolución de Indeterminaciones

Al operar con límites de sucesiones podemos encontrarnos con expresiones cuyo resultado no es evidente directamente. Estas se conocen como **indeterminaciones** y las principales son:
$$\frac{\infty}{\infty}, \quad \frac{0}{0}, \quad \infty - \infty, \quad 0 \cdot \infty, \quad 1^\infty, \quad \infty^0, \quad 0^0$$

### 2.1 Indeterminación $\frac{\infty}{\infty}$ en Polinomios
Si $a_n = P(n)$ y $b_n = Q(n)$ son polinomios de grados $p$ y $q$, entonces:
$$\lim_{n\to\infty} \frac{P(n)}{Q(n)} = \begin{cases} 
\pm\infty & \text{si } p > q \\ 
\frac{\text{coeficiente principal de } P}{\text{coeficiente principal de } Q} & \text{si } p = q \\
0 & \text{si } p < q 
\end{cases}$$

### 2.2 Indeterminación $1^\infty$ (Número $e$)
Si una sucesión tiene la forma $(u_n)^{v_n}$ donde $\lim_{n\to\infty} u_n = 1$ y $\lim_{n\to\infty} v_n = \infty$, se utiliza la equivalencia asociada al número $e$:
$$\lim_{n\to\infty} (u_n)^{v_n} = e^{\lim_{n\to\infty} v_n(u_n - 1)}$$

---

## 3. Criterios Avanzados de Convergencia

Cuando el término general de la sucesión no es simple, se recurre a criterios específicos.

### 3.1 Criterio de la Cota de la Razón (o del Sándwich / Compresión)
Sean $\{a_n\}$, $\{b_n\}$ y $\{c_n\}$ sucesiones tales que $a_n \le b_n \le c_n$ para todo $n \ge N_0$. Si $\lim_{n\to\infty} a_n = \lim_{n\to\infty} c_n = L$, entonces:
$$\lim_{n\to\infty} b_n = L$$

### 3.2 Criterio de Stolz-Cesàro (La Regla de L'Hôpital para Sucesiones)
Sea $\{x_n\}$ una sucesión cualquiera y $\{y_n\}$ una sucesión estrictamente creciente y divergente a $+\infty$ ($\lim_{n\to\infty} y_n = +\infty$). Si existe el límite:
$$\lim_{n\to\infty} \frac{x_n - x_{n-1}}{y_n - y_{n-1}} = L \quad (L \in \mathbb{R} \cup \{\pm\infty\})$$
Entonces:
$$\lim_{n\to\infty} \frac{x_n}{y_n} = L$$

### 3.3 Criterio de la Media Aritmética
Si $\lim_{n\to\infty} a_n = L$, entonces la sucesión de sus medias aritméticas también converge a $L$:
$$\lim_{n\to\infty} \frac{a_1 + a_2 + \dots + a_n}{n} = L$$

### 3.4 Criterio de la Media Geométrica (para términos positivos)
Si $\lim_{n\to\infty} a_n = L$ (con $a_n > 0$), entonces la sucesión de sus medias geométricas también converge a $L$:
$$\lim_{n\to\infty} \sqrt[n]{a_1 \cdot a_2 \cdot \dots \cdot a_n} = L$$

---

## 4. El Toque Informático

### 4.1 Complejidad Temporal Asintótica (Notación O Grande)
En ciencias de la computación, la complejidad temporal de un algoritmo se mide con la **Notación Big-O** ($O$), la cual es conceptualmente el límite de una sucesión.
Decimos que el número de operaciones de un programa con entrada de tamaño $n$ es de orden $g(n)$, denotado por $f(n) = O(g(n))$, si existe una constante $C > 0$ y un umbral $N_0$ tal que para toda entrada mayor que el umbral, el coste no supera a $C \cdot g(n)$:
$$\lim_{n\to\infty} \frac{f(n)}{g(n)} = K < \infty$$

Por ejemplo, comparar un algoritmo de ordenamiento de burbuja ($O(n^2)$) con MergeSort ($O(n\log n)$) equivale a analizar el comportamiento asintótico cuando $n \to \infty$:
$$\lim_{n\to\infty} \frac{n \log n}{n^2} = \lim_{n\to\infty} \frac{\log n}{n} = 0$$
Esto demuestra formalmente que, para entradas grandes, MergeSort es infinitamente más eficiente que el ordenamiento de burbuja.

### 4.2 Análisis de Recurrencias e Iteración: La Sucesión de Fibonacci
Muchas sucesiones en informática se definen de forma recursiva (ecuaciones en diferencias). La más emblemática es la sucesión de Fibonacci:
$$F_0 = 0, \quad F_1 = 1, \quad F_n = F_{n-1} + F_{n-2} \quad \text{para } n \ge 2$$

Analíticamente, el término general viene dado por la **Fórmula de Binet**:
$$F_n = \frac{\phi^n - \psi^n}{\sqrt{5}}$$
donde $\phi = \frac{1+\sqrt{5}}{2} \approx 1.618$ (el número áureo) y $\psi = \frac{1-\sqrt{5}}{2} \approx -0.618$.
Esto implica que el límite del cociente de dos términos consecutivos es precisamente la razón áurea:
$$\lim_{n\to\infty} \frac{F_{n+1}}{F_n} = \phi$$

A continuación, implementamos en Python tres enfoques de cálculo de la sucesión de Fibonacci para analizar cómo la elección algorítmica afecta a la complejidad de la sucesión de tiempos de ejecución.

```python
import time
import numpy as np
import matplotlib.pyplot as plt

# 1. Enfoque Recursivo Ineficiente: O(phi^n)
def fib_recursivo(n):
    if n <= 1:
        return n
    return fib_recursivo(n-1) + fib_recursivo(n-2)

# 2. Enfoque Iterativo con Programación Dinámica: O(n)
def fib_iterativo(n):
    if n <= 1:
        return n
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, a + b
    return b

# 3. Enfoque de Exponenciación Matricial: O(log n)
# Basado en la relación [[F_{n+1}, F_n], [F_n, F_{n-1}]] = [[1, 1], [1, 0]]^n
def fib_matricial(n):
    if n == 0: return 0
    F = np.array([[1, 1], [1, 0]], dtype=object)
    def power(A, p):
        res = np.eye(2, dtype=object)
        base = A
        while p > 0:
            if p % 2 == 1:
                res = np.dot(res, base)
            base = np.dot(base, base)
            p //= 2
        return res
    return power(F, n - 1)[0, 0]

# Comparación de tiempos de ejecución
rango_n = list(range(10, 35))
tiempos_rec = []
tiempos_it = []

for n in rango_n:
    # Tiempo recursivo
    t0 = time.perf_counter()
    fib_recursivo(n)
    tiempos_rec.append(time.perf_counter() - t0)
    
    # Tiempo iterativo
    t0 = time.perf_counter()
    fib_iterativo(n)
    tiempos_it.append(time.perf_counter() - t0)

# Gráfico de tiempos
plt.figure(figsize=(9, 5))
plt.plot(rango_n, tiempos_rec, label="Recursivo: O(1.618^n)", marker='o', color='red')
plt.plot(rango_n, tiempos_it, label="Iterativo: O(n)", marker='s', color='blue')
plt.yscale('log')
plt.xlabel("Término n de Fibonacci")
plt.ylabel("Tiempo de ejecución (segundos, escala logarítmica)")
plt.title("Estudio de Complejidad: Algoritmos para Fibonacci")
plt.legend()
plt.grid(True, which="both", ls="--")
plt.show()
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Calcular el límite de la sucesión:
$$\lim_{n\to\infty} \frac{1^2 + 2^2 + \dots + n^2}{n^3}$$

**Solución utilizando el Criterio de Stolz-Cesàro:**
Identificamos $x_n = 1^2 + 2^2 + \dots + n^2$ e $y_n = n^3$. 
Dado que $\{y_n\}$ es estrictamente creciente y tiende a $+\infty$, aplicamos el criterio:
$$\lim_{n\to\infty} \frac{x_n - x_{n-1}}{y_n - y_{n-1}} = \lim_{n\to\infty} \frac{(1^2 + \dots + n^2) - (1^2 + \dots + (n-1)^2)}{n^3 - (n-1)^3}$$
El numerador se simplifica directamente al término $n$-ésimo:
$$x_n - x_{n-1} = n^2$$
El denominador se expande mediante productos notables:
$$y_n - y_{n-1} = n^3 - (n^3 - 3n^2 + 3n - 1) = 3n^2 - 3n + 1$$

Sustituyendo en el límite:
$$\lim_{n\to\infty} \frac{n^2}{3n^2 - 3n + 1}$$
Como los polinomios del numerador y denominador tienen el mismo grado (grado 2), el límite es el cociente de sus coeficientes principales:
$$\lim_{n\to\infty} \frac{n^2}{3n^2 - 3n + 1} = \frac{1}{3}$$
Por tanto, de acuerdo con el criterio de Stolz-Cesàro:
$$\lim_{n\to\infty} \frac{1^2 + 2^2 + \dots + n^2}{n^3} = \frac{1}{3}$$

---

## 6. Ejercicios Propuestos

1.  Calcular el límite de la sucesión:
    $$\lim_{n\to\infty} \left(\frac{2n - 3}{2n + 5}\right)^{4n+1}$$
2.  Utilizar el criterio de Stolz-Cesàro para calcular:
    $$\lim_{n\to\infty} \frac{\ln(n!)}{n \ln n}$$
    *(Pista: Puedes usar la fórmula de Stirling o el límite del cociente de logaritmos).*
3.  Demostrar la convergencia de la sucesión recursiva definida por $a_1 = \sqrt{2}$ y $a_{n+1} = \sqrt{2 + a_n}$ para $n \ge 1$, y calcular su límite.
