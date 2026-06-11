# Tema 3: Espacios Vectoriales

El concepto de espacio vectorial formaliza la idea de colecciones de objetos (vectores) que se pueden sumar entre sí y multiplicar por números (escalares), preservando propiedades de linealidad. En ingeniería informática, los espacios vectoriales son la base de la representación de datos multidimensionales (embeddings de texto en NLP, características en visión artificial, bases de datos vectoriales) y sustentan la compresión y el procesamiento digital de la información.

---

## 1. Definición y Axiomas del Espacio Vectorial

Sea $\mathbb{K}$ un cuerpo. Un **espacio vectorial** $V$ sobre $\mathbb{K}$ (denotado por $(V, +, \cdot_{\mathbb{K}})$) es un conjunto no vacío de elementos llamados **vectores**, equipado con dos operaciones:
*   **Suma de vectores**: $+ : V \times V \to V$
*   **Multiplicación por un escalar**: $\cdot : \mathbb{K} \times V \to V$

Para que $V$ sea un espacio vectorial, deben cumplirse los 8 axiomas fundamentales:
1.  **Asociatividad de la suma**: $u + (v + w) = (u + v) + w \quad \forall u, v, w \in V$.
2.  **Conmutatividad de la suma**: $u + v = v + u \quad \forall u, v \in V$.
3.  **Elemento neutro de la suma**: Existe $0_V \in V$ tal que $v + 0_V = v \quad \forall v \in V$.
4.  **Elemento opuesto (simétrico)**: Para cada $v \in V$, existe $-v \in V$ tal que $v + (-v) = 0_V$.
5.  **Distributividad del escalar**: $\lambda(u + v) = \lambda u + \lambda v \quad \forall \lambda \in \mathbb{K}, u, v \in V$.
6.  **Distributividad del vector**: $(\lambda + \mu)v = \lambda v + \mu v \quad \forall \lambda, \mu \in \mathbb{K}, v \in V$.
7.  **Compatibilidad de escalares**: $\lambda(\mu v) = (\lambda\mu)v \quad \forall \lambda, \mu \in \mathbb{K}, v \in V$.
8.  **Elemento unitario**: $1_{\mathbb{K}} \cdot v = v \quad \forall v \in V$.

### Subespacio Vectorial
Un subconjunto no vacío $W \subseteq V$ es un **subespacio vectorial** de $V$ si es en sí mismo un espacio vectorial bajo las operaciones de $V$.
> [!TIP]
> **Criterio de caracterización de subespacios:**
> $W \subseteq V$ es un subespacio si y solo si es cerrado bajo combinaciones lineales:
> $$\lambda u + \mu v \in W \quad \forall \lambda, \mu \in \mathbb{K}, \quad \forall u, v \in W$$

---

## 2. Dependencia Lineal, Bases y Dimensión

### 2.1 Combinación Lineal
Un vector $v \in V$ es una **combinación lineal** de los vectores $v_1, v_2, \dots, v_r \in V$ si existen escalares $\lambda_1, \lambda_2, \dots, \lambda_r \in \mathbb{K}$ tales que:
$$v = \lambda_1 v_1 + \lambda_2 v_2 + \dots + \lambda_r v_r$$

El conjunto de todas las combinaciones lineales de un conjunto de vectores se llama **subespacio generado** y se denota por $\text{lin}(v_1, \dots, v_r)$ o $\langle v_1, \dots, v_r \rangle$.

### 2.2 Independencia Lineal
Decimos que un conjunto de vectores $\{v_1, v_2, \dots, v_r\}$ es **linealmente independiente** (L.I.) si la única combinación lineal que da como resultado el vector nulo $0_V$ es aquella donde todos los escalares son cero:
$$\lambda_1 v_1 + \lambda_2 v_2 + \dots + \lambda_r v_r = 0_V \implies \lambda_1 = \lambda_2 = \dots = \lambda_r = 0$$
Si existe alguna solución donde no todos los escalares sean cero, los vectores son **linealmente dependientes** (L.D.).

### 2.3 Bases y Dimensión
Un conjunto de vectores $B = \{e_1, e_2, \dots, e_n\} \subset V$ es una **base** del espacio vectorial $V$ si cumple dos condiciones simultáneamente:
1.  Es linealmente independiente.
2.  Es un sistema generador de $V$ ($V = \langle e_1, \dots, e_n \rangle$).

La **dimensión** de $V$, denotada por $\dim(V)$, es el número de vectores que forman cualquiera de sus bases. Todos los vectores de una base dada tienen un único conjunto de coeficientes en $\mathbb{K}$, conocidos como **coordenadas** del vector respecto a dicha base.

---

## 3. El Toque Informático

### Espacios Vectoriales en Inteligencia Artificial y embeddings
En el campo del Procesamiento del Lenguaje Natural (NLP) y de la Ciencia de Datos, los datos se mapean a vectores en espacios de alta dimensión. Los **embeddings** de palabras (por ejemplo, generados por Word2Vec, GloVe o transformadores modernos de LLMs) asignan a cada palabra un vector en un espacio vectorial continuo (habitualmente $\mathbb{R}^{300}$ o $\mathbb{R}^{768}$).
En este espacio:
*   Las relaciones semánticas corresponden a operaciones algebraicas. La famosa ecuación:
    $$\vec{v}_{\text{rey}} - \vec{v}_{\text{hombre}} + \vec{v}_{\text{mujer}} \approx \vec{v}_{\text{reina}}$$
    se realiza en el espacio vectorial $\mathbb{R}^d$.
