# Tema 8: Resolución Aproximada de Sistemas (Métodos Iterativos)

Cuando tratamos con sistemas de ecuaciones lineales gigantescos (millones de incógnitas), habituales en computación científica, procesamiento de imágenes o análisis de redes sociales, los métodos directos (como Gauss o LU) son inviables debido a su coste computacional $O(n^3)$ y al almacenamiento de memoria. Los **métodos iterativos** resuelven esta limitación aproximando la solución de manera progresiva mediante sucesivas iteraciones más sencillas, siendo idóneos para **matrices dispersas** (matrices donde la inmensa mayoría de sus elementos son ceros).

---

## 1. Concepto General de los Métodos Iterativos

Para resolver $A \cdot x = b$, dividimos (descomponemos) la matriz de coeficientes $A$ como:
$$A = M - N$$
donde $M$ es una matriz fácilmente invertible. Reescribimos el sistema como:
$$(M - N)x = b \implies M x = N x + b \implies x = M^{-1} N x + M^{-1} b$$

Esto define la **ecuación iterativa** de recurrencia:
$$x^{(k+1)} = B \cdot x^{(k)} + c \quad (\text{para } k = 0, 1, 2, \dots)$$
donde:
*   $B = M^{-1} N$ es la **matriz de iteración**.
*   $c = M^{-1} b$ es un vector constante.
*   $x^{(0)}$ es una aproximación inicial arbitraria.

---

## 2. Los Algoritmos de Jacobi y Gauss-Seidel

Descomponemos la matriz $A$ en la suma de sus componentes:
$$A = D - L - U$$
donde $D$ es la diagonal de $A$, $-L$ es la parte triangular inferior estricta y $-U$ es la parte triangular superior estricta.

```
   A = [ D ] - [ L ] - [ U ]
       [   ]   [   ]   [   ]
```

### 2.1 Método de Jacobi
El método de Jacobi calcula la nueva aproximación $x^{(k+1)}$ usando únicamente los valores de la iteración anterior $x^{(k)}$. Matricialmente, elegimos $M = D$ y $N = L + U$:
$$x^{(k+1)} = D^{-1}(L + U)x^{(k)} + D^{-1}b$$

A nivel de componentes, la fórmula para cada incógnita $i$ es:
$$x_i^{(k+1)} = \frac{1}{a_{ii}} \left( b_i - \sum_{j \ne i} a_{ij} x_j^{(k)} \right)$$

### 2.2 Método de Gauss-Seidel
El método de Gauss-Seidel mejora la velocidad al utilizar los nuevos valores $x_j^{(k+1)}$ en cuanto se calculan dentro de la misma iteración. Elegimos $M = D - L$ y $N = U$:
$$(D - L)x^{(k+1)} = U x^{(k)} + b \implies x^{(k+1)} = (D - L)^{-1} U x^{(k)} + (D - L)^{-1} b$$

A nivel de componentes, la fórmula es:
$$x_i^{(k+1)} = \frac{1}{a_{ii}} \left( b_i - \sum_{j < i} a_{ij} x_j^{(k+1)} - \sum_{j > i} a_{ij} x_j^{(k)} \right)$$

---

## 3. Análisis de Convergencia

Un método iterativo converge a la solución exacta para cualquier vector inicial $x^{(0)}$ si y solo si el **radio espectral** de la matriz de iteración $B$ es estrictamente menor que 1:
$$\rho(B) < 1$$
donde el radio espectral $\rho(B)$ es el máximo valor absoluto de los autovalores de $B$:
$$\rho(B) = \max \{|\lambda_i| : \lambda_i \text{ es autovalor de } B\}$$

### Condiciones Suficientes de Convergencia
1.  **Matrices Diagonalmente Dominantes por Filas**: Si en cada fila de $A$, el elemento de la diagonal es mayor en valor absoluto que la suma de los demás elementos de la fila:
    $$|a_{ii}| > \sum_{j \ne i} |a_{ij}| \quad \forall i$$
    Tanto Jacobi como Gauss-Seidel **tienen garantizada la convergencia**.
2.  **Matrices Simétricas y Definidas Positivas**: El método de Gauss-Seidel converge siempre si la matriz es simétrica y definida positiva.

---

## 4. Cálculo Aproximado de Autovalores: Método de las Potencias

Para matrices gigantescas, calcular analíticamente las raíces del polinomio característico es imposible. El **Método de las Potencias** es un algoritmo iterativo que aproxima el **autovalor dominante** (el de mayor valor absoluto $\lambda_1$) y su autovector asociado $v_1$.

Dado un vector inicial $y^{(0)}$, realizamos la recurrencia:
$$y^{(k+1)} = A \cdot x^{(k)}$$
$$x^{(k+1)} = \frac{y^{(k+1)}}{\|y^{(k+1)}\|} \quad (\text{normalización})$$

El cociente de Rayleigh aproxima el autovalor dominante en cada etapa:
$$\lambda_1^{(k)} \approx \frac{(x^{(k)})^T A x^{(k)}}{(x^{(k)})^T x^{(k)}}$$

---

## 5. El Toque Informático

