# Tema 8: Teoría de Muestras y Estimación Puntual

Hasta ahora hemos estudiado la probabilidad: asumiendo que conocemos los parámetros de una población (como su media o su varianza), deducimos qué datos es probable observar. La inferencia estadística realiza el viaje inverso: a partir de los datos concretos observados en una muestra, intentamos estimar o deducir los parámetros desconocidos de la población.

---

## 1. Muestreo Aleatorio Simple y Estadísticos

*   **Población**: Conjunto total de elementos bajo estudio. Su distribución de probabilidad depende de ciertos parámetros desconocidos ($\theta$, como la media poblacional $\mu$ o la varianza $\sigma^2$).
*   **Muestra Aleatoria Simple (M.A.S.)**: Selección de $n$ variables aleatorias $X_1, X_2, \dots, X_n$ independientes e idénticamente distribuidas (i.i.d.), cada una con la misma distribución que la población original.
*   **Estadístico**: Cualquier función calculada exclusivamente sobre los elementos de la muestra, y que por tanto es en sí misma una variable aleatoria con su propia distribución (llamada **distribución muestral**).

### Principales Estadísticos Muestrales
Para estimar los parámetros reales de la población, utilizamos los siguientes estadísticos:

1.  **Media Muestral ($\bar{X}$)**: Estimador de la media poblacional $\mu$.
    $$\bar{X} = \frac{1}{n} \sum_{i=1}^n X_i$$
2.  **Varianza Muestral ($S^2$)**:
    $$S^2 = \frac{1}{n} \sum_{i=1}^n (X_i - \bar{X})^2$$
3.  **Cuasivarianza Muestral ($S_{n-1}^2$ o $S^2$ corregida)**: Estimador corregido de la varianza poblacional $\sigma^2$.
    $$S_{n-1}^2 = \frac{1}{n-1} \sum_{i=1}^n (X_i - \bar{X})^2$$
4.  **Proporción Muestral ($\hat{p}$)**: Para variables cualitativas dicotómicas, estimador de la proporción poblacional $p$.
    $$\hat{p} = \frac{1}{n} \sum_{i=1}^n X_i \quad \text{donde } X_i \in \{0, 1\}$$

---

## 2. Propiedades de los Estimadores Puntuales

Un **estimador** $\hat{\theta}$ es un estadístico diseñado para aproximar el valor de un parámetro poblacional $\theta$. Para evaluar si un estimador es adecuado, estudiamos sus propiedades teóricas:

### A. Sesgo (Insesgadez)
El sesgo se define como la diferencia entre la esperanza matemática del estimador y el valor real del parámetro:
$$\text{Sesgo}(\hat{\theta}) = E[\hat{\theta}] - \theta$$
Un estimador es **insesgado** (o centrado) si su sesgo es cero, es decir:
$$E[\hat{\theta}] = \theta$$
*   **El porqué de dividir por $n-1$**: Si calculamos la esperanza de la varianza muestral $S^2$ obtenemos $E[S^2] = \frac{n-1}{n}\sigma^2$. La varianza muestral tiene sesgo (subestima la dispersión real). En cambio, al corregirla dividiendo por $n-1$, la cuasivarianza cumple $E[S_{n-1}^2] = \sigma^2$, convirtiéndose en un estimador insesgado.

### B. Eficiencia
Entre dos estimadores insesgados $\hat{\theta}_1$ y $\hat{\theta}_2$ de un mismo parámetro, el primero es más eficiente si tiene menor varianza:
$$Var(\hat{\theta}_1) < Var(\hat{\theta}_2)$$
Un estimador con menor varianza se desviará menos del valor real entre diferentes muestras.

### C. Consistencia
Un estimador es consistente si su valor converge en probabilidad al parámetro real a medida que el tamaño de la muestra tiende a infinito ($n \to \infty$). Es decir, el estimador se vuelve "perfecto" si disponemos de infinitos datos.

---

## 3. Método de Máxima Verosimilitud (MLE)

El método de Máxima Verosimilitud (MLE, por sus siglas en inglés) es la técnica más importante para construir estimadores puntuales óptimos.

*   **Intuición**: Si hemos observado una muestra concreta de datos, ¿qué valor de los parámetros poblacionales ($\theta$) haría que dicha muestra fuera la más probable de haber ocurrido? Buscamos maximizar esa "credibilidad" o verosimilitud.

