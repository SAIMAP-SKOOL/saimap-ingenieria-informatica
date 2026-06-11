# Tema 2: Álgebra Matricial y Sistemas de Ecuaciones Lineales

Las matrices son la estructura de datos bidimensional por excelencia en computación. El álgebra matricial permite formular y resolver de manera compacta y eficiente sistemas de ecuaciones lineales gigantescos, los cuales aparecen de forma natural en el renderizado de gráficos 3D, el procesamiento de imágenes, la simulación física de fluidos, la teoría de redes de comunicaciones y la optimización lineal.

---

## 1. Operaciones Matriciales y Determinantes

Una **matriz** $A \in \mathcal{M}_{m \times n}(\mathbb{K})$ es una disposición rectangular de elementos organizados en $m$ filas y $n$ columnas sobre un cuerpo $\mathbb{K}$ (normalmente $\mathbb{R}$ o $\mathbb{C}$).

### 1.1 Operaciones Básicas
*   **Suma**: Dadas $A, B \in \mathcal{M}_{m \times n}(\mathbb{K})$, entonces $(A+B)_{ij} = A_{ij} + B_{ij}$.
*   **Multiplicación por Escalar**: Para $\lambda \in \mathbb{K}$, $(\lambda A)_{ij} = \lambda A_{ij}$.
*   **Multiplicación de Matrices**: Dadas $A \in \mathcal{M}_{m \times p}(\mathbb{K})$ y $B \in \mathcal{M}_{p \times n}(\mathbb{K})$, el producto $C = A \cdot B \in \mathcal{M}_{m \times n}(\mathbb{K})$ se define como:
    $$C_{ij} = \sum_{k=1}^{p} A_{ik} B_{kj}$$
    *Nota*: La multiplicación de matrices **no es conmutativa** ($A \cdot B \ne B \cdot A$, en general).

### 1.2 Determinante de una Matriz Cuadrada
El **determinante** de una matriz cuadrada $A \in \mathcal{M}_{n \times n}(\mathbb{K})$, denotado por $\det(A)$ o $|A|$, es un escalar que resume importantes propiedades geométricas y algebraicas de la matriz.
*   Si $\det(A) \ne 0$: La matriz es **regular** (invertible) y existe la matriz inversa $A^{-1}$ tal que $A \cdot A^{-1} = A^{-1} \cdot A = I_n$.
*   Si $\det(A) = 0$: La matriz es **singular** (no invertible).

### 1.3 Operaciones Elementales de Fila
Son operaciones aplicadas a las filas de una matriz que no alteran el conjunto de soluciones del sistema lineal asociado:
1.  Intercambiar dos filas ($F_i \leftrightarrow F_j$).
2.  Multiplicar una fila por un escalar no nulo ($F_i \leftarrow \lambda F_i$, $\lambda \ne 0$).
3.  Sumar a una fila otra fila multiplicada por un escalar ($F_i \leftarrow F_i + \lambda F_j$).

---

## 2. Resolución de Sistemas de Ecuaciones Lineales (SEL)

Un sistema de $m$ ecuaciones lineales con $n$ incógnitas se escribe matricialmente como:
$$A \cdot x = b$$
donde $A \in \mathcal{M}_{m \times n}(\mathbb{K})$ es la **matriz de coeficientes**, $x \in \mathcal{M}_{n \times 1}(\mathbb{K})$ es el **vector de incógnitas** y $b \in \mathcal{M}_{m \times 1}(\mathbb{K})$ es el **vector de términos independientes**.

### 2.1 Método de Eliminación de Gauss
Consiste en aplicar operaciones elementales de fila sobre la **matriz ampliada** $(A|b)$ para transformarla en una matriz escalonada superior. Posteriormente, las incógnitas se resuelven mediante **sustitución regresiva**.

El **Método de Gauss-Jordan** lleva el proceso un paso más allá, transformando la matriz $A$ en la identidad (o su forma escalonada reducida por filas), permitiendo leer las soluciones de forma directa.

