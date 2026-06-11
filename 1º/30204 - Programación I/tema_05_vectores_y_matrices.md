# Tema 5: Vectores y Matrices (Arrays)

Los tipos de datos primitivos individuales (como `int` o `double`) solo pueden almacenar un valor a la vez. Un **array** (o arreglo/vector) es una estructura de datos estática y homogénea que almacena una colección secuencial de elementos del mismo tipo bajo un único nombre. Su acceso directo en tiempo constante ($O(1)$) lo convierte en la estructura fundamental de almacenamiento de información lineal y matricial.

---

## 1. Definición y Uso de Arrays en C++

Los arrays reservan un bloque de memoria contiguo en el sistema.

### 1.1 Arrays Unidimensionales (Vectores)
Se declaran indicando el tipo, nombre del array y su tamaño (que debe ser una constante entera conocida en tiempo de compilación):
```cpp
int temperaturas[7]; // Array de 7 enteros
```

*Indexación*: Los elementos se acceden mediante índices enteros que van desde `0` hasta `tamaño - 1`.
```cpp
temperaturas[0] = 22; // Primer elemento
temperaturas[6] = 18; // Séptimo (último) elemento
```

> [!CAUTION]
> C++ **no comprueba los límites del array** en tiempo de ejecución. Acceder a un índice fuera del rango (por ejemplo, `temperaturas[7]`) provocará un error de segmentación (Segmentation Fault) o leerá basura de memoria, desestabilizando el programa.

### 1.2 Arrays Bidimensionales (Matrices)
Se declaran con dos conjuntos de corchetes representing filas y columnas:
```cpp
int matriz[3][4]; // Matriz de 3 filas y 4 columnas
```

Se accede mediante dos índices: `matriz[fila][columna]`.
```cpp
matriz[0][0] = 5; // Elemento en la esquina superior izquierda
```

---

## 2. Algoritmos de Búsqueda

La búsqueda consiste en encontrar la posición de un elemento (clave) dentro de un array.

### 2.1 Búsqueda Lineal (Secuencial)
Recorre el array desde el principio hasta el final, comparando cada elemento con la clave buscada.
*   **Complejidad**: $O(n)$ en el peor de los casos (si el elemento no existe o está al final).
*   **Requisito**: Funciona en cualquier array (ordenado o desordenado).

### 2.2 Búsqueda Binaria (Dicotómica)
Divide repetidamente a la mitad el rango de búsqueda. Compara la clave con el elemento central; si no coincide, reduce el rango al sub-array izquierdo o derecho.
*   **Complejidad**: $O(\log n)$ (extremadamente rápida, análoga a la búsqueda en un diccionario).
*   **Requisito**: **El array debe estar ordenado previamente**.

---

## 3. Algoritmos de Ordenación

La ordenación consiste en organizar los elementos del array en un orden lógico (normalmente ascendente).

1.  **Ordenación por Burbuja (Bubble Sort)**: Compara pares de elementos adyacentes y los intercambia si están en el orden incorrecto. Repite el proceso hasta que no se requieran más intercambios.
    *   *Complejidad*: $O(n^2)$
2.  **Ordenación por Selección (Selection Sort)**: Busca el elemento más pequeño del array y lo intercambia con el elemento de la primera posición. Luego busca el menor del resto del array y lo intercambia con la segunda posición, y así sucesivamente.
    *   *Complejidad*: $O(n^2)$
3.  **Ordenación por Inserción (Insertion Sort)**: Toma cada elemento y lo inserta en su posición correcta respecto a los elementos ya ordenados a su izquierda (similar a cómo se ordenan las cartas en la mano).
    *   *Complejidad*: $O(n^2)$

---

## 4. El Toque Informático

### Búsqueda Binaria en Grandes Volúmenes de Datos
Para un array de 1 millón de elementos:
*   La **Búsqueda Lineal** requiere hasta 1 millón de comparaciones.
*   La **Búsqueda Binaria** requiere a lo sumo $\log_2(1000000) \approx 20$ comparaciones.

