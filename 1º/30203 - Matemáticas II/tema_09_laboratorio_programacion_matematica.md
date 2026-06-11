# Tema 9: Laboratorio de Programación Matemática

En el ámbito científico e industrial de la computación, el desarrollo rápido de algoritmos lineales y numéricos se realiza con lenguajes de alto nivel orientados a matrices como **Matlab** o **GNU Octave**. Este laboratorio enseña la sintaxis fundamental, el concepto crítico de la **vectorización de código** (esencial para maximizar la velocidad en CPU/GPU) y la resolución por software de los bloques algebraicos y numéricos estudiados previamente.

---

## 1. Sintaxis Básica de Matlab/Octave

Matlab y Octave tratan a cada variable como una matriz (por defecto de coma flotante de doble precisión).

### 1.1 Creación de Matrices y Vectores
*   **Vectores**:
    ```octave
    v_fila = [1, 2, 3];
    v_columna = [1; 2; 3];
    v_rango = 1:0.5:3; % Genera [1.0, 1.5, 2.0, 2.5, 3.0]
    v_lin = linspace(0, 1, 5); % Genera 5 puntos entre 0 y 1
    ```
*   **Matrices**:
    ```octave
    A = [1, 2; 3, 4];
    I = eye(3);      % Matriz Identidad de 3x3
    Z = zeros(2, 3);  % Matriz de ceros de 2x3
    O = ones(4, 1);   % Vector de unos de 4x1
    R = rand(3, 3);   % Matriz aleatoria uniforme de 3x3
    ```

### 1.2 Indexación y Slicing (Selección de Rangos)
La indexación en Matlab/Octave es **1-indexed** (comienza en 1, no en 0):
```octave
A(1, 2)    % Elemento de la fila 1, columna 2
A(2, :)    % Fila 2 completa
A(:, 3)    % Columna 3 completa
A(1:2, 2:3) % Submatriz formada por las filas 1-2 y columnas 2-3
```

---

## 2. Operaciones Elemento a Elemento vs Operaciones Matriciales

Un error común al programar en Matlab/Octave es confundir las operaciones algebraicas de matrices con las operaciones aritméticas elemento a elemento.

*   **Multiplicación Matricial Algebraica (`*`)**: Realiza el producto interno de filas por columnas. Requiere que las dimensiones coincidan ($m \times p$ con $p \times n$).
    ```octave
    C = A * B;
    ```
*   **Multiplicación Elemento a Elemento (`.*`)**: Multiplica cada componente de $A$ por la correspondiente de $B$. Ambas deben tener el mismo tamaño exacto.
    ```octave
    C = A .* B;
    ```
*   **División y Potenciación Elemento a Elemento (`./`, `.^`)**:
    ```octave
    C = A ./ B;  % Cada elemento A(i,j) dividido por B(i,j)
    D = A .^ 2;  % Eleva cada componente de A al cuadrado
    ```

---

## 3. Vectorización de Código

La **vectorización** consiste en reescribir un algoritmo iterativo basado en bucles (`for` o `while`) mediante operaciones matriciales vectoriales compactas. 

### Por qué es crucial la vectorización
Octave y Matlab son lenguajes interpretados. Ejecutar un bucle `for` con millones de iteraciones introduce un gran coste de interpretación paso a paso en la CPU. 
Al utilizar expresiones vectorizadas, las operaciones se delegan a librerías compiladas de bajo nivel (BLAS y LAPACK) escritas en C/C++ y Fortran, las cuales están altamente optimizadas a nivel de registros de procesador y aprovechan instrucciones de hardware de tipo **SIMD** (Single Instruction, Multiple Data) y paralelismo multinúcleo.

---

## 4. El Toque Informático: Simulación y Benchmarking de Vectorización

A continuación, implementamos una prueba de rendimiento (benchmark) para contrastar el coste temporal de procesar una señal matemática (evaluar $y_i = \sin(x_i) \cdot x_i^2$) mediante un bucle `for` tradicional frente al enfoque vectorizado.

```octave
% Número de puntos de la simulación (1 millón de elementos)
N = 1000000;
x = linspace(-10, 10, N);

% 1. Enfoque No Vectorizado (Bucle For)
tic; % Iniciar cronómetro
y_for = zeros(1, N);
for i = 1:N
    y_for(i) = sin(x(i)) * (x(i)^2);
end
tiempo_for = toc; % Detener cronómetro

% 2. Enfoque Vectorizado (Operaciones con el operador punto '.')
tic;
y_vec = sin(x) .* (x .^ 2);
tiempo_vec = toc;

printf("Estudio de Rendimiento (Evaluación de %d elementos):\n", N);
printf("Tiempo con Bucle For: %.6f segundos\n", tiempo_for);
printf("Tiempo Vectorizado:  %.6f segundos\n", tiempo_vec);
printf("Factor de aceleración (Speedup): %.1f x\n", tiempo_for / tiempo_vec);

% Comprobación de que ambos métodos dan el mismo resultado exacto
max_diff = max(abs(y_for - y_vec));
printf("Diferencia máxima entre resultados: %.2e\n", max_diff);
```

La diferencia de rendimiento suele superar las **50 veces de aceleración** a favor de la vectorización, ilustrando por qué los informáticos deben evitar los bucles anidados en computación científica.

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Escribir un script de Matlab/Octave que:
1.  Genere una matriz aleatoria cuadrada $A$ de tamaño $4 \times 4$.
2.  Calcule su determinante. Si es invertible, resuelva el sistema $A x = b$ para $b = [1; 2; 3; 4]$ usando el operador backslash.
3.  Calcule la descomposición LU de la matriz y verifique que $P \cdot A - L \cdot U$ es nulo.

**Solución (Script de Octave):**
```octave
% 1. Generar la matriz aleatoria
A = rand(4, 4);
b = [1; 2; 3; 4];

% 2. Evaluar determinante y resolver
det_A = det(A);
printf("Determinante de A: %.6f\n", det_A);

if abs(det_A) > 1e-9
    x = A \ b;
    printf("El sistema es compatible determinado. Solución x:\n");
    disp(x);
else
    printf("La matriz es singular. No se puede resolver de forma única.\n");
end

% 3. Descomposición LU y verificación
[L, U, P] = lu(A);
verificacion = P * A - L * U;
error_max = max(max(abs(verificacion)));

printf("Error máximo en la reconstrucción P*A - L*U: %.2e\n", error_max);
```

---

## 6. Ejercicios Propuestos

1.  Crear una matriz $M$ de tamaño $5 \times 5$ con números enteros del 1 al 25 de forma compacta (usando rangos y la función `reshape`). Seleccionar la submatriz central de tamaño $3 \times 3$.
2.  Implementar una función de Octave que reciba una matriz $A$ y verifique si es simétrica ($A = A^T$) y definida positiva (comprobando que todos sus autovalores sean estrictamente positivos).
3.  Vectorizar el siguiente fragmento de código ineficiente:
    ```octave
    n = 10000;
    A = rand(n, 1);
    B = rand(n, 1);
    resultado = 0;
    for i = 1:n
        resultado = resultado + A(i) * B(i);
    end
    ```
    *(Pista: Considera el producto escalar o la multiplicación de matrices de vectores traspuestos).*
