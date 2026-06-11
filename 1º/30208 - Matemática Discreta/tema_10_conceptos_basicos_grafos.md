# Tema 10: Conceptos Básicos de Grafos y Árboles

La teoría de grafos es la rama de las matemáticas discretas encargada de modelar y analizar relaciones binarias entre objetos de un conjunto. En informática, casi cualquier estructura relacional (desde la red de enlaces de Internet hasta el mapa de dependencias de un compilador o una red social) se abstrae y modela físicamente en memoria como un **grafo**. 

---

## 1. Definición Formal y Clasificación

Un **grafo** $G$ es un par ordenado:
$$G = (V, E)$$
donde:
*   $V$: Conjunto no vacío de **vértices** (o nodos).
*   $E$: Conjunto de **aristas** (o arcos), que representan enlaces entre pares de vértices.

### Tipos de Grafos
*   **Grafo No Dirigido**: Las aristas son conjuntos desordenados de dos vértices $\{u, v\}$. La relación es bidireccional (si $u$ está conectado con $v$, $v$ está conectado con $u$).
*   **Grafo Dirigido (Dígrafo)**: Las aristas son pares ordenados $(u, v)$, representados mediante flechas que apuntan de un origen a un destino.
*   **Grafo Valorado (Ponderado)**: Cada arista tiene un peso o coste numérico asociado (por ejemplo, distancia en kilómetros o latencia de red en milisegundos).

```
   (A)-------(B)          (A)------> (B)
    |         |            ^          |
    |         |            |          v
   (C)-------(D)          (C) <------(D)
   No Dirigido              Dirigido
```

---

## 2. Grado de un Vértice y el Lema del Apretón de Manos

El **grado** de un vértice $v$, denotado como $deg(v)$, es el número de aristas incidentes en él.
*   En grafos dirigidos, se divide en **grado de entrada** ($deg^-(v)$) y **grado de salida** ($deg^+(v)$).

### Teorema del Apretón de Manos (Handshaking Lemma)
En cualquier grafo no dirigido, la suma de los grados de todos sus vértices es exactamente igual al doble del número de aristas:
$$\sum_{v \in V} deg(v) = 2 |E|$$

> **Consecuencia**: Todo grafo tiene un número par de vértices con grado impar.

---

## 3. Caminos, Conectividad y Árboles

*   **Camino**: Secuencia de vértices unidos consecutivamente por aristas.
*   **Ciclo**: Camino cerrado que empieza y termina en el mismo vértice sin repetir aristas ni vértices intermedios.
*   **Grafo Conexo**: Un grafo no dirigido es conexo si existe un camino entre cualquier par de vértices.

### Árboles
Un **árbol** es un grafo no dirigido conexo que **no contiene ciclos**.
*   **Propiedad fundamental**: Si un árbol tiene $|V|$ vértices, entonces tiene exactamente $|E| = |V| - 1$ aristas. La eliminación de cualquier arista desconecta el árbol; la adición de cualquier arista nueva crea un ciclo.

---

## 4. Recorridos sobre Grafos: BFS y DFS

Para procesar o buscar información en un grafo de forma sistemática, se usan dos estrategias fundamentales de recorrido:
1.  **Búsqueda en Anchura (BFS - Breadth-First Search)**:
    *   Visita los nodos nivel a nivel (primero los vecinos directos, luego los vecinos de los vecinos).
    *   Utiliza una **cola (FIFO)** como estructura de datos auxiliar.
    *   Es óptimo para hallar el camino con menor número de aristas en grafos no valorados.
2.  **Búsqueda en Profundidad (DFS - Depth-First Search)**:
    *   Sigue un camino explorando lo más profundo posible antes de realizar backtracking (retroceder).
    *   Utiliza una **pila (LIFO)** o recursión nativa del procesador.

---

## 5. El Toque Informático

### Redes de Datos, el Algoritmo PageRank y Bases de Datos NoSQL de Grafos
1.  **PageRank de Google**:
    El motor de búsqueda original de Google modela toda la Web como un dígrafo gigante donde los nodos son páginas web y las aristas son hipervínculos. El algoritmo calcula la relevancia de una página midiendo la probabilidad de que un usuario aleatorio acabe en ella, aplicando cadenas de Markov sobre el dígrafo de la Web.
2.  **Bases de Datos de Grafos (Neo4j)**:
    Las bases de datos relacionales tradicionales (SQL) sufren caídas de rendimiento drásticas al realizar consultas cruzadas complejas (JOINS) en redes interconectadas (como redes sociales o sistemas de recomendación). Las bases de datos de grafos almacenan directamente los nodos y punteros físicos a sus aristas en disco, permitiendo realizar búsquedas de relaciones en tiempo constante.

