# Tema 5: Variables Aleatorias Unidimensionales

En lugar de trabajar directamente con descripciones cualitativas de sucesos (como "cara", "cruz" o "paquete perdido"), resulta más conveniente traducir los resultados de un experimento aleatorio a números. Una variable aleatoria realiza precisamente esta traducción, actuando como un puente matemático entre los sucesos del mundo real y el análisis algebraico.

---

## 1. Concepto de Variable Aleatoria

Una **variable aleatoria (V.A.)** $X$ es una función que asocia a cada resultado elemental de un espacio muestral $\Omega$ un número real único:
$$X: \Omega \to \mathbb{R}$$
Clasificamos las variables aleatorias en dos grandes categorías según la naturaleza de su conjunto de valores posibles: **discretas** y **continuas**.

---

## 2. Variables Aleatorias Discretas

Una V.A. es discreta si toma un número finito o infinito numerable de valores aislados (por ejemplo, el número de peticiones web concurrentes en un servidor o el número de reintentos de conexión).

### A. Función de Probabilidad (o de Masa)
Asocia a cada valor posible $x_i$ la probabilidad exacta de que la variable tome dicho valor:
$$p(x_i) = P(X = x_i)$$
Cumple necesariamente con las siguientes propiedades:
1.  $p(x_i) \ge 0$ para todo $i$.
2.  $\sum_{i} p(x_i) = 1$ (la suma de probabilidades de todos los valores posibles es 1).

### B. Función de Distribución Acumulada ($F(x)$)
Representa la probabilidad de que la variable tome un valor menor o igual a $x$:
$$F(x) = P(X \le x) = \sum_{x_i \le x} p(x_i)$$
Es una función escalonada, no decreciente, que salta en cada punto $x_i$ que tiene probabilidad no nula, cumpliendo que $\lim_{x \to -\infty} F(x) = 0$ y $\lim_{x \to \infty} F(x) = 1$.

---

## 3. Variables Aleatorias Continuas

Una V.A. es continua si puede tomar cualquier valor dentro de un intervalo real (por ejemplo, el tiempo de ejecución de una consulta SQL o la temperatura de funcionamiento de una CPU).

### A. Función de Densidad ($f(x)$)
A diferencia de las discretas, una variable continua no tiene probabilidad puntual: **$P(X = x) = 0$ para cualquier valor exacto $x$**. En su lugar, definimos la **función de densidad** $f(x)$, que describe la concentración relativa de probabilidad en cada punto de la recta real. Cumple:
1.  $f(x) \ge 0$ para todo $x \in \mathbb{R}$.
2.  $\int_{-\infty}^{\infty} f(x) \, dx = 1$ (el área total bajo la curva de densidad es 1).

La probabilidad de que la variable se encuentre en un intervalo de valores es el área bajo la curva en ese rango:
$$P(a \le X \le b) = \int_a^b f(x) \, dx$$

### B. Función de Distribución Acumulada ($F(x)$)
Al igual que en las discretas, acumula la probabilidad desde el extremo izquierdo hasta el punto $x$:
$$F(x) = P(X \le x) = \int_{-\infty}^x f(t) \, dt$$
Por el Teorema Fundamental del Cálculo, se cumple que la densidad es la derivada de la distribución: $f(x) = \frac{d}{dx}F(x)$.

---

## 4. Esperanza Matemática y Varianza: Los Momentos de la V.A.

Para caracterizar una V.A. sin necesidad de ver toda su distribución, calculamos sus valores promedio teóricos (parámetros poblacionales).

### Esperanza Matemática ($E[X]$ o $\mu$)
Es el valor promedio que esperaríamos observar si repitiéramos el experimento infinitas veces. Actúa como el centro de gravedad teórico de la distribución.

*   **Caso Discreto**: $E[X] = \sum_{i} x_i \cdot p(x_i)$
*   **Caso Continuo**: $E[X] = \int_{-\infty}^{\infty} x \cdot f(x) \, dx$

