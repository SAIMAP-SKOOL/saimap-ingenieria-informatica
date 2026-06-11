# Tema 3: Series Matemáticas

Una serie matemática es la suma de los términos de una sucesión infinita. En informática, las series sustentan los algoritmos de aproximación numérica, la teoría de la información y los métodos de discretización de señales. Además, su cálculo práctico introduce un desafío clave en la computación: las limitaciones de precisión de la aritmética de coma flotante.

---

## 1. Definición y Concepto de Convergencia

Dada una sucesión $\{a_n\}_{n=1}^{\infty}$, definimos la **serie matemática** asociada como la suma formal de sus infinitos términos:
$$\sum_{n=1}^{\infty} a_n = a_1 + a_2 + a_3 + \dots$$

Para estudiar su validez matemática, definimos la sucesión de **sumas parciales** $\{S_N\}_{N=1}^{\infty}$:
$$S_N = \sum_{n=1}^{N} a_n = a_1 + a_2 + \dots + a_N$$

*   Si la sucesión de sumas parciales $\{S_N\}$ converge a un límite finito $S \in \mathbb{R}$ cuando $N \to \infty$, decimos que la serie es **convergente** y que su suma es $S$:
    $$\sum_{n=1}^{\infty} a_n = \lim_{N\to\infty} S_N = S$$
*   Si $\lim_{N\to\infty} S_N = \pm\infty$, la serie es **divergente**.
*   Si el límite de $\{S_N\}$ no existe, la serie es **oscilante**.

> [!IMPORTANT]
> **Condición Necesaria de Convergencia (Criterio del Término General):**
> Si la serie $\sum_{n=1}^{\infty} a_n$ es convergente, entonces el límite de su término general cuando $n \to \infty$ debe ser cero:
> $$\lim_{n\to\infty} a_n = 0$$
> *Atención*: El recíproco no es cierto (por ejemplo, la serie armónica cumple que $\lim_{n\to\infty} 1/n = 0$, pero es divergente). Si el límite no es cero, podemos afirmar inmediatamente que la serie diverge.

---

## 2. Series Notables

Existen ciertas series cuyo comportamiento y suma exacta se conocen de antemano y sirven como referencia de comparación.

### 2.1 Series Geométricas
Tienen la forma:
$$\sum_{n=0}^{\infty} a r^n = a + ar + ar^2 + ar^3 + \dots \quad (\text{donde } a \ne 0)$$
El número $r$ es la **razón** de la serie.
*   **Convergencia**: Converge si y solo si $|r| < 1$.
*   **Suma**: Si converge, su suma es:
    $$S = \frac{a}{1 - r}$$

### 2.2 Series Armónicas y Armónicas Generalizadas ($p$-series)
La **serie armónica** es:
$$\sum_{n=1}^{\infty} \frac{1}{n} = 1 + \frac{1}{2} + \frac{1}{3} + \dots \quad (\text{Divergente})$$

La **serie armónica generalizada** ($p$-serie) tiene la forma:
$$\sum_{n=1}^{\infty} \frac{1}{n^p}$$
*   **Convergencia**: Converge si y solo si $p > 1$.
*   **Divergencia**: Diverge si $p \le 1$.

---

## 3. Criterios de Convergencia para Series de Términos Positivos ($a_n \ge 0$)

Cuando no es posible calcular de forma directa la suma parcial de la serie, determinamos su convergencia mediante criterios cualitativos.

### 3.1 Criterio de Comparación Directa
Sean $\sum a_n$ y $\sum b_n$ series de términos positivos tales que $a_n \le b_n$ para todo $n \ge N_0$:
*   Si $\sum b_n$ converge $\implies \sum a_n$ converge.
*   Si $\sum a_n$ diverge $\implies \sum b_n$ diverge.

