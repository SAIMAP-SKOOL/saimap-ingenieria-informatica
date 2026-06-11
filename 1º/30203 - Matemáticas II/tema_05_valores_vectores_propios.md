# Tema 5: Valores y Vectores Propios (Diagonalización)

El cálculo de autovalores y autovectores permite simplificar representaciones matriciales complejas, revelando direcciones preferentes de variación espacial y comportamiento dinámico. En ingeniería informática, la diagonalización es el núcleo de la compresión de datos y reducción de ruido (PCA), de los motores de búsqueda modernos (PageRank de Google), de la clasificación automática y del análisis de sistemas dinámicos recursivos.

---

## 1. Definición y Polinomio Característico

Sea $A \in \mathcal{M}_{n \times n}(\mathbb{K})$ una matriz cuadrada sobre un cuerpo $\mathbb{K}$.

### Autovalores y Autovectores
Decimos que un escalar $\lambda \in \mathbb{K}$ es un **valor propio** (autovalor o eigenvalue) de $A$ si existe un vector no nulo $v \in \mathcal{M}_{n \times 1}(\mathbb{K})$ ($v \ne 0$) tal que:
$$A \cdot v = \lambda v$$
El vector $v$ se denomina **vector propio** (autovector o eigenvector) asociado al autovalor $\lambda$.

Geométricamente, multiplicar un autovector por la matriz $A$ no cambia su dirección en el espacio; únicamente altera su longitud por el factor de escala $\lambda$.

### Polinomio Característico
Para hallar los autovalores de $A$, reescribimos la ecuación como:
$$(A - \lambda I_n)v = 0$$
Dado que buscamos soluciones no nulas ($v \ne 0$), el sistema homogéneo debe ser compatible indeterminado, lo cual ocurre si y solo si el determinante de la matriz de coeficientes es nulo. Esto define la **ecuación característica**:
$$p(\lambda) = \det(A - \lambda I_n) = 0$$

El determinante $p(\lambda)$ es un polinomio de grado $n$ en $\lambda$, conocido como **polinomio característico**. Las raíces de este polinomio son precisamente los autovalores de la matriz.

---

## 2. Multiplicidades y Condiciones de Diagonalización

### Multiplicidad Algebraica ($m_{\text{alg}}$)
Es la multiplicidad de $\lambda_i$ como raíz del polinomio característico.

### Multiplicidad Geométrica ($m_{\text{geom}}$)
Es la dimensión del subespacio propio asociado a $\lambda_i$, denotado por $V_{\lambda_i}$ (que contiene todos los autovectores asociados a $\lambda_i$ junto con el vector nulo):
$$V_{\lambda_i} = \ker(A - \lambda_i I_n)$$
$$m_{\text{geom}}(\lambda_i) = \dim(V_{\lambda_i}) = n - \text{rango}(A - \lambda_i I_n)$$

> [!WARNING]
> Para cualquier autovalor $\lambda$, siempre se cumple que:
> $1 \le m_{\text{geom}}(\lambda) \le m_{\text{alg}}(\lambda)$

### 2.3 Diagonalizabilidad de una Matriz
Decimos que una matriz cuadrada $A \in \mathcal{M}_{n \times n}(\mathbb{K})$ es **diagonalizable** si existe una matriz invertible $P$ (matriz de paso) y una matriz diagonal $D$ tales que:
$$P^{-1} \cdot A \cdot P = D \quad \implies \quad A = P \cdot D \cdot P^{-1}$$

### Teorema Fundamental de Diagonalización
Una matriz $A$ de tamaño $n \times n$ es diagonalizable en $\mathbb{K}$ si y solo si:
1.  El polinomio característico se descompone completamente en factores lineales sobre $\mathbb{K}$ (todas sus raíces pertenecen a $\mathbb{K}$).
2.  Para cada autovalor $\lambda_i$, su multiplicidad geométrica es igual a su multiplicidad algebraica:
    $$m_{\text{geom}}(\lambda_i) = m_{\text{alg}}(\lambda_i) \quad \forall i$$

En este caso:
*   La matriz diagonal $D$ contiene los autovalores en su diagonal principal.
*   La matriz $P$ contiene como columnas a los autovectores asociados correspondientes en el mismo orden.

---

## 3. El Toque Informático

