# Tema 7: Integración

La integración es la operación inversa de la derivación. Mientras que la derivada mide tasas de cambio local, la integral acumula variaciones continuas para obtener totales (áreas, volúmenes, acumulaciones temporales). En ingeniería informática, la integración es fundamental para el procesamiento digital de señales, el análisis de datos de probabilidad continua (Machine Learning y Ciencia de Datos), la visión artificial y la simulación física continua.

---

## 1. Integral Indefinida y Métodos de Integración

Dada una función $f(x)$, llamamos **primitiva** o **antiderivada** a una función $F(x)$ tal que:
$$F'(x) = f(x)$$

La **integral indefinida** representa el conjunto de todas las primitivas de $f(x)$ y se denota como:
$$\int f(x) \, dx = F(x) + C$$
donde $C \in \mathbb{R}$ es la constante de integración.

### Métodos Clásicos de Integración
Para calcular primitivas de forma analítica se utilizan cuatro métodos principales:

1.  **Integración por Partes**: Derivado directamente de la regla de derivación del producto.
    $$\int u \, dv = u \cdot v - \int v \, du$$
2.  **Integración por Cambio de Variable (Sustitución)**: Basado en la regla de la cadena.
    $$\int f(g(x)) g'(x) \, dx = \int f(u) \, du \quad (\text{donde } u = g(x))$$
3.  **Integración de Funciones Racionales**: Consiste en descomponer cocientes de polinomios $\frac{P(x)}{Q(x)}$ en fracciones simples:
    *   Si $\text{grado}(P) \ge \text{grado}(Q)$, primero se realiza la división polinómica.
    *   Si $\text{grado}(P) < \text{grado}(Q)$, se factoriza el denominador $Q(x)$ y se descompone en sumas del tipo $\frac{A}{x-a}$, $\frac{B}{(x-a)^2}$, o $\frac{Mx+N}{x^2+px+q}$.

---

## 2. Integral Definida e Integración de Riemann

La **integral de Riemann** formaliza la definición de área bajo una curva dividiendo el dominio en rectángulos elementales.

### Sumas de Riemann
Sea $f(x)$ una función acotada en $[a, b]$. Dividimos el intervalo en $n$ subintervalos de ancho $\Delta x = \frac{b-a}{n}$. 
La **integral definida** se define como el límite de la suma de las áreas de estos rectángulos cuando el ancho tiende a cero (es decir, $n \to \infty$):
$$\int_{a}^{b} f(x) \, dx = \lim_{n\to\infty} \sum_{i=1}^{n} f(x_i^*) \Delta x$$
donde $x_i^*$ es un punto cualquiera dentro del $i$-ésimo subintervalo.

```
     y ^              * curva f(x)
       |             /|
       |          +-+ |
       |          | |*|
       |        +-+ | |
       |        | | | |
  -----+--------+-+-+-+-------> x
       0        a     b
```

---

## 3. Teoremas Fundamentales del Cálculo (TFC)

Estos teoremas conectan formalmente el cálculo diferencial con el cálculo integral.

### Primer Teorema Fundamental del Cálculo
Sea $f(x)$ una función continua en el intervalo $[a, b]$. Definimos la función área (o función integral) $F(x)$ en $[a, b]$ como:
$$F(x) = \int_{a}^{x} f(t) \, dt$$
Entonces, $F(x)$ es derivable en $(a, b)$ y se cumple que:
$$F'(x) = f(x)$$

*Consecuencia*: La derivación y la integración son operaciones recíprocas.

