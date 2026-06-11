# Tema 7: Combinatoria Básica y Principios de Recuento

La combinatoria es el arte y la ciencia de contar. En ingeniería informática, determinar el número de elementos de un conjunto discreto o el número de formas en que puede ocurrir un evento es esencial para evaluar el consumo de memoria de una estructura de datos, calcular la probabilidad de fallos y estimar el tiempo de ejecución de algoritmos exhaustivos (análisis de complejidad temporal).

---

## 1. Principios Fundamentales del Recuento

Toda la combinatoria se apoya en dos reglas lógicas fundamentales:

*   **Principio de la Adición (Regla del "O")**:
    Si un evento $A$ puede ocurrir de $n$ maneras distintas y un evento $B$ de $m$ maneras, y ambos eventos **no pueden ocurrir simultáneamente** (son mutuamente excluyentes), entonces el número de formas en que puede ocurrir $A$ o $B$ es:
    $$\text{Total} = n + m$$
*   **Principio de la Multiplicación (Regla del "Y")**:
    Si un experimento consta de dos etapas sucesivas, donde la primera etapa puede tener $n$ resultados distintos y la segunda etapa $m$ resultados, el número total de formas en que pueden ocurrir ambas etapas de forma consecutiva es:
    $$\text{Total} = n \cdot m$$

---

## 2. Técnicas de Recuento Clásicas

Para elegir la fórmula adecuada al resolver un problema de recuento, debemos hacernos dos preguntas fundamentales:
1.  ¿**Importa el orden** en el que colocamos los elementos?
2.  ¿Se pueden **repetir** los elementos?

```
                                  ¿Importa el orden?
                                    /           \
                                  SÍ             NO (Combinaciones)
                                 /                 \
                     ¿Se usan todos los elementos?   ¿Hay repetición?
                         /           \                 /           \
                       SÍ             NO             SÍ             NO
                 (Permutaciones) (Variaciones)     (CR_n,k)       (C_n,k)
```

### 2.1 Variaciones (El orden importa; NO se usan todos los elementos)
*   **Sin repetición**: Formas de elegir y ordenar $k$ elementos de un conjunto de $n$:
    $$V_{n,k} = \frac{n!}{(n-k)!}$$
*   **Con repetición**:
    $$VR_{n,k} = n^k$$

### 2.2 Permutaciones (El orden importa; SÍ se usan todos los elementos)
*   **Sin repetición**: Formas de ordenar $n$ elementos distintos:
    $$P_n = n!$$
*   **Con repetición**: Cuando algunos elementos están repetidos ($a$ veces el primero, $b$ el segundo...):
    $$PR_n^{a,b,c,\dots} = \frac{n!}{a! \cdot b! \cdot c! \dots}$$

### 2.3 Combinaciones (El orden NO importa)
*   **Sin repetición**: Formas de seleccionar un subgrupo de $k$ elementos de un conjunto de $n$:
    $$C_{n,k} = \binom{n}{k} = \frac{n!}{k!(n-k)!}$$
*   **Con repetición**: Formas de seleccionar $k$ elementos de un conjunto de $n$ pudiendo repetir elementos:
    $$CR_{n,k} = \binom{n+k-1}{k} = \frac{(n+k-1)!}{k!(n-1)!}$$

---

## 3. El Principio del Palomar (Dirichlet)

El **Principio del Palomar** afirma que si distribuimos $n$ objetos (palomas) en $m$ contenedores (nidos), y $n > m$, entonces **al menos un contenedor** debe albergar más de un objeto.

*   **Formulación General**: Si $n$ objetos se colocan en $m$ cajas, entonces al menos una caja contendrá al menos $\lceil n/m \rceil$ objetos (donde $\lceil x \rceil$ representa la función techo, el menor entero mayor o igual que $x$).
*   **Utilidad**: Aunque parezca obvio, se utiliza para demostrar la existencia de límites o patrones en teoría de grafos, algoritmos de compresión y tablas hash.

---

## 4. El Toque Informático

### Entropía de Contraseñas y Explosión Combinatoria
La seguridad de las contraseñas se rige por las variaciones con repetición. Supongamos que definimos una contraseña de longitud $L$:
*   Si solo usamos dígitos (10 caracteres posibles), el espacio de búsqueda para un atacante por fuerza bruta es de $10^L$.
*   Si usamos minúsculas, mayúsculas y dígitos (62 caracteres posibles), el espacio crece de forma exponencial a $62^L$.

