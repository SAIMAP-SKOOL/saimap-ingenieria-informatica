# Tema 8: Relaciones de Recurrencia Lineales

Una relación de recurrencia es una ecuación que define una sucesión matemática de forma recursiva, es decir, el término actual $a_n$ se expresa en función de uno o más términos anteriores (como $a_{n-1}, a_{n-2}$, etc.). En ingeniería informática, resolver una relación de recurrencia consiste en hallar una "fórmula cerrada" que nos permita calcular directamente el término $a_n$ para cualquier $n$ sin tener que computar todos los términos anteriores. Esto es fundamental para analizar el coste computacional de algoritmos recursivos.

---

## 1. Definición y Clasificación

Una relación de recurrencia lineal de orden $k$ con coeficientes constantes tiene la forma:
$$a_n = c_1 a_{n-1} + c_2 a_{n-2} + \dots + c_k a_{n-k} + F(n)$$
donde $c_1, \dots, c_k$ son constantes reales ($c_k \neq 0$) y $F(n)$ es una función de $n$.

*   **Orden ($k$)**: El número de términos anteriores de los que depende $a_n$.
*   **Homogénea**: Si $F(n) = 0$ para todo $n$.
*   **No Homogénea**: Si $F(n) \neq 0$.
*   **Condiciones Iniciales**: Para hallar una solución única, necesitamos conocer los primeros $k$ valores de la sucesión ($a_0, a_1, \dots, a_{k-1}$).

---

## 2. Resolución de Recurrencias Lineales Homogéneas

Para resolver la relación homogénea de orden 2:
$$a_n - c_1 a_{n-1} - c_2 a_{n-2} = 0$$

Proponemos una solución de la forma $a_n = r^n$. Sustituyendo y dividiendo por $r^{n-2}$ obtenemos el **polinomio característico**:
$$r^2 - c_1 r - c_2 = 0$$

Calculamos las raíces de este polinomio. Según la naturaleza de las raíces:

### Caso 1: Raíces Reales y Distintas ($r_1 \neq r_2$)
La solución general es una combinación lineal de las potencias de las raíces:
$$a_n = \alpha_1 r_1^n + \alpha_2 r_2^n$$

### Caso 2: Raíz Real Múltiple o Doble ($r_1 = r_2 = r$)
La solución general añade un factor multiplicador lineal $n$:
$$a_n = (\alpha_1 + \alpha_2 n) r^n$$

*Los coeficientes $\alpha_1$ y $\alpha_2$ se determinan sustituyendo las condiciones iniciales de $a_0$ y $a_1$.*

---

## 3. Resolución de Recurrencias Lineales No Homogéneas

La solución general de la ecuación no homogénea $a_n = c_1 a_{n-1} + c_2 a_{n-2} + F(n)$ se compone de la suma de dos partes:
$$a_n = a_n^{(h)} + a_n^{(p)}$$
donde:
*   $a_n^{(h)}$: Solución general de la relación homogénea asociada (haciendo $F(n)=0$).
*   $a_n^{(p)}$: Solución particular que tiene una forma similar a la función forzante $F(n)$.

### Método de Coeficientes Indeterminados para la Solución Particular $a_n^{(p)}$

| Si $F(n)$ es de la forma: | Proponemos $a_n^{(p)}$ de la forma: |
| :--- | :--- |
| **Polinomio de grado $d$** ($An^d + \dots$) | $n^s (p_d n^d + \dots + p_0)$ |
| **Exponencial** ($A \cdot m^n$) | $n^s (p_0 \cdot m^n)$ |

*(Donde $s$ es la multiplicidad de la base de la exponencial o la raíz $1$ en el polinomio característico de la parte homogénea, para evitar redundancias).*

---

## 4. El Toque Informático

### Coste de Algoritmos Recursivos y la Trampa de Fibonacci
El análisis de algoritmos divide el coste temporal de un programa recursivo en una relación de recurrencia:
*   **Divide y Vencerás**: El algoritmo de ordenación Merge Sort divide el problema en dos subproblemas de tamaño $n/2$ y realiza un trabajo lineal $O(n)$ para mezclarlos. Su recurrencia es:
    $$T(n) = 2 T(n/2) + n \quad \implies \quad T(n) \in O(n \log n)$$
*   **La trampa de la recursión ineficiente**: La definición de Fibonacci ($F_n = F_{n-1} + F_{n-2}$) programada de forma recursiva directa genera un árbol de llamadas exponencial ($O(1.618^n)$). Resolver este problema requiere usar **Programación Dinámica** para almacenar los resultados intermedios y reducir la complejidad a $O(n)$ lineal.

A continuación, implementamos en Python una comparación empírica de tiempo entre el cálculo de Fibonacci recursivo ingenuo y el método iterativo lineal.