### Segundo Teorema Fundamental (Regla de Barrow)
Si $f(x)$ es continua en $[a, b]$ y $F(x)$ es cualquier primitiva de $f(x)$ en dicho intervalo (es decir, $F'(x) = f(x)$):
$$\int_{a}^{b} f(x) \, dx = F(b) - F(a) = \Big[ F(x) \Big]_a^b$$

---

## 4. Integrales Impropias

Ampliamos la definición de integral definida cuando el intervalo de integración no es acotado o la función presenta singularidades.

### 4.1 Integrales de Primera Especie (Límites infinitos)
El intervalo de integración se extiende al infinito:
$$\int_{a}^{\infty} f(x) \, dx = \lim_{t\to\infty} \int_{a}^{t} f(x) \, dx$$
La integral se dice **convergente** si el límite existe y es finito; en caso contrario, se dice **divergente**.

### 4.2 Integrales de Segunda Especie (Funciones no acotadas)
La función presenta una asíntota vertical (tiende a infinito) en algún punto del intervalo de integración:
*   Si $f(x)$ es discontinua en el extremo derecho $b$:
    $$\int_{a}^{b} f(x) \, dx = \lim_{t\to b^-} \int_{a}^{t} f(x) \, dx$$

---

## 5. El Toque Informático

### Integración en Probabilidad Continua
En ciencia de datos, machine learning y procesamiento digital de imágenes, es común trabajar con variables aleatorias continuas. La probabilidad de que una variable aleatoria $X$ tome un valor dentro de un intervalo $[a, b]$ es el área bajo su **función de densidad de probabilidad** (PDF):
$$P(a \le X \le b) = \int_{a}^{b} f(x) \, dx$$

El ejemplo más emblemático es la **distribución normal (gaussiana)** con media $\mu = 0$ y desviación estándar $\sigma = 1$:
$$f(x) = \frac{1}{\sqrt{2\pi}} e^{-\frac{x^2}{2}}$$

Esta función no tiene una primitiva expresable mediante funciones elementales (es decir, la integral indefinida $\int e^{-x^2} \, dx$ no se puede escribir con polinomios, trigonométricas o logaritmos simples). Por tanto, para calcular esta integral definida en un ordenador se recurre a aproximaciones numéricas o funciones especiales implementadas en librerías científicas (`scipy`).

A continuación, implementamos en Python un script que calcula la probabilidad acumulada de una distribución normal usando integración simbólica/numérica con `sympy` y `scipy.integrate`.

```python
import numpy as np
import sympy as sp
from scipy.integrate import quad

# 1. Enfoque Simbólico con SymPy
def probabilidad_normal_simbolica(a, b):
    x = sp.Symbol('x')
    # Definimos la PDF de la normal estándar
    pdf = (1 / sp.sqrt(2 * sp.pi)) * sp.exp(-x**2 / 2)
    # Calculamos la integral definida simbólicamente
    probabilidad = sp.integrate(pdf, (x, a, b))
    return probabilidad.evalf()

# 2. Enfoque Numérico Rápido con SciPy
def probabilidad_normal_numerica(a, b):
    # PDF normal estándar definida en Python
    pdf = lambda x: (1.0 / np.sqrt(2.0 * np.pi)) * np.exp(-x**2 / 2.0)
    # quad realiza la integración de Riemann numérica adaptativa de Gauss-Kronrod
    resultado, error_estimado = quad(pdf, a, b)
    return resultado, error_estimado

# Queremos calcular la probabilidad dentro de "una desviación estándar" [-1, 1]
# Teóricamente, esto debería ser aproximadamente 68.27% (regla empírica)
lim_inf = -1.0
lim_sup = 1.0

prob_simb = probabilidad_normal_simbolica(lim_inf, lim_sup)
prob_num, error_num = probabilidad_normal_numerica(lim_inf, lim_sup)

print(f"Intervalo de integración: [{lim_inf}, {lim_sup}]")
print(f"Probabilidad Simbólica (SymPy): {prob_simb}")
print(f"Probabilidad Numérica (SciPy):  {prob_num:.8f} (Error estimado: {error_num:.2e})")
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Calcular la integral definida:
$$\int_{0}^{1} x e^x \, dx$$

**Solución usando la Regla de Barrow e Integración por Partes:**
1.  **Encontrar la primitiva analítica (integral indefinida)**:
    $$\int x e^x \, dx$$
    Aplicamos integración por partes con la fórmula $\int u \, dv = u \cdot v - \int v \, du$:
    *   Elegimos $u = x \implies du = dx$
    *   Elegimos $dv = e^x \, dx \implies v = e^x$
    Sustituyendo en la fórmula:
    $$\int x e^x \, dx = x e^x - \int e^x \, dx = x e^x - e^x = e^x(x - 1)$$
    Nuestra primitiva es $F(x) = e^x(x - 1)$.
2.  **Aplicar el Segundo Teorema Fundamental del Cálculo (Barrow)**:
    $$\int_{0}^{1} x e^x \, dx = \Big[ e^x(x - 1) \Big]_0^1$$
    *   Evaluando en el extremo superior ($x = 1$):
        $$F(1) = e^1(1 - 1) = e \cdot 0 = 0$$
    *   Evaluando en el extremo inferior ($x = 0$):
        $$F(0) = e^0(0 - 1) = 1 \cdot (-1) = -1$$
3.  **Restar los valores obtenidos**:
    $$\int_{0}^{1} x e^x \, dx = F(1) - F(0) = 0 - (-1) = 1$$

El área exacta bajo la curva en el intervalo $[0, 1]$ es $1$.

---

## 7. Ejercicios Propuestos

1.  Calcular la integral indefinida mediante el método de descomposición en fracciones simples:
    $$\int \frac{2x + 3}{x^2 - 3x + 2} \, dx$$
2.  Calcular la siguiente integral definida por cambio de variable:
    $$\int_{0}^{2} x \sqrt{x^2 + 1} \, dx$$
3.  Determinar la convergencia o divergencia de la siguiente integral impropia de primera especie:
    $$\int_{1}^{\infty} \frac{1}{x^3} \, dx$$
    Si es convergente, calcular su valor exacto.
