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
