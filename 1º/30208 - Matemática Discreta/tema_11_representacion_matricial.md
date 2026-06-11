# Tema 11: Representación Matricial de Grafos

Para procesar grafos en computadores, es necesario traducir las estructuras visuales de vértices y aristas en estructuras de datos de memoria eficientes. Las dos formas principales de codificación matemática y digital son las **Matrices de Adyacencia** y las **Listas de Adyacencia**. La matriz de adyacencia, en particular, conecta de forma brillante la teoría de grafos con el álgebra lineal, permitiendo resolver problemas combinatorios de caminos mediante simples multiplicaciones de matrices.

---

## 1. Métodos de Representación en Computadores

### 1.1 Listas de Adyacencia
Asocian a cada vértice un vector dinámico o lista enlazada conteniendo sus vecinos directos.
*   **Espacio en memoria**: $O(|V| + |E|)$.
*   **Uso ideal**: Para **grafos dispersos (sparse graphs)**, donde el número de aristas es mucho menor que el máximo posible ($|E| \ll |V|^2$). La inmensa mayoría de las redes del mundo real (como los enlaces web o redes eléctricas) son dispersas.

### 1.2 Matriz de Adyacencia
Es una matriz cuadrada $A$ de dimensiones $|V| \times |V|$ donde las filas y columnas corresponden a los vértices del grafo.

$$A_{ij} = \begin{cases} 1 & \text{si existe una arista entre } v_i \text{ y } v_j \\ 0 & \text{en caso contrario} \end{cases}$$

*   **Espacio en memoria**: $O(|V|^2)$, independientemente del número de aristas.
*   **Uso ideal**: Para **grafos densos (dense graphs)** ($|E| \approx |V|^2$), o cuando se requiere verificar en tiempo constante $O(1)$ si existe un enlace entre dos nodos.

```
      Grafo                 Matriz de Adyacencia
     (1)---(2)                  1  2  3  4
      |   /                 1  [0  1  1  0]
      |  /                  2  [1  0  1  0]
     (3)   (4)              3  [1  1  0  0]
                            4  [0  0  0  0]
```

*   **Propiedades (Grafos No Dirigidos)**:
    *   La matriz $A$ es **simétrica** respecto a la diagonal principal ($A = A^T$).
    *   Si el grafo no contiene bucles (lazos de un nodo a sí mismo), la diagonal principal se compone exclusivamente de ceros.
    *   La suma de los elementos de la fila $i$ es igual al grado del vértice $v_i$.

---

## 2. El Teorema de Caminos de Longitud $k$

Una de las aplicaciones más sorprendentes de la representación matricial es el cálculo del número de caminos entre nodos utilizando potencias de matrices.

### Teorema
Sea $A$ la matriz de adyacencia de un grafo $G$. El elemento situado en la fila $i$ y columna $j$ de la matriz potencia $A^k$ (donde $k \in \mathbb{Z}^+$):
$$(A^k)_{ij}$$
es igual al **número de caminos distintos de longitud exactamente $k$** que existen entre el vértice $v_i$ y el vértice $v_j$.

---

## 3. Matriz de Incidencia

Es una matriz $M$ de dimensiones $|V| \times |E|$ (filas vértices, columnas aristas).
*   En un grafo no dirigido:
    $$M_{ij} = \begin{cases} 1 & \text{si el vértice } v_i \text{ es incidente en la arista } e_j \\ 0 & \text{en caso contrario} \end{cases}$$
*   Cada columna tiene exactamente dos valores $1$, correspondientes a los dos vértices extremos que conecta la arista.

---

## 4. El Toque Informático

### Rendimiento de Algoritmos según la Representación (Trade-offs)
En programación, elegir la representación correcta impacta críticamente el rendimiento de las operaciones básicas:

| Operación | Matriz de Adyacencia | Lista de Adyacencia |
| :--- | :---: | :---: |
| **Espacio en Memoria** | $O(|V|^2)$ | $O(|V| + |E|)$ |
| **¿Existe arista $(u, v)$?** | $O(1)$ | $O(deg(u))$ |
| **Listar vecinos de $u$** | $O(|V|)$ | $O(deg(u))$ |
| **Insertar/Eliminar arista** | $O(1)$ | $O(deg(u))$ |