A continuación, implementamos en Python una estructura de grafo no dirigido y los algoritmos de recorrido BFS y DFS en consola.

```python
from collections import deque

class Grafo:
    def __init__(self):
        # Representación mediante lista de adyacencia (diccionario)
        self.adj = {}
        
    def agregar_vertice(self, v):
        if v not in self.adj:
            self.adj[v] = []
            
    def agregar_arista(self, u, v):
        self.agregar_vertice(u)
        self.agregar_vertice(v)
        self.adj[u].append(v)
        self.adj[v].append(u) # Bidireccional

    # Recorrido BFS (Búsqueda en Anchura)
    def bfs(self, inicio):
        visitados = set([inicio])
        cola = deque([inicio])
        resultado = []
        
        while cola:
            v = cola.popleft()
            resultado.append(v)
            for vecino in self.adj[v]:
                if vecino not in visitados:
                    visitados.add(vecino)
                    cola.append(vecino)
        return resultado

    # Recorrido DFS (Búsqueda en Profundidad)
    def dfs(self, inicio, visitados=None, resultado=None):
        if visitados is None:
            visitados = set()
        if resultado is None:
            resultado = []
            
        visitados.add(inicio)
        resultado.append(inicio)
        for vecino in self.adj[inicio]:
            if vecino not in visitados:
                self.dfs(vecino, visitados, resultado)
        return resultado

# Construimos un grafo de prueba
g = Grafo()
g.agregar_arista('A', 'B')
g.agregar_arista('A', 'C')
g.agregar_arista('B', 'D')
g.agregar_arista('C', 'E')

print("Recorrido BFS empezando en 'A':", g.bfs('A'))
print("Recorrido DFS empezando en 'A':", g.dfs('A'))
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Demostrar formalmente que en cualquier grafo no dirigido, el número de vértices que tienen grado impar es par.

**Solución:**
1.  **Partimos del Lema del Apretón de Manos**:
    $$\sum_{v \in V} deg(v) = 2 |E|$$
2.  Dividimos el conjunto de vértices $V$ en dos subgrupos disjuntos: el conjunto $V_{\text{par}}$ (vértices de grado par) y el conjunto $V_{\text{impar}}$ (vértices de grado impar):
    $$\sum_{v \in V_{\text{par}}} deg(v) + \sum_{v \in V_{\text{impar}}} deg(v) = 2 |E|$$
3.  Analizamos los sumandos:
    *   El término de la derecha $2 |E|$ es siempre un número par (por ser múltiplo de $2$).
    *   La primera suma $\sum_{v \in V_{\text{par}}} deg(v)$ es par, pues sumamos únicamente números pares.
4.  Despejamos la segunda suma:
    $$\sum_{v \in V_{\text{impar}}} deg(v) = \underbrace{2 |E|}_{\text{Par}} - \underbrace{\sum_{v \in V_{\text{par}}} deg(v)}_{\text{Par}} \implies \text{Resultado Par}$$
5.  Como cada término individual dentro de la suma $\sum_{v \in V_{\text{impar}}} deg(v)$ es un número impar, la única forma en que una suma de números impares dé un resultado par es que sumemos un **número par de términos**.
    
Por lo tanto, el cardinal del conjunto $V_{\text{impar}}$ debe ser par. Q.E.D.

### Ejercicio 2
Un grafo tiene 6 vértices con los siguientes grados: $\{2, 2, 3, 4, 3, 2\}$. Determinar el número de aristas del grafo.

**Solución:**
Aplicamos el Lema del Apretón de Manos de forma directa:
1.  Sumamos los grados de todos los vértices:
    $$\sum deg(v) = 2 + 2 + 3 + 4 + 3 + 2 = 16$$
2.  Igualamos a dos veces el número de aristas:
    $$2 |E| = 16 \quad \implies \quad |E| = \frac{16}{2} = 8$$

El grafo tiene exactamente 8 aristas.

---

## 7. Ejercicios Propuestos

1.  Dibuja un grafo conexo de 5 vértices que sea euleriano (tenga un ciclo euleriano) y otro de 5 vértices que no lo sea, explicando en base a los grados de los vértices la diferencia.
2.  Demuestra que todo árbol con más de un vértice tiene al menos dos vértices colgantes (hojas, es decir, de grado 1).
3.  Explica la diferencia entre las estructuras de datos cola (FIFO) y pila (LIFO) y cómo influyen en el comportamiento del recorrido de un grafo (BFS vs DFS).
