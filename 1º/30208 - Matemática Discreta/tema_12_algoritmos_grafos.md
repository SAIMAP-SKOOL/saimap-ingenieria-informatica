# Tema 12: Algoritmos sobre Grafos

La resolución práctica de problemas complejos en informática (como calcular la ruta óptima de reparto de un mensajero, trazar la red de fibra óptica de una ciudad o enrutar paquetes por Internet) exige el uso de algoritmos clásicos sobre grafos. En este tema, abordamos los dos problemas algorítmicos fundamentales en grafos ponderados: el cálculo de los **Caminos Mínimos** (Algoritmo de Dijkstra) y la construcción de los **Árboles de Expansión Mínima** (Algoritmos de Prim y Kruskal).

---

## 1. El Problema de los Caminos Mínimos: Algoritmo de Dijkstra

Dado un grafo ponderado con pesos no negativos y un vértice de origen $s \in V$, el **Algoritmo de Dijkstra** determina el camino de coste mínimo desde $s$ hasta todos los demás vértices del grafo.

### Principio de Funcionamiento (Codicioso / Greedy)
1.  Mantiene una estimación de la distancia mínima $d[v]$ para cada vértice. Inicialmente $d[s] = 0$ y $d[v] = \infty$ para los demás.
2.  Marca todos los nodos como no visitados y los inserta en una **cola de prioridad** ordenada por distancia mínima estimada.
3.  En cada paso, extrae el vértice $u$ no visitado con menor distancia $d[u]$ (comportamiento codicioso).
4.  **Relajación de aristas**: Para cada vecino $v$ de $u$, comprueba si el camino a través de $u$ es más corto que la distancia estimada actual:
    $$\text{Si } d[u] + w(u, v) < d[v] \quad \implies \quad d[v] = d[u] + w(u, v)$$
5.  Repite el proceso hasta vaciar la cola.

*   **Complejidad**: Implementado con una cola de prioridad basada en montículos binarios (Min-Heap), su coste es de **$O((|V| + |E|) \log |V|)$**.

---

## 2. Árboles de Expansión Mínima (MST)

Dado un grafo no dirigido conexo y ponderado, un **Árbol de Expansión Mínima (MST)** es un subgrafo que contiene a todos los vértices de la red ($V$), es un árbol (conexo y acíclico) y la **suma de los pesos de sus aristas es la mínima posible**.

### 2.1 Algoritmo de Prim
Comienza desde un nodo raíz inicial y va haciendo crecer el árbol nodo a nodo. En cada paso, selecciona la arista de menor peso que conecta un nodo ya perteneciente al árbol con un nodo externo. Es muy eficiente en grafos densos utilizando colas de prioridad ($O(|E| \log |V|)$).

### 2.2 Algoritmo de Kruskal
Funciona ordenando las aristas del grafo de menor a mayor peso y seleccionando aristas individualmente de forma codiciosa:
1.  Ordena el conjunto de aristas $E$ por peso ascendente.
2.  Crea un bosque inicial donde cada vértice es un conjunto disjunto aislado.
3.  Para cada arista $(u, v)$ en orden:
    *   Si $u$ y $v$ pertenecen a componentes conexas distintas (no forman ciclo), **se añade la arista al MST** y se unen (fusionan) ambas componentes.
    *   Si pertenecen al mismo conjunto, se descarta la arista (pues añadirla cerraría un ciclo).
4.  El algoritmo termina al seleccionar exactamente $|V| - 1$ aristas.

### La Estructura Disjoint-Set (Union-Find)
Para verificar rápidamente si dos nodos forman ciclo y unirlos, Kruskal utiliza una estructura de **conjuntos disjuntos (Union-Find)**. Esta estructura reduce el coste de detección a casi tiempo constante $O(\alpha(|V|))$, donde $\alpha$ es la inversa de la función de Ackermann (crecimiento extremadamente lento).

---

## 3. El Toque Informático

### Enrutamiento de Internet (OSPF) y Minimización de Costes Físicos
1.  **OSPF (Open Shortest Path First)**:
    Es el protocolo de enrutamiento interno más utilizado en los routers de las redes corporativas y proveedores de Internet. Cada router almacena un mapa del dígrafo de la red y ejecuta localmente el **Algoritmo de Dijkstra** para calcular la ruta más rápida (con menor retraso de propagación) para redirigir los paquetes de datos hacia su destino.
2.  **Cableado de Redes (MST)**:
    Si una empresa de telecomunicaciones desea conectar 20 ciudades con fibra óptica subterranea, no necesita cablearlas todas entre sí (grafo completo). Para minimizar los kilómetros de zanja excavada, calcula el **MST** mediante **Kruskal** o **Prim**, garantizando que todas las ciudades queden interconectadas con el mínimo coste total de cableado posible.

A continuación, implementamos en C++ el Algoritmo de Kruskal utilizando la estructura de datos Union-Find para resolver el MST de un grafo ponderado.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Estructura para representar una arista ponderada
struct Arista {
    int origen, destino, peso;
    
    // Sobrecarga del operador menor para ordenar por peso
    bool operator<(const Arista& otra) const {
        return peso < otra.peso;
    }
};

// Estructura de Conjuntos Disjuntos (Disjoint-Set / Union-Find)
struct UnionFind {
    vector<int> padre, rango;
    
    UnionFind(int n) {
        padre.resize(n);
        rango.resize(n, 0);
        for (int i = 0; i < n; i++) {
            padre[i] = i; // Cada nodo es su propio padre al inicio
        }
    }
    
    // Operación Find con compresión de caminos
    int buscar(int i) {
        if (padre[i] == i)
            return i;
        return padre[i] = buscar(padre[i]);
    }
    
