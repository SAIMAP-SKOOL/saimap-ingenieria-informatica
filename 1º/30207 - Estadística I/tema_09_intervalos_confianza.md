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
