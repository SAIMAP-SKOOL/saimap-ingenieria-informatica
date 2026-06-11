# Tema 1: Números Complejos

Los números complejos amplían el conjunto de los números reales para permitir soluciones a ecuaciones que carecen de ellas en el dominio real, como $x^2 + 1 = 0$. En la ingeniería informática, su aplicación se extiende desde la renderización de gráficos 2D y el procesamiento de señales digitales (transformadas de Fourier) hasta la computación cuántica y la simulación física.

---

## 1. Definición y Representación Geométrica en $\mathbb{C}$

Definimos el conjunto de los **números complejos** (denotado por $\mathbb{C}$) como el conjunto de todos los pares ordenados $(a, b)$ de números reales, dotado de dos operaciones fundamentales: la suma y la multiplicación.

Introducimos la **unidad imaginaria**, denotada por $i$, que satisface la propiedad fundamental:
$$i^2 = -1 \quad \implies \quad i = \sqrt{-1}$$

Un número complejo $z \in \mathbb{C}$ se representa comúnmente en su **forma binómica**:
$$z = a + bi$$
donde:
*   $\text{Re}(z) = a$ es la **parte real** de $z$.
*   $\text{Im}(z) = b$ es la **parte imaginaria** de $z$.

### Plano Complejo (Plano de Argand)
Geométricamente, representamos $\mathbb{C}$ como un plano bidimensional, donde el eje horizontal representa la parte real (eje real) y el eje vertical representa la parte imaginaria (eje imaginario). Cada número complejo $z = a + bi$ se asocia con un punto de coordenadas $(a, b)$, conocido como **afijo** de $z$.

```
       Im (Eje Imaginario)
        ^
        |
      b + . . . . . . * z = (a, b) = a + bi
        |            /|
        |           / |
        |        r /  |
        |         /   |
        |        / \theta
  ------|-------+-----+------> Re (Eje Real)
        |       0     a
        |
```

---

## 2. Formas de Representación

Un número complejo $z = a + bi$ puede expresarse en tres formas matemáticas fundamentales que facilitan diferentes tipos de cálculos algebraicos y geométricos.

### 2.1 Forma Binómica
$$z = a + bi$$
Es idónea para operaciones de suma y resta.

### 2.2 Forma Polar (o Trigonométrica)
Definimos el **módulo** $r = |z|$ como la distancia desde el origen al afijo $(a, b)$:
$$r = |z| = \sqrt{a^2 + b^2}$$

Definimos el **argumento** $\theta = \arg(z)$ como el ángulo medido en radianes desde el eje real positivo en sentido antihorario:
$$\theta = \arctan\left(\frac{b}{a}\right)$$

> [!WARNING]
> La función $\arctan(b/a)$ ordinaria devuelve valores en el rango $[-\pi/2, \pi/2]$. Debemos ajustar el cuadrante del ángulo según los signos de $a$ y $b$:
> *   **I Cuadrante ($a > 0, b \ge 0$):** $\theta = \arctan(b/a)$
> *   **II Cuadrante ($a < 0, b \ge 0$):** $\theta = \arctan(b/a) + \pi$
> *   **III Cuadrante ($a < 0, b < 0$):** $\theta = \arctan(b/a) - \pi$ (o $\arctan(b/a) + \pi$)
> *   **IV Cuadrante ($a > 0, b < 0$):** $\theta = \arctan(b/a)$

Con $r$ y $\theta$, la forma trigonométrica es:
$$z = r(\cos\theta + i\sin\theta)$$
Se abrevia como $z = r_{\theta}$.

### 2.3 Forma Exponencial
Basada en la **fórmula de Euler**:
$$e^{i\theta} = \cos\theta + i\sin\theta$$

Cualquier número complejo $z$ se escribe de forma exponencial como:
$$z = r e^{i\theta}$$
Esta forma es extremadamente potente para multiplicaciones, divisiones y potencias, ya que hereda las propiedades normales de los exponentes exponenciales.

---

## 3. Operaciones Aritméticas en $\mathbb{C}$

Sean $z_1 = a + bi = r_1 e^{i\theta_1}$ y $z_2 = c + di = r_2 e^{i\theta_2}$.

### 3.1 Suma y Resta (Forma Binómica)
Se suman o restan las partes reales y las partes imaginarias por separado:
$$z_1 \pm z_2 = (a \pm c) + (b \pm d)i$$

