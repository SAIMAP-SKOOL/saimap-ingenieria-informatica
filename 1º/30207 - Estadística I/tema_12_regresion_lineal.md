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