*   La independencia lineal es crucial: si un subconjunto de características de datos (features) es linealmente dependiente, significa que hay redundancia absoluta de datos (multicolinealidad), lo cual desestabiliza los algoritmos de entrenamiento.

A continuación, implementamos en Matlab/Octave un script para verificar si un conjunto de vectores es linealmente independiente y calcular las coordenadas de un vector respecto a una base determinada.

```octave
% Definimos una base B del espacio vectorial R^3
e1 = [1; 0; 1];
e2 = [1; 1; 0];
e3 = [0; 1; 1];

% Formamos la matriz de la base colocando los vectores como columnas
B = [e1, e2, e3];

% 1. Verificar si B es una base (independencia lineal y dimension 3)
% Si det(B) != 0, los vectores son L.I. y forman una base de R^3
det_B = det(B);
printf("Determinante de la matriz de vectores: %.4f\n", det_B);

if abs(det_B) > 1e-9
    printf("El conjunto de vectores es Linealmente Independiente (L.I.) y forma una Base.\n");
else
    printf("Los vectores son Linealmente Dependientes (L.D.). No forman una base.\n");
end

% 2. Dado un vector v en la base canónica, calcular sus coordenadas en la base B
v = [4; 3; 5];

% Buscamos los coeficientes c = [c1; c2; c3] tales que:
% c1*e1 + c2*e2 + c3*e3 = v  ===>  B * c = v
coordenadas_c = B \ v;

printf("\nVector v (en base canónica):\n");
disp(v);
printf("Coordenadas del vector v en la base B (c1, c2, c3):\n");
disp(coordenadas_c);

% Verificación de la reconstrucción del vector v
v_reconstruido = B * coordenadas_c;
printf("Vector reconstruido (B * c):\n");
disp(v_reconstruido);
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Determinar si el conjunto de vectores $W = \{(x, y, z) \in \mathbb{R}^3 : 2x - y + z = 0\}$ es un subespacio vectorial de $\mathbb{R}^3$. Si lo es, hallar una base y su dimensión.

**Solución:**
1.  **Demostrar que es un subespacio usando el criterio característico**:
    *   **Paso 1**: Comprobar si contiene al vector nulo.
        $$2(0) - (0) + (0) = 0 \implies (0, 0, 0) \in W \quad (\text{No es vacío})$$
    *   **Paso 2**: Tomamos dos vectores arbitrarios de $W$, $u = (x_1, y_1, z_1)$ y $v = (x_2, y_2, z_2)$, y dos escalares $\lambda, \mu \in \mathbb{R}$.
        Debemos verificar si $w = \lambda u + \mu v = (\lambda x_1 + \mu x_2, \lambda y_1 + \mu y_2, \lambda z_1 + \mu z_2) \in W$.
        Evaluamos la condición de pertenencia para $w$:
        $$2(\lambda x_1 + \mu x_2) - (\lambda y_1 + \mu y_2) + (\lambda z_1 + \mu z_2)$$
        Reordenamos asociando los coeficientes $\lambda$ y $\mu$:
        $$\lambda (2x_1 - y_1 + z_1) + \mu (2x_2 - y_2 + z_2)$$
        Dado que $u, v \in W$, sabemos que $2x_1 - y_1 + z_1 = 0$ y $2x_2 - y_2 + z_2 = 0$. Sustituyendo:
        $$\lambda (0) + \mu (0) = 0$$
        Por tanto, la combinación lineal pertenece a $W$, demostrando que **$W$ es un subespacio vectorial** de $\mathbb{R}^3$.
2.  **Hallar una base de $W$**:
    Despejamos una variable de la ecuación del subespacio, por ejemplo $y$:
    $$y = 2x + z$$
    Cualquier vector $(x, y, z) \in W$ puede escribirse sustituyendo $y$:
    $$(x, 2x+z, z) = (x, 2x, 0) + (0, z, z) = x(1, 2, 0) + z(0, 1, 1)$$
    Los vectores $v_1 = (1, 2, 0)$ y $v_2 = (0, 1, 1)$ generan $W$. Como no son proporcionales, son linealmente independientes.
    Por lo tanto, una base de $W$ es:
    $$B_W = \{(1, 2, 0), (0, 1, 1)\}$$
3.  **Dimensión**:
    El número de vectores de la base es 2, por tanto, $\dim(W) = 2$.

---

## 5. Ejercicios Propuestos

1.  Determinar si los siguientes vectores de $\mathbb{R}^3$ son linealmente independientes o dependientes:
    $$u_1 = (1, 1, 1), \quad u_2 = (2, 1, 0), \quad u_3 = (0, 1, 2)$$
2.  Sea $P_2[x]$ el espacio de los polinomios de grado menor o igual a 2. Demostrar que el conjunto de polinomios $B = \{1, 1+x, x+x^2\}$ es una base de $P_2[x]$, y hallar las coordenadas del polinomio $p(x) = 2 - 3x + x^2$ respecto a dicha base.
3.  Hallar la dimensión y una base del subespacio intersección $U \cap V$ en $\mathbb{R}^4$, donde:
    $$U = \{(x, y, z, t) \in \mathbb{R}^4 : x + y + z = 0\}, \quad V = \{(x, y, z, t) \in \mathbb{R}^4 : z - t = 0\}$$