```python
import time

# Fibonacci recursivo ingenuo: O(2^n) o O(1.618^n)
def fib_recursivo(n):
    if n <= 1:
        return n
    return fib_recursivo(n-1) + fib_recursivo(n-2)

# Fibonacci iterativo (Programación Dinámica): O(n)
def fib_iterativo(n):
    if n <= 1:
        return n
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, a + b
    return b

# Medición de tiempos
n_test = 30

start = time.time()
res_rec = fib_recursivo(n_test)
time_rec = time.time() - start

start = time.time()
res_it = fib_iterativo(n_test)
time_it = time.time() - start

print(f"Fibonacci({n_test}) = {res_it}")
print(f"Tiempo Recursivo Ingenuo: {time_rec:.6f} segundos")
print(f"Tiempo Iterativo (Dinámico): {time_it:.6f} segundos")
print(f"El método iterativo es {time_rec / max(time_it, 1e-9):.1f} veces más rápido.")
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Resolver la relación de recurrencia homogénea de segundo orden:
$$a_n = 5a_{n-1} - 6a_{n-2} \quad (\text{para } n \ge 2)$$
con las condiciones iniciales $a_0 = 1$ y $a_1 = 4$.

**Solución:**
1.  **Formular la ecuación característica**:
    $$r^2 - 5r + 6 = 0$$
2.  **Calcular las raíces**:
    $$(r - 2)(r - 3) = 0 \quad \implies \quad r_1 = 2, \quad r_2 = 3$$
    Puesto que las raíces son reales y distintas, la solución general es:
    $$a_n = \alpha_1 \cdot 2^n + \alpha_2 \cdot 3^n$$
3.  **Aplicar condiciones iniciales**:
    *   Para $n = 0$: $\alpha_1 \cdot 2^0 + \alpha_2 \cdot 3^0 = 1 \implies \alpha_1 + \alpha_2 = 1$
    *   Para $n = 1$: $\alpha_1 \cdot 2^1 + \alpha_2 \cdot 3^1 = 4 \implies 2\alpha_1 + 3\alpha_2 = 4$
4.  **Resolver el sistema de ecuaciones**:
    *   De la primera ecuación: $\alpha_1 = 1 - \alpha_2$.
    *   Sustituyendo en la segunda: $2(1 - \alpha_2) + 3\alpha_2 = 4 \implies 2 - 2\alpha_2 + 3\alpha_2 = 4 \implies \alpha_2 = 2$.
    *   Por lo tanto: $\alpha_1 = 1 - 2 = -1$.
5.  **Fórmula cerrada final**:
    $$a_n = -2^n + 2 \cdot 3^n \quad (\text{para } n \ge 0)$$

### Ejercicio 2
Resolver la relación de recurrencia no homogénea:
$$a_n = 2a_{n-1} + 3^n$$
con condición inicial $a_0 = 1$.

**Solución:**
1.  **Resolver la parte homogénea** ($a_n = 2a_{n-1}$):
    *   Ecuación característica: $r - 2 = 0 \implies r = 2$.
    *   Solución homogénea: $a_n^{(h)} = \alpha \cdot 2^n$.
2.  **Hallar la solución particular $a_n^{(p)}$**:
    *   La parte no homogénea es $F(n) = 3^n$. La base es $3$, que no es raíz del polinomio característico (el cual es $2$).
    *   Proponemos: $a_n^{(p)} = P_0 \cdot 3^n$.
    *   Sustituyendo en la relación no homogénea original:
        $$P_0 \cdot 3^n = 2(P_0 \cdot 3^{n-1}) + 3^n$$
    *   Dividimos por $3^{n-1}$:
        $$3 P_0 = 2 P_0 + 3 \quad \implies \quad P_0 = 3$$
    *   Por tanto, la solución particular es: $a_n^{(p)} = 3 \cdot 3^n = 3^{n+1}$.
3.  **Combinar soluciones**:
    $$a_n = a_n^{(h)} + a_n^{(p)} = \alpha \cdot 2^n + 3^{n+1}$$
4.  **Aplicar condición inicial $a_0 = 1$**:
    $$a_0 = \alpha \cdot 2^0 + 3^{0+1} = 1 \implies \alpha + 3 = 1 \implies \alpha = -2$$
5.  **Fórmula cerrada final**:
    $$a_n = -2 \cdot 2^n + 3^{n+1} = -2^{n+1} + 3^{n+1}$$

---

## 6. Ejercicios Propuestos

1.  Resuelve la relación de recurrencia $a_n = 4a_{n-1} - 4a_{n-2}$ con condiciones iniciales $a_0 = 2$ y $a_1 = 8$. Identifica qué caso de raíces del polinomio característico se presenta.
2.  Determina la fórmula de recurrencia no homogénea $a_n = 3a_{n-1} + 2n$ con $a_0 = 1$.
3.  Investiga el **Teorema Maestro (Master Theorem)** utilizado en el análisis de algoritmos y explica cómo resuelve de forma directa relaciones de recurrencia del tipo $T(n) = aT(n/b) + f(n)$ para algoritmos de división y conquista.