### Pasos para el Cálculo del MLE:
1.  **Función de Verosimilitud ($L(\theta)$)**: Es la probabilidad conjunta de los datos observados expresada como función del parámetro $\theta$. Debido a la independencia del muestreo (m.a.s.), es el producto de las funciones de probabilidad/densidad individuales:
    $$L(\theta) = \prod_{i=1}^n f(x_i; \theta)$$
2.  **Función de Log-Verosimilitud ($\ln L(\theta)$)**: Trabajar con productos y sus derivadas es complejo. Como el logaritmo neperiano es una función estrictamente creciente, maximizar $L(\theta)$ equivale a maximizar $\ln L(\theta)$, lo que transforma los productos en sumas sencillas:
    $$\ln L(\theta) = \sum_{i=1}^n \ln f(x_i; \theta)$$
3.  **Derivación y Maximización**: Derivamos respecto a $\theta$, igualamos a cero para encontrar el máximo crítico, y resolvemos para despejar $\theta$:
    $$\frac{d}{d\theta} \ln L(\theta) = 0 \implies \hat{\theta}_{\text{MLE}}$$

---

## 4. Práctica de Estimación Puntual en R

En R podemos calcular los estadísticos de una muestra usando funciones de resumen. Asimismo, R cuenta con librerías para estimar parámetros por MLE numéricamente (como `fitdistrplus`).

```R
# Muestra de tiempos de renderizado de frames (en segundos)
frames <- c(1.2, 1.5, 0.9, 1.4, 2.1, 1.3, 1.7)

# 1. Estimación de la media poblacional (mu)
mean(frames) # Retorna 1.443

# 2. Estimación de la varianza poblacional (sigma^2) usando la cuasivarianza
var(frames) # Retorna 0.1495

# 3. Desviación típica (cuasidesviación)
sd(frames) # Retorna 0.3866

# MLE numérico en R:
# Si asumimos que los datos siguen una distribución Exponencial con tasa lambda
# R tiene librerías como fitdistrplus, o podemos calcularlo directamente usando
# el estimador analítico de MLE de la exponencial (lambda = 1/media)
lambda_mle <- 1 / mean(frames)
print(lambda_mle) # Retorna ~0.693
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1 (Cálculo Analítico de MLE)
Sea $X$ una variable aleatoria que sigue una distribución de Poisson de parámetro $\lambda$ desconocido, cuya función de probabilidad es $P(X = x) = \frac{e^{-\lambda}\lambda^x}{x!}$. Dada una muestra aleatoria simple $x_1, x_2, \dots, x_n$:
1. Obtener analíticamente el estimador de máxima verosimilitud de $\lambda$.

**Solución**:
1.  **Construir la función de verosimilitud $L(\lambda)$**:
    $$L(\lambda) = \prod_{i=1}^n \frac{e^{-\lambda} \lambda^{x_i}}{x_i!}$$
2.  **Tomar logaritmos para obtener la log-verosimilitud $\ln L(\lambda)$**:
    $$\ln L(\lambda) = \ln \left( \prod_{i=1}^n \frac{e^{-\lambda} \lambda^{x_i}}{x_i!} \right) = \sum_{i=1}^n \ln \left( \frac{e^{-\lambda} \lambda^{x_i}}{x_i!} \right)$$
    Apliando propiedades de logaritmos:
    $$\ln L(\lambda) = \sum_{i=1}^n \left[ -\lambda + x_i \ln(\lambda) - \ln(x_i!) \right] = -n\lambda + \ln(\lambda)\sum_{i=1}^n x_i - \sum_{i=1}^n \ln(x_i!)$$
3.  **Derivar con respecto a $\lambda$**:
    $$\frac{d}{d\lambda} \ln L(\lambda) = -n + \frac{1}{\lambda}\sum_{i=1}^n x_i$$
4.  **Igualar a cero para hallar el máximo**:
    $$-n + \frac{1}{\hat{\lambda}}\sum_{i=1}^n x_i = 0 \implies n = \frac{1}{\hat{\lambda}}\sum_{i=1}^n x_i \implies \hat{\lambda} = \frac{1}{n}\sum_{i=1}^n x_i = \bar{x}$$
    *Conclusión: El estimador de máxima verosimilitud para la tasa $\lambda$ de una distribución de Poisson es la propia media muestral $\bar{X}$.*
    *(Nota: Para verificar que es un máximo, la segunda derivada es $-\frac{1}{\lambda^2}\sum x_i < 0$, lo que confirma la existencia de un máximo).*
