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
