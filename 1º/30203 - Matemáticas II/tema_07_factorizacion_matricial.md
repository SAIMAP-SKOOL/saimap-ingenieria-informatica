# Tema 7: Factorización Matricial y Coste Computacional

La resolución directa de sistemas de ecuaciones lineales mediante el método de Gauss es computacionalmente costosa y puede verse afectada por inestabilidad numérica. Las factorizaciones matriciales descomponen una matriz en un producto de matrices más sencillas (triangulares, ortogonales, simétricas) que facilitan cálculos repetidos. En la ingeniería informática, comprender estas descomposiciones es crucial para optimizar algoritmos científicos, simulaciones físicas de alto rendimiento y análisis estructural de redes.

---

## 1. Descomposición LU (con y sin pivotaje)

La **factorización LU** expresa una matriz cuadrada $A \in \mathcal{M}_{n \times n}(\mathbb{R})$ como el producto de una matriz triangular inferior con unos en la diagonal $L$ (Lower) y una matriz triangular superior $U$ (Upper):
$$A = L \cdot U$$

```
   A = [ * * * ]      L = [ 1 0 0 ]      U = [ * * * ]
       [ * * * ]          [ * 1 0 ]          [ 0 * * ]
       [ * * * ]          [ * * 1 ]          [ 0 0 * ]
```

Una vez que tenemos la descomposición $L \cdot U$, resolver el sistema $A \cdot x = b$ se reduce a resolver dos sistemas triangulares sucesivos sumamente rápidos ($O(n^2)$):
1.  Resolver $L \cdot y = b$ mediante **sustitución directa**.
2.  Resolver $U \cdot x = y$ mediante **sustitución regresiva**.

### Estrategia de Pivotaje Parcial (Factorización PLU)
Si durante la eliminación de Gauss un elemento de la diagonal (pivote) es cero o cercano a cero, la división matemática introduce graves errores de redondeo.
Para resolver esto se aplica **pivotaje parcial**: intercambiamos filas para colocar el elemento de mayor valor absoluto de la columna en la diagonal. Matricialmente, esto se representa como:
$$P \cdot A = L \cdot U$$
donde $P$ es una **matriz de permutación** (matriz identidad con sus filas intercambiadas).

---

## 2. Factorización de Cholesky

Si la matriz $A$ es **simétrica** ($A = A^T$) y **definida positiva** ($x^T A x > 0$ para todo vector no nulo $x$), se puede descomponer de forma más eficiente que LU como el producto de una matriz triangular inferior $L$ por su propia traspuesta:
$$A = L \cdot L^T$$

### Ventaja Computacional
*   Requiere únicamente la mitad de operaciones en punto flotante que la descomposición LU estándar.
*   Es numéricamente muy estable y no requiere pivotaje.

---

## 3. Factorización QR

La **factorización QR** descompone una matriz $A \in \mathcal{M}_{m \times n}(\mathbb{R})$ ($m \ge n$) en el producto de una matriz ortogonal $Q$ ($Q^T Q = I$) y una matriz triangular superior $R$:
$$A = Q \cdot R$$

Se implementa mediante tres enfoques matemáticos principales:
1.  **Ortogonalización de Gram-Schmidt** (puede sufrir inestabilidad por errores de redondeo).
2.  **Transformaciones de Householder** (reflexiones ortogonales, muy estable).
3.  **Rotaciones de Givens** (rotaciones locales, ideal para matrices dispersas).

---

## 4. Coste Computacional y Análisis Algorítmico

En informática, la eficiencia se mide por el número de **FLOPs** (operaciones de punto flotante: sumas, restas, productos y divisiones). A continuación, se detalla el coste de las principales operaciones para matrices de tamaño $n \times n$:

| Operación / Factorización | Coste Computacional (FLOPs) | Complejidad Temporal |
| :--- | :--- | :--- |
| Multiplicación clásica $A \cdot B$ | $2n^3$ | $O(n^3)$ |
| Cálculo de la Inversa $A^{-1}$ | $2n^3$ | $O(n^3)$ |
| Descomposición LU / Eliminación de Gauss | $\displaystyle \frac{2}{3}n^3$ | $O(n^3)$ |
| Descomposición de Cholesky | $\displaystyle \frac{1}{3}n^3$ | $O(n^3)$ |
| Descomposición QR (Householder) | $\displaystyle \frac{4}{3}n^3$ | $O(n^3)$ |
| Sustitución directa/regresiva (triangular) | $n^2$ | $O(n^2)$ |

> [!IMPORTANT]
> **Por qué no calcular la inversa ($A^{-1}$)**:
> Resolver un sistema mediante $x = A^{-1}b$ es computacionalmente ineficiente y numéricamente inestable en comparación con las factorizaciones.
> Si tenemos múltiples vectores de carga $b$ (como en animaciones de física frame a frame), realizamos la factorización **una sola vez** ($O(n^3)$) y en cada frame resolvemos mediante sustituciones rápidas ($O(n^2)$), en lugar de volver a invertir la matriz.