Por ejemplo, para una contraseña de 8 caracteres:
*   Solo dígitos: $10^8 = 100.000.000$ combinaciones (se rompe en milisegundos).
*   Alfanumérica: $62^8 \approx 2.18 \times 10^{14}$ combinaciones (requiere meses o años de cómputo en hardware estándar).

Este crecimiento exponencial se conoce como **explosión combinatoria** y es la razón por la cual los algoritmos de complejidad exponencial $O(2^n)$ o $O(n!)$ (como el del viajante de comercio por fuerza bruta) son computacionalmente inviables para valores de $n > 20$.

A continuación, implementamos en Python una utilidad que calcula factoriales y coeficientes combinatorios, demostrando la rapidez de la explosión combinatoria.

```python
import math

# Función para calcular combinaciones sin repetición C(n, k)
def combinaciones(n, k):
    if k < 0 or k > n:
        return 0
    return math.factorial(n) // (math.factorial(k) * math.factorial(n - k))

# Simulación: Crecimiento de factoriales (Explosión Combinatoria)
print("Ejemplo de Explosión Combinatoria (n!):")
for i in [5, 10, 15, 20]:
    print(f"  {i:2d}! = {math.factorial(i):20d}")

# Cálculo de combinaciones posibles en una mano de poker (5 cartas de un mazo de 52)
n_cartas = 52
k_mano = 5
poker_comb = combinaciones(n_cartas, k_mano)
print(f"\nCombinaciones distintas para una mano de póker (C({n_cartas}, {k_mano})): {poker_comb}")
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
En una asignatura de Ingeniería Informática hay 8 estudiantes de primer curso y 6 de segundo curso. Se desea formar un grupo de trabajo compuesto por 3 estudiantes de primero y 2 de segundo. ¿Cuántos grupos distintos se pueden formar?

**Solución:**
1.  **Analizar las etapas del recuento**:
    *   Etapa 1: Elegir 3 estudiantes de primero de un total de 8. El orden de elección no importa (es un subgrupo), por lo que usamos combinaciones sin repetición:
        $$C_{8,3} = \binom{8}{3} = \frac{8!}{3! \cdot 5!} = \frac{8 \cdot 7 \cdot 6}{3 \cdot 2 \cdot 1} = 56$$
    *   Etapa 2: Elegir 2 estudiantes de segundo de un total de 6. Igualmente, usamos combinaciones sin repetición:
        $$C_{6,2} = \binom{6}{2} = \frac{6!}{2! \cdot 4!} = \frac{6 \cdot 5}{2 \cdot 1} = 15$$
2.  **Combinar las etapas (Principio de la Multiplicación)**:
    Como debemos formar el grupo eligiendo estudiantes de primero **y** estudiantes de segundo:
    $$\text{Total} = C_{8,3} \cdot C_{6,2} = 56 \cdot 15 = 840$$

Se pueden formar exactamente 840 grupos de trabajo distintos.

### Ejercicio 2
Demostrar mediante el Principio del Palomar que en cualquier conjunto de 8 enteros elegidos al azar, al menos dos de ellos deben devolver el mismo resto al dividirse entre 7.

**Solución:**
1.  **Identificar los elementos (palomas)**:
    Los objetos a distribuir son los 8 números enteros elegidos al azar ($n = 8$).
2.  **Identificar los contenedores (nidos)**:
    Las cajas representan los posibles restos que se obtienen al realizar una división entera entre 7. Según el teorema de la división, el resto $r$ satisface $0 \le r < 7$. Por lo tanto, hay exactamente 7 restos posibles: $\{0, 1, 2, 3, 4, 5, 6\}$, por lo que tenemos $m = 7$ cajas.
3.  **Aplicar el Principio del Palomar**:
    Dado que tenemos $n = 8$ enteros (palomas) y $m = 7$ posibles restos (nidos), y $8 > 7$, el Principio del Palomar nos garantiza que al menos dos enteros deben ser colocados en el mismo contenedor, es decir, **deben devolver el mismo resto módulo 7**.

---

## 6. Ejercicios Propuestos

1.  ¿Cuántas palabras de 5 letras (tengan o no sentido en castellano) se pueden formar utilizando las letras de la palabra "NODO"? Justifica qué tipo de estructura combinatoria estás usando.
2.  Un servidor de correo electrónico recibe 100 correos en un intervalo de un minuto. Demuestra que al menos dos correos tuvieron que haber llegado exactamente en el mismo segundo de ese minuto.
3.  Calcula el número de formas en que un programador puede distribuir 8 tareas idénticas entre 3 servidores distintos si algunos servidores pueden quedarse vacíos (pista: combinaciones con repetición).
