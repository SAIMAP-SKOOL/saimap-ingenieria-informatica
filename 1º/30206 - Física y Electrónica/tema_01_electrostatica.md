# Tema 1: Electrostática y Campo Eléctrico

La electrostática estudia las cargas eléctricas en reposo y las fuerzas que se ejercen entre ellas. En la ingeniería informática, comprender las leyes del campo eléctrico es indispensable para comprender el almacenamiento de bits en memorias estáticas (Flash y EEPROM), el funcionamiento de las pantallas táctiles capacitivas y el diseño de la microarquitectura de los circuitos integrados de silicio.

---

## 1. Carga Eléctrica y Ley de Coulomb

La **carga eléctrica** es una propiedad intrínseca de la materia. Se presenta en dos tipos: positiva (protones) y negativa (electrones). La carga está cuantizada en unidades de la carga elemental $e \approx 1.602 \cdot 10^{-19} \, \text{C}$ (Culombios).

### Ley de Coulomb
Establece que la fuerza electrostática $\vec{F}$ ejercida entre dos cargas puntuales en reposo $q_1$ y $q_2$ separadas una distancia $r$ es directamente proporcional al producto de las cargas e inversamente proporcional al cuadrado de la distancia que las separa:
$$\vec{F}_{12} = \frac{1}{4\pi\varepsilon_0} \frac{q_1 q_2}{r^2} \hat{r}_{12}$$

donde:
*   $\varepsilon_0 \approx 8.854 \cdot 10^{-12} \, \text{C}^2/\text{N}\cdot\text{m}^2$ es la **permitividad eléctrica del vacío**.
*   La constante de Coulomb es $K_e = \frac{1}{4\pi\varepsilon_0} \approx 8.99 \cdot 10^9 \, \text{N}\cdot\text{m}^2/\text{C}^2$.
*   $\hat{r}_{12}$ es el vector unitario que apunta desde la carga 1 hacia la carga 2.
*   **Atracción/Repulsión**: Cargas del mismo signo se repelen; cargas de signo opuesto se atraen.

---

## 2. Campo Eléctrico ($\vec{E}$) y Potencial Eléctrico ($V$)

### 2.1 Campo Eléctrico
El **campo eléctrico** $\vec{E}$ es una perturbación vectorial del espacio circundante producida por una o más cargas. Se define como la fuerza por unidad de carga de prueba positiva colocada en dicho punto:
$$\vec{E} = \frac{\vec{F}}{q_0}$$

Para una sola carga puntual $q$ en el origen:
$$\vec{E} = \frac{1}{4\pi\varepsilon_0} \frac{q}{r^2} \hat{r}$$

*Principio de Superposición*: Para un sistema de múltiples cargas puntuales, el campo total es la suma vectorial de los campos individuales:
$$\vec{E}_{\text{total}} = \sum_i \vec{E}_i$$

### 2.2 Potencial Eléctrico ($V$)
El potencial eléctrico $V$ en un punto es la **energía potencial por unidad de carga** en dicho punto. A diferencia del campo eléctrico, el potencial es una magnitud **escalar**, lo que simplifica enormemente los cálculos:
$$V = \frac{U}{q_0} = \frac{1}{4\pi\varepsilon_0} \sum_i \frac{q_i}{r_i}$$

El campo eléctrico y el potencial se relacionan a través del operador gradiente:
$$\vec{E} = -\nabla V = -\left( \frac{\partial V}{\partial x} \hat{i} + \frac{\partial V}{\partial y} \hat{j} + \frac{\partial V}{\partial z} \hat{k} \right)$$

---

## 3. Ley de Gauss para el Campo Eléctrico

La **Ley de Gauss** relaciona el flujo del campo eléctrico a través de una superficie cerrada (superficie gaussiana) con la carga neta encerrada en el interior de dicha superficie:

$$\Phi_E = \oint_S \vec{E} \cdot d\vec{S} = \frac{Q_{\text{encerrada}}}{\varepsilon_0}$$

Esta ley, una de las cuatro ecuaciones fundamentales de Maxwell, es extremadamente potente para calcular el campo eléctrico en configuraciones con alta simetría (simetría esférica, cilíndrica o plana).

---

## 4. El Toque Informático

### Simulación de las Líneas de Campo de un Dipolo Eléctrico
Un dipolo eléctrico está formado por dos cargas puntuales de igual magnitud y signos opuestos ($+q$ y $-q$) separadas por una distancia pequeña. 
Visualizar las líneas de fuerza del campo es vital para entender el comportamiento de la polarización en dieléctricos y la propagación de ondas en antenas.

