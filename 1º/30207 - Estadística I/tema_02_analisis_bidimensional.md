# Tema 2: Análisis Exploratorio Bidimensional

En la práctica de la ingeniería de software y la ciencia de datos, rara vez analizamos variables aisladas. Con frecuencia nos interesa comprender la relación entre dos variables distintas; por ejemplo, la relación entre el uso de CPU ($X$) y el consumo de energía del servidor ($Y$), o el tamaño de la base de datos ($X$) y el tiempo de respuesta de las consultas ($Y$). El análisis exploratorio bidimensional proporciona las herramientas para estudiar estas dependencias.

---

## 1. Organización de Datos Bidimensionales: Tablas de Contingencia

Cuando estudiamos dos variables de forma simultánea, representamos cada observación como un par ordenado $(x_i, y_i)$. Para resumir la información de variables cualitativas o discretas con pocos valores, utilizamos una **tabla de contingencia** o de doble entrada.

Supongamos que la variable $X$ puede tomar $r$ valores distintos ($x_1, \dots, x_r$) y la variable $Y$ puede tomar $c$ valores distintos ($y_1, \dots, y_c$):

*   **Frecuencia Absoluta Conjunta ($n_{ij}$)**: Número de observaciones que presentan simultáneamente el valor $x_i$ de la variable $X$ y el valor $y_j$ de la variable $Y$.
*   **Frecuencia Marginal de $X$ ($n_{i\cdot}$)**: Frecuencia absoluta del valor $x_i$ sin importar el valor de $Y$:
    $$n_{i\cdot} = \sum_{j=1}^c n_{ij}$$
*   **Frecuencia Marginal de $Y$ ($n_{\cdot j}$)**: Frecuencia absoluta del valor $y_j$ sin importar el valor de $X$:
    $$n_{\cdot j} = \sum_{i=1}^r n_{ij}$$
*   **Frecuencia Relativa Condicionada**:
    *   De $Y$ dado que $X = x_i$: $f_{j|i} = \frac{n_{ij}}{n_{i\cdot}}$
    *   De $X$ dado que $Y = y_j$: $f_{i|j} = \frac{n_{ij}}{n_{\cdot j}}$

---

## 2. Covarianza: Dirección de la Relación

La **covarianza muestral** ($s_{xy}$) mide la dirección de la relación lineal entre dos variables cuantitativas:
$$s_{xy} = \frac{1}{n} \sum_{i=1}^n (x_i - \bar{x})(y_i - \bar{y})$$
Su fórmula simplificada de cálculo es:
$$s_{xy} = \left(\frac{1}{n} \sum_{i=1}^n x_i y_i\right) - \bar{x}\bar{y}$$

*   **Intuición Física**:
    *   Si cuando $X$ está por encima de su media ($\bar{x}$), $Y$ también tiende a estar por encima de la suya ($\bar{y}$), el producto $(x_i - \bar{x})(y_i - \bar{y})$ será positivo.
    *   Si cuando $X$ es inferior a su media, $Y$ también es inferior, el producto de dos números negativos vuelve a ser **positivo**.
    *   Por tanto, si las variables varían en la misma dirección, la covarianza es **positiva** ($s_{xy} > 0$).
    *   Si varían en direcciones opuestas, la mayoría de los productos serán negativos y la covarianza será **negativa** ($s_{xy} < 0$).
    *   Si no hay una tendencia clara, los términos positivos y negativos se cancelarán mutuamente y la covarianza se aproximará a **cero**.
*   **Limitación**: Al igual que la varianza, sus unidades son el producto de las unidades de $X$ e $Y$ (por ejemplo, $\text{núcleos} \times \text{vatios}$), lo que impide calibrar la fuerza o intensidad de dicha relación.

---

## 3. Coeficiente de Correlación Lineal de Pearson: Intensidad de la Relación

Para normalizar la covarianza y eliminar el efecto de las unidades y la escala, dividimos la covarianza por el producto de las desviaciones típicas de ambas variables. Se define el coeficiente de correlación lineal de Pearson ($r$):
$$r = \frac{s_{xy}}{s_x s_y}$$

*   **Propiedades clave**:
    *   Está acotado matemáticamente: $-1 \le r \le 1$.
    *   **$r = 1$**: Correlación lineal positiva perfecta. Todos los puntos se sitúan exactamente sobre una recta con pendiente positiva.
    *   **$r = -1$**: Correlación lineal negativa perfecta. Los puntos se alinean sobre una recta con pendiente negativa.
    *   **$r \approx 0$**: Ausencia de relación lineal. Las variables pueden ser independientes o estar relacionadas mediante una función no lineal (por ejemplo, cuadrática).
*   **Regla de Oro**: *"Correlación no implica causalidad"*. El hecho de que dos variables tengan un coeficiente de correlación cercano a 1 no significa que una sea la causa de la otra; puede existir una tercera variable oculta (confusora) que influya sobre ambas.

---

## 4. Representación Gráfica: Diagrama de Dispersión

El **diagrama de dispersión** (o *scatterplot*) es una gráfica bidimensional donde cada observación se representa como un punto en un sistema de coordenadas cartesianas, con $X$ en el eje horizontal e $Y$ en el eje vertical. Es la mejor herramienta visual para detectar a golpe de vista la existencia de tendencias lineales, no lineales, dispersión y presencia de valores atípicos bidimensionales.