### 3.2 Multiplicación
*   **En forma binómica:**
    $$z_1 \cdot z_2 = (a + bi)(c + di) = ac + adi + bci + bdi^2 = (ac - bd) + (ad + bc)i$$
*   **En forma polar/exponencial:**
    $$z_1 \cdot z_2 = (r_1 e^{i\theta_1})(r_2 e^{i\theta_2}) = (r_1 r_2) e^{i(\theta_1 + \theta_2)}$$
    *(Multiplicamos los módulos y sumamos los argumentos).*

### 3.3 División
*   **En forma binómica (multiplicando por el conjugado del denominador):**
    El conjugado de $z_2 = c+di$ es $\bar{z}_2 = c-di$.
    $$\frac{z_1}{z_2} = \frac{a+bi}{c+di} \cdot \frac{c-di}{c-di} = \frac{(ac + bd) + (bc - ad)i}{c^2 + d^2} = \frac{ac+bd}{c^2+d^2} + \frac{bc-ad}{c^2+d^2}i$$
*   **En forma polar/exponencial:**
    $$\frac{z_1}{z_2} = \frac{r_1 e^{i\theta_1}}{r_2 e^{i\theta_2}} = \left(\frac{r_1}{r_2}\right) e^{i(\theta_1 - \theta_2)}$$
    *(Dividimos los módulos y restamos los argumentos).*

### 3.4 Potenciación (Fórmula de De Moivre)
Para un número entero $n$:
$$z^n = (r(\cos\theta + i\sin\theta))^n = r^n(\cos(n\theta) + i\sin(n\theta))$$
O en forma exponencial:
$$z^n = (r e^{i\theta})^n = r^n e^{i n\theta}$$

---

## 4. Radicación en $\mathbb{C}$

Dado $z = r e^{i\theta}$, buscamos las soluciones $w = R e^{i\phi}$ a la ecuación $w^n = z$.
Por la fórmula de De Moivre:
$$R^n e^{i n\phi} = r e^{i\theta}$$

Esto implica que:
1.  $R^n = r \implies R = \sqrt[n]{r}$ (raíz real positiva del módulo).
2.  $n\phi = \theta + 2k\pi \implies \phi_k = \frac{\theta + 2k\pi}{n}$ para $k = 0, 1, 2, \dots, n-1$.

Por tanto, un número complejo $z \ne 0$ tiene exactamente **$n$ raíces $n$-ésimas** distintas, denotadas por $w_k$:
$$w_k = \sqrt[n]{r} \exp\left(i \frac{\theta + 2k\pi}{n}\right) \quad \text{para } k = 0, 1, \dots, n-1$$

### Interpretación Geométrica
Los afijos de las $n$ raíces de un número complejo se sitúan sobre una circunferencia de radio $\sqrt[n]{r}$ centrada en el origen y forman los vértices de un **polígono regular de $n$ lados**.

---

## 5. El Toque Informático

### 5.1 Rotaciones en Gráficos Computacionales 2D
En gráficos bidimensionales, rotar un punto $(x, y)$ un ángulo $\alpha$ alrededor del origen es algebraicamente idéntico a multiplicar el número complejo $p = x + yi$ por el número complejo unitario de rotación $R = e^{i\alpha} = \cos\alpha + i\sin\alpha$:

$$p' = p \cdot R = (x + yi)(\cos\alpha + i\sin\alpha) = (x\cos\alpha - y\sin\alpha) + (x\sin\alpha + y\cos\alpha)i$$

Esto produce exactamente las coordenadas de la matriz de rotación 2D estándar:
$$\begin{pmatrix} x' \\ y' \end{pmatrix} = \begin{pmatrix} \cos\alpha & -\sin\alpha \\ \sin\alpha & \cos\alpha \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix}$$

### 5.2 Generación de Fractales (El Conjunto de Julia)
El conjunto de Julia se define mediante la iteración de la función compleja cuadratura:
$$z_{n+1} = z_n^2 + c$$
donde $c \in \mathbb{C}$ es una constante. Dependiendo del valor inicial $z_0$, la sucesión puede divergir al infinito o permanecer acotada.

A continuación, implementamos en Python una visualización del conjunto de Julia para mostrar el poder del plano complejo en la generación de patrones auto-similares.