### 2.2 Teorema de Rouché-Frobenius (Clasificación de Sistemas)
Denotemos por $\text{rango}(A)$ el número de filas no nulas en su forma escalonada. Sea $(A|b)$ la matriz ampliada del sistema de $n$ incógnitas.
1.  **Sistema Compatible (tiene solución)**: $\text{rango}(A) = \text{rango}(A|b)$
    *   **Determinado (SCD - solución única)**: $\text{rango}(A) = n$
    *   **Indeterminado (SCI - infinitas soluciones)**: $\text{rango}(A) < n$ (el sistema posee $n - \text{rango}(A)$ grados de libertad o parámetros libres).
2.  **Sistema Incompatible (SI - no tiene solución)**: $\text{rango}(A) \ne \text{rango}(A|b)$

---

## 3. El Toque Informático

### 3.1 Representación de Grafos
En ciencias de la computación, un grafo (como una red de servidores, enlaces web o mapas de transporte) se representa formalmente mediante su **Matriz de Adyacencia** $W$. Si el grafo tiene $n$ vértices, $W \in \mathcal{M}_{n \times n}$ se define como:
$$W_{ij} = \begin{cases} 1 & \text{si hay conexión entre el vértice } i \text{ y el } j \\ 0 & \text{en caso contrario} \end{cases}$$

Una propiedad notable del álgebra matricial es que el término $(W^k)_{ij}$ de la matriz elevada a la potencia $k$ indica exactamente el **número de caminos de longitud $k$** que existen entre el nodo $i$ y el nodo $j$.

### 3.2 Complejidad de la Multiplicación Matricial
La multiplicación matricial ingenua basada en la definición clásica requiere $O(n^3)$ operaciones de punto flotante (FLOPs).
En 1969, Volker Strassen diseñó un algoritmo recursivo de tipo "divide y vencerás" que reduce la complejidad a **$O(n^{2.807})$** dividiendo las matrices en subbloques y reutilizando combinaciones lineales para reducir el número de multiplicaciones de 8 a 7 en cada nivel recursivo. Hoy en día, las librerías científicas optimizan estas operaciones a nivel de caché del procesador para maximizar el rendimiento.

A continuación, implementamos en Matlab/Octave la reducción escalonada mediante `rref` y la resolución de sistemas usando el operador backslash `\`.

```octave
% Definición del Sistema de Ecuaciones Lineales
% 2x + y - z = 8
% -3x - y + 2z = -11
% -2x + y + 2z = -3

A = [ 2,  1, -1;
     -3, -1,  2;
     -2,  1,  2];
     
b = [8; -11; -3];

% 1. Comprobación teórica de rangos usando Rouché-Frobenius
rango_A = rank(A);
rango_ampliada = rank([A, b]);
n_incognitas = size(A, 2);

printf("Análisis de Rouché-Frobenius:\n");
printf("Rango(A) = %d\n", rango_A);
printf("Rango(A|b) = %d\n", rango_ampliada);

if rango_A == rango_ampliada
    if rango_A == n_incognitas
        printf("Sistema Compatible Determinado (SCD) - Solución única.\n");
    else
        printf("Sistema Compatible Indeterminado (SCI) - Infinitas soluciones.\n");
    end
else
    printf("Sistema Incompatible (SI) - Sin solución.\n");
end

% 2. Resolución rápida en Matlab/Octave usando el operador de división a izquierda (Backslash)
% Este operador utiliza internamente descomposición LU y solver adaptativo optimizado.
x_sol = A \ b;

printf("\nSolución del sistema calculada mediante 'A \\ b':\n");
disp(x_sol);

% 3. Inspección detallada del proceso Gauss-Jordan usando rref (Reduced Row Echelon Form)
M_ampliada = [A, b];
[R, pivotes] = rref(M_ampliada);