---

## 5. Independencia Estadística

Se dice que dos variables $X$ e $Y$ son estadísticamente independientes si la distribución de una de ellas no se ve alterada por los valores que toma la otra. En términos de frecuencias en la tabla de contingencia, se cumple la condición de independencia si para todas las celdas $(i, j)$:
$$n_{ij} = \frac{n_{i\cdot} n_{\cdot j}}{n} \quad \text{o bien} \quad f_{ij} = f_{i\cdot} f_{\cdot j}$$
Si esta igualdad se viola significativamente en alguna de las celdas, las variables presentan algún grado de asociación o dependencia.

---

## 6. Práctica Bidimensional en R

R nos permite realizar todo el flujo de análisis exploratorio bidimensional de forma nativa:

```R
# Muestra de datos: uso de CPU (%) y consumo del servidor (Vatios)
uso_cpu <- c(10, 25, 45, 50, 60, 75, 90, 95)
consumo <- c(120, 135, 140, 160, 175, 190, 210, 220)

# 1. Calcular la covarianza muestral
cov_muestral <- cov(uso_cpu, consumo) # Retorna 1121.429

# 2. Calcular el coeficiente de correlación de Pearson
correlacion <- cor(uso_cpu, consumo) # Retorna 0.9904 (Correlación casi perfecta)

# 3. Dibujar el diagrama de dispersión
plot(uso_cpu, consumo, 
     main="Relación CPU vs Consumo", 
     xlab="Uso de CPU (%)", 
     ylab="Consumo Energético (W)", 
     pch=19, col="blue")

# 4. Tablas de contingencia en R (para variables cualitativas)
# Supongamos tipos de fallos de red (Física, Lógica) en dos zonas (A, B)
tipo_fallo <- c("Física", "Lógica", "Lógica", "Física", "Física")
zona <- c("A", "A", "B", "B", "A")

# Crear la tabla de contingencia
tabla <- table(tipo_fallo, zona)
print(tabla)

# Obtener frecuencias relativas marginales y conjuntas
prop.table(tabla) # Frecuencias relativas conjuntas
```

---

## 7. Ejercicios Resueltos

### Ejercicio 1
Se miden dos variables en un clúster de servidores: $X$ = "Número de peticiones HTTP concurrentes (en cientos)" e $Y$ = "Uso de memoria RAM (en GB)". Se dispone de las siguientes observaciones para 5 intervalos de tiempo: $\{(1, 2), (2, 3), (3, 5), (4, 4), (5, 6)\}$.
1. Calcular la covarianza de la muestra.
2. Calcular el coeficiente de correlación lineal de Pearson e interpretar el resultado.

**Solución**:
1.  **Cálculo de la covarianza**:
    *   Tamaño de la muestra $n = 5$.
    *   Medias muestrales:
        $$\bar{x} = \frac{1 + 2 + 3 + 4 + 5}{5} = 3$$
        $$\bar{y} = \frac{2 + 3 + 5 + 4 + 6}{5} = 4$$
    *   Productos $x_i y_i$:
        $$\sum_{i=1}^5 x_i y_i = (1 \times 2) + (2 \times 3) + (3 \times 5) + (4 \times 4) + (5 \times 6) = 2 + 6 + 15 + 16 + 30 = 69$$
    *   Covarianza muestral ($s_{xy}$):
        $$s_{xy} = \left(\frac{1}{n} \sum x_i y_i\right) - \bar{x}\bar{y} = \frac{69}{5} - (3 \times 4) = 13.8 - 12 = 1.8$$
        *Dado que $s_{xy} = 1.8 > 0$, existe una relación lineal directa (positiva) entre el número de peticiones concurrentes y el uso de RAM.*

2.  **Coeficiente de Correlación de Pearson ($r$)**:
    *   Necesitamos calcular primero las desviaciones típicas $s_x$ y $s_y$.
    *   Varianza de $X$:
        $$\sum x_i^2 = 1^2 + 2^2 + 3^2 + 4^2 + 5^2 = 55$$
        $$s_x^2 = \frac{55}{5} - 3^2 = 11 - 9 = 2 \implies s_x = \sqrt{2} \approx 1.414$$
    *   Varianza de $Y$:
        $$\sum y_i^2 = 2^2 + 3^2 + 5^2 + 4^2 + 6^2 = 4 + 9 + 25 + 16 + 36 = 90$$
        $$s_y^2 = \frac{90}{5} - 4^2 = 18 - 16 = 2 \implies s_y = \sqrt{2} \approx 1.414$$
    *   Coeficiente $r$:
        $$r = \frac{s_{xy}}{s_x s_y} = \frac{1.8}{\sqrt{2} \sqrt{2}} = \frac{1.8}{2} = 0.90$$
    *   **Interpretación**: El valor de $r = 0.90$ indica una fuerte correlación lineal positiva entre el volumen de peticiones concurrentes y el consumo de RAM. A medida que aumenta la carga de peticiones, el uso de memoria RAM se incrementa de forma muy aproximada a una línea recta.