A continuación, implementamos en Matlab/Octave los métodos de Jacobi y Gauss-Seidel para resolver el sistema diagonalmente dominante:
$$\begin{pmatrix} 4 & 1 & 1 \\ 1 & 5 & 2 \\ 1 & 2 & 5 \end{pmatrix} \begin{pmatrix} x_1 \\ x_2 \\ x_3 \end{pmatrix} = \begin{pmatrix} 6 \\ 8 \\ 8 \end{pmatrix}$$
Su solución exacta es $x = [1; 1; 1]$.

```octave
% Definición de la matriz A y el vector b
A = [4, 1, 1;
     1, 5, 2;
     1, 2, 5];
b = [6; 8; 8];

n = size(A, 1);
x0 = zeros(n, 1); % Estimación inicial
tol = 1e-6;
max_iter = 100;

% 1. Implementación del Método de Jacobi
function [x, it] = jacobi_iterativo(A, b, x0, tol, max_iter)
    n = length(b);
    x = x0;
    x_prev = x0;
    D = diag(diag(A));
    LU = A - D; % L + U con signo cambiado
    
    for it = 1:max_iter
        % Ecuación matricial de Jacobi: x^(k+1) = D^-1 * (-(L+U)*x^(k) + b)
        x = D \ (-LU * x_prev + b);
        
        if norm(x - x_prev, inf) < tol
            return;
        end
        x_prev = x;
    end
end

% 2. Implementación del Método de Gauss-Seidel
function [x, it] = gauss_seidel_iterativo(A, b, x0, tol, max_iter)
    n = length(b);
    x = x0;
    x_prev = x0;
    
    for it = 1:max_iter
        % Nivel de componentes para aprovechar valores actualizados en el momento
        for i = 1:n
            suma = b(i);
            for j = 1:n
                if j != i
                    suma = suma - A(i, j) * x(j);
                end
            end
            x(i) = suma / A(i, i);
        end
        
        if norm(x - x_prev, inf) < tol
            return;
        end
        x_prev = x;
    end
end

% Ejecución y comparación
[x_jac, it_jac] = jacobi_iterativo(A, b, x0, tol, max_iter);
[x_gs, it_gs] = gauss_seidel_iterativo(A, b, x0, tol, max_iter);

printf("Resultados de los Métodos Iterativos:\n");
printf("Jacobi:      Iteraciones: %d | Solución: [%s]\n", it_jac, sprintf(" %.4f", x_jac));
printf("Gauss-Seidel: Iteraciones: %d | Solución: [%s]\n", it_gs, sprintf(" %.4f", x_gs));
```

Gauss-Seidel requiere aproximadamente la mitad de iteraciones que Jacobi gracias a la retroalimentación inmediata de las incógnitas calculadas.

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Determinar si se garantiza la convergencia de los métodos de Jacobi y Gauss-Seidel para resolver el siguiente sistema:
$$\begin{cases} 
5x - y + 2z = 6 \\ 
x + 4y - z = 4 \\ 
2x + y - 6z = -3 
\end{cases}$$

**Solución analizando la Dominancia Diagonal:**
1.  **Escribir la matriz de coeficientes $A$**:
    $$A = \begin{pmatrix} 5 & -1 & 2 \\ 1 & 4 & -1 \\ 2 & 1 & -6 \end{pmatrix}$$
2.  **Comprobar la condición de dominancia diagonal estricta por filas**:
    *   **Fila 1**: $|a_{11}| = |5| = 5$.
        Suma de los demás elementos de la fila: $|a_{12}| + |a_{13}| = |-1| + |2| = 1 + 2 = 3$.
        Como $5 > 3$, se cumple para la fila 1.
    *   **Fila 2**: $|a_{22}| = |4| = 4$.
        Suma de los demás elementos: $|a_{21}| + |a_{23}| = |1| + |-1| = 1 + 1 = 2$.
        Como $4 > 2$, se cumple para la fila 2.
    *   **Fila 3**: $|a_{33}| = |-6| = 6$.
        Suma de los demás elementos: $|a_{31}| + |a_{32}| = |2| + |1| = 2 + 1 = 3$.
        Como $6 > 3$, se cumple para la fila 3.
3.  **Conclusión**:
    Dado que la matriz $A$ es estrictamente diagonalmente dominante por filas, la teoría matemática **garantiza de forma absoluta la convergencia** de los métodos de Jacobi y Gauss-Seidel para cualquier vector de partida inicial $x^{(0)}$.

---

## 7. Ejercicios Propuestos

1.  Dada la matriz de iteración de Jacobi:
    $$B = \begin{pmatrix} 0 & 0.5 \\ 0.5 & 0 \end{pmatrix}$$
    Calcular sus autovalores y determinar el radio espectral $\rho(B)$. ¿Garantiza esto la convergencia del método?
2.  Escribir analíticamente la primera iteración completa ($x^{(1)}$) del método de Jacobi y del método de Gauss-Seidel partiendo de $x^{(0)} = (0, 0, 0)$ para el sistema:
    $$\begin{cases} 
    3x + y = 4 \\ 
    x + 4y = 5 
    \end{cases}$$
3.  Explicar por qué los métodos iterativos resultan sumamente ventajosos para matrices de adyacencia de la Web (algoritmo PageRank) frente a la eliminación de Gauss directa.
    *(Pista: Considera la cantidad de elementos nulos y la conservación de la dispersión de la matriz).*