    // Operación Union por rango
    void unir(int i, int j) {
        int raiz_i = buscar(i);
        int raiz_j = buscar(j);
        if (raiz_i != raiz_j) {
            if (rango[raiz_i] < rango[raiz_j]) {
                padre[raiz_i] = raiz_j;
            } else if (rango[raiz_i] > rango[raiz_j]) {
                padre[raiz_j] = raiz_i;
            } else {
                padre[raiz_j] = raiz_i;
                rango[raiz_i]++;
            }
        }
    }
};

// Función principal del algoritmo de Kruskal
void kruskal(int num_vertices, vector<Arista>& aristas) {
    // 1. Ordenamos las aristas de menor a mayor peso
    sort(aristas.begin(), aristas.end());
    
    UnionFind uf(num_vertices);
    vector<Arista> mst;
    int peso_total_mst = 0;
    
    // 2. Procesamos secuencialmente cada arista
    for (const auto& arista : aristas) {
        int conjunto_origen = uf.buscar(arista.origen);
        int conjunto_destino = uf.buscar(arista.destino);
        
        // Si no pertenecen al mismo conjunto, no forman ciclo
        if (conjunto_origen != conjunto_destino) {
            mst.push_back(arista);
            peso_total_mst += arista.peso;
            uf.unir(conjunto_origen, conjunto_destino);
        }
    }
    
    // Mostramos el resultado del MST
    cout << "Aristas en el MST:" << endl;
    for (const auto& arista : mst) {
        cout << arista.origen << " - " << arista.destino << " (Peso: " << arista.peso << ")" << endl;
    }
    cout << "Peso total del MST: " << peso_total_mst << endl;
}

int main() {
    int vertices = 4;
    vector<Arista> aristas = {
        {0, 1, 10},
        {0, 2, 6},
        {0, 3, 5},
        {1, 3, 15},
        {2, 3, 4}
    };
    
    kruskal(vertices, aristas);
    return 0;
}
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Aplicar el algoritmo de Kruskal paso a paso para hallar el Árbol de Expansión Mínima del siguiente grafo no dirigido:
*   Vértices: $\{0, 1, 2, 3\}$.
*   Aristas ponderadas: $e_1=(0, 3)$ peso 5; $e_2=(2, 3)$ peso 4; $e_3=(0, 2)$ peso 6; $e_4=(0, 1)$ peso 10; $e_5=(1, 3)$ peso 15.

**Solución:**
1.  **Ordenar las aristas por peso**:
    1.  $(2, 3)$ - peso 4
    2.  $(0, 3)$ - peso 5
    3.  $(0, 2)$ - peso 6
    4.  $(0, 1)$ - peso 10
    5.  $(1, 3)$ - peso 15
2.  **Inicializar conjuntos disjuntos**: $\{0\}, \{1\}, \{2\}, \{3\}$.
3.  **Procesar aristas secuencialmente**:
    *   Arista 1: $(2, 3)$, peso 4. Vértice 2 y 3 están en conjuntos distintos. **Seleccionamos arista**. Unimos conjuntos: $\{0\}, \{1\}, \{2, 3\}$. MST actual: $\{(2,3)\}$.
    *   Arista 2: $(0, 3)$, peso 5. Vértice 0 y 3 están en conjuntos distintos. **Seleccionamos arista**. Unimos conjuntos: $\{1\}, \{0, 2, 3\}$. MST actual: $\{(2,3), (0,3)\}$.
    *   Arista 3: $(0, 2)$, peso 6. Vértices 0 y 2 ya están en el mismo conjunto ($\{0, 2, 3\}$). Añadirla crearía el ciclo $(0-2-3-0)$. **Descartamos arista**.
    *   Arista 4: $(0, 1)$, peso 10. Vértice 0 y 1 están en conjuntos distintos. **Seleccionamos arista**. Unimos conjuntos: $\{0, 1, 2, 3\}$. MST actual: $\{(2,3), (0,3), (0,1)\}$.
4.  **Finalización**: Hemos seleccionado 3 aristas ($|V| - 1 = 3$). Las componentes están unificadas.
    *   Peso total del MST = $4 + 5 + 10 = 19$.

### Ejercicio 2
Un algoritmo de Dijkstra se ejecuta en un router. Explica por qué el algoritmo no funcionaría de forma correcta si una de las líneas de comunicación tuviera un coste negativo (por ejemplo, $w(u, v) = -5$).

**Solución:**
Dijkstra es un algoritmo de naturaleza **codiciosa (greedy)**. Una vez que selecciona un nodo con la menor distancia provisional y lo marca como visitado, asume que su distancia mínima es definitiva y **nunca vuelve a evaluarlo**.
Si existen aristas de peso negativo, este principio falla: podría ocurrir que un camino posterior que pase por dicha arista negativa reduzca el coste de un nodo ya considerado óptimo, requiriendo reevaluar nodos cerrados. Para grafos con pesos negativos, se debe utilizar el **Algoritmo de Bellman-Ford** (con complejidad superior $O(|V||E|)$) o el algoritmo de Floyd-Warshall.

---

## 5. Ejercicios Propuestos

1.  Dibuja un grafo ponderado de 5 vértices y aplica paso a paso el algoritmo de Dijkstra para calcular los caminos mínimos partiendo desde el vértice 0.
2.  Explica la diferencia de enfoque entre el algoritmo de Prim y el de Kruskal para hallar el MST. ¿Bajo qué circunstancias de densidad de red es más eficiente implementar cada uno de ellos?
3.  ¿Cómo funciona la técnica de **Compresión de Caminos (Path Compression)** en el método Union-Find y cómo reduce la complejidad temporal en la detección de ciclos de Kruskal?
