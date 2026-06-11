# Tema 6: Ortogonalidad

La ortogonalidad extiende el concepto geométrico de perpendicularidad a espacios abstractos de dimensión general. En ingeniería informática, los conceptos de ortogonalidad sustentan la regresión por mínimos cuadrados para ajustar curvas a datos experimentales (Machine Learning), la compresión de señales digitales (transformadas de Fourier y coseno discretas) y las métricas de distancia en motores de búsqueda (similitud de coseno).

---

## 1. Producto Escalar, Norma y Distancia

Sea $V$ un espacio vectorial sobre el cuerpo $\mathbb{R}$.

### Producto Escalar
Un **producto escalar** en $V$ es una aplicación que asocia a cada par de vectores $u, v \in V$ un número real, denotado por $\langle u, v \rangle$ (o $u \cdot v$), que satisface las siguientes propiedades para todo $u, v, w \in V$ y $\lambda \in \mathbb{R}$:
1.  **Simetría**: $\langle u, v \rangle = \langle v, u \rangle$
2.  **Linealidad en la primera componente**: $\langle \lambda u + \mu v, w \rangle = \lambda \langle u, w \rangle + \mu \langle v, w \rangle$
3.  **Definida positiva**: $\langle v, v \rangle \ge 0$, y $\langle v, v \rangle = 0 \iff v = 0_V$.

Un espacio vectorial real dotado de un producto escalar se denomina **espacio euclídeo**.

### Norma y Distancia
*   Definimos la **norma** (longitud) de un vector como:
    $$\|v\| = \sqrt{\langle v, v \rangle}$$
*   Definimos la **distancia** entre dos vectores como:
    $$d(u, v) = \|u - v\|$$
*   Definimos el **ángulo** $\theta$ entre dos vectores no nulos a partir de la relación:
    $$\cos\theta = \frac{\langle u, v \rangle}{\|u\| \cdot \|v\|}$$

---

## 2. Ortogonalidad y Proyección Ortogonal

Dos vectores $u$ y $v$ son **ortogonales** (perpendiculares) si su producto escalar es nulo:
$$u \perp v \iff \langle u, v \rangle = 0$$

Un conjunto de vectores $S = \{v_1, \dots, v_r\}$ es **ortogonal** si todos sus vectores son mutuamente ortogonales ($v_i \cdot v_j = 0$ para todo $i \ne j$). Si además cada vector tiene norma unitaria ($\|v_i\| = 1$), el conjunto es **ortonormal**.

### Proyección Ortogonal sobre un Subespacio
Sea $W$ un subespacio de $V$ con una base ortonormal $\{u_1, u_2, \dots, u_k\}$. Para cualquier vector $v \in V$, definimos su **proyección ortogonal sobre $W$**, denotada por $\text{proy}_W(v)$, como:
$$\text{proy}_W(v) = \langle v, u_1 \rangle u_1 + \langle v, u_2 \rangle u_2 + \dots + \langle v, u_k \rangle u_k$$

Geométricamente, $\text{proy}_W(v)$ es el vector en $W$ que está a la **mínima distancia** de $v$.

```
         v ^
           | \
           |  \  v - proy_W(v) (Ortogonal a W)
           |   \
  ---------+---->----------- W
           0    proy_W(v)
```

---

## 3. Proceso de Ortogonalización de Gram-Schmidt

Dado un conjunto de vectores linealmente independientes $\{v_1, v_2, \dots, v_n\}$ que forma una base del espacio $V$, el **Método de Gram-Schmidt** construye de forma constructiva una base ortogonal $\{u_1, u_2, \dots, u_n\}$ del mismo espacio:

1.  $$u_1 = v_1$$
2.  $$u_2 = v_2 - \frac{\langle v_2, u_1 \rangle}{\langle u_1, u_1 \rangle} u_1$$
3.  $$u_3 = v_3 - \frac{\langle v_3, u_1 \rangle}{\langle u_1, u_1 \rangle} u_1 - \frac{\langle v_3, u_2 \rangle}{\langle u_2, u_2 \rangle} u_2$$
4.  En general, para el paso $k$:
    $$u_k = v_k - \sum_{i=1}^{k-1} \frac{\langle v_k, u_i \rangle}{\langle u_i, u_i \rangle} u_i$$

Para obtener una base **ortonormal** $\{q_1, \dots, q_n\}$, normalizamos cada uno de los vectores resultantes:
$$q_i = \frac{u_i}{\|u_i\|}$$

---

## 4. El Toque Informático

### 4.1 Motores de Recomendación (Métrica de Similitud de Coseno)
En sistemas de filtrado colaborativo (como los de Netflix o Amazon) y en recuperación de información, comparamos perfiles de usuarios o documentos. Estos perfiles se representan como vectores de características de alta dimensión.
Para medir la similitud semántica sin importar el tamaño del documento o el número total de interacciones del usuario, calculamos el **coseno del ángulo entre los vectores** (Similitud de Coseno):
$$\text{Similitud}(A, B) = \cos\theta = \frac{A \cdot B}{\|A\| \cdot \|B\|}$$
*   Si $\cos\theta = 1$: Los perfiles son idénticos en dirección (misma proporción de gustos).
*   Si $\cos\theta = 0$: Los perfiles son ortogonales (no comparten intereses).

