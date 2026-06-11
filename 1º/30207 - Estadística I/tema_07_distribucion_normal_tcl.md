# Tema 7: La Distribución Normal y el Teorema Central del Límite

La distribución Normal (o Gaussiana) es la distribución de probabilidad más importante de la estadística. Además de modelar multitud de fenómenos reales (como el ruido de señal en electrónica o las estaturas humanas), sirve como el puente fundamental hacia la inferencia estadística gracias al Teorema Central del Límite.

---

## 1. La Distribución Normal ($X \sim N(\mu, \sigma)$)

Una variable aleatoria continua $X$ sigue una distribución Normal de parámetros $\mu$ (media) y $\sigma$ (desviación típica) si su función de densidad es:
$$f(x) = \frac{1}{\sigma \sqrt{2\pi}} e^{-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^2} \quad \text{para } x \in \mathbb{R}$$

### Propiedades de la Campana de Gauss
*   **Simetría**: Es perfectamente simétrica respecto a su media $\mu$. Esto significa que $f(\mu - x) = f(\mu + x)$.
*   **Medidas de centralización**: La media, la mediana y la moda coinciden exactamente en el valor $\mu$.
*   **Regla Empírica del Sigmas (68-95-99)**:
    *   El $68.27\%$ del área bajo la curva se encuentra a menos de 1 desviación típica de la media: $P(\mu - \sigma \le X \le \mu + \sigma) \approx 0.683$.
    *   El $95.45\%$ se encuentra a menos de 2 desviaciones típicas: $P(\mu - 2\sigma \le X \le \mu + 2\sigma) \approx 0.954$.
    *   El $99.73\%$ se encuentra a menos de 3 desviaciones típicas: $P(\mu - 3\sigma \le X \le \mu + 3\sigma) \approx 0.997$.

```
                 Campana de Gauss (Simetría)
                           
                            |
                         *  *  *
                      *     |     *
                    *       |       *
                  *         |         *
                *   68%     |   68%     *
              *             |             *
             *  <-  \sigma  |  \sigma  ->  *
   ---------*---------------|---------------*---------
                           \mu
```

---

## 2. Tipificación y Uso de Tablas ($Z \sim N(0, 1)$)

La distribución normal con $\mu = 0$ y $\sigma = 1$ se denomina **Normal Estándar** y se denota habitualmente por la variable $Z$. Sus probabilidades acumuladas están tabuladas en las famosas "tablas de la Normal".

### Fórmula de Tipificación
Para calcular probabilidades de cualquier variable $X \sim N(\mu, \sigma)$, la transformamos en una normal estándar $Z$ mediante la operación:
$$Z = \frac{X - \mu}{\sigma}$$

Por ejemplo:
$$P(X \le x) = P\left( \frac{X-\mu}{\sigma} \le \frac{x-\mu}{\sigma} \right) = P(Z \le z) = \Phi(z)$$
donde $\Phi(z)$ es la función de distribución acumulada de la normal estándar.

### Reglas de Simetría para Buscar en Tablas
Dado que las tablas suelen mostrar únicamente valores de $P(Z \le z)$ para $z \ge 0$, debemos usar las propiedades de simetría para buscar otras probabilidades:

1.  **Probabilidad de valor negativo**:
    $$P(Z \le -z) = P(Z \ge z) = 1 - P(Z \le z)$$
2.  **Probabilidad en un intervalo**:
    $$P(a \le Z \le b) = P(Z \le b) - P(Z \le a)$$
3.  **Probabilidad de mayor que**:
    $$P(Z \ge z) = 1 - P(Z \le z)$$

---

## 3. El Teorema Central del Límite (TCL)

El Teorema Central del Límite es uno de los resultados más potentes de las matemáticas. Establece que:

Sean $X_1, X_2, \dots, X_n$ variables aleatorias independientes e idénticamente distribuidas (i.i.d.) procedentes de **cualquier distribución** (incluso discretas o muy asimétricas), que posea una media $\mu$ y una varianza $\sigma^2$ finitas. A medida que el tamaño muestral $n$ crece (típicamente $n \ge 30$), la distribución de la **media muestral** $\bar{X}$ se aproxima a una distribución Normal:

$$\bar{X} \approx N\left( \mu, \frac{\sigma}{\sqrt{n}} \right)$$

De igual modo, la **suma muestral** $S_n = \sum X_i$ se aproxima a una normal:
$$S_n \approx N(n\mu, \sigma\sqrt{n})$$

*   **Importancia en Ingeniería**: Nos permite aproximar el comportamiento de la media de un conjunto de mediciones sin necesidad de conocer la distribución original de la población.

---

## 4. Aproximación de Distribuciones Discretas por la Normal

Bajo ciertas condiciones, podemos aproximar distribuciones discretas mediante la Normal, facilitando el cálculo en muestras grandes donde los factoriales o potencias son inmanejables.

### A. Aproximación de Binomial por Normal
Si $X \sim B(n, p)$ con $np \ge 5$ y $n(1-p) \ge 5$, podemos aproximar $X$ mediante una variable continua $Y \sim N(\mu = np, \sigma = \sqrt{np(1-p)})$.