### 3.2 Criterio de Comparación por Límite
Sean $\sum a_n$ y $\sum b_n$ series de términos positivos. Si existe el límite:
$$\lim_{n\to\infty} \frac{a_n}{b_n} = L \quad (0 < L < \infty)$$
Ambas series tienen el mismo comportamiento (ambas convergen o ambas divergen).

### 3.3 Criterio del Cociente (o de D'Alembert)
Sea $\sum a_n$ una serie de términos positivos. Supongamos que existe el límite:
$$\lim_{n\to\infty} \frac{a_{n+1}}{a_n} = L$$
*   Si $L < 1$, la serie converge.
*   Si $L > 1$ (o $L = \infty$), la serie diverge.
*   Si $L = 1$, el criterio no es concluyente (se debe usar otro método).

### 3.4 Criterio de la Raíz (o de Cauchy)
Sea $\sum a_n$ una serie de términos positivos. Supongamos que existe el límite:
$$\lim_{n\to\infty} \sqrt[n]{a_n} = L$$
*   Si $L < 1$, la serie converge.
*   Si $L > 1$, la serie diverge.
*   Si $L = 1$, el criterio no es concluyente.

### 3.5 Criterio de la Integral (de Maclaurin-Cauchy)
Sea $f(x)$ una función continua, positiva y decreciente en el intervalo $[1, \infty)$ tal que $f(n) = a_n$. Entonces:
$$\sum_{n=1}^{\infty} a_n \text{ converge} \iff \int_{1}^{\infty} f(x) \, dx \text{ es convergente (finito)}$$

---

## 4. Series Alternadas e Integración de Signo

Una **serie alternada** es aquella cuyos términos cambian alternativamente de signo:
$$\sum_{n=1}^{\infty} (-1)^{n-1} a_n = a_1 - a_2 + a_3 - a_4 + \dots \quad (\text{con } a_n > 0)$$

### Criterio de Leibniz para Series Alternadas
La serie alternada $\sum_{n=1}^{\infty} (-1)^{n-1} a_n$ converge si cumple las dos condiciones siguientes:
1.  La sucesión $\{a_n\}$ es monótona decreciente: $a_{n+1} \le a_n$ para todo $n \ge N_0$.
2.  El límite del término general es cero: $\lim_{n\to\infty} a_n = 0$.

### Convergencia Absoluta y Condicional
*   Decimos que una serie $\sum a_n$ **converge absolutamente** si la serie de sus valores absolutos $\sum |a_n|$ es convergente.
*   Decimos que $\sum a_n$ **converge condicionalmente** si la serie es convergente, pero la serie de sus valores absolutos $\sum |a_n|$ es divergente (por ejemplo, la serie armónica alternada $\sum \frac{(-1)^{n-1}}{n}$).

---

## 5. El Toque Informático

### 5.1 Precisión de Coma Flotante (IEEE 754) e Inestabilidad en la Suma de Series
En un ordenador, los números reales se representan bajo el estándar **IEEE 754** usando mantisa y exponente. La limitación del número de bits de la mantisa introduce el **error de redondeo**.
Al sumar una serie infinita numéricamente (por ejemplo, sumando términos en un bucle), si sumamos un término muy pequeño a una suma acumulada grande, el exponente del término pequeño se ajusta para alinearse con el de la suma. Al desplazar la mantisa a la derecha, los bits menos significativos del término pequeño se pierden por completo.
Este fenómeno se conoce como **pérdida de significación por absorción**.

### 5.2 El Algoritmo de Suma de Kahan
Para mitigar la acumulación de errores de redondeo en bucles que suman grandes series de números, William Kahan diseñó la **Suma de Kahan**. Este algoritmo almacena una variable correctora para "recordar" el error perdido en el paso anterior y añadirlo compensado en la siguiente iteración.

