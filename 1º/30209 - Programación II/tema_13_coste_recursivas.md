# Tema 13: Análisis de Coste en Algoritmos Recursivos

El análisis de la complejidad temporal de las funciones recursivas es más complejo que el de las iterativas, ya que no disponemos de bucles explícitos que podamos contar o transformar en sumatorios directos. En su lugar, el coste de un algoritmo recursivo $T(N)$ se define mediante una **ecuación de recurrencia** (una ecuación que se define en términos de sí misma). Estudiaremos cómo plantear estas ecuaciones, cómo resolverlas por expansión y cómo aplicar el potente **Teorema Máster**.

---

## 1. Formulación de Ecuaciones de Recurrencia

Una ecuación de recurrencia para un coste $T(N)$ consta siempre de dos partes:

1.  **Caso Base**: El coste de ejecutar el caso de parada (que suele ser un coste constante $c_0$).
2.  **Caso Inductivo**: El coste del cuerpo del método, que incluye el coste del código no recursivo ($f(N)$, para dividir y combinar el problema) más el coste de las llamadas recursivas sobre subproblemas más pequeños.

### Ejemplo: Búsqueda Binaria
El algoritmo divide el array por la mitad en cada paso, realiza una comparación constante y hace una única llamada recursiva sobre la mitad seleccionada:
*   Caso Base ($N = 1$): $T(1) = c_0$ (coste constante de comparar).
*   Caso Inductivo ($N > 1$): $T(N) = T\left(\frac{N}{2}\right) + c_1$ (llamada sobre la mitad del tamaño más coste de comparación constante).

---

## 2. Resolución por el Método de Expansión (o Sustitución)

Consiste en sustituir sucesivamente la definición de la función en su propio caso recursivo para detectar un patrón matemático en función del paso de expansión $k$:

### Ejemplo: Factorial Recursivo
Ecuación de recurrencia:
*   $T(0) = c_0$
*   $T(N) = T(N-1) + c_1 \quad (\text{para } N > 0)$

### Expansión paso a paso:
*   Paso 1: $T(N) = T(N-1) + c_1$
*   Paso 2: Como $T(N-1) = T(N-2) + c_1$, sustituimos:
    $$T(N) = [T(N-2) + c_1] + c_1 = T(N-2) + 2c_1$$
*   Paso 3: $T(N) = T(N-3) + 3c_1$
*   Paso $k$: Detectamos el patrón general:
    $$T(N) = T(N-k) + k \cdot c_1$$

El proceso se detiene cuando alcanzamos el caso base, es decir, cuando la entrada del término recursivo es 0:
$$N - k = 0 \implies k = N$$

Sustituyendo $k = N$ en la ecuación del paso $k$:
$$T(N) = T(0) + N \cdot c_1 = c_0 + c_1 \cdot N \implies \Theta(N) \quad (\text{Coste lineal})$$

---

## 3. El Teorema Máster

El Teorema Máster proporciona una "receta" matemática directa para resolver ecuaciones de recurrencia basadas en la estrategia de **Divide y Vencerás**, donde el problema se divide en $a$ subproblemas de tamaño $N/b$, y el coste de dividir y combinar los resultados es de orden polinomial $\Theta(N^k)$.

La ecuación general debe tener la forma:
$$T(N) = a \cdot T\left(\frac{N}{b}\right) + \Theta(N^k)$$

Donde $a \ge 1$, $b > 1$, y $k \ge 0$. Comparamos el valor de $a$ con el término $b^k$:

### Caso 1: $a > b^k$ (Domina la recursión)
El coste principal reside en la gran cantidad de llamadas recursivas secundarias.
$$T(N) \in \Theta\left( N^{\log_b a} \right)$$

### Caso 2: $a = b^k$ (Coste equilibrado)
El coste de las llamadas recursivas y el coste de combinación están en equilibrio.
$$T(N) \in \Theta\left( N^k \cdot \log N \right)$$

### Caso 3: $a < b^k$ (Domina la combinación)
El coste principal reside en el trabajo de dividir y combinar los subproblemas ($N^k$).
$$T(N) \in \Theta\left( N^k \right)$$

---

## 4. Ejemplos de Aplicación del Teorema Máster

### A. Búsqueda Binaria
*   Ecuación: $T(N) = 1 \cdot T(N/2) + \Theta(1)$
*   Parámetros: $a = 1$, $b = 2$, $k = 0$ (pues $\Theta(1) = \Theta(N^0)$).
*   Comparación: $b^k = 2^0 = 1$.
*   Caso del Teorema: Como $a = b^k$ ($1 = 1$), aplica el **Caso 2**:
    $$T(N) \in \Theta(N^0 \cdot \log N) \implies \Theta(\log N)$$

### B. Ordenación por Mezcla (MergeSort)
*   Ecuación: $T(N) = 2 \cdot T(N/2) + \Theta(N)$ (dos llamadas a la mitad del tamaño más coste lineal de mezclar).
*   Parámetros: $a = 2$, $b = 2$, $k = 1$ (pues $\Theta(N) = \Theta(N^1)$).
*   Comparación: $b^k = 2^1 = 2$.
*   Caso del Teorema: Como $a = b^k$ ($2 = 2$), aplica el **Caso 2**:
    $$T(N) \in \Theta(N^1 \cdot \log N) \implies \Theta(N \log N)$$

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Calcular la complejidad asintótica de un algoritmo cuya ecuación de recurrencia de coste es:
*   $T(1) = c$
*   $T(N) = 3 \cdot T\left(\frac{N}{2}\right) + n^2$

**Solución**:
1.  **Identificar parámetros para el Teorema Máster**:
    La ecuación tiene la forma $T(N) = a \cdot T(N/b) + \Theta(N^k)$.
    *   $a = 3$ (se generan 3 subproblemas).
    *   $b = 2$ (el tamaño de cada subproblema se divide por 2).
    *   $k = 2$ (el trabajo de combinación es cuadrático, $N^2$).

2.  **Comparar $a$ con $b^k$**:
    *   $a = 3$
    *   $b^k = 2^2 = 4$
    Comparando ambos valores:
    $$a < b^k \quad (3 < 4)$$

3.  **Aplicar el caso del Teorema**:
    Al cumplirse que $a < b^k$, estamos en el **Caso 3** (el coste está dominado por el trabajo de combinación no recursivo de la función principal):
    $$T(N) \in \Theta(N^k) \implies \Theta(N^2)$$

*Conclusión: La complejidad temporal asintótica del algoritmo en el peor caso es cuadrática $\Theta(N^2)$.*