Por ejemplo, si un grafo modela una red de 100.000 usuarios (nodos), una matriz de adyacencia exigiría almacenar $100.000^2 = 10^{10}$ enteros ($\approx 40 \, \text{GB}$ de RAM). Si cada usuario tiene de media solo 50 amigos, una lista de adyacencia requerirá únicamente almacenar los punteros de $50 \times 100.000 = 5 \times 10^6$ conexiones ($\approx 20 \, \text{MB}$ de memoria), permitiendo que quepa en la caché del procesador.

A continuación, implementamos en Python una simulación utilizando la librería `numpy` para comprobar el Teorema de Caminos de Longitud $k$.

```python
import numpy as np

# Definimos la matriz de adyacencia de un grafo de 4 vértices
# 1-2, 1-3, 2-3, 3-4 (Vértices indexados de 0 a 3)
A = np.array([
    [0, 1, 1, 0],
    [1, 0, 1, 0],
    [1, 1, 0, 1],
    [0, 0, 1, 0]
])

print("Matriz de Adyacencia A (Caminos de longitud 1):")
print(A)

# Calculamos A^2 (Caminos de longitud 2)
A_2 = np.linalg.matrix_power(A, 2)
print("\nMatriz A^2 (Caminos de longitud 2):")
print(A_2)

# Calculamos A^3 (Caminos de longitud 3)
A_3 = np.linalg.matrix_power(A, 3)
print("\nMatriz A^3 (Caminos de longitud 3):")
print(A_3)

# Verificación de caminos de longitud 3 entre nodo 0 y nodo 3:
n_caminos = A_3[0, 3]
print(f"\nNúmero de caminos de longitud 3 entre nodo 0 y nodo 3: {n_caminos}")
print("Caminos físicos equivalentes a comprobar: (0->1->2->3) y (0->2->1->2) [inválido, contiene lazo], etc.")
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Dado el grafo no dirigido compuesto por 3 vértices conectados en línea ($1-2-3$):
1. Escribir su matriz de adyacencia $A$.
2. Calcular $A^2$ y determinar cuántos caminos de longitud 2 existen entre el nodo 1 y el nodo 3.

**Solución:**
1.  **Construir la matriz de adyacencia $A$**:
    Los enlaces son $\{1, 2\}$ y $\{2, 3\}$.
    $$A = \begin{pmatrix} 0 & 1 & 0 \\ 1 & 0 & 1 \\ 0 & 1 & 0 \end{pmatrix}$$
2.  **Calcular la matriz potencia $A^2$**:
    Multiplicamos la matriz por sí misma:
    $$A^2 = A \cdot A = \begin{pmatrix} 0 & 1 & 0 \\ 1 & 0 & 1 \\ 0 & 1 & 0 \end{pmatrix} \cdot \begin{pmatrix} 0 & 1 & 0 \\ 1 & 0 & 1 \\ 0 & 1 & 0 \end{pmatrix}$$
    Calculamos los componentes de la primera fila:
    *   Fila 1, Columna 1: $(0\cdot0 + 1\cdot1 + 0\cdot0) = 1$.
    *   Fila 1, Columna 2: $(0\cdot1 + 1\cdot0 + 0\cdot1) = 0$.
    *   Fila 1, Columna 3: $(0\cdot0 + 1\cdot1 + 0\cdot0) = 1$.
    
    Siguiendo el producto para el resto de las filas resulta:
    $$A^2 = \begin{pmatrix} 1 & 0 & 1 \\ 0 & 2 & 0 \\ 1 & 0 & 1 \end{pmatrix}$$
3.  **Identificar el número de caminos**:
    El elemento $(A^2)_{1,3}$ está en la fila 1 y columna 3, y su valor es **1**.
    Esto significa que existe exactamente **1 camino** de longitud 2 entre el nodo 1 y el nodo 3.
    *Verificación física*: El único camino de longitud 2 es el camino $1 \to 2 \to 3$.

---

## 6. Ejercicios Propuestos

1.  Dibuja el grafo dirigido correspondiente a la siguiente matriz de adyacencia:
    $$A = \begin{pmatrix} 0 & 1 & 0 \\ 0 & 0 & 2 \\ 1 & 0 & 0 \end{pmatrix}$$
    (Nota: Los valores mayores a 1 indican multígrafos o aristas paralelas).
2.  Deduce algebraicamente por qué en cualquier matriz de adyacencia de un grafo no dirigido, el número de elementos con valor "1" en toda la matriz es igual a $2|E|$.
3.  Escribe un pseudocódigo o función en C++ para listar todos los vecinos de un nodo $u$ utilizando una representación basada en matrices de adyacencia frente a una representación de listas de adyacencia, comparando sus costes temporales asociados.