### 4.2 Ajuste por Mínimos Cuadrados (Regresión Lineal)
Cuando queremos ajustar un modelo predictivo lineal $y = X\beta$ a datos ruidosos, el sistema de ecuaciones suele ser incompatible (más ecuaciones que incógnitas).
La mejor aproximación en sentido geométrico consiste en proyectar ortogonalmente el vector de datos observados $y$ sobre el subespacio imagen de la matriz $X$. Esto se resuelve analíticamente mediante las **Ecuaciones Normales**:
$$X^T X \beta = X^T y \implies \beta = (X^T X)^{-1} X^T y$$

A continuación, implementamos en Matlab/Octave el método de Gram-Schmidt para construir una base ortonormal.

```octave
% Definimos una base original V de R^3
V = [1,  1,  0;
     1,  0,  1;
     0,  1,  1]; % Cada columna es un vector v_i

n = size(V, 2);
U = zeros(size(V)); % Matriz para guardar la base ortogonal

% Algoritmo de Gram-Schmidt
for k = 1:n
    U(:, k) = V(:, k);
    for j = 1:(k-1)
        % Calculamos la proyección y la restamos
        proyeccion = (U(:, j)' * V(:, k)) / (U(:, j)' * U(:, j));
        U(:, k) = U(:, k) - proyeccion * U(:, j);
    end
end

% Normalización para obtener base ortonormal Q
Q = zeros(size(U));
for k = 1:n
    Q(:, k) = U(:, k) / norm(U(:, k));
end

printf("Base Original V:\n");
disp(V);
printf("Base Ortogonal calculada U:\n");
disp(U);
printf("Base Ortonormal final Q:\n");
disp(Q);

% Verificación de la ortonormalidad: Q^T * Q debe ser igual a la Identidad
identidad_aprox = Q' * Q;
printf("Verificación Q^T * Q:\n");
disp(identidad_aprox);
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Aplicar el método de Gram-Schmidt para ortogonalizar la base de $\mathbb{R}^2$ dada por los vectores $v_1 = (1, 1)$ y $v_2 = (1, 2)$ utilizando el producto escalar usual.

**Solución:**
1.  **Calcular el primer vector ortogonal $u_1$**:
    $$u_1 = v_1 = (1, 1)$$
2.  **Calcular el segundo vector ortogonal $u_2$**:
    $$u_2 = v_2 - \frac{\langle v_2, u_1 \rangle}{\langle u_1, u_1 \rangle} u_1$$
    Calculamos los productos escalares:
    *   $\langle v_2, u_1 \rangle = (1, 2) \cdot (1, 1) = 1 \cdot 1 + 2 \cdot 1 = 3$
    *   $\langle u_1, u_1 \rangle = (1, 1) \cdot (1, 1) = 1^2 + 1^2 = 2$
    Sustituimos en la fórmula:
    $$u_2 = (1, 2) - \frac{3}{2} (1, 1) = (1, 2) - \left(\frac{3}{2}, \frac{3}{2}\right) = \left(-\frac{1}{2}, \frac{1}{2}\right)$$
3.  **Comprobación**:
    $$u_1 \cdot u_2 = (1, 1) \cdot \left(-\frac{1}{2}, \frac{1}{2}\right) = -\frac{1}{2} + \frac{1}{2} = 0 \quad (\text{Correcto, son ortogonales})$$
4.  **Normalización para base ortonormal**:
    *   $\|u_1\| = \sqrt{2} \implies q_1 = \left(\frac{1}{\sqrt{2}}, \frac{1}{\sqrt{2}}\right)$
    *   $\|u_2\| = \sqrt{\left(-\frac{1}{2}\right)^2 + \left(\frac{1}{2}\right)^2} = \sqrt{\frac{1}{4} + \frac{1}{4}} = \sqrt{\frac{1}{2}} = \frac{1}{\sqrt{2}}$
        $$q_2 = \frac{u_2}{\|u_2\|} = \sqrt{2}\left(-\frac{1}{2}, \frac{1}{2}\right) = \left(-\frac{1}{\sqrt{2}}, \frac{1}{\sqrt{2}}\right)$$

La base ortogonal es $\{ (1, 1), (-1/2, 1/2) \}$ y la base ortonormal es $\{ (1/\sqrt{2}, 1/\sqrt{2}), (-1/\sqrt{2}, 1/\sqrt{2}) \}$.

---

## 6. Ejercicios Propuestos

1.  Dados los vectores de características en un motor de búsqueda:
    $$A = (1, 2, 0, 1), \quad B = (2, 1, 1, 0)$$
    Calcular su similitud de coseno y explicar cuantitativamente qué tan similares son semánticamente.
2.  Hallar la proyección ortogonal del vector $v = (1, 5)$ sobre la recta (subespacio) generada por el vector $u = (3, 4)$ en $\mathbb{R}^2$.
3.  Demostrar algebraicamente que si un conjunto de vectores no nulos $\{v_1, \dots, v_k\}$ es ortogonal, entonces es linealmente independiente.
    *(Pista: Plantea la ecuación de combinación lineal a cero y multiplica escalarmente por cada vector del conjunto).*