```python
import numpy as np
import matplotlib.pyplot as plt

def generar_julia(c, ancho=800, alto=800, iter_max=300):
    # Definimos el plano complejo
    x = np.linspace(-1.5, 1.5, ancho)
    y = np.linspace(-1.5, 1.5, alto)
    x_grid, y_grid = np.meshgrid(x, y)
    
    # Creamos la matriz de números complejos z = x + iy
    z = x_grid + 1j * y_grid
    
    # Matriz para almacenar en qué iteración diverge cada punto
    fractal = np.zeros(z.shape, dtype=int)
    
    # Máscara lógica para los puntos que no han divergido
    activo = np.ones(z.shape, dtype=bool)
    
    for i in range(iter_max):
        # Iteración z = z^2 + c solo para puntos activos
        z[activo] = z[activo]**2 + c
        
        # Un punto diverge si su módulo supera 2 (módulo al cuadrado > 4)
        diverge = np.abs(z) > 2.0
        
        # Guardamos la iteración de divergencia
        fractal[diverge & activo] = i
        
        # Desactivamos los puntos divergentes
        activo[diverge] = False
        
    return fractal

# Constante de Julia clásica (puedes probar con otros valores como -0.8 + 0.156j)
c_julia = -0.7 + 0.27015j
fractal = generar_julia(c_julia)

# Visualización
plt.figure(figsize=(8, 8))
plt.imshow(fractal, cmap='twilight_shifted', extent=[-1.5, 1.5, -1.5, 1.5])
plt.colorbar(label='Velocidad de divergencia (iteraciones)')
plt.title(f"Conjunto de Julia para $c = {c_julia}$")
plt.xlabel("Re(z)")
plt.ylabel("Im(z)")
plt.show()
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Calcular todas las raíces cúbicas de $z = -8i$ y representarlas geométricamente.

**Solución:**
1.  **Expresar $z$ en forma polar:**
    *   Módulo: $r = |-8i| = \sqrt{0^2 + (-8)^2} = 8$.
    *   Argumento: El número está sobre el eje imaginario negativo, por tanto, $\theta = \frac{3\pi}{2}$ (o $-\frac{\pi}{2}$).
    *   Entonces, $z = 8 e^{i \frac{3\pi}{2}}$.

2.  **Calcular las raíces cúbicas usando la fórmula de radicación:**
    $$w_k = \sqrt[3]{8} \exp\left(i \frac{\frac{3\pi}{2} + 2k\pi}{3}\right) \quad \text{para } k = 0, 1, 2$$
    $$w_k = 2 \exp\left(i \left(\frac{\pi}{2} + \frac{2k\pi}{3}\right)\right)$$

3.  **Evaluar para cada valor de $k$:**
    *   **$k = 0$:**
        $$w_0 = 2 e^{i \pi/2} = 2(\cos(\pi/2) + i\sin(\pi/2)) = 2(0 + i) = 2i$$
    *   **$k = 1$:**
        $$w_1 = 2 e^{i (\pi/2 + 2\pi/3)} = 2 e^{i (7\pi/6)}$$
        $$w_1 = 2\left(\cos\frac{7\pi}{6} + i\sin\frac{7\pi}{6}\right) = 2\left(-\frac{\sqrt{3}}{2} - \frac{1}{2}i\right) = -\sqrt{3} - i$$
    *   **$k = 2$:**
        $$w_2 = 2 e^{i (\pi/2 + 4\pi/3)} = 2 e^{i (11\pi/6)}$$
        $$w_2 = 2\left(\cos\frac{11\pi}{6} + i\sin\frac{11\pi}{6}\right) = 2\left(\frac{\sqrt{3}}{2} - \frac{1}{2}i\right) = \sqrt{3} - i$$

4.  **Representación geométrica:**
    Los afijos $w_0 = 2i$, $w_1 = -\sqrt{3} - i$, $w_2 = \sqrt{3} - i$ están en un círculo de radio 2 y forman los vértices de un triángulo equilátero.

---

## 7. Ejercicios Propuestos

1.  Expresar el número complejo $z = -1 + \sqrt{3}i$ en forma polar y exponencial, y hallar $z^6$ usando la fórmula de De Moivre.
2.  Resolver en $\mathbb{C}$ la ecuación polinómica $z^4 + 16 = 0$.
3.  Demostrar que para cualquier $z \in \mathbb{C}$, el producto $z \cdot \bar{z}$ es un número real no negativo igual a $|z|^2$.