printf("Matriz ampliada reducida por filas (Gauss-Jordan):\n");
disp(R);
printf("Nodos pivote identificados en las columnas: ");
disp(pivotes);
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Clasificar y resolver mediante el método de Gauss el siguiente sistema lineal sobre $\mathbb{R}$:
$$\begin{cases} 
x + 2y - z = 3 \\ 
2x + 5y - 4z = 5 \\ 
3x + 4y + 2z = 12 
\end{cases}$$

**Solución:**
1.  **Escribir la matriz ampliada del sistema**:
    $$(A|b) = \begin{pmatrix} 1 & 2 & -1 & | & 3 \\ 2 & 5 & -4 & | & 5 \\ 3 & 4 & 2 & | & 12 \end{pmatrix}$$
2.  **Hacer ceros debajo del primer pivote ($a_{11} = 1$)**:
    *   $F_2 \leftarrow F_2 - 2F_1$
        $$\begin{pmatrix} 2 & 5 & -4 & | & 5 \end{pmatrix} - \begin{pmatrix} 2 & 4 & -2 & | & 6 \end{pmatrix} = \begin{pmatrix} 0 & 1 & -2 & | & -1 \end{pmatrix}$$
    *   $F_3 \leftarrow F_3 - 3F_1$
        $$\begin{pmatrix} 3 & 4 & 2 & | & 12 \end{pmatrix} - \begin{pmatrix} 3 & 6 & -3 & | & 9 \end{pmatrix} = \begin{pmatrix} 0 & -2 & 5 & | & 3 \end{pmatrix}$$
    La matriz ampliada resultante es:
    $$\begin{pmatrix} 1 & 2 & -1 & | & 3 \\ 0 & 1 & -2 & | & -1 \\ 0 & -2 & 5 & | & 3 \end{pmatrix}$$
3.  **Hacer ceros debajo del segundo pivote ($a_{22} = 1$)**:
    *   $F_3 \leftarrow F_3 + 2F_2$
        $$\begin{pmatrix} 0 & -2 & 5 & | & 3 \end{pmatrix} + \begin{pmatrix} 0 & 2 & -4 & | & -2 \end{pmatrix} = \begin{pmatrix} 0 & 0 & 1 & | & 1 \end{pmatrix}$$
    La matriz final escalonada es:
    $$\begin{pmatrix} 1 & 2 & -1 & | & 3 \\ 0 & 1 & -2 & | & -1 \\ 0 & 0 & 1 & | & 1 \end{pmatrix}$$
4.  **Clasificación del sistema (Teorema de Rouché-Frobenius)**:
    *   El número de filas no nulas es 3 $\implies \text{rango}(A) = \text{rango}(A|b) = 3$.
    *   Como el número de incógnitas es $n = 3$, el rango es igual a $n$.
    *   Por tanto, es un **Sistema Compatible Determinado (SCD)** con solución única.
5.  **Sustitución regresiva**:
    *   De $F_3$: $z = 1$
    *   De $F_2$: $y - 2z = -1 \implies y - 2(1) = -1 \implies y = 1$
    *   De $F_1$: $x + 2y - z = 3 \implies x + 2(1) - 1 = 3 \implies x + 1 = 3 \implies x = 2$
    
    La solución única es $x = 2$, $y = 1$, $z = 1$.

---

## 5. Ejercicios Propuestos

1.  Dada la matriz de adyacencia de un grafo de tres nodos:
    $$W = \begin{pmatrix} 0 & 1 & 1 \\ 1 & 0 & 0 \\ 1 & 1 & 0 \end{pmatrix}$$
    Calcular $W^2$ y determinar el número de caminos de longitud 2 que existen desde el nodo 3 al nodo 1.
2.  Discutir el siguiente sistema de ecuaciones lineales en función del parámetro real $k$:
    $$\begin{cases} 
    x + y + kz = 1 \\ 
    x + ky + z = 1 \\ 
    kx + y + z = 1 
    \end{cases}$$
3.  Demostrar que para dos matrices cualesquiera $A, B \in \mathcal{M}_{n \times n}(\mathbb{K})$, se cumple la propiedad distributiva de determinantes: $\det(A \cdot B) = \det(A) \cdot \det(B)$.