Este enorme ahorro de tiempo ilustra por qué en ingeniería informática es preferible incurrir en el coste de ordenar un array una sola vez (mediante algoritmos rápidos como QuickSort o MergeSort de $O(n\log n)$) para después realizar miles de búsquedas binarias instantáneas de $O(\log n)$.

A continuación, implementamos en C++ el algoritmo de ordenación por selección para ordenar un vector, y posteriormente realizamos una búsqueda binaria en él.

```cpp
#include <iostream>

// Función para ordenar un array usando Selección Directa (Selection Sort)
void ordenar_seleccion(int arr[], int n) {
    for (int i = 0; i < n - 1; ++i) {
        int idx_minimo = i;
        for (int j = i + 1; j < n; ++j) {
            if (arr[j] < arr[idx_minimo]) {
                idx_minimo = j;
            }
        }
        // Intercambiamos el menor encontrado con el elemento en la posición i
        int aux = arr[i];
        arr[i] = arr[idx_minimo];
        arr[idx_minimo] = aux;
    }
}

// Función que realiza la Búsqueda Binaria (O(log n))
int busqueda_binaria(const int arr[], int n, int clave) {
    int izquierda = 0;
    int derecha = n - 1;
    
    while (izquierda <= derecha) {
        int medio = izquierda + (derecha - izquierda) / 2;
        
        if (arr[medio] == clave) {
            return medio; // Elemento encontrado, devolvemos su índice
        }
        if (arr[medio] < clave) {
            izquierda = medio + 1; // Descartamos la mitad izquierda
        } else {
            derecha = medio - 1; // Descartamos la mitad derecha
        }
    }
    return -1; // Elemento no encontrado
}

int main() {
    const int TAM = 8;
    int datos[TAM] = {34, 12, 5, 89, 56, 21, 8, 77};
    
    std::cout << "Vector original desordenado: ";
    for (int i = 0; i < TAM; ++i) std::cout << datos[i] << " ";
    std::cout << std::endl;
    
    // 1. Ordenamos el vector (requisito para búsqueda binaria)
    ordenar_seleccion(datos, TAM);
    
    std::cout << "Vector ordenado por seleccion: ";
    for (int i = 0; i < TAM; ++i) std::cout << datos[i] << " ";
    std::cout << std::endl;
    
    // 2. Realizamos la búsqueda
    int buscar = 21;
    int posicion = busqueda_binaria(datos, TAM, buscar);
    
    if (posicion != -1) {
        std::cout << "El elemento " << buscar << " se encuentra en el indice: " << posicion << std::endl;
    } else {
        std::cout << "El elemento " << buscar << " no esta en el vector." << std::endl;
    }
    
    return 0;
}
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Escribir una función en C++ que reciba una matriz de tamaño $3 \times 3$ de enteros y devuelva la suma de todos los elementos situados en su diagonal principal (traza de la matriz).

**Solución:**
```cpp
#include <iostream>

const int FILAS = 3;
const int COLUMNAS = 3;

// La diagonal principal solo existe en matrices cuadradas (fila == columna)
int calcular_traza(const int matriz[FILAS][COLUMNAS]) {
    int suma = 0;
    for (int i = 0; i < FILAS; ++i) {
        suma += matriz[i][i]; // Acceso directo al elemento diagonal
    }
    return suma;
}

int main() {
    int M[FILAS][COLUMNAS] = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9}
    };
    
    int traza = calcular_traza(M);
    std::cout << "La traza de la matriz (suma diagonal) es: " << traza << std::endl; // 1 + 5 + 9 = 15
    return 0;
}
```

---

## 6. Ejercicios Propuestos

1.  Escribir una función que reciba un array de enteros y devuelva tanto el valor máximo como el valor mínimo presentes en el mismo.
2.  Implementar la función de búsqueda lineal en C++ y realizar una traza de ejecución comparando el número de iteraciones que realiza frente a la búsqueda binaria para encontrar el número `89` en el array de ejemplo del Tema 5.
3.  ¿Por qué los arrays en C++ se pasan siempre por referencia de forma implícita al invocarse en funciones? ¿Qué implicaciones tiene esto en la gestión de memoria y el rendimiento?