### 3.1 El Algoritmo PageRank de Google
En los inicios de Google, Larry Page y Sergey Brin diseñaron el algoritmo PageRank para ordenar las páginas de los resultados de búsqueda. Internet se modela como un grafo dirigido de enlaces:
*   Si una página $j$ enlaza a la página $i$, se le transfiere una parte de su importancia.
*   Definimos la **Matriz de Transición de Google** $G$, que es estocástica (la suma de sus columnas es 1).
*   La importancia (PageRank) de todas las páginas de la red corresponde al vector de estado estacionario $v$ que cumple:
    $$G \cdot v = 1 \cdot v$$
    Es decir, el PageRank es el **vector propio asociado al autovalor dominante $\lambda = 1$** de la matriz $G$. Se calcula de forma masiva e iterativa mediante el **Método de las Potencias**.

### 3.2 Reducción de Dimensionalidad (PCA)
En machine learning, cuando los datos tienen cientos de variables, el **Análisis de Componentes Principales** (PCA) los proyecta a un espacio de menor dimensión. Las direcciones en las que los datos varían más corresponden a los **autovectores** de la **Matriz de Covarianza** de los datos, ordenados de mayor a menor según sus autovalores correspondientes (varianza explicada).

A continuación, implementamos en Matlab/Octave el cálculo de autovalores, autovectores y reconstrucción de la diagonalización.

```octave
% Definimos la matriz A
A = [ 4, -1,  6;
      2,  1,  6;
      2, -1,  8];

% 1. Calcular autovalores y autovectores usando la función 'eig'
% D es la matriz diagonal de autovalores
% P es la matriz cuyas columnas son los autovectores
[P, D] = eig(A);

printf("Matriz de Autovectores P (columnas):\n");
disp(P);
printf("Matriz Diagonal de Autovalores D:\n");
disp(D);

% 2. Verificar la igualdad teórica A * P = P * D
LHS = A * P;
RHS = P * D;

printf("Verificación A * P (Lado Izquierdo):\n");
disp(LHS);
printf("Verificación P * D (Lado Derecho):\n");
disp(RHS);

% Comprobar numéricamente la diferencia para tolerar errores de redondeo de coma flotante
error_max = max(max(abs(LHS - RHS)));
printf("Diferencia máxima absoluta: %.2e\n", error_max);

% 3. Reconstruir la matriz original A = P * D * inv(P)
A_reconstruida = P * D * inv(P);
printf("\nMatriz A reconstruida (P * D * P^-1):\n");
disp(A_reconstruida);
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Determinar si la matriz $A$ es diagonalizable sobre $\mathbb{R}$:
$$A = \begin{pmatrix} 3 & 0 \\ 1 & 3 \end{pmatrix}$$

**Solución:**
1.  **Calcular el polinomio característico**:
    $$p(\lambda) = \det(A - \lambda I_2) = \det\begin{pmatrix} 3 - \lambda & 0 \\ 1 & 3 - \lambda \end{pmatrix} = (3 - \lambda)^2 = 0$$
    La única raíz es $\lambda = 3$ con **multiplicidad algebraica $m_{\text{alg}}(3) = 2$**.
2.  **Calcular la multiplicidad geométrica $m_{\text{geom}}(3)$**:
    $$m_{\text{geom}}(3) = 2 - \text{rango}(A - 3I_2)$$
    Calculamos $A - 3I_2$:
    $$A - 3I_2 = \begin{pmatrix} 3 - 3 & 0 \\ 1 & 3 - 3 \end{pmatrix} = \begin{pmatrix} 0 & 0 \\ 1 & 0 \end{pmatrix}$$
    El rango de esta matriz es claramente 1 (tiene una fila no nula).
    Por tanto:
    $$m_{\text{geom}}(3) = 2 - 1 = 1$$
3.  **Conclusión**:
    Dado que:
    $$m_{\text{geom}}(3) = 1 \ne m_{\text{alg}}(3) = 2$$
    No se cumple la igualdad de multiplicidades. Por lo tanto, la matriz $A$ **no es diagonalizable** en $\mathbb{R}$.

---

## 5. Ejercicios Propuestos

1.  Dada la matriz $A = \begin{pmatrix} 1 & 2 \\ 2 & 1 \end{pmatrix}$, hallar sus autovalores y sus autovectores asociados, y construir las matrices de diagonalización $D$ y $P$.
2.  Demostrar que una matriz $A$ de tamaño $n \times n$ y su traspuesta $A^T$ poseen exactamente los mismos autovalores.
    *(Pista: Compara sus polinomios característicos y las propiedades del determinante de la traspuesta).*
3.  Explicar cómo se comporta la potencia $k$-ésima de una matriz diagonalizable $A^k$ utilizando su descomposición diagonal $P \cdot D \cdot P^{-1}$. ¿Cómo simplifica esto el cálculo algorítmico frente a la multiplicación directa repetida?