#### Corrección por Continuidad (o de Yates)
Como estamos aproximando una distribución discreta (donde los valores son puntos) por una continua (donde los puntos tienen probabilidad cero), debemos ajustar los límites sumando o restando $0.5$:
*   $P(X \le k) \approx P(Y \le k + 0.5)$
*   $P(X \ge k) \approx P(Y \ge k - 0.5)$
*   $P(X = k) \approx P(k - 0.5 \le Y \le k + 0.5)$

### B. Aproximación de Poisson por Normal
Si $X \sim P(\lambda)$ con $\lambda \ge 10$, podemos aproximar $X$ mediante una variable continua $Y \sim N(\mu = \lambda, \sigma = \sqrt{\lambda})$, aplicando la misma corrección de continuidad.

---

## 5. Práctica con R

R calcula probabilidades normales exactas eliminando la necesidad de recurrir a las tablas físicas:

```R
# 1. P(Z <= 1.96) para una Normal Estándar
pnorm(1.96) # Retorna ~0.9750

# 2. P(X <= 85) en una Normal con media 100 y desv. típica 15
pnorm(85, mean = 100, sd = 15) # Retorna ~0.1586

# 3. Encontrar el valor critico z tal que P(Z <= z) = 0.95 (Cuantil 95)
qnorm(0.95) # Retorna 1.6448

# 4. Simulación del TCL
# Generamos 1000 medias muestrales, cada una calculada sobre n=40 datos exponenciales (muy asimétricos)
medias_muestrales <- replicate(1000, mean(rexp(40, rate = 2)))

# Dibujar el histograma de las medias (se verá acampanado y simétrico)
hist(medias_muestrales, breaks=30, prob=TRUE, 
     main="Demostración visual del TCL", col="lightblue", xlab="Media Muestral")
# Superponer la curva normal teórica
curve(dnorm(x, mean=0.5, sd=0.5/sqrt(40)), add=TRUE, col="red", lwd=2)
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1 (Tipificación)
El tiempo de respuesta de un servidor web sigue una distribución normal con media $\mu = 150$ milisegundos y desviación típica $\sigma = 20$ ms.
1. Calcular la probabilidad de que una petición tarde menos de 130 ms.
2. Calcular la probabilidad de que una petición tarde entre 140 y 170 ms.

**Solución**:
Sea $X$ = "Tiempo de respuesta en ms", con $X \sim N(150, 20)$.
1.  **Probabilidad de tardar menos de 130 ms ($P(X \le 130)$)**:
    Tipificamos el valor 130:
    $$z = \frac{130 - 150}{20} = \frac{-20}{20} = -1.00$$
    Buscamos en la normal estándar usando la simetría:
    $$P(X \le 130) = P(Z \le -1.00) = 1 - P(Z \le 1.00) = 1 - 0.8413 = 0.1587$$
    *(Existe un 15.87% de probabilidad de que tarde menos de 130 ms).*

2.  **Probabilidad en el intervalo $[140, 170]$**:
    Tipificamos ambos extremos:
    $$z_{1} = \frac{140 - 150}{20} = -0.50 \quad \text{y} \quad z_{2} = \frac{170 - 150}{20} = 1.00$$
    Planteamos la probabilidad acumulada:
    $$P(140 \le X \le 170) = P(-0.50 \le Z \le 1.00) = P(Z \le 1.00) - P(Z \le -0.50)$$
    $$P(Z \le -0.50) = 1 - P(Z \le 0.50) = 1 - 0.6915 = 0.3085$$
    $$P(140 \le X \le 170) = 0.8413 - 0.3085 = 0.5328$$
    *(Hay una probabilidad del 53.28% de que el tiempo de respuesta esté en ese intervalo).*

### Ejercicio 2 (TCL)
El peso de los paquetes de datos transmitidos en una red tiene una media de 500 bytes y una desviación típica de 80 bytes. Si seleccionamos una muestra aleatoria de 100 paquetes de forma independiente, calcular la probabilidad de que el peso medio de la muestra sea superior a 510 bytes.

**Solución**:
Sea $X_i$ el peso de cada paquete, con media $\mu = 500$ y $\sigma = 80$. La distribución de $X_i$ es desconocida, pero como el tamaño de la muestra es grande ($n = 100 \ge 30$), aplicamos el Teorema Central del Límite:
La media muestral $\bar{X}$ sigue una distribución Normal aproximada:
$$\bar{X} \approx N\left( \mu, \frac{\sigma}{\sqrt{n}} \right) = N\left( 500, \frac{80}{\sqrt{100}} \right) = N\left( 500, 8 \right)$$
Queremos calcular $P(\bar{X} > 510)$:
Tipificamos el valor 510 para nuestra variable $\bar{X}$ con desviación típica $\sigma_{\bar{X}} = 8$:
$$z = \frac{510 - 500}{8} = \frac{10}{8} = 1.25$$
Calculamos la probabilidad:
$$P(\bar{X} > 510) = P(Z > 1.25) = 1 - P(Z \le 1.25) = 1 - 0.8944 = 0.1056$$
*(Hay una probabilidad de 10.56% de que la media del peso de los 100 paquetes exceda los 510 bytes).*
