# Tema 4: Aplicaciones Lineales

Las aplicaciones lineales (o transformaciones lineales) describen mapeos entre espacios vectoriales que preservan la estructura de suma de vectores y multiplicación escalar. En ingeniería informática, las aplicaciones lineales son las herramientas fundamentales para manipular coordenadas en gráficos computacionales 3D (motores de renderizado de videojuegos y CAD), procesamiento de señales lineales y transformadas geométricas en visión artificial.

---

## 1. Definición y Propiedades de las Aplicaciones Lineales

Sean $V$ y $W$ espacios vectoriales sobre el mismo cuerpo $\mathbb{K}$. Una aplicación $f: V \to W$ es una **aplicación lineal** (o transformación lineal) si satisface las dos condiciones siguientes para todo $u, v \in V$ y todo $\lambda \in \mathbb{K}$:
1.  **Aditividad**: $f(u + v) = f(u) + f(v)$
2.  **Homogeneidad**: $f(\lambda u) = \lambda f(u)$

Estas dos propiedades pueden resumirse en una única condición de preservación de combinaciones lineales:
$$f(\lambda u + \mu v) = \lambda f(u) + \mu f(v) \quad \forall \lambda, \mu \in \mathbb{K}, \quad \forall u, v \in V$$

Propiedades inmediatas:
*   $f(0_V) = 0_W$
*   $f(-u) = -f(u)$

---

## 2. Núcleo (Kernel) e Imagen

Asociados a cualquier aplicación lineal $f: V \to W$, definimos dos subespacios vectoriales críticos que caracterizan el comportamiento geométrico y algebraico de la transformación.

### 2.1 Núcleo (Kernel)
El **núcleo** de $f$, denotado por $\ker(f)$ o $\text{Nuc}(f)$, es el conjunto de todos los vectores en el espacio de partida $V$ que se mapean al vector nulo del espacio de llegada $W$:
$$\ker(f) = \{v \in V : f(v) = 0_W\}$$
*   $\ker(f)$ es un subespacio vectorial de $V$.
*   **Monomorfismo (Inyectiva)**: $f$ es inyectiva si y solo si su núcleo contiene únicamente al vector nulo: $\ker(f) = \{0_V\}$.

### 2.2 Imagen
La **imagen** de $f$, denotada por $\text{Im}(f)$, es el conjunto de todos los vectores en el espacio de llegada $W$ que provienen de al menos un vector de $V$:
$$\text{Im}(f) = \{w \in W : \exists v \in V \text{ tal que } f(v) = w\}$$
*   $\text{Im}(f)$ es un subespacio vectorial de $W$.
*   **Epimorfismo (Sobreyectiva)**: $f$ es sobreyectiva si y solo si su imagen coincide con todo el espacio de llegada: $\text{Im}(f) = W$.

### 2.3 Teorema de las Dimensiones (Teorema del Rango-Nulidad)
Si $V$ es de dimensión finita, entonces:
$$\dim(V) = \dim(\ker(f)) + \dim(\text{Im}(f))$$

---

## 3. Matriz Asociada y Cambio de Base

Cualquier aplicación lineal entre espacios de dimensión finita puede representarse mediante una multiplicación matricial.

### Matriz Asociada
Sean $B_V = \{v_1, \dots, v_n\}$ una base de $V$ y $B_W = \{w_1, \dots, w_m\}$ una base de W. La **matriz asociada a $f$ respecto a las bases $B_V$ y $B_W$**, denotada por $M(f)_{B_V}^{B_W}$ (o simplemente $A$), es la matriz de tamaño $m \times n$ cuyas columnas son las coordenadas en $B_W$ de las imágenes de los vectores de la base $B_V$:
$$A = \begin{pmatrix} [f(v_1)]_{B_W} & [f(v_2)]_{B_W} & \dots & [f(v_n)]_{B_W} \end{pmatrix}$$

El cálculo de la aplicación se reduce a una multiplicación de matriz por vector:
$$[f(v)]_{B_W} = A \cdot [v]_{B_V}$$

### Cambio de Base y Semejanza
Si cambiamos de bases en $V$ y $W$, la nueva matriz asociada $A'$ se calcula mediante la relación:
$$A' = Q^{-1} \cdot A \cdot P$$
donde $P$ es la matriz de cambio de base en el espacio de partida y $Q$ es la matriz de cambio de base en el espacio de llegada.

---

## 4. El Toque Informático

### Transformaciones y Coordenadas Homogéneas en Gráficos 3D
En gráficos por computadora (como OpenGL o DirectX), los objetos 3D se representan mediante vértices. Para animar u observar la escena desde una cámara, aplicamos transformaciones como traslaciones, rotaciones y escalados.

La rotación y el escalado son transformaciones lineales en $\mathbb{R}^3$, pero la traslación no lo es (ya que $f(v) = v + t$ no cumple $f(0) = 0$). Para unificar todas estas transformaciones en una sola multiplicación matricial que el hardware gráfico (GPU) pueda procesar en paralelo, los informáticos idearon las **Coordenadas Homogéneas**.
Se añade una cuarta coordenada ficticia $w = 1$ a cada vector 3D:
$$\vec{p} = \begin{pmatrix} x \\ y \\ z \\ 1 \end{pmatrix}$$
Una transformación afín completa (rotación + traslación) se aplica mediante una matriz de transformación de $4 \times 4$:
$$\begin{pmatrix} x' \\ y' \\ z' \\ 1 \end{pmatrix} = \begin{pmatrix} 
R_{11} & R_{12} & R_{13} & t_x \\ 
R_{21} & R_{22} & R_{23} & t_y \\ 
R_{31} & R_{32} & R_{33} & t_z \\ 
0 & 0 & 0 & 1 
\end{pmatrix} \begin{pmatrix} x \\ y \\ z \\ 1 \end{pmatrix}$$