A continuación, implementamos en Matlab/Octave las descomposiciones LU y Cholesky.

```octave
% Definimos una matriz simétrica y definida positiva A
A = [ 4, 12, -16;
     12, 37, -43;
    -16, -43, 98];

b = [24; 86; -182];

% 1. Descomposición LU con pivotaje parcial (P * A = L * U)
[L_lu, U_lu, P_lu] = lu(A);

printf("Matriz de Permutación P:\n");
disp(P_lu);
printf("Matriz Triangular Inferior L (LU):\n");
disp(L_lu);
printf("Matriz Triangular Superior U (LU):\n");
disp(U_lu);

% 2. Descomposición de Cholesky (A = L_chol * L_chol')
% Nota: En Matlab/Octave, 'chol' devuelve por defecto la triangular superior U (A = U'*U)
% Para obtener la inferior L, pasamos el parámetro 'lower'.
L_chol = chol(A, 'lower');

printf("Matriz de Cholesky L (triangular inferior):\n");
disp(L_chol);

% Verificar que L * L^T = A
printf("Verificación L * L^T:\n");
disp(L_chol * L_chol');

% 3. Resolución del sistema usando Cholesky
% L * y = b  ===> y = L \ b  (Sustitución directa)
% L' * x = y ===> x = L' \ y (Sustitución regresiva)
y = L_chol \ b;
x_sol = L_chol' \ y;

printf("Solución del sistema calculada mediante Cholesky:\n");
disp(x_sol);
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Calcular de forma manual la descomposición $LU$ (sin pivotaje) de la siguiente matriz:
$$A = \begin{pmatrix} 2 & 1 \\ 4 & 7 \end{pmatrix}$$

**Solución:**
1.  **Hacer la eliminación de Gauss sobre $A$ para hallar $U$**:
    Queremos eliminar el elemento $a_{21} = 4$.
    Para ello multiplicamos la fila 1 por el multiplicador $m_{21} = \frac{a_{21}}{a_{11}} = \frac{4}{2} = 2$.
    *   $F_2 \leftarrow F_2 - m_{21}F_1 = F_2 - 2F_1$
        $$\begin{pmatrix} 4 & 7 \end{pmatrix} - 2\begin{pmatrix} 2 & 1 \end{pmatrix} = \begin{pmatrix} 4 & 7 \end{pmatrix} - \begin{pmatrix} 4 & 2 \end{pmatrix} = \begin{pmatrix} 0 & 5 \end{pmatrix}$$
    La matriz resultante escalonada superior es $U$:
    $$U = \begin{pmatrix} 2 & 1 \\ 0 & 5 \end{pmatrix}$$
2.  **Construir la matriz triangular inferior $L$**:
    La matriz $L$ tiene 1s en la diagonal y los multiplicadores en la parte inferior:
    $$L = \begin{pmatrix} 1 & 0 \\ m_{21} & 1 \end{pmatrix} = \begin{pmatrix} 1 & 0 \\ 2 & 1 \end{pmatrix}$$
3.  **Verificación del producto $L \cdot U$**:
    $$L \cdot U = \begin{pmatrix} 1 & 0 \\ 2 & 1 \end{pmatrix} \begin{pmatrix} 2 & 1 \\ 0 & 5 \end{pmatrix} = \begin{pmatrix} 1\cdot 2 + 0\cdot 0 & 1\cdot 1 + 0\cdot 5 \\ 2\cdot 2 + 1\cdot 0 & 2\cdot 1 + 1\cdot 5 \end{pmatrix} = \begin{pmatrix} 2 & 1 \\ 4 & 7 \end{pmatrix} = A \quad (\text{Correcto})$$

---

## 6. Ejercicios Propuestos

1.  Dada la matriz $A = \begin{pmatrix} 4 & 2 \\ 2 & 10 \end{pmatrix}$, hallar de forma manual su descomposición de Cholesky $A = L \cdot L^T$.
2.  Describir el efecto de multiplicar una matriz $A$ por una matriz de permutación $P$ a la izquierda ($P \cdot A$) frente a multiplicarla por la derecha ($A \cdot P$).
3.  Sea un sistema físico modelado por $A x = b$. Si $A$ es una matriz de $1000 \times 1000$ y necesitamos resolver el sistema para 50 vectores $b$ distintos:
    *   Calcular el coste aproximado en operaciones de punto flotante (FLOPs) si calculamos $A^{-1}$ y multiplicamos $x = A^{-1}b$.
    *   Calcular el coste si calculamos la factorización $LU$ y resolvemos mediante sustituciones.
    *   Comparar ambos enfoques y determinar cuál es más eficiente.