A continuación, escribimos un script en Python que calcula el vector de campo eléctrico en una cuadrícula bidimensional y traza el campo del dipolo mediante `matplotlib`.

```python
import numpy as np
import matplotlib.pyplot as plt

def calcular_campo_carga(q, pos_carga, x_grid, y_grid):
    # k constante de Coulomb
    k = 8.99e9
    
    # Vectores desde la carga a cada punto de la malla
    rx = x_grid - pos_carga[0]
    ry = y_grid - pos_carga[1]
    r3 = (rx**2 + ry**2)**(1.5)
    
    # Campo eléctrico: E = k * q * r / r^3
    # Evitamos divisiones por cero en el punto exacto de la carga
    r3 = np.where(r3 == 0, 1e-20, r3)
    
    Ex = k * q * rx / r3
    Ey = k * q * ry / r3
    return Ex, Ey

# Crear malla 2D
x = np.linspace(-5, 5, 100)
y = np.linspace(-5, 5, 100)
X, Y = np.meshgrid(x, y)

# Definimos el dipolo: Carga + en (-1, 0) y Carga - en (1, 0)
q_mas, pos_mas = 1e-6, [-1.0, 0.0]
q_menos, pos_menos = -1e-6, [1.0, 0.0]

Ex1, Ey1 = calcular_campo_carga(q_mas, pos_mas, X, Y)
Ex2, Ey2 = calcular_campo_carga(q_menos, pos_menos, X, Y)

# Superposición de campos
Ex_total = Ex1 + Ex2
Ey_total = Ey1 + Ey2

# Gráfico
plt.figure(figsize=(8, 8))
plt.streamplot(X, Y, Ex_total, Ey_total, color='blue', density=1.5, linewidth=1, arrowsize=1.2)
plt.scatter([pos_mas[0]], [pos_mas[1]], color='red', s=100, label='Carga positiva (+q)')
plt.scatter([pos_menos[0]], [pos_menos[1]], color='black', s=100, label='Carga negativa (-q)')
plt.title("Líneas de Campo Eléctrico de un Dipolo")
plt.xlabel("x (metros)")
plt.ylabel("y (metros)")
plt.legend()
plt.grid(True)
plt.show()
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Calcular el potencial eléctrico en el origen ($0, 0$) generado por dos cargas puntuales: $q_1 = 2 \, \mu\text{C}$ situada en el punto $(0, 3) \, \text{m}$ y $q_2 = -4 \, \mu\text{C}$ situada en el punto $(4, 0) \, \text{m}$.

**Solución:**
1.  **Identificar las distancias al origen**:
    *   Para $q_1$: la distancia al origen es $r_1 = \sqrt{0^2 + 3^2} = 3 \, \text{m}$.
    *   Para $q_2$: la distancia al origen es $r_2 = \sqrt{4^2 + 0^2} = 4 \, \text{m}$.
2.  **Calcular los potenciales individuales usando $V = \frac{K_e q}{r}$**:
    *   Constante $K_e \approx 9 \cdot 10^9 \, \text{N}\cdot\text{m}^2/\text{C}^2$.
    *   $V_1 = \frac{9 \cdot 10^9 \cdot (2 \cdot 10^{-6})}{3} = \frac{18 \cdot 10^3}{3} = 6000 \, \text{V}$.
    *   $V_2 = \frac{9 \cdot 10^9 \cdot (-4 \cdot 10^{-6})}{4} = \frac{-36 \cdot 10^3}{4} = -9000 \, \text{V}$.
3.  **Sumar los potenciales (superposición escalar)**:
    $$V_{\text{total}} = V_1 + V_2 = 6000 - 9000 = -3000 \, \text{V}$$

El potencial eléctrico en el origen es $-3000 \, \text{V}$ (o $-3 \, \text{kV}$).

---

## 6. Ejercicios Propuestos

1.  Determinar la magnitud y dirección de la fuerza que experimenta una carga de prueba $q_0 = 1 \, \text{nC}$ situada en el punto medio entre dos cargas de $q_1 = 5 \, \mu\text{C}$ y $q_2 = 5 \, \mu\text{C}$ separadas por $2 \, \text{m}$.
2.  Utilizando la Ley de Gauss, deducir la expresión del campo eléctrico a una distancia $r$ de una línea de carga infinita con densidad lineal de carga uniforme $\lambda$.
3.  ¿Qué relación existe entre la dirección de las líneas de campo eléctrico en un punto y las superficies equipotenciales en ese mismo lugar del espacio?
