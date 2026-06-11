# MANUAL COMPLETO DE ESTADÍSTICA I
### Grado en Ingeniería Informática - 1º Curso

Este documento unifica todos los temas del plan de estudio de estadística descriptiva, probabilidad, modelos de distribución, intervalos de confianza, contrastes de hipótesis y regresión lineal en un único manual para facilitar su lectura, impresión o conversión a formatos como PDF.

---

## Índice General del Manual

*   **Bloque 1: Estadística Descriptiva y Fundamentos de Probabilidad (Semanas 1 a 5)**
    *   [Tema 1: Análisis Exploratorio de Datos Unidimensionales e Introducción a R](#tema-1-análisis-exploratorio-de-datos-unidimensionales-e-introducción-a-r)
    *   [Tema 2: Análisis Exploratorio Bidimensional](#tema-2-análisis-exploratorio-bidimensional)
    *   [Tema 3: Teoría de la Probabilidad](#tema-3-teoría-de-la-probabilidad)
    *   [Tema 4: Teoremas Fundamentales de la Probabilidad](#tema-4-teoremas-fundamentales-de-la-probabilidad)
*   **Bloque 2: Variables Aleatorias y Modelos de Distribución (Semanas 6 a 10)**
    *   [Tema 5: Variables Aleatorias Unidimensionales](#tema-5-variables-aleatorias-unidimensionales)
    *   [Tema 6: Modelos de Distribución de Probabilidad](#tema-6-modelos-de-distribución-de-probabilidad)
    *   [Tema 7: La Distribución Normal y el Teorema Central del Límite](#tema-7-la-distribución-normal-y-el-teorema-central-del-límite)
*   **Bloque 3: Inferencia Estadística y Modelado (Semanas 11 a 15)**
    *   [Tema 8: Teoría de Muestras y Estimación Puntual](#tema-8-teoría-de-muestras-y-estimación-puntual)
    *   [Tema 9: Estimación por Intervalos de Confianza](#tema-9-estimación-por-intervalos-de-confianza)
    *   [Tema 10: Contrastes de Hipótesis Paramétricos](#tema-10-contrastes-de-hipótesis-paramétricos)
    *   [Tema 11: Contrastes No Paramétricos](#tema-11-contrastes-no-paramétricos)
    *   [Tema 12: El Modelo de Regresión Lineal Simple](#tema-12-el-modelo-de-regresión-lineal-simple)
*   **Secciones Finales**
    *   [Glosario de Términos](#glosario-de-términos)
    *   [Bibliografía Recomendada](#bibliografía-recomendada)

<div style="page-break-after: always;"></div>

# Tema 1: Análisis Exploratorio de Datos Unidimensionales e Introducción a R

El análisis exploratorio de datos (EDA, por sus siglas en inglés) es el primer paso en cualquier estudio estadístico. Su objetivo es resumir y describir las características esenciales de un conjunto de datos recopilados sobre una única variable, identificando patrones, anomalías y la estructura general de la información.

---

## 1. Conceptos Básicos y Tablas de Frecuencias

Dada una muestra de tamaño $n$ de una variable estadística $X$, denotamos los valores observados por $x_1, x_2, \dots, x_n$. Si agrupamos los datos en $k$ valores distintos o intervalos de clase, definimos:

*   **Frecuencia Absoluta ($n_i$)**: Número de veces que se repite el valor $x_i$ en la muestra. Se cumple que $\sum_{i=1}^k n_i = n$.
*   **Frecuencia Relativa ($f_i$)**: Proporción de la muestra que presenta el valor $x_i$. Se define como $f_i = \frac{n_i}{n}$. La suma de todas las frecuencias relativas es 1 ($\sum_{i=1}^k f_i = 1$).
*   **Frecuencia Absoluta Acumulada ($N_i$)**: Suma de las frecuencias absolutas de los valores menores o iguales a $x_i$. Es decir, $N_i = \sum_{j=1}^i n_j$.
*   **Frecuencia Relativa Acumulada ($F_i$)**: Proporción de observaciones menores o iguales a $x_i$. Es decir, $F_i = \frac{N_i}{n} = \sum_{j=1}^i f_j$.

### Ejemplo de Tabla de Frecuencias
Supongamos que medimos el número de núcleos de CPU utilizados en un servidor por 10 procesos diferentes: $\{2, 4, 2, 8, 4, 2, 4, 4, 8, 2\}$.

*   Muestra ordenada: $\{2, 2, 2, 2, 4, 4, 4, 4, 8, 8\}$. Tamaño muestral $n = 10$.

| Valor ($x_i$) | Frec. Absoluta ($n_i$) | Frec. Relativa ($f_i$) | Frec. Abs. Acumulada ($N_i$) | Frec. Rel. Acumulada ($F_i$) |
|:---:|:---:|:---:|:---:|:---:|
| **2** | 4 | 0.4 | 4 | 0.4 |
| **4** | 4 | 0.4 | 8 | 0.8 |
| **8** | 2 | 0.2 | 10 | 1.0 |

---

## 2. Medidas de Centralización: ¿Dónde está el centro?

Las medidas de centralización identifican un valor singular que actúa como el "centro" o representante de la distribución.

### A. Media Aritmética ($\bar{x}$)
Es el promedio aritmético de los datos. Se calcula como:
$$\bar{x} = \frac{1}{n} \sum_{i=1}^n x_i$$

*   **Intuición Física**: La media es el **centro de gravedad** de los datos. Si colocamos los datos sobre una regla graduada en sus respectivas posiciones, la media representa el punto de apoyo exacto en el que la regla se mantendría en equilibrio perfecto.
*   **Sensibilidad**: Es muy sensible a valores extremos (valores atípicos o *outliers*). Un solo valor extremadamente alto arrastrará la media significativamente.

### B. Mediana ($Me$)
Es el valor que ocupa la posición central una vez que los datos han sido ordenados de menor a mayor. Divide la distribución en dos partes con idéntico número de observaciones (50% por debajo y 50% por encima).
*   Si $n$ es impar, es el valor en la posición $\frac{n+1}{2}$.
*   Si $n$ es par, es la media aritmética de los valores en las posiciones $\frac{n}{2}$ y $\frac{n}{2} + 1$.
*   **Intuición**: Es una medida **robusta**, no se ve afectada por valores extremos. En informática, si analizamos la latencia de una red y el 99% de los paquetes tardan 10ms pero uno tarda 10000ms debido a una pérdida temporal, la mediana seguirá reflejando los 10ms, mientras que la media se inflará de forma poco realista.

### C. Moda ($Mo$)
Es el valor (o valores) con mayor frecuencia absoluta ($n_i$) en el conjunto de datos.
*   Una distribución puede tener una moda (unimodal), varias (bimodal, multimodal) o ninguna si todos los datos aparecen con la misma frecuencia.

---

## 3. Medidas de Posición: Cuartiles y Percentiles

Los cuantiles dividen los datos ordenados en intervalos de igual proporción de datos.

*   **Percentil $P_p$ (para $p \in (0, 100)$)**: Es el valor tal que el $p\%$ de los datos ordenados son menores o iguales a él.
*   **Cuartiles**: Dividen la muestra en 4 partes iguales (del 25% cada una):
    *   **Primer Cuartil ($Q_1$)**: Equivale al Percentil 25 ($P_{25}$).
    *   **Segundo Cuartil ($Q_2$)**: Equivale al Percentil 50 ($P_{50}$), que es la **Mediana**.
    *   **Tercer Cuartil ($Q_3$)**: Equivale al Percentil 75 ($P_{75}$).

---

## 4. Medidas de Dispersión: ¿Cómo de agrupados están los datos?

Las medidas de centralización no bastan. Dos muestras pueden tener la misma media pero distribuciones completamente distintas. Por ejemplo, las muestras $\{10, 10, 10\}$ y $\{0, 10, 20\}$ tienen media 10, pero la segunda muestra está mucho más dispersa.

### A. Rango o Recorrido ($R$)
Es la diferencia entre el valor máximo y el mínimo de la muestra: $R = x_{max} - x_{min}$. Es muy básico y sensible a outliers.

### B. Varianza Muestral ($s^2$)
Es la media de las desviaciones al cuadrado respecto a la media de la muestra:
$$s^2 = \frac{1}{n} \sum_{i=1}^n (x_i - \bar{x})^2$$
O su forma simplificada para el cálculo rápido:
$$s^2 = \left( \frac{1}{n} \sum_{i=1}^n x_i^2 \right) - \bar{x}^2$$

*   **Intuición Física**: Mide la "distancia promedio" a la que se encuentran las observaciones respecto a su centro de gravedad (la media). Elevamos al cuadrado por dos razones fundamentales:
    1.  Para que los desvíos negativos (datos por debajo de la media) y positivos (datos por encima) no se cancelen entre sí (la suma directa $\sum (x_i - \bar{x})$ siempre es 0).
    2.  Para penalizar de manera no lineal los valores que están muy alejados del centro.
*   **Unidades**: La varianza se mide en unidades al cuadrado (por ejemplo, si los datos son segundos, la varianza está en $\text{segundos}^2$), lo que dificulta su interpretación directa.

*Nota: En inferencia estadística, suele usarse la **cuasivarianza** ($s_{n-1}^2$), que divide por $n-1$ en lugar de por $n$, para obtener un estimador insesgado de la varianza poblacional.*
$$s_{n-1}^2 = \frac{1}{n-1} \sum_{i=1}^n (x_i - \bar{x})^2$$

### C. Desviación Típica o Estándar ($s$)
Es la raíz cuadrada positiva de la varianza:
$$s = \sqrt{s^2}$$
*   **Intuición**: Devuelve la medida de dispersión a las **mismas unidades originales** que los datos observados (segundos, metros, etc.), facilitando su interpretación directa junto a la media.

### D. Coeficiente de Variación ($CV$)
Mide la dispersión relativa de los datos respecto a la media. Es una medida adimensional (sin unidades), ideal para comparar la dispersión de dos variables con distintas escalas:
$$CV = \frac{s}{|\bar{x}|}$$
*   Si $CV < 0.3$, se suele interpretar que la distribución es homogénea y la media es muy representativa.

---

## 5. Medidas de Forma: Asimetría y Curtosis

*   **Asimetría (Skewness)**: Determina si los datos se distribuyen de forma simétrica en torno a la media.
    *   $As = 0$: Simétrica.
    *   $As > 0$: Asimetría positiva (cola derecha más larga, la media suele ser mayor que la mediana).
    *   $As < 0$: Asimetría negativa (cola izquierda más larga, la media es menor que la mediana).
*   **Curtosis o Apuntamiento (Kurtosis)**: Mide el grado de concentración de los datos en la zona central de la distribución comparada con la Normal.
    *   **Mesocúrtica** ($g_2 = 0$): Apuntamiento similar a la distribución Normal.
    *   **Leptocúrtica** ($g_2 > 0$): Muy apuntada y con colas pesadas (datos muy concentrados en el centro y extremos).
    *   **Platicúrtica** ($g_2 < 0$): Plana, datos muy dispersos.

---

## 6. Introducción Práctica a R

R es un entorno interactivo de línea de comandos. Los conceptos fundamentales para empezar son:

### Vectores y Variables
En R, la asignación se realiza mediante el operador `<-`. Los vectores se crean con la función combinadora `c()`.

```R
# Asignar un valor a una variable
n_procesos <- 10

# Crear un vector de datos (latencias de disco en milisegundos)
latencias <- c(12, 15, 11, 14, 115, 13, 16, 12, 14, 18)
```

### Estadísticos Básicos en R
R proporciona funciones directas para calcular las medidas descriptivas estudiadas:

```R
# Calcular la media
media_lat <- mean(latencias) # Retorna 24

# Calcular la mediana
mediana_lat <- median(latencias) # Retorna 14 (robusta al outlier de 115ms)

# Rango
rango_val <- range(latencias) # Retorna el mínimo y el máximo: c(11, 115)

# Desviación típica muestral (cuasidesviación en R)
desv_tipica <- sd(latencias)

# Varianza muestral (cuasivarianza en R)
varianza <- var(latencias)

# Resumen estadístico rápido (mínimo, primer cuartil, mediana, media, tercer cuartil y máximo)
summary(latencias)

# Percentil 90 (útil para SLAs en informática: el 90% de las peticiones tardan menos que este valor)
quantile(latencias, probs = 0.90)
```

### Gráficos Exploratorios
Para visualizar la distribución rápidamente en R:

```R
# Histograma
hist(latencias, main="Distribución de Latencias", xlab="milisegundos", col="skyblue")

# Diagrama de caja (Boxplot) para detectar outliers
boxplot(latencias, main="Diagrama de Caja de Latencias", ylab="ms", col="tomato")
```

---

## 7. Ejercicios Resueltos

### Ejercicio 1
Se analiza el tiempo de compilación (en segundos) de un software en 8 ejecuciones distintas: $\{12, 15, 9, 14, 35, 11, 13, 10\}$.
1. Calcular la media, mediana y desviación típica muestral (cuasidesviación típica).
2. Justificar qué medida de centralización (media o mediana) describe mejor el comportamiento habitual de la compilación.

**Solución**:
1.  **Cálculos manuales**:
    *   Muestra ordenada: $\{9, 10, 11, 12, 13, 14, 15, 35\}$. Tamaño $n = 8$.
    *   **Media**:
        $$\bar{x} = \frac{9 + 10 + 11 + 12 + 13 + 14 + 15 + 35}{8} = \frac{119}{8} = 14.875\text{ s}$$
    *   **Mediana**: Como $n = 8$ es par, tomamos la media de las posiciones centrales 4 y 5 (valores 12 y 13):
        $$Me = \frac{12 + 13}{2} = 12.5\text{ s}$$
    *   **Cuasivarianza ($s_{n-1}^2$)**:
        $$\sum x_i^2 = 9^2 + 10^2 + 11^2 + 12^2 + 13^2 + 14^2 + 15^2 + 35^2 = 2241$$
        $$s_{n-1}^2 = \frac{1}{n-1} \left( \sum x_i^2 - n\bar{x}^2 \right) = \frac{1}{7} \left( 2241 - 8(14.875)^2 \right) = \frac{1}{7}(2241 - 1770.125) = 67.268\text{ s}^2$$
    *   **Cuasidesviación típica ($s_{n-1}$)**:
        $$s_{n-1} = \sqrt{67.268} \approx 8.202\text{ s}$$
2.  **Justificación**: La mediana ($12.5\text{ s}$) describe mejor el comportamiento habitual. La muestra contiene un valor atípico alto ($35\text{ s}$) que distorsiona la media aritmética hacia arriba ($14.875\text{ s}$), situándola por encima de 7 de las 8 observaciones realizadas.


<div style="page-break-after: always;"></div>

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


<div style="page-break-after: always;"></div>

# Tema 3: Teoría de la Probabilidad

La probabilidad es la base matemática sobre la que se construye la inferencia estadística y el aprendizaje automático. Nos permite modelar y cuantificar la incertidumbre inherente a los sistemas físicos, de red y de software (como la tasa de pérdida de paquetes o la probabilidad de fallo de un servidor).

---

## 1. Experimento Aleatorio, Espacio Muestral y Sucesos

*   **Experimento Aleatorio**: Aquel cuyo resultado no se puede predecir con certeza antes de realizarse, aun cuando se repita bajo las mismas condiciones (por ejemplo, el tiempo exacto que tardará en completarse una petición de red).
*   **Espacio Muestral ($\Omega$)**: Conjunto de todos los resultados posibles del experimento aleatorio.
    *   *Ejemplo*: Si lanzamos una moneda tres veces, $\Omega = \{CCC, CCX, CXC, XCC, CXX, XCX, XXC, XXX\}$.
*   **Suceso (o Evento)**: Cualquier subconjunto del espacio muestral ($A \subseteq \Omega$).
    *   **Suceso Elemental**: Formado por un único resultado posible.
    *   **Suceso Compuesto**: Formado por varios resultados.
    *   **Suceso Seguro**: Coincide con $\Omega$. Siempre ocurre.
    *   **Suceso Imposible ($\emptyset$)**: No contiene ningún elemento. Nunca ocurre.

### Álgebra de Sucesos y Operaciones
Sean $A, B \subseteq \Omega$ dos sucesos:
1.  **Unión ($A \cup B$)**: Ocurre si ocurre al menos uno de los dos sucesos ($A$ o $B$).
2.  **Intersección ($A \cap B$)**: Ocurre si ocurren simultáneamente ambos sucesos ($A$ y $B$).
3.  **Contrario o Complementario ($\bar{A}$ o $A^c$)**: Ocurre si no ocurre $A$. Se cumple que $A \cup \bar{A} = \Omega$ y $A \cap \bar{A} = \emptyset$.
4.  **Sucesos Mutuamente Excluyentes o Incompatibles**: Si no pueden ocurrir a la vez, es decir, $A \cap B = \emptyset$.

**Leyes de De Morgan**:
*   $\overline{A \cup B} = \bar{A} \cap \bar{B}$
*   $\overline{A \cap B} = \bar{A} \cup \bar{B}$

---

## 2. Definición de Probabilidad

La probabilidad $P(A)$ es una medida numérica asignada a un suceso $A$ que cuantifica la verosimilitud de su ocurrencia.

### A. Definición Clásica (Regla de Laplace)
Si todos los resultados del espacio muestral son igualmente probables (equiprobables):
$$P(A) = \frac{\text{Casos Favorables a } A}{\text{Casos Posibles en } \Omega}$$

### B. Definición Frecuentista
Si repetimos un experimento $N$ veces en las mismas condiciones, y el suceso $A$ ocurre $n_A$ veces, su frecuencia relativa es $f_N(A) = \frac{n_A}{N}$. La probabilidad es el límite cuando el número de repeticiones tiende a infinito:
$$P(A) = \lim_{N \to \infty} \frac{n_A}{N}$$
*(Esta definición da origen a la **Ley de los Grandes Números**, que establece que a mayor número de pruebas, la frecuencia observada se estabiliza en torno al valor real de la probabilidad).*

### C. Definición Axiomática de Kolmogorov
Es la definición matemática formal. Una probabilidad $P$ es una función que asocia a cada suceso $A$ un número real tal que cumple tres axiomas básicos:
1.  **Axioma 1**: $P(A) \ge 0$ (la probabilidad no puede ser negativa).
2.  **Axioma 2**: $P(\Omega) = 1$ (la certeza absoluta tiene probabilidad 1).
3.  **Axioma 3**: Si $A_1, A_2, \dots$ son sucesos mutuamente excluyentes dos a dos ($A_i \cap A_j = \emptyset$ para todo $i \neq j$), entonces:
    $$P\left( \bigcup_{i=1}^\infty A_i \right) = \sum_{i=1}^\infty P(A_i)$$

### Propiedades Derivadas
A partir de estos axiomas se deducen teoremas fundamentales:
*   $P(\bar{A}) = 1 - P(A)$
*   $P(\emptyset) = 0$
*   Si $A \subseteq B \implies P(A) \le P(B)$
*   $P(A \cup B) = P(A) + P(B) - P(A \cap B)$

---

## 3. Probabilidad Condicionada e Independencia

La probabilidad condicionada modela cómo cambia la probabilidad de un suceso cuando disponemos de información previa sobre la ocurrencia de otro.

### Probabilidad Condicionada
La probabilidad de que ocurra el suceso $A$ sabiendo que ya ha ocurrido el suceso $B$ (con $P(B) > 0$) se define como:
$$P(A|B) = \frac{P(A \cap B)}{P(B)}$$

*   **Intuición**: Al saber que $B$ ha ocurrido, nuestro espacio de posibilidades se reduce. El nuevo "universo" de sucesos posibles ya no es $\Omega$, sino únicamente $B$. La probabilidad de $A$ dentro de este nuevo universo es la porción de $A$ que también pertenece a $B$ ($A \cap B$), escalada por la medida del nuevo universo ($P(B)$).

De la definición de probabilidad condicionada se deriva la regla de la multiplicación para la intersección:
$$P(A \cap B) = P(B) \cdot P(A|B) = P(A) \cdot P(B|A)$$

### Independencia de Sucesos
Dos sucesos $A$ y $B$ son estadísticamente independientes si el hecho de que ocurra uno no afecta la probabilidad de ocurrencia del otro. Es decir, si se cumple cualquiera de las siguientes condiciones equivalentes:
1.  $P(A|B) = P(A)$
2.  $P(B|A) = P(B)$
3.  $P(A \cap B) = P(A) \cdot P(B)$

---

## 4. Simulación Frecuentista en R

Podemos usar R para demostrar la Ley de los Grandes Números simulando lanzamientos de una moneda y observando cómo la frecuencia relativa de caras converge a 0.5 a medida que aumenta el número de lanzamientos:

```R
# Simular el lanzamiento de una moneda (0 = Cruz, 1 = Cara)
# Lanzamos la moneda 10.000 veces
n_lanzamientos <- 10000
resultados <- sample(c(0, 1), size = n_lanzamientos, replace = TRUE, prob = c(0.5, 0.5))

# Calcular las frecuencias acumuladas de caras
caras_acumuladas <- cumsum(resultados)
ensayos <- 1:n_lanzamientos
frecuencia_relativa <- caras_acumuladas / ensayos

# Graficar la convergencia de la probabilidad
plot(ensayos, frecuencia_relativa, type="l", ylim=c(0.2, 0.8),
     main="Ley de los Grandes Números (Simulación en R)",
     xlab="Número de ensayos", ylab="Frecuencia relativa de Caras",
     col="darkgreen", lwd=2)
abline(h=0.5, col="red", lty=2, lwd=2) # Línea teórica en 0.5
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
En una red local corporativa, la probabilidad de que un paquete de datos sufra una colisión es $P(C) = 0.15$, la probabilidad de que sufra un retraso de cola es $P(R) = 0.25$, y la probabilidad de que ocurran ambos problemas simultáneamente es $P(C \cap R) = 0.05$.
1. Calcular la probabilidad de que un paquete sufra colisión o retraso de cola.
2. Calcular la probabilidad de que un paquete no sufra ninguno de los dos inconvenientes.
3. Determinar si los sucesos "Colisión" y "Retraso de cola" son independientes.

**Solución**:
1.  **Probabilidad de la unión $P(C \cup R)$**:
    $$P(C \cup R) = P(C) + P(R) - P(C \cap R) = 0.15 + 0.25 - 0.05 = 0.35$$
    *(La probabilidad de sufrir colisión, retraso o ambos es de 0.35 o 35%).*

2.  **Probabilidad de no sufrir ninguno**:
    El suceso "ninguno" equivale al complementario de la unión: $\overline{C \cup R}$.
    $$P(\overline{C \cup R}) = 1 - P(C \cup R) = 1 - 0.35 = 0.65$$
    *(La probabilidad de que un paquete viaje de forma limpia es de 0.65 o 65%).*

3.  **Comprobación de Independencia**:
    Dos sucesos son independientes si $P(C \cap R) = P(C) \cdot P(R)$.
    *   Por un lado tenemos: $P(C \cap R) = 0.05$.
    *   Por otro lado: $P(C) \cdot P(R) = 0.15 \times 0.25 = 0.0375$.
    Como $0.05 \neq 0.0375$, los sucesos **no son independientes** (son dependientes). El hecho de que un paquete sufra una colisión altera la probabilidad de que sufra un retraso de cola.


<div style="page-break-after: always;"></div>

# Tema 4: Teoremas Fundamentales de la Probabilidad

El análisis probabilístico estructurado requiere herramientas que permitan descomponer problemas complejos en subproblemas más sencillos. Las dos herramientas fundamentales para ello son el Teorema de la Probabilidad Total y el Teorema de Bayes, esenciales en computación para algoritmos de clasificación (como el Clasificador Naive Bayes) e inferencia probabilística.

---

## 1. Partición del Espacio Muestral

Para aplicar estos teoremas, primero debemos estructurar el espacio muestral. Decimos que una colección de sucesos $A_1, A_2, \dots, A_k$ constituye una **partición** del espacio muestral $\Omega$ si cumple dos condiciones:

1.  **Exclusividad**: Son mutuamente excluyentes entre sí (no pueden ocurrir a la vez):
    $$A_i \cap A_j = \emptyset \quad \text{para todo } i \neq j$$
2.  **Exhaustividad**: Su unión cubre completamente el espacio muestral:
    $$\bigcup_{i=1}^k A_i = \Omega$$

Bajo una partición, cualquier suceso arbitrario $B$ puede representarse como la unión de sus intersecciones con cada uno de los elementos de la partición:
$$B = (B \cap A_1) \cup (B \cap A_2) \cup \dots \cup (B \cap A_k)$$

---

## 2. Teorema de la Probabilidad Total

Si conocemos las probabilidades a priori de los sucesos de una partición, $P(A_i)$, y las probabilidades condicionadas de un suceso $B$ dado cada $A_i$, $P(B|A_i)$, podemos calcular la probabilidad absoluta de $B$ mediante el **Teorema de la Probabilidad Total**:

$$P(B) = \sum_{i=1}^k P(A_i) \cdot P(B|A_i)$$

*   **Intuición Física**: Imagina que deseas llegar a un servidor de destino y hay $k$ rutas físicas disponibles ($A_1, \dots, A_k$). Cada ruta tiene una probabilidad de ser elegida, $P(A_i)$, y una probabilidad de que el paquete se pierda en ella, $P(B|A_i)$. La probabilidad total de perder el paquete ($P(B)$) es la suma ponderada de las probabilidades de pérdida en cada ruta individual, donde los pesos son las probabilidades de elegir cada ruta.

---

## 3. Teorema de Bayes

Una vez que ha ocurrido el suceso $B$, podemos preguntarnos cuál es la probabilidad de que haya sido causado por un elemento de la partición en particular ($A_j$). El **Teorema de Bayes** nos permite actualizar nuestra creencia a priori $P(A_j)$ incorporando la nueva evidencia de que $B$ ha ocurrido:

$$P(A_j|B) = \frac{P(A_j \cap B)}{P(B)} = \frac{P(A_j) \cdot P(B|A_j)}{\sum_{i=1}^k P(A_i) \cdot P(B|A_i)}$$

*   **Intuición**: Es la **fórmula del aprendizaje**. Nos permite "invertir" la relación de condicionamiento.
    *   $P(A_j)$ es la **probabilidad a priori** (nuestra creencia inicial antes de observar el efecto).
    *   $P(B|A_j)$ es la **verosimilitud** (la probabilidad de observar el efecto $B$ si la causa fuera $A_j$).
    *   $P(A_j|B)$ es la **probabilidad a posteriori** (la creencia actualizada tras observar el efecto $B$).
    *   El denominador es la probabilidad total de observar el efecto, actuando como una constante de normalización.

### Aplicación Práctica: Filtros Bayesianos de Spam
Un filtro de spam analiza la presencia de ciertas palabras clave. Supongamos que un email contiene la palabra "Gratis" ($B$). Conocemos la probabilidad de que un email sea spam, $P(\text{Spam})$, y la probabilidad de que aparezca "Gratis" si el email es spam, $P(\text{"Gratis"|Spam})$. Aplicando Bayes, calculamos la probabilidad a posteriori de que el correo sea spam dado que contiene dicha palabra: $P(\text{Spam|"Gratis"})$.

---

## 4. Cálculo Probabilístico en R

Podemos automatizar los cálculos de Bayes y Probabilidad Total en R mediante variables estructuradas:

```R
# Supongamos tres servidores de base de datos A, B y C que atienden el tráfico.
# Probabilidades a priori de asignación de consultas:
p_servidores <- c(A = 0.50, B = 0.30, C = 0.20)

# Probabilidades de error de consulta en cada servidor:
p_error_dado_servidor <- c(A = 0.01, B = 0.03, C = 0.05)

# 1. Aplicar el Teorema de la Probabilidad Total para obtener P(Error)
p_error_total <- sum(p_servidores * p_error_dado_servidor)
print(paste("Probabilidad Total de Error:", p_error_total)) 
# Retorna 0.024 (2.4% de las consultas fallan en total)

# 2. Aplicar el Teorema de Bayes
# ¿Si una consulta falló, cuál es la probabilidad de que proceda del servidor C?
p_C_dado_error <- (p_servidores["C"] * p_error_dado_servidor["C"]) / p_error_total
print(paste("Probabilidad de que el causante sea C:", p_C_dado_error))
# Retorna 0.4166 (A pesar de procesar solo el 20% del tráfico, C causa el 41.7% de los errores)
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
En una fábrica de software, el código es desarrollado por tres programadores: Ana (escribe el 50% de las líneas de código), Carlos (30%) y Elena (20%). Por estadísticas previas, se sabe que la probabilidad de introducir un bug por cada línea escrita es de 0.02 para Ana, 0.05 para Carlos y 0.08 para Elena.
1. Si seleccionamos una línea de código al azar, calcular la probabilidad de que contenga un bug.
2. Si se detecta un bug en una línea de código durante la auditoría, calcular la probabilidad de que dicha línea haya sido escrita por Carlos.

**Solución**:
Definimos los sucesos de la partición:
*   $A$: Línea escrita por Ana ($P(A) = 0.50$).
*   $C$: Línea escrita por Carlos ($P(C) = 0.30$).
*   $E$: Línea escrita por Elena ($P(E) = 0.20$).
*   $B$: La línea contiene un bug (el efecto observado).
    *   $P(B|A) = 0.02$
    *   $P(B|C) = 0.05$
    *   $P(B|E) = 0.08$

1.  **Probabilidad Total de Bug ($P(B)$)**:
    $$P(B) = P(A) \cdot P(B|A) + P(C) \cdot P(B|C) + P(E) \cdot P(B|E)$$
    $$P(B) = (0.50 \times 0.02) + (0.30 \times 0.05) + (0.20 \times 0.08)$$
    $$P(B) = 0.010 + 0.015 + 0.016 = 0.041$$
    *(La probabilidad de encontrar un bug en una línea elegida al azar es del 4.1%).*

2.  **Teorema de Bayes para calcular $P(C|B)$**:
    $$P(C|B) = \frac{P(C) \cdot P(B|C)}{P(B)} = \frac{0.30 \times 0.05}{0.041} = \frac{0.015}{0.041} \approx 0.3659$$
    *(Si la línea contiene un bug, hay una probabilidad aproximada del 36.59% de que haya sido escrita por Carlos).*


<div style="page-break-after: always;"></div>

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


<div style="page-break-after: always;"></div>

# Tema 6: Modelos de Distribución de Probabilidad

Determinados patrones de comportamiento aleatorio se repiten con tanta frecuencia en la naturaleza y en la tecnología que merecen ser estudiados mediante modelos matemáticos parametrizados estándar. Si logramos identificar que nuestro problema real se ajusta a uno de estos modelos, podremos conocer de inmediato su función de probabilidad/densidad, su esperanza y su varianza sin necesidad de realizar integraciones o sumatorios complejos.

---

## 1. Modelos de Distribución Discretos

### A. Distribución Binomial ($X \sim B(n, p)$)
Modela el número de "éxitos" al repetir $n$ veces un ensayo de Bernoulli (un experimento con solo dos posibles resultados: éxito con probabilidad $p$, y fracaso con probabilidad $q = 1-p$) de manera independiente.
*   **Función de Probabilidad**:
    $$P(X = k) = \binom{n}{k} p^k (1-p)^{n-k} \quad \text{donde } k \in \{0, 1, 2, \dots, n\}$$
    Recordemos que el número combinatorio es $\binom{n}{k} = \frac{n!}{k!(n-k)!}$.
*   **Esperanza**: $E[X] = n \cdot p$
*   **Varianza**: $Var(X) = n \cdot p \cdot (1-p)$
*   *Aplicación*: Modelar la cantidad de bits erróneos recibidos en un paquete de red de 1024 bits si la tasa de error por bit es independiente.

### B. Distribución de Poisson ($X \sim P(\lambda)$)
Modela el número de eventos que ocurren en un intervalo de tiempo o espacio continuo (por ejemplo, número de peticiones a un servidor web por segundo). Se asume que los eventos ocurren de forma independiente a una tasa media constante $\lambda > 0$ por unidad de intervalo.
*   **Función de Probabilidad**:
    $$P(X = k) = \frac{e^{-\lambda} \lambda^k}{k!} \quad \text{donde } k \in \{0, 1, 2, \dots\}$$
*   **Esperanza**: $E[X] = \lambda$
*   **Varianza**: $Var(X) = \lambda$
*   *Aplicación*: Predecir la probabilidad de que un servidor web reciba exactamente 15 peticiones en un segundo si su tráfico promedio es de 10 peticiones/segundo.

### C. Distribución Geométrica ($X \sim G(p)$)
Modela el número de ensayos de Bernoulli independientes que debemos realizar hasta observar el primer éxito.
*   **Función de Probabilidad**:
    $$P(X = k) = (1-p)^{k-1} p \quad \text{donde } k \in \{1, 2, 3, \dots\}$$
*   **Esperanza**: $E[X] = \frac{1}{p}$
*   **Varianza**: $Var(X) = \frac{1-p}{p^2}$
*   *Aplicación*: Modelar el número de intentos de retransmisión de un paquete que realiza un protocolo TCP antes de que sea recibido con éxito.

---

## 2. Modelos de Distribución Continuos

### A. Distribución Uniforme ($X \sim U(a, b)$)
Modela variables donde todos los intervalos de igual longitud dentro del soporte $[a, b]$ tienen la misma probabilidad de ocurrir. Presenta una densidad constante ("plana").
*   **Función de Densidad**:
    $$f(x) = \begin{cases} 
      \frac{1}{b-a} & \text{si } a \le x \le b \\ 
      0 & \text{en el resto} 
   \end{cases}$$
*   **Esperanza**: $E[X] = \frac{a+b}{2}$
*   **Varianza**: $Var(X) = \frac{(b-a)^2}{12}$
*   *Aplicación*: Tiempos de retraso por redondeo en operaciones numéricas o generación de números pseudoaleatorios en programación.

### B. Distribución Exponencial ($X \sim Exp(\lambda)$)
Modela el tiempo que transcurre entre dos eventos consecutivos de un proceso de Poisson (por ejemplo, el tiempo de vida útil de un componente electrónico o el tiempo que transcurre entre la llegada de dos clientes a una cola). $\lambda$ representa la misma tasa media del proceso de Poisson.
*   **Función de Densidad**:
    $$f(x) = \begin{cases} 
      \lambda e^{-\lambda x} & \text{si } x \ge 0 \\ 
      0 & \text{si } x < 0 
   \end{cases}$$
*   **Función de Distribución Acumulada**:
    $$F(x) = P(X \le x) = 1 - e^{-\lambda x} \quad \text{si } x \ge 0$$
*   **Esperanza**: $E[X] = \frac{1}{\lambda}$
*   **Varianza**: $Var(X) = \frac{1}{\lambda^2}$

#### Propiedad de Falta de Memoria
Es la característica distintiva de la distribución exponencial. Establece que la probabilidad de que ocurra el evento en el futuro es independiente de cuánto tiempo haya transcurrido ya:
$$P(X > s + t \mid X > s) = P(X > t) \quad \text{para todo } s, t > 0$$
*Intuición*: Si la bombilla de un servidor no se ha fundido tras 100 horas ($X > 100$), la probabilidad de que dure otras 50 horas más ($X > 150$) es exactamente igual a la probabilidad de que dure 50 horas partiendo desde nueva. La bombilla "no envejece".

---

## 3. Trabajo con Distribuciones en R

R dispone de cuatro prefijos para todas las distribuciones estudiadas:
*   `d`: Función de densidad/probabilidad puntual (ej. `dbinom`, `dpois`).
*   `p`: Función de distribución acumulada (ej. `pbinom`, `pexp`).
*   `q`: Función de cuantiles (inversa de la acumulada, ej. `qnorm`).
*   `r`: Generación de números aleatorios (ej. `rbinom`, `rpois`).

```R
# A. Binomial: Lanzamos 10 monedas (p = 0.5), probabilidad de obtener exactamente 7 caras
dbinom(7, size = 10, prob = 0.5) # P(X = 7)

# Probabilidad de obtener 5 o menos caras
pbinom(5, size = 10, prob = 0.5) # P(X <= 5)

# B. Poisson: Servidor recibe de media 8 peticiones/min. P(X = 12)
dpois(12, lambda = 8)

# C. Geométrica: Probabilidad de éxito en tcp p=0.4. P(primer éxito en el 3er intento)
# Ojo: En R, dgeom cuenta el número de FRACASOS antes del primer éxito (k-1)
# Para k=3 (primer éxito en el 3er intento), hay 2 fracasos previos.
dgeom(2, prob = 0.4) 

# D. Exponencial: Tasa de peticiones lambda = 0.5/segundo.
# Probabilidad de que el tiempo hasta la próxima petición sea menor de 3 segundos
pexp(3, rate = 0.5) # P(X <= 3)
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1 (Binomial)
Un canal de comunicaciones transmite tramas de datos. Se sabe que la probabilidad de que una trama se reciba con error es $p = 0.05$ y que los errores ocurren de forma independiente. Si se transmite un bloque de 20 tramas:
1. Calcular la probabilidad de que se reciban exactamente 2 tramas con error.
2. Calcular la probabilidad de que se reciba al menos una trama con error.
3. Calcular el número esperado de tramas erróneas y su desviación típica.

**Solución**:
La variable $X$ = "Número de tramas erróneas de un total de 20" sigue una distribución Binomial $X \sim B(n = 20, p = 0.05)$.
1.  **Probabilidad de exactamente 2 errores ($P(X = 2)$)**:
    $$P(X = 2) = \binom{20}{2} (0.05)^2 (0.95)^{18} = \frac{20 \times 19}{2} \times 0.0025 \times 0.3972 = 190 \times 0.0025 \times 0.3972 \approx 0.1887$$
    *(La probabilidad de tener exactamente 2 tramas erróneas es del 18.87%).*

2.  **Probabilidad de al menos un error ($P(X \ge 1)$)**:
    Es más rápido calcular a través del suceso contrario (ningún error, $X = 0$):
    $$P(X \ge 1) = 1 - P(X = 0) = 1 - \binom{20}{0} (0.05)^0 (0.95)^{20} = 1 - 1 \times 1 \times 0.3585 = 0.6415$$
    *(Existe un 64.15% de probabilidad de tener al menos una trama errónea).*

3.  **Esperanza y Desviación típica**:
    *   **Esperanza**: $E[X] = n \cdot p = 20 \times 0.05 = 1\text{ trama errónea}$.
    *   **Varianza**: $Var(X) = n \cdot p \cdot (1-p) = 20 \times 0.05 \times 0.95 = 0.95\text{ tramas}^2$.
    *   **Desviación típica**: $\sigma = \sqrt{0.95} \approx 0.9747\text{ tramas erróneas}$.


<div style="page-break-after: always;"></div>

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


<div style="page-break-after: always;"></div>

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


<div style="page-break-after: always;"></div>

# Tema 9: Estimación por Intervalos de Confianza

La estimación puntual proporciona un único valor numérico como aproximación de un parámetro poblacional. Sin embargo, no nos dice nada acerca de la precisión o el error asociado a dicha estimación. Para resolver esto, recurrimos a los **intervalos de confianza**, que proporcionan un rango de valores creíbles para el parámetro, asociado a una medida de certeza estadística.

---

## 1. Concepto de Intervalo de Confianza

Un **intervalo de confianza (I.C.)** para un parámetro desconocido $\theta$ es un intervalo de extremos aleatorios $[L_1, L_2]$ calculado a partir de la muestra, de modo que:
$$P(L_1 \le \theta \le L_2) = 1 - \alpha$$

*   **$1-\alpha$**: **Nivel de confianza** (habitualmente $95\%$ o $0.95$). Representa la probabilidad a priori de que el intervalo que construyamos atrape el parámetro real.
*   **$\alpha$**: **Nivel de significación** (la probabilidad de cometer error, usualmente $5\%$ o $0.05$).
*   **Interpretación Correcta (¡Importante!)**: El parámetro real $\theta$ es una constante física fija pero desconocida; no es una variable aleatoria y no tiene "probabilidad" de estar en el intervalo una vez calculado. Lo que es aleatorio son los extremos del intervalo $[L_1, L_2]$. Por tanto, decir que un intervalo de confianza al $95\%$ es $[10, 15]$ significa que *si repitiéramos el muestreo 100 veces en idénticas condiciones, aproximadamente 95 de los 100 intervalos construidos contendrían el parámetro real*.

---

## 2. Intervalos de Confianza para una Población

### A. Para la Media ($\mu$) de una Población Normal

#### Caso 1: Varianza Poblacional $\sigma^2$ Conocida
Usamos la distribución normal estándar $Z \sim N(0, 1)$:
$$I.C._{1-\alpha}(\mu) = \left[ \bar{X} - z_{\alpha/2} \frac{\sigma}{\sqrt{n}}, \, \bar{X} + z_{\alpha/2} \frac{\sigma}{\sqrt{n}} \right]$$
donde $z_{\alpha/2}$ es el cuantil de la normal estándar tal que $P(Z > z_{\alpha/2}) = \alpha/2$. (Para un 95% de confianza, $z_{0.025} = 1.96$).

#### Caso 2: Varianza Poblacional $\sigma^2$ Desconocida (Caso Habitual)
Al desconocer $\sigma$, debemos estimarla mediante la cuasidesviación típica $S_{n-1}$. Esto introduce una fuente adicional de incertidumbre, por lo que sustituimos la distribución normal por la **t de Student** con $n-1$ grados de libertad ($t_{n-1}$):
$$I.C._{1-\alpha}(\mu) = \left[ \bar{X} - t_{\alpha/2, n-1} \frac{S_{n-1}}{\sqrt{n}}, \, \bar{X} + t_{\alpha/2, n-1} \frac{S_{n-1}}{\sqrt{n}} \right]$$

### B. Para la Varianza ($\sigma^2$) de una Población Normal
La dispersión muestral se comporta según la distribución **Chi-cuadrado** ($\chi^2$) con $n-1$ grados de libertad:
$$I.C._{1-\alpha}(\sigma^2) = \left[ \frac{(n-1)S_{n-1}^2}{\chi^2_{\alpha/2, n-1}}, \, \frac{(n-1)S_{n-1}^2}{\chi^2_{1-\alpha/2, n-1}} \right]$$
*Nota: Dado que la distribución Chi-cuadrado no es simétrica, debemos calcular por separado los dos cuantiles del denominador.*

### C. Para la Proporción Poblacional ($p$) (Muestras Grandes)
Si el tamaño muestral es grande ($n \ge 30$, $n\hat{p} \ge 5$ y $n(1-\hat{p}) \ge 5$), aplicamos el TCL para aproximar por la Normal:
$$I.C._{1-\alpha}(p) = \left[ \hat{p} - z_{\alpha/2} \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}, \, \hat{p} + z_{\alpha/2} \sqrt{\frac{\hat{p}(1-\hat{p})}{n}} \right]$$

---

## 3. Intervalos de Confianza para Dos Poblaciones (Comparaciones)

*   **Diferencia de Medias ($\mu_1 - \mu_2$)**: Utilizado para determinar si el rendimiento promedio de dos algoritmos es diferente. Si el intervalo para $\mu_1 - \mu_2$ contiene al **cero**, concluimos que no hay evidencia estadística de que sus medias difieran.
*   **Cociente de Varianzas ($\sigma_1^2 / \sigma_2^2$)**: Utilizado para comparar la estabilidad de dos sistemas. Se basa en la distribución **F de Snedecor** ($F_{n_1-1, n_2-1}$). Si el intervalo contiene al **uno**, asumimos varianzas iguales (homocedasticidad).

---

## 4. Práctica de Intervalos de Confianza en R

R calcula intervalos de confianza directamente dentro de sus funciones de contrastes de hipótesis:

```R
# Muestra de tiempos de respuesta de un Servidor A
servidor_A <- c(102, 98, 105, 95, 101, 99, 100, 103)

# 1. Intervalo de confianza al 95% para la media (sigma desconocida)
t.test(servidor_A, conf.level = 0.95)$conf.int
# Retorna los extremos del intervalo: [98.11, 102.63] ms

# 2. Intervalo de confianza para una proporción
# Supongamos que 15 de 100 paquetes se perdieron
prop.test(x = 15, n = 100, conf.level = 0.95)$conf.int
# Retorna: [0.0889, 0.2398] (Proporción estimada entre 8.9% y 24.0%)

# 3. Obtener cuantiles teóricos manualmente
# Cuantil t de Student para un 95% de confianza (alfa/2 = 0.025) con 7 grados de libertad
qt(0.975, df = 7) # Retorna 2.3646
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Deseamos estimar el tiempo medio de ejecución de un script de base de datos. Se ejecuta el script en 10 ocasiones independientes y se registran los siguientes tiempos (en segundos): $\{4.2, 4.8, 3.9, 4.5, 4.1, 4.7, 4.3, 4.6, 4.0, 4.4\}$. Asumiendo que los tiempos siguen una distribución normal:
1. Calcular el intervalo de confianza al 95% para el tiempo medio real de ejecución.

**Solución**:
1.  **Cálculo de estadísticos muestrales**:
    *   Tamaño muestral $n = 10$. Grados de libertad $d.f. = 9$.
    *   **Media muestral $\bar{x}$**:
        $$\bar{x} = \frac{4.2 + 4.8 + 3.9 + 4.5 + 4.1 + 4.7 + 4.3 + 4.6 + 4.0 + 4.4}{10} = 4.35\text{ s}$$
    *   **Cuasivarianza muestral $s_{n-1}^2$**:
        $$\sum x_i^2 = 4.2^2 + 4.8^2 + \dots + 4.4^2 = 190.05$$
        $$s_{n-1}^2 = \frac{1}{9} \left( 190.05 - 10 \times (4.35)^2 \right) = \frac{1}{9} (190.05 - 189.225) = 0.0917\text{ s}^2$$
    *   **Cuasidesviación típica $s_{n-1}$**:
        $$s_{n-1} = \sqrt{0.0917} \approx 0.3028\text{ s}$$
2.  **Cuantil de la t de Student ($t_{\alpha/2, n-1}$)**:
    Para un nivel de confianza del 95% ($1-\alpha = 0.95 \implies \alpha/2 = 0.025$) y $n-1 = 9$ grados de libertad, buscamos en la tabla de la t de Student:
    $$t_{0.025, 9} = 2.262$$
3.  **Construcción del Intervalo**:
    $$\text{Margen de error } E = t_{0.025, 9} \frac{s_{n-1}}{\sqrt{n}} = 2.262 \times \frac{0.3028}{\sqrt{10}} = 2.262 \times 0.09575 \approx 0.2166\text{ s}$$
    $$I.C._{0.95}(\mu) = [\bar{x} - E, \, \bar{x} + E] = [4.35 - 0.2166, \, 4.35 + 0.2166] = [4.133, \, 4.567]\text{ s}$$
    *Conclusión: Con un nivel de confianza del 95%, estimamos que el tiempo medio real de ejecución del script está comprendido entre 4.133 y 4.567 segundos.*


<div style="page-break-after: always;"></div>

# Tema 10: Contrastes de Hipótesis Paramétricos

En el método científico y en la ingeniería de sistemas, constantemente necesitamos tomar decisiones basadas en datos: ¿es el nuevo sistema de caché más rápido que el antiguo?, ¿es la tasa de fallos de la base de datos menor al 1%?, ¿difieren los rendimientos de dos algoritmos de ordenación? Un contraste de hipótesis es la herramienta formal para responder a estas preguntas bajo control del azar.

---

## 1. Planteamiento de un Contraste de Hipótesis

Un contraste de hipótesis es una regla de decisión estadística que nos permite elegir entre dos afirmaciones opuestas acerca de un parámetro poblacional:

*   **Hipótesis Nula ($H_0$)**: Representa la situación de "no cambio", "no efecto" o el statu quo. Se asume como verdadera por defecto hasta que los datos de la muestra demuestren firmemente lo contrario.
    *   *Ejemplo*: El tiempo medio de respuesta es de 100ms ($H_0: \mu = 100$).
*   **Hipótesis Alternativa ($H_1$)**: Afirmación opuesta a $H_0$, que representa la sospecha, la novedad o la hipótesis que el investigador desea probar.
    *   *Ejemplo*: El tiempo medio es diferente a 100ms ($H_1: \mu \neq 100$) (contraste bilateral) o es menor a 100ms ($H_1: \mu < 100$) (contraste unilateral).

---

## 2. Errores de Decisión y Potencia

Al tomar una decisión basada en una muestra aleatoria, existe riesgo de error. Los errores posibles se resumen en la siguiente tabla:

| Realidad \ Decisión | No Rechazar $H_0$ | Rechazar $H_0$ |
|:---:|:---:|:---:|
| **$H_0$ es Verdadera** | Decisión Correcta ($1-\alpha$) | **Error Tipo I ($\alpha$)** |
| **$H_0$ es Falsa** | **Error Tipo II ($\beta$)** | Decisión Correcta ($1-\beta$, Potencia) |

*   **Error Tipo I ($\alpha$)**: Rechazar $H_0$ cuando en realidad es verdadera. Es un falso positivo (concluir que el nuevo algoritmo es mejor cuando no lo es). La probabilidad máxima admisible de cometer este error la fija el investigador a priori y se llama **nivel de significación** ($\alpha$, típicamente 0.05).
*   **Error Tipo II ($\beta$)**: No rechazar $H_0$ cuando en realidad es falsa. Es un falso negativo (no detectar que el nuevo algoritmo sí es mejor).
*   **Potencia del Contraste ($1-\beta$)**: Probabilidad de rechazar $H_0$ cuando es falsa. Mide la capacidad del contraste para detectar un efecto real.

---

## 3. El p-valor: La Brújula de Decisión

El **p-valor** (probabilidad de significación) es la probabilidad de observar un valor muestral tan extremo o más que el obtenido en nuestra muestra, asumiendo que la hipótesis nula $H_0$ es estrictamente verdadera.

```
       Visualización del p-valor en una Normal (Bilateral)
       
                        |
                     *  *  *
                  *  |  |  |  *
                *    |  |  |    *
              *      |  |  |      *
       [Rechazo]     |  |  |     [Rechazo]
        p-valor/2    |  |  |      p-valor/2
     -----*----------|--|--|----------*-----
        -z_obs       |  0  |        z_obs
```

*   **Intuición**:
    *   Si el p-valor es **muy grande**, significa que nuestros datos son perfectamente habituales y esperables bajo el supuesto de $H_0$. No hay motivo para desconfiar de ella.
    *   Si el p-valor es **muy pequeño** (por ejemplo, 0.001), significa que sería extraordinariamente raro (1 entre 1000 veces) observar nuestros datos si $H_0$ fuera cierta. Dado que ya hemos observado esos datos, concluimos que la hipótesis nula debe ser falsa y la rechazamos.

### La Regla de Oro para el Examen
Grábate esta regla de decisión a fuego:

$$\text{Si } p\text{-valor} < \alpha \implies \text{Rechazar } H_0$$
$$\text{Si } p\text{-valor} \ge \alpha \implies \text{No Rechazar } H_0$$

*(Con un nivel de significación habitual $\alpha = 0.05$, si el p-valor es menor que 0.05, rechazamos la hipótesis nula).*

---

## 4. Contrastes Paramétricos de Una y Dos Muestras

*   **Una muestra**: Evaluamos si el parámetro de una población es igual a un valor de referencia $\theta_0$.
    *   *Ejemplo*: ¿Es el tiempo medio de carga inferior a 3 segundos? ($H_0: \mu = 3$ vs $H_1: \mu < 3$).
*   **Dos muestras independientes**: Comparamos el mismo parámetro entre dos poblaciones diferentes.
    *   *Ejemplo*: ¿Difiere el tiempo medio del Algoritmo A de la velocidad del Algoritmo B? ($H_0: \mu_A = \mu_B$ vs $H_1: \mu_A \neq \mu_B$).
*   **Muestras emparejadas**: Comparamos mediciones tomadas sobre los mismos individuos antes y después de un tratamiento.
    *   *Ejemplo*: Tiempos de acceso a archivos antes y después de desfragmentar un disco duro.

---

## 5. Contrastes Paramétricos en R

R ejecuta los contrastes paramétricos de forma automática proporcionando el p-valor y el intervalo de confianza:

```R
# Rendimiento del Algoritmo Nuevo vs Algoritmo Estándar (tiempos en ms)
nuevo <- c(45, 48, 43, 44, 46, 42, 45, 41)
estandar <- c(50, 52, 49, 48, 51, 53, 50, 49)

# 1. Contraste de dos muestras para comparar medias (t-test)
# H0: mu_nuevo = mu_estandar vs H1: mu_nuevo < mu_estandar (unilateral izquierdo)
resultado <- t.test(nuevo, estandar, alternative = "less", var.equal = TRUE)

# Mostrar el p-valor obtenido
resultado$p.value
# Retorna ~3.5e-06 (0.0000035, valor muy inferior a 0.05)

# Decisión: p-valor < 0.05, por lo tanto rechazamos H0.
# Conclusión: Existe evidencia estadísticamente significativa de que el 
# nuevo algoritmo tiene un tiempo medio inferior al estándar.
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Un proveedor de internet asegura que la latencia media de su red de fibra óptica es de 20 ms. Un cliente escéptico mide la latencia de 36 conexiones independientes en horas puntas y obtiene una media muestral $\bar{x} = 22$ ms con una cuasidesviación típica $s_{n-1} = 5$ ms.
1. Plantear las hipótesis nula y alternativa para comprobar si la latencia real es mayor a la declarada por el proveedor.
2. Calcular el estadístico de contraste e indicar la región de rechazo con un nivel de significación $\alpha = 0.05$.
3. Calcular el p-valor e indicar la conclusión del contraste.

**Solución**:
1.  **Planteamiento de Hipótesis**:
    *   $H_0: \mu = 20$ ms (la latencia real coincide con lo declarado).
    *   $H_1: \mu > 20$ ms (la latencia real es superior, sospecha del cliente). Es un contraste unilateral derecho.

2.  **Estadístico de Contraste**:
    Como el tamaño muestral $n = 36 \ge 30$, el teorema central del límite nos asegura la normalidad de la media muestral. Aunque $\sigma^2$ es desconocida, estimamos con la cuasidesviación típica $s_{n-1} = 5$.
    El estadístico de contraste bajo la hipótesis nula es:
    $$t_{\text{obs}} = \frac{\bar{x} - \mu_0}{s_{n-1}/\sqrt{n}} = \frac{22 - 20}{5/\sqrt{36}} = \frac{2}{5/6} = \frac{12}{5} = 2.40$$
    **Región de rechazo**:
    Para un nivel de significación $\alpha = 0.05$ en un contraste unilateral derecho, buscamos el valor crítico en la distribución t de Student con $n-1 = 35$ grados de libertad (o aproximando por la normal estándar $z_{0.05} = 1.645$):
    $$t_{\text{crit}} = t_{0.05, 35} \approx 1.69$$
    Como el estadístico observado $t_{\text{obs}} = 2.40$ es mayor que el valor crítico $1.69$, cae dentro de la **región de rechazo**.

3.  **Cálculo del p-valor y conclusión**:
    El p-valor es la probabilidad de que la variable de contraste tome un valor mayor al observado:
    $$p\text{-valor} = P(Z > 2.40) = 1 - P(Z \le 2.40) = 1 - 0.9918 = 0.0082$$
    Como el $p\text{-valor} = 0.0082 < \alpha = 0.05$, rechazamos la hipótesis nula $H_0$.
    **Conclusión**: Existe evidencia estadísticamente significativa de que la latencia media real de la red es superior a los 20 ms declarados por el proveedor.


<div style="page-break-after: always;"></div>

# Tema 11: Contrastes No Paramétricos

Los contrastes de hipótesis analizados en el tema anterior se denominan paramétricos porque asumen que los datos provienen de una familia de distribuciones conocida (generalmente una Normal) y sus hipótesis afirman algo sobre sus parámetros ($\mu, \sigma^2, p$). Sin embargo, en muchas situaciones prácticas no podemos garantizar que los datos sigan una distribución normal, o bien trabajamos con variables puramente cualitativas. Para estos escenarios recurrimos a los **contrastes no paramétricos**, que no requieren supuestos rígidos sobre la forma de la distribución poblacional.

---

## 1. Test de Bondad de Ajuste de la Chi-cuadrado ($\chi^2$)

El objetivo de este contraste es determinar si los datos de una muestra se ajustan razonablemente a una distribución teórica propuesta (como una Binomial, Poisson, Exponencial o Normal).

### Planteamiento de Hipótesis:
*   $H_0$: Los datos de la muestra proceden de la distribución teórica especificada.
*   $H_1$: Los datos no proceden de dicha distribución.

### Metodología del Contraste:
1.  Agrupamos la muestra de tamaño $n$ en $k$ clases o categorías excluyentes.
2.  Registramos las **frecuencias observadas ($O_i$)** de datos en cada clase.
3.  Calculamos las **frecuencias esperadas teóricas ($E_i$)**, que representan cuántas observaciones cabría esperar en cada clase si $H_0$ fuera estrictamente cierta:
    $$E_i = n \cdot p_i$$
    donde $p_i$ es la probabilidad teórica de pertenecer a la clase $i$. (Requisito: Todas las frecuencias esperadas deben cumplir $E_i \ge 5$; de lo contrario, deben agruparse clases contiguas).
4.  Calculamos el **estadístico de contraste $\chi^2$ de Pearson**:
    $$\chi^2_{\text{obs}} = \sum_{i=1}^k \frac{(O_i - E_i)^2}{E_i}$$
    *   *Intuición*: Si la discrepancia entre lo observado ($O_i$) y lo esperado bajo la teoría ($E_i$) es muy pequeña, $\chi^2_{\text{obs}}$ será cercano a cero. Si es muy grande, $\chi^2_{\text{obs}}$ tomará un valor elevado.
5.  **Grados de libertad (g.l.)**: El estadístico bajo $H_0$ sigue una distribución $\chi^2$ con:
    $$\text{g.l.} = k - 1 - p$$
    donde $p$ es el número de parámetros poblacionales estimados a partir de los propios datos de la muestra para definir la distribución teórica (por ejemplo, si estimamos la media $\mu$ para calcular la normal teórica, $p=1$).

---

## 2. Test de Independencia de la Chi-cuadrado ($\chi^2$)

Utilizado para analizar la relación entre dos variables cualitativas estructuradas en una tabla de contingencia de dimensiones $r \times c$ (donde $r$ es el número de filas y $c$ el número de columnas).

### Planteamiento de Hipótesis:
*   $H_0$: Las dos variables son independientes.
*   $H_1$: Las dos variables están relacionadas (son dependientes).

### Estadístico del Contraste:
Las frecuencias observadas en la celda $(i, j)$ se denotan por $O_{ij}$. Si $H_0$ es cierta (las variables son independientes), la frecuencia esperada en dicha celda ($E_{ij}$) se calcula como:
$$E_{ij} = \frac{n_{i\cdot} \cdot n_{\cdot j}}{n}$$
donde $n_{i\cdot}$ es el total de la fila $i$, $n_{\cdot j}$ el total de la columna $j$, y $n$ el tamaño total de la muestra.

Calculamos el estadístico de discrepancia:
$$\chi^2_{\text{obs}} = \sum_{i=1}^r \sum_{j=1}^c \frac{(O_{ij} - E_{ij})^2}{E_{ij}}$$

Bajo $H_0$, este estadístico sigue una distribución $\chi^2$ con grados de libertad:
$$\text{g.l.} = (r - 1)(c - 1)$$

---

## 3. Contrastes No Paramétricos en R

R ejecuta el test de la Chi-cuadrado de forma nativa mediante la función `chisq.test()`:

```R
# 1. Ejemplo de Test de Independencia
# Queremos ver si el tipo de navegador (Chrome, Firefox, Safari)
# influye en que se complete una compra online (Sí, No)
compras <- matrix(c(120, 90, 40,   # Sí completó
                     80, 70, 50),  # No completó
                   nrow = 2, byrow = TRUE)
colnames(compras) <- c("Chrome", "Firefox", "Safari")
rownames(compras) <- c("Si", "No")

# Ejecutar el test Chi-cuadrado
resultado <- chisq.test(compras)
print(resultado$p.value)
# Retorna ~0.0886

# Decisión: Como p-valor = 0.0886 >= 0.05, NO rechazamos H0.
# Conclusión: No hay evidencia estadísticamente significativa para afirmar 
# que la compra dependa del navegador utilizado.
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1 (Bondad de Ajuste)
Un administrador de sistemas monitoriza la llegada de errores graves en un servidor web durante 100 horas. Observa el número de errores por hora y obtiene los siguientes datos:
*   0 errores: en 40 horas.
*   1 error: en 35 horas.
*   2 errores: en 15 horas.
*   3 o más errores: en 10 horas.

Asumiendo que el número medio de errores por hora es $\lambda = 1.0$:
1. Realizar un test de bondad de ajuste con nivel de significación $\alpha = 0.05$ para comprobar si los datos siguen una distribución de Poisson de parámetro $\lambda = 1.0$.

**Solución**:
1.  **Hipótesis**:
    *   $H_0$: El número de errores por hora sigue una distribución de Poisson con $\lambda = 1.0$.
    *   $H_1$: El número de errores por hora no sigue esa distribución.

2.  **Cálculo de Probabilidades Teóricas y Frecuencias Esperadas**:
    Bajo $H_0 \sim P(\lambda = 1.0)$, las probabilidades puntuales son $p_k = \frac{e^{-1} 1^k}{k!}$:
    *   $p_0 = e^{-1} \approx 0.3679 \implies E_0 = 100 \times 0.3679 = 36.79$
    *   $p_1 = e^{-1} \approx 0.3679 \implies E_1 = 100 \times 0.3679 = 36.79$
    *   $p_2 = \frac{e^{-1}}{2} \approx 0.1839 \implies E_2 = 100 \times 0.1839 = 18.39$
    *   $p_{\ge 3} = 1 - (p_0 + p_1 + p_2) = 1 - 0.9197 = 0.0803 \implies E_{\ge 3} = 100 \times 0.0803 = 8.03$
    *(Nota: La frecuencia esperada de la última clase $8.03 \ge 5$, por lo que no es necesario agrupar).*

3.  **Cálculo de la discrepancia $\chi^2_{\text{obs}}$**:

| Errores ($i$) | Observado ($O_i$) | Esperado ($E_i$) | $(O_i - E_i)^2 / E_i$ |
|---|---|---|---|
| 0 | 40 | 36.79 | $\frac{3.21^2}{36.79} = 0.280$ |
| 1 | 35 | 36.79 | $\frac{(-1.79)^2}{36.79} = 0.087$ |
| 2 | 15 | 18.39 | $\frac{(-2.39)^2}{18.39} = 0.311$ |
| $\ge 3$ | 10 | 8.03 | $\frac{1.97^2}{8.03} = 0.483$ |
| **Suma** | **100** | **100** | **$\chi^2_{\text{obs}} = 1.161$** |

4.  **Grados de Libertad y Región de Rechazo**:
    El número de clases es $k = 4$. No hemos estimado ningún parámetro de la muestra para definir el test ya que $\lambda = 1.0$ era un valor dado a priori ($p = 0$).
    $$\text{g.l.} = k - 1 - p = 4 - 1 - 0 = 3$$
    Buscamos en la tabla de la distribución Chi-cuadrado para $\alpha = 0.05$ con 3 grados de libertad:
    $$\chi^2_{0.05, 3} = 7.815$$
    Como $\chi^2_{\text{obs}} = 1.161 \le 7.815$, **no rechazamos la hipótesis nula**.

5.  **Conclusión**:
    No hay evidencia estadística suficiente para rechazar que la tasa de errores del servidor sigue una distribución de Poisson de media $\lambda = 1.0$ errores por hora. Los datos se ajustan bien a este modelo.


<div style="page-break-after: always;"></div>

# Tema 12: El Modelo de Regresión Lineal Simple

La regresión lineal es una de las técnicas de modelado predictivo más antiguas y ampliamente utilizadas. En informática, se emplea constantemente para estimar tendencias de rendimiento, predecir el crecimiento del volumen de datos, dimensionar la infraestructura de servidores o servir como el algoritmo básico de aprendizaje supervisado (Machine Learning).

---

## 1. Especificación del Modelo

El objetivo de la regresión lineal simple es modelar la relación matemática entre una variable explicativa o independiente ($X$) y una variable de respuesta o dependiente ($Y$), utilizando una línea recta.

La ecuación poblacional del modelo es:
$$Y_i = \beta_0 + \beta_1 X_i + u_i$$

Donde:
*   **$\beta_0$** (Intercepto): Valor esperado de $Y$ cuando $X = 0$. Es el punto de corte de la recta con el eje vertical.
*   **$\beta_1$** (Pendiente): Mide el efecto marginal de $X$ sobre $Y$. Es la cantidad de unidades que cambia $Y$ por cada incremento unitario en $X$.
*   **$u_i$** (Perturbación aleatoria o Error): Variable aleatoria no observable que recoge el ruido aleatorio, errores de medición o factores omitidos que influyen sobre $Y$ pero no están explicados por $X$. Se asume que $u_i \sim N(0, \sigma^2)$ de forma independiente.

---

## 2. Estimación por Mínimos Cuadrados Ordinarios (MCO)

Dado un conjunto de $n$ datos observados $\{(x_1, y_1), \dots, (x_n, y_n)\}$, deseamos encontrar la recta estimada:
$$\hat{Y}_i = \hat{\beta}_0 + \hat{\beta}_1 X_i$$

*   **Intuición**: El método de Mínimos Cuadrados Ordinarios (MCO) busca los valores concretos de $\hat{\beta}_0$ y $\hat{\beta}_1$ que minimizan la suma de las distancias verticales (residuos) al cuadrado entre los puntos reales y la recta de predicción. Es decir, minimiza la Suma de Cuadrados de los Residuos:
    $$\text{Minimizar } \sum_{i=1}^n e_i^2 = \sum_{i=1}^n (y_i - \hat{y}_i)^2$$

Las fórmulas de los estimadores de MCO obtenidas mediante cálculo diferencial son:
$$\hat{\beta}_1 = \frac{s_{xy}}{s_x^2}$$
$$\hat{\beta}_0 = \bar{y} - \hat{\beta}_1 \bar{x}$$

Donde $s_{xy}$ es la covarianza muestral de la muestra y $s_x^2$ la varianza muestral de $X$.

---

## 3. Residuos e Interpretación Geométrica

*   **Residuo ($e_i$)**: Diferencia entre el valor observado y el valor predicho por el modelo para el individuo $i$:
    $$e_i = y_i - \hat{y}_i$$
*   Los residuos tienen media cero ($\sum e_i = 0$) y representan la parte del comportamiento de $Y$ que nuestro modelo lineal no ha sido capaz de explicar.

---

## 4. Bondad de Ajuste: El Coeficiente de Determinación ($R^2$)

Para cuantificar si la recta de regresión estimada representa fielmente la nube de puntos o si, por el contrario, las predicciones son muy imprecisas, definimos el **coeficiente de determinación ($R^2$)**:

$$R^2 = \frac{\text{Variabilidad Explicada por la Regresión}}{\text{Variabilidad Total de } Y} = 1 - \frac{\sum e_i^2}{\sum (y_i - \bar{y})^2}$$

*   **Propiedades**:
    *   Está acotado: $0 \le R^2 \le 1$.
    *   **$R^2 = 1$**: El ajuste es perfecto. Todos los datos observados caen exactamente sobre la recta estimada.
    *   **$R^2 = 0$**: La variable $X$ no explica absolutamente nada de la variabilidad de $Y$ a través de una relación lineal.
    *   **En Regresión Simple**: El coeficiente $R^2$ coincide exactamente con el cuadrado del coeficiente de correlación lineal de Pearson ($R^2 = r^2$).

---

## 5. Inferencia sobre la Pendiente: ¿Es útil el modelo?

Para comprobar si la variable $X$ realmente ayuda a explicar e influir sobre $Y$, realizamos un contraste de hipótesis sobre el parámetro teórico $\beta_1$:
*   $H_0: \beta_1 = 0$ (la variable $X$ no influye linealmente sobre $Y$. La recta es horizontal).
*   $H_1: \beta_1 \neq 0$ (existe relación lineal significativa entre $X$ e $Y$).

Si rechazamos $H_0$ (p-valor $< 0.05$), concluimos que el modelo es linealmente significativo y útil para predecir $Y$ a través de $X$.

---

## 6. Modelado de Regresión en R

R permite ajustar modelos lineales fácilmente con la función `lm()` (linear model):

```R
# Datos: Tamaño de la base de datos (GB) y tiempo de consulta (ms)
tamano_db <- c(1, 2, 5, 10, 20, 30, 50)
tiempo_ms <- c(15, 25, 45, 110, 210, 320, 480)

# 1. Ajustar el modelo lineal Y = beta0 + beta1 * X
modelo <- lm(tiempo_ms ~ tamano_db)

# 2. Ver resumen estadístico detallado del modelo
summary(modelo)
# El summary nos mostrará:
# - Estimaciones para beta0 (Intercept) y beta1 (tamano_db).
# - Los p-valores de significación para cada parámetro (Pr(>|t|)).
# - El coeficiente de determinación R-squared (Multiple R-squared).

# 3. Dibujar la nube de puntos y la recta estimada
plot(tamano_db, tiempo_ms, main="Regresión Lineal: DB vs Tiempo",
     xlab="Tamaño DB (GB)", ylab="Tiempo de Respuesta (ms)", pch=19, col="darkblue")
abline(modelo, col="red", lwd=2) # Añade la recta estimada de MCO
```

---

## 7. Ejercicios Resueltos

### Ejercicio 1
Se analizan datos de 5 servidores para estudiar la relación entre la carga de CPU promedio ($X$, en %) y la temperatura interna del chasis ($Y$, en ºC). Se dispone de las siguientes observaciones: $\{(10, 40), (30, 50), (50, 60), (70, 70), (90, 80)\}$.
1. Estimar por MCO la recta de regresión lineal $Y = \beta_0 + \beta_1 X$.
2. Interpretar detalladamente los coeficientes obtenidos.
3. Calcular el coeficiente de determinación $R^2$ e interpretar el resultado.

**Solución**:
1.  **Cálculo de coeficientes de MCO**:
    *   Tamaño de la muestra $n = 5$.
    *   Medias muestrales:
        $$\bar{x} = \frac{10 + 30 + 50 + 70 + 90}{5} = 50\%$$
        $$\bar{y} = \frac{40 + 50 + 60 + 70 + 80}{5} = 60^\circ\text{C}$$
    *   Covarianza $s_{xy}$:
        $$\sum x_i y_i = (10 \times 40) + (30 \times 50) + (50 \times 60) + (70 \times 70) + (90 \times 80)$$
        $$\sum x_i y_i = 400 + 1500 + 3000 + 4900 + 7200 = 17000$$
        $$s_{xy} = \left(\frac{17000}{5}\right) - (50 \times 60) = 3400 - 3000 = 400$$
    *   Varianza de $X$ ($s_x^2$):
        $$\sum x_i^2 = 10^2 + 30^2 + 50^2 + 70^2 + 90^2 = 100 + 900 + 2500 + 4900 + 8100 = 16500$$
        $$s_x^2 = \left(\frac{16500}{5}\right) - 50^2 = 3300 - 2500 = 800$$
    *   Estimación de la Pendiente ($\hat{\beta}_1$):
        $$\hat{\beta}_1 = \frac{s_{xy}}{s_x^2} = \frac{400}{800} = 0.5$$
    *   Estimación del Intercepto ($\hat{\beta}_0$):
        $$\hat{\beta}_0 = \bar{y} - \hat{\beta}_1 \bar{x} = 60 - (0.5 \times 50) = 60 - 25 = 35$$
    *   La recta estimada es:
        $$\hat{Y} = 35 + 0.5 X$$

2.  **Interpretación de coeficientes**:
    *   **$\hat{\beta}_0 = 35$**: Si la carga de CPU es del 0%, la temperatura esperada del servidor es de 35ºC.
    *   **$\hat{\beta}_1 = 0.5$**: Por cada aumento del 1% en la carga de la CPU, la temperatura del servidor aumenta en promedio 0.5ºC de forma lineal.

3.  **Cálculo de $R^2$**:
    *   Calculamos la varianza de $Y$ ($s_y^2$):
        $$\sum y_i^2 = 40^2 + 50^2 + 60^2 + 70^2 + 80^2 = 1600 + 2500 + 3600 + 4900 + 6400 = 19000$$
        $$s_y^2 = \left(\frac{19000}{5}\right) - 60^2 = 3800 - 3600 = 200$$
    *   Calculamos el coeficiente de correlación de Pearson $r$:
        $$r = \frac{s_{xy}}{s_x s_y} = \frac{400}{\sqrt{800}\sqrt{200}} = \frac{400}{\sqrt{160000}} = \frac{400}{400} = 1.0$$
    *   Por tanto, el coeficiente $R^2$ es:
        $$R^2 = r^2 = 1.0^2 = 1.0$$
    *   **Interpretación**: El valor de $R^2 = 1.0$ (100%) indica que el ajuste lineal es perfecto. El 100% de la variabilidad observada en la temperatura se explica completamente a través del uso de la CPU mediante el modelo de regresión lineal. Todos los servidores se alinean perfectamente sobre la recta estimada.


<div style="page-break-after: always;"></div>

# Glosario de Términos

*   **Asimetría (Skewness)**: Medida que determina si los datos de una distribución se extienden de manera uniforme en torno a la media o si presentan una cola más larga hacia uno de los lados.
*   **Axiomas de Kolmogorov**: Tres postulados matemáticos formulados por Andréi Kolmogorov en 1933 que establecen los requisitos mínimos que debe cumplir una función para ser considerada una probabilidad válida.
*   **Coeficiente de Determinación ($R^2$)**: Proporción de la variabilidad total de la variable dependiente $Y$ que es explicada o predicha por el modelo de regresión lineal. Oscila entre 0 y 1.
*   **Coeficiente de Correlación de Pearson ($r$)**: Medida adimensional que evalúa la dirección y fuerza de la relación lineal entre dos variables cuantitativas. Acotada entre -1 y 1.
*   **Covarianza**: Medida que indica la dirección (positiva o negativa) de la relación lineal entre dos variables cuantitativas a partir de sus desvíos conjuntos respecto a sus medias.
*   **Cuasivarianza**: Estimador corregido de la varianza poblacional que divide la suma de desvíos al cuadrado por $n-1$ (en lugar de $n$) para eliminar el sesgo sistemático.
*   **Desviación Típica o Estándar**: Raíz cuadrada de la varianza. Mide la dispersión en las mismas unidades físicas originales que los datos observados.
*   **Error Tipo I ($\alpha$)**: Error cometido al rechazar la hipótesis nula cuando en realidad es verdadera (falso positivo).
*   **Error Tipo II ($\beta$)**: Error cometido al no rechazar la hipótesis nula cuando en realidad es falsa (falso negativo).
*   **Esperanza Matemática ($E[X]$)**: Valor promedio teórico a largo plazo de una variable aleatoria si el experimento se repitiera un número infinito de veces.
*   **Estimador**: Función matemática aplicada sobre una muestra (estadístico) con el propósito de aproximar el valor de un parámetro poblacional desconocido.
*   **Función de Densidad**: Función que describe la distribución de probabilidad de una variable aleatoria continua. El área bajo su curva determina la probabilidad de que la variable tome valores en un intervalo.
*   **Hipótesis Nula ($H_0$)**: Afirmación sobre un parámetro poblacional que se asume como cierta por defecto, representando ausencia de efecto o cambio.
*   **Intervalo de Confianza**: Rango de valores construido a partir de una muestra que, con un nivel de confianza predeterminado ($1-\alpha$), se espera que contenga el parámetro poblacional real.
*   **Mínimos Cuadrados Ordinarios (MCO)**: Método de optimización para estimar los coeficientes de una regresión lineal mediante la minimización de la suma de los residuos (errores de predicción) al cuadrado.
*   **Muestra Aleatoria Simple (m.a.s.)**: Subconjunto de observaciones independientes extraídas de una población, donde cada observación sigue la misma distribución de probabilidad que la población original (i.i.d.).
*   **p-valor**: Probabilidad de obtener un resultado muestral tan extremo o más que el observado, asumiendo que la hipótesis nula ($H_0$) es verdadera. Si es inferior a $\alpha$, se rechaza $H_0$.
*   **Teorema de Bayes**: Fórmula que permite actualizar la probabilidad a priori de un suceso o causa una vez que se ha observado nueva evidencia o efecto.
*   **Teorema Central del Límite (TCL)**: Teorema que garantiza que la media o la suma de una muestra de variables independientes e idénticamente distribuidas tiende a una distribución Normal cuando el tamaño muestral es grande ($n \ge 30$), sin importar la distribución original de la población.
*   **Varianza**: Promedio de las desviaciones al cuadrado de una variable respecto a su media o esperanza. Mide la dispersión o variabilidad absoluta.

<div style="page-break-after: always;"></div>

# Bibliografía Recomendada

1.  **Devore, J. L. (2016).** *Probability and Statistics for Engineering and the Sciences* (9th ed.). Cengage Learning.
    *   *Nota*: Un clásico didáctico de referencia internacional que incluye abundantes aplicaciones prácticas e intuición para ingenierías.
2.  **Walpole, R. E., Myers, R. H., Myers, S. L., & Ye, K. (2012).** *Probability & Statistics for Engineers & Scientists* (9th ed.). Prentice Hall.
    *   *Nota*: Obra de gran rigor teórico y claridad conceptual sobre variables aleatorias, inferencia estadística y regresión.
3.  **Crawley, M. J. (2012).** *The R Book* (2nd ed.). Wiley.
    *   *Nota*: Considerada la "biblia" práctica para el dominio completo de la programación y la visualización estadística en el entorno R.
4.  **Montgomery, D. C., & Runger, G. C. (2018).** *Applied Statistics and Probability for Engineers* (7th ed.). Wiley.
    *   *Nota*: Enfoque excelente sobre control de procesos y diseño de experimentos, conectando la teoría con la toma de decisiones en ingeniería.
