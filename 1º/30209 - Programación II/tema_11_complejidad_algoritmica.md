# Tema 11: Complejidad y Coste Algorítmico

Al diseñar software, no solo debemos asegurar que un algoritmo sea correcto; también debemos garantizar que sea **eficiente**. Si un programa tarda 5 segundos en procesar 100 registros pero tarda 5 días en procesar 10.000, no es viable. La teoría de la complejidad algorítmica proporciona las herramientas matemáticas para analizar de forma abstracta la cantidad de recursos (tiempo de procesamiento y espacio de memoria) que consume un algoritmo en función del tamaño de la entrada.

---

## 1. El Concepto de Operación Elemental (O.E.)

Medir el rendimiento de un programa en unidades de tiempo reales (segundos o milisegundos) es problemático porque depende de factores ajenos al algoritmo: la potencia de la CPU, el compilador, la carga del sistema operativo o el lenguaje de programación.

Para independizarnos del hardware, medimos el coste temporal calculando la cantidad de **Operaciones Elementales (O.E.)** que ejecuta el algoritmo. Una O.E. es una instrucción sencilla cuyo tiempo de ejecución está acotado superiormente por una constante (por ejemplo, asignaciones, operaciones aritméticas básicas, acceso a una posición de un array o comparaciones lógicas).

Denotamos por $T(N)$ la función que representa la cantidad de O.E. ejecutadas por el algoritmo para una entrada de tamaño $N$.

---

## 2. Notaciones Asintóticas

No nos interesa el valor exacto de la ecuación de coste $T(N)$ (por ejemplo, $T(N) = 3N^2 + 5N + 8$), sino su comportamiento de crecimiento a gran escala, es decir, cuando el tamaño de la entrada tiende a infinito ($N \to \infty$). Para simplificar y clasificar este comportamiento, definimos las notaciones asintóticas:

### A. Cota Superior: Notación $O$ (O Grande)
Establece una cota superior o límite máximo para el crecimiento de la función. Decimos que $f(N) \in O(g(N))$ si la función $f(N)$ crece, como máximo, tan rápido como $g(N)$ multiplicada por una constante:
$$f(N) \in O(g(N)) \iff \exists c > 0, N_0 > 0 \text{ tal que } f(N) \le c \cdot g(N), \quad \forall N \ge N_0$$

### B. Cota Inferior: Notación $\Omega$ (Omega)
Establece una cota inferior o límite mínimo de coste. Indica que el algoritmo tardará al menos esa cantidad:
$$f(N) \in \Omega(g(N)) \iff \exists c > 0, N_0 > 0 \text{ tal que } f(N) \ge c \cdot g(N), \quad \forall N \ge N_0$$

### C. Cota Ajustada: Notación $\Theta$ (Theta)
Establece que el coste crece exactamente con el mismo orden de magnitud que la función de referencia:
$$f(N) \in \Theta(g(N)) \iff f(N) \in O(g(N)) \quad \land \quad f(N) \in \Omega(g(N))$$

```
               Representación de Cotas Asintóticas
               
         Coste |             / c*g(n)  [Cota Superior O]
               |            /
               |       *---*  f(n)     [Función de Coste]
               |      /     \
               |     /       *  c'*g(n) [Cota Inferior \Omega]
               +--------------------
                                   N
```

### Escala Común de Complejidad (de mejor a peor):
$$O(1) \subset O(\log N) \subset O(N) \subset O(N \log N) \subset O(N^2) \subset O(N^3) \subset O(2^N) \subset O(N!)$$

---

## 3. Análisis en el Peor, Mejor y Caso Medio

El número de operaciones de un algoritmo puede variar no solo por el tamaño de la entrada $N$, sino por la disposición concreta de los datos:

*   **Peor Caso ($T_{\text{peor}}(N)$)**: Es la cantidad máxima de operaciones que ejecutará el algoritmo sobre cualquier conjunto de entrada de tamaño $N$. Proporciona una **garantía de seguridad** (el tiempo de ejecución real nunca superará esta cota).
*   **Mejor Caso ($T_{\text{mejor}}(N)$)**: Es la cantidad mínima de operaciones ejecutadas bajo la entrada más favorable.
*   **Caso Medio ($T_{\text{medio}}(N)$)**: Es la esperanza matemática del coste del algoritmo, ponderando la probabilidad de ocurrencia de cada posible entrada de tamaño $N$. Representa el comportamiento habitual en la práctica.

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Analizar la complejidad temporal (mejor, peor y caso medio) del algoritmo de **Búsqueda Lineal** en un array no ordenado de tamaño $N$:
```java
public static int buscar(int[] array, int valor) {
    int n = array.length;
    for (int i = 0; i < n; i++) {
        if (array[i] == valor) {
            return i; // Elemento encontrado
        }
    }
    return -1; // Elemento no encontrado
}
```

**Solución**:

1.  **Mejor Caso**:
    *   *Escenario*: El elemento buscado se encuentra en la primera posición del array (`array[0] == valor`).
    *   *Análisis*: El bucle realiza exactamente una única iteración y sale de la función mediante el `return`.
    *   *Complejidad*: $T_{\text{mejor}}(N) = \text{cte} \implies \Theta(1)$ (Coste constante).

2.  **Peor Caso**:
    *   *Escenario*: El elemento buscado no se encuentra en el array o se encuentra en la última posición (`array[N-1]`).
    *   *Análisis*: El bucle `for` se ejecuta completo realizando exactamente $N$ iteraciones de coste constante.
    *   *Complejidad*: $T_{\text{peor}}(N) = c \cdot N + d \implies \Theta(N)$ (Coste lineal).

3.  **Caso Medio**:
    *   *Asunciones*: Asumimos que el elemento está en el array y que tiene la misma probabilidad ($1/N$) de estar en cualquiera de las posiciones de la $0$ a la $N-1$.
    *   *Análisis*: Si el elemento está en la posición $i$, se realizan $i+1$ iteraciones. La media de iteraciones es:
        $$\text{Media de iteraciones} = \sum_{i=0}^{N-1} (i+1) \cdot P(\text{estar en pos } i) = \sum_{i=0}^{N-1} (i+1) \cdot \frac{1}{N} = \frac{1}{N} \sum_{j=1}^{N} j$$
        Recordamos la suma de los primeros $N$ enteros:
        $$\text{Media de iteraciones} = \frac{1}{N} \cdot \frac{N(N+1)}{2} = \frac{N+1}{2} \approx \frac{N}{2}$$
    *   *Complejidad*: $T_{\text{medio}}(N) \approx c \cdot \frac{N}{2} \implies \Theta(N)$ (Coste lineal).
    *(Nota: El coste medio y el peor caso comparten el mismo orden de complejidad asintótica $\Theta(N)$, lo que indica que a gran escala el algoritmo crece de forma lineal en ambos escenarios).*