A continuación, implementamos en Matlab/Octave una aplicación lineal de rotación 2D y la aplicamos sobre un conjunto de puntos de un objeto poligonal de forma vectorizada.

```octave
% 1. Definición del objeto poligonal en 2D (un cuadrado unitario)
% Puntos organizados como columnas: [x; y]
puntos = [0, 1, 1, 0, 0;  % Coordenadas X
          0, 0, 1, 1, 0]; % Coordenadas Y

% 2. Matriz de la transformación lineal de Rotación de ángulo theta
theta = pi / 4; % Rotación de 45 grados (en radianes)
R = [cos(theta), -sin(theta);
     sin(theta),  cos(theta)];

% 3. Aplicación de la transformación de forma vectorizada (multiplicación matricial)
puntos_rotados = R * puntos;

% Visualización gráfica
figure;
plot(puntos(1, :), puntos(2, :), 'b-o', 'LineWidth', 2, 'DisplayName', 'Original');
hold on;
plot(puntos_rotados(1, :), puntos_rotados(2, :), 'r-s', 'LineWidth', 2, 'DisplayName', 'Rotado 45°');
grid on;
axis equal;
xlabel('Eje X');
ylabel('Eje Y');
title('Transformación Lineal: Rotación 2D');
legend('show');
hold off;

% Imprimir las coordenadas transformadas
printf("Coordenadas Originales:\n");
disp(puntos);
printf("Coordenadas Rotadas:\n");
disp(puntos_rotados);
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Sea la aplicación lineal $f: \mathbb{R}^3 \to \mathbb{R}^2$ definida por:
$$f(x, y, z) = (x - 2y, 2x - 4y + z)$$
1.  Hallar la matriz asociada a $f$ respecto a las bases canónicas.
2.  Calcular una base y la dimensión de su núcleo $\ker(f)$.
3.  Determinar la dimensión de su imagen $\text{Im}(f)$.

**Solución:**
1.  **Matriz asociada respecto a bases canónicas $C_3$ y $C_2$**:
    Calculamos las imágenes de los vectores de la base canónica de $\mathbb{R}^3$:
    *   $f(1, 0, 0) = (1 - 2(0), 2(1) - 4(0) + 0) = (1, 2)$
    *   $f(0, 1, 0) = (0 - 2(1), 2(0) - 4(1) + 0) = (-2, -4)$
    *   $f(0, 0, 1) = (0 - 2(0), 2(0) - 4(0) + 1) = (0, 1)$
    
    Colocando estas imágenes como columnas de la matriz $A$:
    $$A = \begin{pmatrix} 1 & -2 & 0 \\ 2 & -4 & 1 \end{pmatrix}$$
2.  **Calcular una base y dimensión del núcleo**:
    El núcleo es el conjunto de vectores $(x, y, z)$ tales que $f(x, y, z) = (0, 0)$:
    $$\begin{cases} x - 2y = 0 \\ 2x - 4y + z = 0 \end{cases}$$
    Resolvemos el sistema. De la primera ecuación: $x = 2y$.
    Sustituyendo en la segunda: $2(2y) - 4y + z = 0 \implies 4y - 4y + z = 0 \implies z = 0$.
    Las soluciones son de la forma $(2y, y, 0) = y(2, 1, 0)$.
    Por tanto, una base del núcleo es:
    $$B_{\ker(f)} = \{(2, 1, 0)\}$$
    La dimensión del núcleo es $\dim(\ker(f)) = 1$.
3.  **Calcular dimensión de la imagen**:
    Aplicamos el Teorema de las Dimensiones:
    $$\dim(\mathbb{R}^3) = \dim(\ker(f)) + \dim(\text{Im}(f)) \implies 3 = 1 + \dim(\text{Im}(f)) \implies \dim(\text{Im}(f)) = 2$$
    *(Nota: Como la dimensión de la imagen es igual a la dimensión del espacio de llegada R^2, la aplicación es sobreyectiva).*

---

## 6. Ejercicios Propuestos

1.  Demostrar si la aplicación $g: \mathbb{R}^2 \to \mathbb{R}^2$ dada por $g(x, y) = (x^2, x + y)$ es lineal o no lo es.
2.  Dada la aplicación lineal de proyección $P: \mathbb{R}^3 \to \mathbb{R}^3$ con matriz asociada respecto a la base canónica:
    $$A = \begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 0 \end{pmatrix}$$
    Determinar su núcleo e imagen, y describir geométricamente el efecto físico de esta aplicación en el espacio tridimensional.
3.  Sea $f: \mathbb{R}^2 \to \mathbb{R}^2$ una aplicación lineal cuya matriz en la base canónica es $A = \begin{pmatrix} 1 & 2 \\ 2 & 1 \end{pmatrix}$. Calcular la matriz asociada a $f$ en la nueva base $B = \{(1, 1), (1, -1)\}$.
