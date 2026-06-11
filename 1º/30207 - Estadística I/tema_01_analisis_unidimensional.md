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
