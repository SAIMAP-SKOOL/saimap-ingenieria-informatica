# Tema 12: Análisis de Coste en Estructuras Iterativas

El cálculo del coste temporal en algoritmos secuenciales e iterativos (bucles) se realiza aplicando una serie de reglas algebraicas estructuradas. Veremos cómo analizar bucles simples, bucles anidados independientes y, especialmente, la resolución matemática mediante **sumatorios** para bucles anidados dependientes.

---

## 1. Reglas Operativas Fundamentales

Para evaluar el tiempo de ejecución en código iterativo, aplicamos las siguientes directrices:

### A. Regla de la Secuencia
Si un bloque de código consta de dos partes consecutivas $S_1$ y $S_2$, con costes $T_1(N)$ y $T_2(N)$ respectivamente, el coste total es su suma. Asintóticamente, domina la parte más compleja:
$$O(T_1(N) + T_2(N)) = \max(O(T_1(N)), \, O(T_2(N)))$$

### B. Regla del Condicional (if-else)
El coste en el peor caso de una estructura condicional es el coste de evaluar la condición más el coste máximo entre sus dos ramas:
$$T_{\text{condicional}} = T(\text{condición}) + \max(T(\text{rama } try), \, T(\text{rama } else))$$

---

## 2. Análisis de Bucles Simples

El coste de un bucle es la suma de los costes de cada una de sus iteraciones. Si el coste del cuerpo es constante ($O(1)$), el análisis se reduce a contar el número de iteraciones:

### Incremento Lineal (Coste Lineal)
```java
for (int i = 0; i < N; i++) { /* O.E. constante */ }
```
El índice crece de 1 en 1. El bucle se ejecuta $N$ veces. Coste: $\Theta(N)$.

### Incremento Multiplicativo (Coste Logarítmico)
```java
for (int i = 1; i < N; i = i * 2) { /* O.E. constante */ }
```
El índice se duplica en cada iteración ($1, 2, 4, 8, \dots, 2^k$). El bucle se detiene cuando $2^k \ge N \implies k \ge \log_2 N$. Coste: $\Theta(\log N)$.

---

## 3. Bucles Anidados Independientes

Ocurre cuando el bucle interno tiene límites de iteración constantes o que solo dependen del tamaño del problema $N$, sin verse afectados por el índice del bucle externo.

```java
for (int i = 0; i < N; i++) {
    for (int j = 0; j < N; j++) {
        // O.E. de coste constante c
    }
}
```
*   El bucle externo se ejecuta $N$ veces.
*   En cada iteración del bucle externo, el bucle interno se ejecuta siempre de forma completa e idéntica $N$ veces.
*   El coste total es el producto de iteraciones:
    $$T(N) = N \times N \times c = c \cdot N^2 \implies \Theta(N^2) \quad (\text{Complejidad cuadrática})$$

---

## 4. Bucles Anidados Dependientes (Uso de Sumatorios)

Ocurre cuando los límites de iteración del bucle interno dependen directamente del valor activo del índice del bucle externo. En estos casos, no podemos multiplicar directamente; debemos plantear un **sumatorio** que represente el comportamiento variable del código y resolverlo algebraicamente.

### Ejemplo Clásico:
```java
for (int i = 0; i < N; i++) {
    for (int j = 0; j < i; j++) {
        // O.E. de coste constante c
    }
}
```

*   Cuando $i = 0$: el bucle interno se ejecuta 0 veces.
*   Cuando $i = 1$: el bucle interno se ejecuta 1 vez.
*   Cuando $i = 2$: el bucle interno se ejecuta 2 veces.
*   ...
*   Cuando $i = N-1$: el bucle interno se ejecuta $N-1$ veces.

### Planteamiento Matemático:
$$T(N) = \sum_{i=0}^{N-1} \left( \sum_{j=0}^{i-1} c \right)$$

El sumatorio interno $\sum_{j=0}^{i-1} c$ es simplemente sumar la constante $c$ un total de $i$ veces, lo que resulta en $c \cdot i$:
$$T(N) = \sum_{i=0}^{N-1} (c \cdot i) = c \cdot \sum_{i=0}^{N-1} i$$

Aplicamos la fórmula matemática de la suma aritmética ($\sum_{i=0}^{M} i = \frac{M(M+1)}{2}$):
$$T(N) = c \cdot \frac{(N-1)N}{2} = \frac{c}{2} \cdot (N^2 - N) \implies \Theta(N^2)$$

*(A pesar de que el bucle interno no se ejecuta completo todas las veces, el orden de complejidad sigue siendo cuadrático $\Theta(N^2)$).*

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Calcular la complejidad temporal en el peor caso para el siguiente fragmento de código:
```java
int k = 0;
for (int i = 0; i < N; i++) {
    for (int j = N; j > i; j = j / 2) {
        k++;
    }
}
```

**Solución**:
1.  **Analizar el bucle interno**:
    El índice $j$ empieza en $N$ y decrece dividiéndose por 2 (`j = j / 2`) en cada paso hasta ser menor o igual a $i$.
    El número de iteraciones en función de $i$ viene dado por la cantidad de veces que podemos dividir el rango entre $N$ e $i$ por 2. Esto equivale a:
    $$\text{Iteraciones internas} \approx \log_2\left( \frac{N}{i+1} \right) = \log_2 N - \log_2(i+1)$$

2.  **Plantear el sumatorio del coste total**:
    $$T(N) = \sum_{i=0}^{N-1} \left( \log_2\left( \frac{N}{i+1} \right) \right) = \sum_{i=0}^{N-1} (\log_2 N - \log_2(i+1))$$
    Separando los sumatorios:
    $$T(N) = \left( \sum_{i=0}^{N-1} \log_2 N \right) - \sum_{i=0}^{N-1} \log_2(i+1)$$
    *   La primera parte es sumar la constante $\log_2 N$ un total de $N$ veces: $N \log_2 N$.
    *   La segunda parte es $\sum_{i=0}^{N-1} \log_2(i+1) = \log_2(1) + \log_2(2) + \dots + \log_2(N) = \log_2(N!)$.
    Utilizando la **aproximación de Stirling** para el logaritmo del factorial: $\log_2(N!) \approx N \log_2 N - N \log_2 e$.
    Sustituyendo:
    $$T(N) \approx N \log_2 N - (N \log_2 N - N \log_2 e) = N \log_2 e \implies \Theta(N)$$

*Conclusión: Aunque contenga un bucle interno dependiente con divisiones, el orden de complejidad asintótico de este algoritmo es lineal $\Theta(N)$.*
