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