A continuación, implementamos en Python una simulación que calcula la suma de la famosa **Serie de Basel** (cuyo valor exacto es $\pi^2/6 \approx 1.6449340668482264$):
$$\sum_{n=1}^{\infty} \frac{1}{n^2} = \frac{\pi^2}{6}$$
Comparamos una suma ordinaria en coma flotante simple de 32 bits (para amplificar y hacer notable la pérdida de precisión en menos términos) frente a la Suma de Kahan.

```python
import numpy as np

# Definimos el número de términos para aproximar
N = 10_000_000

# Valor teórico exacto de la serie de Basel
valor_teorico = (np.pi ** 2) / 6

# 1. Suma ordinaria de precisión simple (float32)
def suma_ordinaria(n_terminos):
    suma = np.float32(0.0)
    for i in range(1, n_terminos + 1):
        termino = np.float32(1.0 / (i * i))
        suma += termino
    return suma

# 2. Algoritmo de Suma de Kahan (float32)
def suma_kahan(n_terminos):
    suma = np.float32(0.0)
    c = np.float32(0.0) # Variable para compensar el error perdido
    for i in range(1, n_terminos + 1):
        termino = np.float32(1.0 / (i * i))
        y = termino - c           # Suma el error acumulado anterior
        t = suma + y              # Intenta acumular el valor
        c = (t - suma) - y        # Calcula el error real que se ha perdido por redondeo
        suma = t
    return suma

res_ord = suma_ordinaria(N)
res_kah = suma_kahan(N)

print(f"Valor Teórico Exacto:  {valor_teorico:.16f}")
print(f"Suma Ordinaria (32b):  {res_ord:.16f} | Error absoluto: {abs(valor_teorico - res_ord):.2e}")
print(f"Suma de Kahan (32b):   {res_kah:.16f} | Error absoluto: {abs(valor_teorico - res_kah):.2e}")
```

La Suma de Kahan reduce drásticamente el error de redondeo acumulado, acercándose a la precisión teórica con un coste computacional mínimo.

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Determinar si la serie $\sum_{n=1}^{\infty} \frac{n^2}{2^n}$ es convergente o divergente.

**Solución utilizando el Criterio del Cociente (D'Alembert):**
El término general es $a_n = \frac{n^2}{2^n}$, que es positivo para todo $n \ge 1$.
Calculamos el término siguiente:
$$a_{n+1} = \frac{(n+1)^2}{2^{n+1}}$$

Ahora hallamos el límite del cociente de los términos consecutivos:
$$L = \lim_{n\to\infty} \frac{a_{n+1}}{a_n} = \lim_{n\to\infty} \frac{\frac{(n+1)^2}{2^{n+1}}}{\frac{n^2}{2^n}}$$
Simplificamos los términos exponenciales:
$$\frac{2^n}{2^{n+1}} = \frac{2^n}{2^n \cdot 2} = \frac{1}{2}$$

Por lo tanto:
$$L = \lim_{n\to\infty} \frac{(n+1)^2}{2 \cdot n^2} = \frac{1}{2} \lim_{n\to\infty} \left(\frac{n+1}{n}\right)^2$$
Puesto que $\lim_{n\to\infty} \frac{n+1}{n} = 1$:
$$L = \frac{1}{2} \cdot 1^2 = \frac{1}{2}$$

Dado que $L = \frac{1}{2} < 1$, por el criterio del cociente de D'Alembert la serie **converge**.

---

## 7. Ejercicios Propuestos

1.  Determinar la convergencia o divergencia de la siguiente serie aplicando el criterio de comparación por límite o la raíz:
    $$\sum_{n=1}^{\infty} \left(\frac{3n + 1}{4n + 3}\right)^n$$
2.  Estudiar la convergencia absoluta y condicional de la serie alternada:
    $$\sum_{n=1}^{\infty} \frac{(-1)^{n-1}}{\sqrt{n}}$$
3.  Calcular el valor exacto de la suma de la siguiente serie geométrica:
    $$\sum_{n=1}^{\infty} \frac{3 \cdot 2^{n-1}}{5^n}$$