**Propiedades de la Esperanza**:
*   Es un operador lineal: $E[aX + b] = aE[X] + b$ para cualquier par de constantes $a, b$.
*   Si $X$ e $Y$ son independientes, entonces $E[X \cdot Y] = E[X] \cdot E[Y]$.

### Varianza ($Var(X)$ o $\sigma^2$)
Mide la dispersión o variabilidad de los valores respecto a su esperanza teórica:
$$Var(X) = E[(X - E[X])^2] = E[X^2] - (E[X])^2$$

*   **Caso Discreto**: $E[X^2] = \sum_{i} x_i^2 \cdot p(x_i)$
*   **Caso Continuo**: $E[X^2] = \int_{-\infty}^{\infty} x^2 \cdot f(x) \, dx$

**Propiedades de la Varianza**:
*   $Var(aX + b) = a^2 Var(X)$ (las traslaciones no afectan a la dispersión, y los escalados afectan al cuadrado).
*   La desviación típica poblacional es $\sigma = \sqrt{Var(X)}$.

---

## 5. Simulación de Variables Aleatorias en R

R permite trabajar de forma integrada con funciones de distribución (`p`), densidad (`d`) y generación de números aleatorios (`r`):

```R
# Supongamos una variable aleatoria continua que mide el tiempo de respuesta (en ms)
# y sigue una distribución conocida con densidad dada en R.
# R tiene funciones específicas para cada distribución (ej. Normal, Exponencial).

# Ejemplo con una variable normal teórica de media 100 y desviación típica 15:
# 1. P(X <= 120): Función de distribución acumulada (pnorm)
prob_menor_120 <- pnorm(120, mean = 100, sd = 15)
print(prob_menor_120) # Retorna ~0.9087

# 2. Generar 500 simulaciones de esta variable aleatoria (rnorm)
simulaciones <- rnorm(500, mean = 100, sd = 15)

# 3. Calcular la esperanza muestral (debe ser cercana a 100)
mean(simulaciones)

# 4. Calcular la varianza muestral (debe ser cercana a 15^2 = 225)
var(simulaciones)
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1 (Discreta)
El número de consultas de bases de datos fallidas por minuto en un servidor ($X$) tiene la siguiente función de probabilidad:

| $x_i$ | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| $p(x_i)$ | 0.6 | 0.2 | 0.1 | 0.1 |

1. Calcular la función de distribución acumulada $F(x)$.
2. Calcular la esperanza matemática $E[X]$ e interpretar el resultado.
3. Calcular la varianza $Var(X)$ y la desviación típica $\sigma$.

**Solución**:
1.  **Función de Distribución $F(x)$**:
    $$F(x) = \begin{cases} 
      0 & \text{si } x < 0 \\ 
      0.6 & \text{si } 0 \le x < 1 \\ 
      0.8 & \text{si } 1 \le x < 2 \\ 
      0.9 & \text{si } 2 \le x < 3 \\ 
      1.0 & \text{si } x \ge 3 
   \end{cases}$$

2.  **Esperanza Matemática $E[X]$**:
    $$E[X] = \sum x_i \cdot p(x_i) = (0 \times 0.6) + (1 \times 0.2) + (2 \times 0.1) + (3 \times 0.1) = 0 + 0.2 + 0.2 + 0.3 = 0.7$$
    *Interpretación: A largo plazo, el servidor promedia 0.7 consultas fallidas por minuto.*

3.  **Varianza $Var(X)$**:
    Primero calculamos el segundo momento $E[X^2]$:
    $$E[X^2] = \sum x_i^2 \cdot p(x_i) = (0^2 \times 0.6) + (1^2 \times 0.2) + (2^2 \times 0.1) + (3^2 \times 0.1) = 0 + 0.2 + 0.4 + 0.9 = 1.5$$
    Ahora restamos el cuadrado de la esperanza:
    $$Var(X) = E[X^2] - (E[X])^2 = 1.5 - (0.7)^2 = 1.5 - 0.49 = 1.01$$
    La desviación típica es:
    $$\sigma = \sqrt{1.01} \approx 1.005\text{ fallos/minuto}$$
