# MANUAL COMPLETO DE FÍSICA Y ELECTRÓNICA
### Grado en Ingeniería Informática - 1º Curso

Este documento unifica todos los temas del plan de estudio de electromagnetismo, teoría de circuitos y electrónica básica en un único manual para facilitar su lectura, impresión o conversión a formatos como PDF.

---

## Índice General

*   **Bloque 1: Campo Eléctrico, Magnetismo y Oscilaciones**
    *   [Tema 1: Electrostática y Campo Eléctrico](#tema-1-electrostática-y-campo-eléctrico)
    *   [Tema 2: Propiedades Eléctricas de la Materia](#tema-2-propiedades-eléctricas-de-la-materia-aislantes-conductores-y-su-relación-con-resistencias-y-condensadores)
    *   [Tema 3: Campo Magnético y Propiedades Magnéticas de la Materia](#tema-3-campo-magnético-y-propiedades-magnéticas-de-la-materia-fundamento-de-las-bobinas)
    *   [Tema 4: Ondas Electromagnéticas: Señales y Transmisión de Información](#tema-4-ondas-electromagnéticas-señales-y-transmisión-de-información)
*   **Bloque 2: Teoría de Circuitos**
    *   [Tema 5: Circuitos Eléctricos: Fundamentos y Leyes de Tensión y Corriente](#tema-5-circuitos-eléctricos-fundamentos-y-leyes-de-tensión-y-corriente-leyes-de-kirchhoff)
    *   [Tema 6: Técnicas para el Análisis de Circuitos Resistivos](#tema-6-técnicas-para-el-análisis-de-circuitos-resistivos-nudos-mallas-thévenin-y-norton)
    *   [Tema 7: Circuitos Básicos con Condensadores y Bobinas (Comportamiento Transitorio)](#tema-7-circuitos-básicos-con-condensadores-y-bobinas-comportamiento-transitorio)
    *   [Tema 8: Circuitos Resistivos con Fuentes Senoidales (Corriente Alterna y Fasores)](#tema-8-circuitos-con-fuentes-senoidales-corriente-alterna-y-fasores)
    *   [Tema 9: Fundamentos de Instalaciones Eléctricas](#tema-9-fundamentos-de-instalaciones-eléctricas)
*   **Bloque 3: Electrónica Básica y Digital**
    *   [Tema 10: Fundamentos de Electrónica: El Diodo y el Transistor](#tema-10-fundamentos-de-electrónica-el-diodo-y-el-transistor)
    *   [Tema 11: Familias Lógicas Digitales: TTL y CMOS](#tema-11-familias-lógicas-digitales-ttl-y-cmos)
*   **Secciones Finales**
    *   [Glosario de Términos](#glosario-de-términos)
    *   [Bibliografía Recomendada](#bibliografía-recomendada)

<div style="page-break-after: always;"></div>

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


<div style="page-break-after: always;"></div>

# Tema 2: Propiedades Eléctricas de la Materia

El comportamiento de los dispositivos electrónicos depende de cómo interactúan sus materiales constituyentes con los portadores de carga (electrones). Este tema clasifica los materiales en conductores, aislantes y dieléctricos, y analiza los dos componentes pasivos fundamentales de almacenamiento y limitación de corriente: el condensador y la resistencia.

---

## 1. Conductores y Aislantes (Dieléctricos)

### 1.1 Conductores
Son materiales que poseen electrones libres en su banda de conducción (típicamente metales como cobre, aluminio u oro) que pueden moverse con facilidad ante la presencia de un campo eléctrico.
*   **Conductores en equilibrio electrostático**:
    1.  El campo eléctrico en el interior del conductor es nulo ($\vec{E}_{\text{int}} = 0$).
    2.  Cualquier exceso de carga neta se acumula exclusivamente en la superficie exterior.
    3.  El potencial eléctrico es el mismo en todos los puntos del conductor (superficie equipotencial).

### 1.2 Aislantes y Dieléctricos
Son materiales donde los electrones están fuertemente ligados a los núcleos atómicos, impidiendo el flujo de corriente. Cuando un aislante se introduce en un campo eléctrico, se denomina **dieléctrico**. Los átomos o moléculas del dieléctrico se polarizan (forman dipolos microscópicos orientados con el campo externo), lo que genera un campo eléctrico inducido interno opuesto al campo externo, **reduciendo el campo neto**.

---

## 2. Resistencia Eléctrica y Ley de Ohm

### Resistencia ($R$)
Es la oposición que presenta un material al paso de la corriente eléctrica. La resistencia de un conductor cilíndrico de longitud $L$ y sección transversal $S$ es:
$$R = \rho \frac{L}{S}$$
donde $\rho$ (ohmios por metro, $\Omega\cdot\text{m}$) es la **resistividad**, una propiedad intrínseca del material que depende de la temperatura.

### Ley de Ohm
Establece que la caída de tensión o diferencia de potencial $V$ en los extremos de una resistencia es directamente proporcional a la intensidad de corriente $I$ que circula por ella:
$$V = I \cdot R$$

---

## 3. Condensadores y Capacidad ($C$)

Un **condensador** (o capacitor) es un dispositivo formado por dos conductores cargados con cargas iguales y opuestas ($+Q$ y $-Q$) separados por un aislante o vacío. Su función principal es almacenar energía en forma de campo eléctrico.

### Capacidad ($C$)
Es la relación entre la magnitud de la carga en cualquiera de las placas y la diferencia de potencial entre ellas:
$$C = \frac{Q}{V}$$
La unidad de medida es el **Faradio** ($\text{F} = \text{C}/\text{V}$). Dado que el Faradio es una unidad gigante, se utilizan submúltiplos ($\mu\text{F}$, $\text{nF}$, $\text{pF}$).

Para un condensador de placas planas y paralelas de área $A$ separadas por una distancia $d$:
*   En el vacío: $C_0 = \varepsilon_0 \frac{A}{d}$
*   Con un dieléctrico: $C = \kappa C_0 = \varepsilon \frac{A}{d}$ (donde $\kappa \ge 1$ es la **constante dieléctrica** del material).

### Asociación de Condensadores
*   **En Paralelo**: Tienen la misma tensión. La capacidad equivalente es la suma:
    $$C_{\text{eq}} = C_1 + C_2 + \dots$$
*   **En Serie**: Almacenan la misma carga. La inversa de la capacidad equivalente es la suma de las inversas:
    $$\frac{1}{C_{\text{eq}}} = \frac{1}{C_1} + \frac{1}{C_2} + \dots$$

---

## 4. El Toque Informático

### 4.1 Celdas de Memoria DRAM (Dynamic RAM)
Las celdas de las memorias de acceso aleatorio dinámico (**DRAM**) almacenan los bits '1' y '0' como la presencia o ausencia de carga en un condensador microscópico integrado en el silicio (con capacidad del orden de los femtofaradios, $10^{-15} \, \text{F}$).
*   **La necesidad del refresco (Refresh Rate)**: Puesto que ningún aislante es perfecto, la carga del condensador se fuga lentamente a través del transistor de acceso. Para evitar la pérdida de los datos, el controlador de memoria debe leer y volver a escribir (refrescar) cada una de los millones de celdas de la DRAM cada pocos milisegundos (típicamente cada 64 ms), lo que genera consumo de energía y reduce levemente el rendimiento de lectura del procesador.

### 4.2 Resistencias de Pull-Up y Pull-Down
En electrónica digital y en microcontroladores (como Arduino o Raspberry Pi), cuando un pin de entrada no está conectado a nada (por ejemplo, un interruptor abierto), se encuentra en un estado de **alta impedancia o flotante** ($Hi-Z$). El ruido electromagnético ambiental puede hacer que el pin fluctúe erráticamente entre '0' y '1'.
Para fijar un estado lógico por defecto, se conectan resistencias de Pull-Up (a $V_{cc}$, típicamente 5V o 3.3V) o Pull-Down (a masa, GND), con valores típicos de $10 \, \text{k}\Omega$:

```
    Pull-Up:                             Pull-Down:
    Vcc (5V)                             Entrada (MCU) <------+
       |                                                      |
     [ R ] (10k)                                            [ R ] (10k)
       |                                                      |
    Entrada (MCU) <------+                                   GND
                         |                                    |
                    [ Interruptor ]                      [ Interruptor ]
                         |                                    |
                        GND                                  Vcc
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Tres condensadores de capacidades $C_1 = 2 \, \mu\text{F}$, $C_2 = 4 \, \mu\text{F}$ y $C_3 = 4 \, \mu\text{F}$ se conectan de la siguiente manera: $C_2$ y $C_3$ están en serie entre sí, y esta combinación se conecta en paralelo con $C_1$. Calcular la capacidad equivalente total del circuito.

**Solución:**
1.  **Calcular la equivalente de la rama en serie ($C_{23}$)**:
    $$\frac{1}{C_{23}} = \frac{1}{C_2} + \frac{1}{C_3} = \frac{1}{4} + \frac{1}{4} = \frac{2}{4} = \frac{1}{2} \implies C_{23} = 2 \, \mu\text{F}$$
2.  **Calcular la equivalente en paralelo con $C_1$**:
    $$C_{\text{eq}} = C_1 + C_{23} = 2 \, \mu\text{F} + 2 \, \mu\text{F} = 4 \, \mu\text{F}$$

La capacidad equivalente del circuito es $4 \, \mu\text{F}$.

---

## 6. Ejercicios Propuestos

1.  Una barra de cobre de $2 \, \text{m}$ de longitud tiene una sección transversal cuadrada de $1 \, \text{mm}$ de lado. Si la resistividad del cobre es $\rho = 1.7 \cdot 10^{-8} \, \Omega\cdot\text{m}$, calcular su resistencia eléctrica y la caída de tensión cuando circula una corriente de $2 \, \text{A}$.
2.  Un condensador de placas paralelas tiene una capacidad de $100 \, \text{pF}$ en el vacío. Si se duplica el área de las placas, se reduce a la mitad la distancia entre ellas y se introduce un dieléctrico con constante $\kappa = 5$, ¿cuál será su nueva capacidad?
3.  Explicar por qué la presencia de un dieléctrico incrementa la capacidad de un condensador cuando este se mantiene conectado a una diferencia de potencial constante frente a cuando se encuentra aislado y cargado.


<div style="page-break-after: always;"></div>

# Tema 3: Campo Magnético y Propiedades Magnéticas

El magnetismo está íntimamente ligado a la electricidad; las corrientes eléctricas (cargas en movimiento) son las generadoras de los campos magnéticos, y las variaciones temporales de los campos magnéticos inducen corrientes eléctricas. En la ingeniería informática, estos principios físicos sustentan el almacenamiento de datos en discos duros tradicionales (HDD), las fuentes de alimentación conmutadas (transformadores y bobinas) y las nuevas tecnologías de memoria no volátil magnética (MRAM).

---

## 1. Fuerza de Lorentz y Leyes de Campo Magnético ($\vec{B}$)

### Fuerza de Lorentz
Una carga puntual $q$ que se mueve con una velocidad $\vec{v}$ en el seno de un campo magnético $\vec{B}$ experimenta una fuerza magnética dada por el producto vectorial:
$$\vec{F} = q (\vec{v} \times \vec{B})$$

Propiedades:
*   La magnitud de la fuerza es $F = |q| v B \sin\theta$.
*   La fuerza es siempre **perpendicular** a la velocidad y al campo (regla de la mano derecha). Por tanto, la fuerza magnética **no realiza trabajo** sobre la carga (no altera su velocidad lineal, solo desvía su trayectoria).

### 1.2 Ley de Biot-Savart
Determina el campo magnético elemental $d\vec{B}$ generado por un filamento de corriente infinitesimal $I d\vec{l}$ en un punto del espacio situado a una distancia $r$:
$$d\vec{B} = \frac{\mu_0}{4\pi} \frac{I (d\vec{l} \times \hat{r})}{r^2}$$
donde $\mu_0 = 4\pi \cdot 10^{-7} \, \text{T}\cdot\text{m}/\text{A}$ es la **permeabilidad magnética del vacío**.

### 1.3 Ley de Ampère
La circulación del campo magnético a lo largo de cualquier curva cerrada $C$ es proporcional a la corriente neta que atraviesa la superficie delimitada por dicha curva:
$$\oint_C \vec{B} \cdot d\vec{l} = \mu_0 I_{\text{encerrada}}$$

---

## 2. Propiedades Magnéticas de la Materia

Los materiales reaccionan ante campos magnéticos externos debido al espín y movimiento orbital de sus electrones.

1.  **Diamagnetismo**: Presente en materiales sin momentos magnéticos permanentes. El campo inducido se opone débilmente al externo (ej. cobre, silicio).
2.  **Paramagnetismo**: Momentos magnéticos atómicos desordenados térmicamente. Se alinean débilmente a favor del campo externo (ej. aluminio, aire).
3.  **Ferromagnetismo**: Poseen fuertes interacciones de canje que alinean los momentos magnéticos en regiones llamadas **dominios magnéticos**. Mantienen una magnetización permanente al retirar el campo externo (ej. hierro, cobalto, níquel). Son la base del almacenamiento magnético.

---

## 3. Inducción Electromagnética y Bobinas (Inductores)

### Ley de Faraday-Lenz
La variación temporal del flujo magnético $\Phi_B = \iint \vec{B} \cdot d\vec{S}$ a través de una espira conductora induce una **fuerza electromotriz** (fem, $\mathcal{E}$) en ella:
$$\mathcal{E} = -\frac{d\Phi_B}{dt}$$
*La Ley de Lenz (el signo negativo)*: Establece que la corriente inducida tendrá una dirección tal que su propio campo magnético se oponga al cambio de flujo que la produjo.

### Las Bobinas (Inductores)
Un inductor es un componente pasivo formado por un hilo conductor enrollado (solenoide) que almacena energía en forma de campo magnético cuando circula una corriente. Su propiedad característica es la **inductancia** ($L$):
$$L = \frac{N \Phi_B}{I}$$
La unidad de medida es el **Henrio** ($\text{H}$).

La caída de tensión en los extremos de una bobina debido a la autoinducción es:
$$v(t) = L \frac{di(t)}{dt}$$

---

## 4. El Toque Informático

### 4.1 Grabación Magnética en Discos Duros (HDD)
Los platos de un disco duro están recubiertos de una película ferromagnética muy fina.
*   **Escritura**: El cabezal de escritura es un electroimán (bobina arrollada a un núcleo magnético). Al pasar pulsos de corriente, la Ley de Ampère genera un campo magnético concentrado en la punta que magnetiza permanentemente pequeños dominios orientándolos en un sentido u otro (representando '1's y '0's).
*   **Lectura**: Tradicionalmente se basaba en la Ley de Faraday: el paso de la cabeza lectora sobre los cambios de polarización magnética en el plato giratorio inducía pulsos eléctricos de tensión en la bobina lectora. Hoy en día se utilizan sensores magnetorresistivos gigantes (GMR) que cambian su resistencia eléctrica en presencia de campos magnéticos.

### 4.2 Memorias MRAM (Magnetoresistive RAM)
Es una tecnología de memoria no volátil que almacena bits utilizando estados magnéticos en lugar de carga eléctrica (como la DRAM). Cada celda consta de una **unión túnel magnética** (MTJ) formada por dos capas ferromagnéticas separadas por un aislante ultrafino:
*   Una capa tiene magnetización fija.
*   La otra capa es libre y puede reorientarse mediante corrientes eléctricas.
Si las magnetizaciones son paralelas, la resistencia al paso de corriente por efecto túnel es muy baja ('0'); si son antiparalelas, la resistencia es alta ('1'). Combina la velocidad de la SRAM con la no volatilidad de la memoria Flash.

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Un solenoide (bobina) de $10 \, \text{cm}$ de longitud tiene 500 espiras arrolladas y una sección transversal de $2 \, \text{cm}^2$. Si su interior contiene aire (permeabilidad $\mu_0$), calcular su inductancia autoinducida $L$. Si la corriente que circula por ella disminuye de forma uniforme de $3 \, \text{A}$ a $0 \, \text{A}$ en $10 \, \text{ms}$, calcular la magnitud de la fuerza electromotriz inducida $\mathcal{E}$.

**Solución:**
1.  **Calcular el campo magnético en el interior del solenoide**:
    $$B = \mu_0 \frac{N}{l} I$$
2.  **Calcular el flujo magnético a través de una espira ($\Phi_B = B \cdot A$)**:
    $$\Phi_B = \left(\mu_0 \frac{N}{l} I\right) A$$
3.  **Calcular la inductancia $L = \frac{N \Phi_B}{I}$**:
    $$L = \mu_0 \frac{N^2 A}{l}$$
    *   $N = 500$ espiras.
    *   $A = 2 \, \text{cm}^2 = 2 \cdot 10^{-4} \, \text{m}^2$.
    *   $l = 10 \, \text{cm} = 0.1 \, \text{m}$.
    *   $\mu_0 = 4\pi \cdot 10^{-7}$.
    Sustituyendo:
    $$L = (4\pi \cdot 10^{-7}) \frac{500^2 \cdot (2 \cdot 10^{-4})}{0.1} = (4\pi \cdot 10^{-7}) \frac{250000 \cdot 2 \cdot 10^{-4}}{0.1} = (4\pi \cdot 10^{-7}) \frac{50}{0.1} = 4\pi \cdot 10^{-7} \cdot 500 = 6.28 \cdot 10^{-4} \, \text{H} \approx 0.628 \, \text{mH}$$
4.  **Calcular la fem inducida usando $\mathcal{E} = -L \frac{\Delta i}{\Delta t}$**:
    *   $\Delta i = 0 - 3 = -3 \, \text{A}$.
    *   $\Delta t = 10 \cdot 10^{-3} \, \text{s} = 0.01 \, \text{s}$.
    *   $\frac{\Delta i}{\Delta t} = \frac{-3}{0.01} = -300 \, \text{A}/\text{s}$.
    Sustituyendo:
    $$\mathcal{E} = -(6.28 \cdot 10^{-4}) \cdot (-300) = 0.1884 \, \text{V}$$

La inductancia es de $0.628 \, \text{mH}$ y la fem inducida es de $0.188 \, \text{V}$ (o $188 \, \text{mV}$).

---

## 6. Ejercicios Propuestos

1.  Una partícula cargada con $q = 2 \, \text{nC}$ se mueve a una velocidad de $\vec{v} = 3 \cdot 10^5 \hat{i} \, \text{m}/\text{s}$ en un campo magnético uniforme de $\vec{B} = 0.5 \hat{k} \, \text{T}$. Calcular el vector de fuerza magnética que actúa sobre ella.
2.  Deducir el campo magnético generado en el centro de una espira circular conductora de radio $R$ por la que circula una corriente constante $I$ aplicando la Ley de Biot-Savart.
3.  ¿Cómo se puede amortiguar la oscilación de corriente generada en un circuito eléctrico basándose en la Ley de Lenz?


<div style="page-break-after: always;"></div>

# Tema 4: Ondas Electromagnéticas: Señales y Transmisión de Información

Las ondas electromagnéticas son la base física de todas las comunicaciones modernas. La luz visible, las ondas de radio de la WiFi, los enlaces de telefonía móvil y los pulsos de la fibra óptica son manifestaciones del mismo fenómeno físico: la propagación de oscilaciones acopladas de campos eléctricos y magnéticos a través del espacio.

---

## 1. Síntesis de las Ecuaciones de Maxwell

A mediados del siglo XIX, James Clerk Maxwell unificó de forma matemática el electromagnetismo en cuatro ecuaciones fundamentales:

| Ecuación | Nombre | Significado Físico Cualitativo |
| :--- | :--- | :--- |
| $\displaystyle \oint \vec{E} \cdot d\vec{S} = \frac{Q_{\text{enc}}}{\varepsilon_0}$ | Ley de Gauss (Electricidad) | Las cargas eléctricas son las fuentes generadoras de las líneas de campo eléctrico. Existen monopolos eléctricos. |
| $\displaystyle \oint \vec{B} \cdot d\vec{S} = 0$ | Ley de Gauss (Magnetismo) | No existen cargas magnéticas aisladas (monopolos magnéticos). Las líneas de campo magnético son cerradas. |
| $\displaystyle \oint \vec{E} \cdot d\vec{l} = -\frac{d\Phi_B}{dt}$ | Ley de Faraday-Lenz | Un campo magnético variable en el tiempo induce un campo eléctrico rotacional (corriente inducida). |
| $\displaystyle \oint \vec{B} \cdot d\vec{l} = \mu_0 I_{\text{enc}} + \mu_0 \varepsilon_0 \frac{d\Phi_E}{dt}$ | Ley de Ampère-Maxwell | Los campos magnéticos se generan tanto por corrientes eléctricas de conducción como por **campos eléctricos variables** (término de corriente de desplazamiento). |

La genialidad de Maxwell consistió en añadir el término de **corriente de desplazamiento** ($\mu_0 \varepsilon_0 \frac{d\Phi_E}{dt}$) a la Ley de Ampère, lo que permitió demostrar analíticamente la existencia de ondas electromagnéticas autosostenidas.

---

## 2. Propagación de Ondas y Velocidad de la Luz

De las ecuaciones de Maxwell se deduce que un campo eléctrico variable en el tiempo crea un campo magnético variable en el espacio circundante, el cual a su vez induce un campo eléctrico, propagándose indefinidamente como una **onda electromagnética transversal** (donde los campos $\vec{E}$ y $\vec{B}$ oscilan perpendiculares entre sí y a la dirección de propagación $\vec{k}$).

En el vacío, la velocidad de propagación $c$ (velocidad de la luz) se deriva directamente de las constantes fundamentales de electricidad y magnetismo:
$$c = \frac{1}{\sqrt{\varepsilon_0 \mu_0}} \approx 3 \cdot 10^8 \, \text{m}/\text{s}$$

En un medio material con índice de refracción $n \ge 1$:
$$v = \frac{c}{n}$$

La relación fundamental de onda es:
$$v = \lambda \cdot f$$
donde $\lambda$ es la **longitud de onda** (metros) y $f$ es la **frecuencia** (Hercios, $\text{Hz}$).

---

## 3. Espectro Electromagnético y Comunicaciones Informáticas

El **espectro electromagnético** es la clasificación de las ondas según su frecuencia o longitud de onda:

```
  Frecuencia (Hz): 10^3 (Radio) -> 10^9 (Microondas) -> 10^14 (Infrarrojo) -> Visible -> 10^18 (Rayos X)
  Longitud (m):   10^3         -> 10^-1               -> 10^-6             -> 10^-7    -> 10^-10
```

### Canales de Comunicaciones de Red
1.  **WiFi y Bluetooth (Microondas)**:
    *   **Banda de 2.4 GHz**: Tiene mayor longitud de onda, lo que le permite atravesar mejor obstáculos físicos (paredes), pero ofrece menor ancho de banda y sufre interferencia de microondas y dispositivos domésticos.
    *   **Banda de 5 GHz**: Ofrece mayor velocidad de transmisión debido a su mayor frecuencia, pero su atenuación es mucho más alta al interactuar con obstáculos sólidos.
2.  **Fibra Óptica (Infrarrojo/Luz)**: Utiliza pulsos de luz en el rango del infrarrojo cercano guiados por el principio de **reflexión interna total** dentro de hilos de vidrio ultrapuro, permitiendo la transmisión de terabits por segundo sin interferencias electromagnéticas.

---

## 4. Transmisión de Información y Modulación de Señales

Una onda senoidal pura (onda portadora) no transporta información. Para transmitir datos ('1's y '0's o voz), debemos alterar alguna de sus características físicas en un proceso llamado **modulación**:

1.  **Modulación en Amplitud (AM / ASK)**: Se varía la amplitud de la onda portadora.
2.  **Modulación en Frecuencia (FM / FSK)**: Se varía la frecuencia.
3.  **Modulación en Fase (PM / PSK)**: Se altera la fase o desfase temporal.
4.  **Modulación de Amplitud en Cuadratura (QAM)**: Combina variaciones de amplitud y fase para codificar múltiples bits por símbolo, siendo la tecnología detrás de las conexiones de fibra (FTTH) y redes de telefonía 4G/5G.

---

## 5. El Toque Informático

### Atenuación Física del Medio y Capacidad de Canal (Teorema de Shannon-Hartley)
Los ingenieros de redes deben diseñar protocolos considerando las leyes físicas del electromagnetismo. La capacidad máxima de transmisión de datos $C$ (bits por segundo) de un canal inalámbrico con un ancho de banda de frecuencias $B$ y una relación señal/ruido $S/N$ viene dada por el Teorema de Shannon-Hartley:
$$C = B \log_2\left(1 + \frac{S}{N}\right)$$
La atenuación electromagnética (pérdida de potencia de la señal en el aire) crece con el cuadrado de la frecuencia, lo que limita el alcance práctico de los estándares WiFi de alta velocidad (como WiFi 6E y 7 en la banda de 6 GHz).

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Una señal de una red WiFi opera a una frecuencia de $2.4 \, \text{GHz}$ ($2.4 \cdot 10^9 \, \text{Hz}$) en el vacío. Calcular la longitud de onda $\lambda$ de dicha señal.

**Solución:**
1.  **Identificar las variables**:
    *   Velocidad de propagación en el vacío: $c \approx 3 \cdot 10^8 \, \text{m}/\text{s}$.
    *   Frecuencia: $f = 2.4 \cdot 10^9 \, \text{Hz}$.
2.  **Aplicar la fórmula fundamental de onda $c = \lambda \cdot f$**:
    $$\lambda = \frac{c}{f}$$
3.  **Calcular**:
    $$\lambda = \frac{3 \cdot 10^8}{2.4 \cdot 10^9} = \frac{3}{24} = 0.125 \, \text{m}$$

La longitud de onda de la señal WiFi de 2.4 GHz es de $12.5 \, \text{cm}$.

---

## 7. Ejercicios Propuestos

1.  Una señal de luz infrarroja se propaga por el núcleo de una fibra óptica de silicio con un índice de refracción de $n = 1.5$. Calcular la velocidad de propagación de la luz en el interior de la fibra.
2.  Explicar qué modificación teórica introdujo Maxwell en la Ley de Ampère para resolver la inconsistencia del condensador durante el proceso de carga y descarga, y cómo definió la corriente de desplazamiento.
3.  ¿Por qué las señales de radio AM de onda corta pueden rodear la curvatura de la Tierra a grandes distancias mediante rebote ionosférico mientras que las microondas de la red móvil requieren visibilidad directa entre antenas?


<div style="page-break-after: always;"></div>

# Tema 5: Circuitos Eléctricos: Fundamentos y Leyes de Kirchhoff

La teoría de circuitos es la simplificación física del electromagnetismo clásico (aproximación de parámetros concentrados). En lugar de resolver ecuaciones vectoriales de campo eléctrico y magnético complejos, modelamos el comportamiento de los circuitos utilizando variables escalares sencillas como la tensión y la corriente. Este tema sienta las bases lógicas indispensables para comprender cómo fluye la energía en el hardware de un computador.

---

## 1. Conceptos Básicos y Topología de Circuitos

Un **circuito eléctrico** es una interconexión de elementos eléctricos pasivos (resistencias, condensadores) y activos (fuentes de tensión, baterías) que forman al menos una trayectoria cerrada para la corriente.

### 1.1 Variables del Circuito
*   **Corriente o Intensidad ($I$, $i(t)$)**: Velocidad de flujo de carga eléctrica a través de un conductor. Se mide en Amperios ($\text{A} = \text{C}/\text{s}$).
*   **Tensión o Diferencia de Potencial ($V$, $v(t)$)**: Energía requerida para mover una carga unitaria a través de un elemento. Se mide en Voltios ($\text{V} = \text{J}/\text{C}$).
*   **Potencia ($P$, $p(t)$)**: Energía consumida o entregada por unidad de tiempo. Se mide en Vatios ($\text{W} = \text{J}/\text{s}$):
    $$p(t) = v(t) \cdot i(t)$$

### 1.2 Convenio Pasivo de Signos
*   **Elemento Pasivo (Absorbe potencia)**: La corriente entra por la terminal positiva ($+$) del voltaje del elemento $\implies P = V \cdot I > 0$.
*   **Elemento Activo (Suministra potencia)**: La corriente sale por la terminal positiva ($+$) $\implies P = -V \cdot I < 0$.
*   *Conservación de la Energía*: La suma de la potencia absorbida por todos los elementos de un circuito cerrado debe ser exactamente cero: $\sum P = 0$.

### 1.3 Elementos Topológicos
*   **Nudo (o Nodo)**: Punto de conexión común de tres o más elementos.
*   **Rama**: Elemento individual o camino que une dos nudos.
*   **Lazo (Bucle)**: Cualquier trayectoria cerrada que se puede recorrer sin repetir nudos.
*   **Malla**: Lazo que no contiene ningún otro lazo en su interior.

---

## 2. Leyes de Kirchhoff

Las leyes de Kirchhoff describen el comportamiento de las corrientes y tensiones en base a las leyes de conservación física de la carga y la energía.

### 2.1 Primera Ley de Kirchhoff (Ley de Corrientes, LCK)
Basada en la **conservación de la carga eléctrica**: la carga no se crea ni se destruye en un nudo.
> **LCK**: La suma algebraica de las corrientes que entran a cualquier nudo es igual a la suma de las corrientes que salen de él:
> $$\sum I_{\text{entrantes}} = \sum I_{\text{salientes}} \quad \implies \quad \sum_{j=1}^{k} I_j = 0$$

```
           I1 ->  |  <- I2
                  * (Nudo)
                 / \
         I3 <-  /   \  -> I4
```
Para el nudo anterior: $I_1 + I_2 = I_3 + I_4$.

### 2.2 Segunda Ley de Kirchhoff (Ley de Tensiones, LKT)
Basada en la **conservación de la energía**: el potencial eléctrico en un punto es único, por lo que dar una vuelta completa a un lazo y volver al mismo punto debe dar una variación de energía nula.
> **LKT**: La suma algebraica de las caídas de tensión a lo largo de cualquier lazo cerrado es exactamente cero:
> $$\sum_{j=1}^{m} V_j = 0$$

---

## 3. El Toque Informático

### Montaje Práctico en Placa de Prototipos (Protoboard)
En las sesiones obligatorias de laboratorio, los circuitos teóricos se montan físicamente en una **Protoboard**. Comprender su topología interna evita cortocircuitos destructivos para los componentes electrónicos.

```
  Bus de alimentación (+Vcc)   O===================================O  (Conexión Horizontal)
  Bus de masa (GND)            O===================================O  (Conexión Horizontal)
  
  Pistas de conexión de          O   O   O   O   O   O   O   O   O   O
  componentes                    |   |   |   |   |   |   |   |   |   |  (Conexión Vertical de 5 nodos)
                                 O   O   O   O   O   O   O   O   O   O
                               [Canal de separación central - Aislamiento]
                                 O   O   O   O   O   O   O   O   O   O
                                 |   |   |   |   |   |   |   |   |   |  (Conexión Vertical de 5 nodos)
                                 O   O   O   O   O   O   O   O   O   O
```

*   **Buses Laterales (Alimentación)**: Conectados internamente de forma **horizontal** a lo largo de toda la placa. Se usan para distribuir la tensión de alimentación (+Vcc) y la masa (GND) al circuito.
*   **Columnas de Trabajo (Nodos)**: Conectadas internamente en tiras de 5 agujeros de forma **vertical**. Cada columna es eléctricamente un único **nudo** del circuito.
*   **Canal Central**: Divide la protoboard por la mitad y sirve de aislamiento. Es donde se deben pinchar los circuitos integrados (chips) para que los pines de un lado no entren en cortocircuito con los del otro.

---

## 4. Ejercicios Resueltos

### Ejercicio 1
En el circuito de la malla única de la figura, calcular la intensidad de corriente $I$ que circula por él y verificar la conservación de la potencia.
El circuito consta de una fuente de tensión independiente de $12 \, \text{V}$ en serie con dos resistencias de $4 \, \Omega$ y $2 \, \Omega$.

```
     +---[ 4 ohm ]---+
     |               |
   (12V)           [2 ohm]
     |               |
     +---------------+
```

**Solución:**
1.  **Plantear la LKT en el lazo cerrado**:
    Recorremos el lazo en sentido horario. Entramos por la terminal negativa de la fuente de tensión y por la positiva de las caídas de las resistencias:
    $$-12 + V_{4\Omega} + V_{2\Omega} = 0$$
2.  **Aplicar la Ley de Ohm a las caídas de las resistencias**:
    $$-12 + 4 \cdot I + 2 \cdot I = 0 \implies 6I = 12 \implies I = 2 \, \text{A}$$
3.  **Verificación de la conservación de la potencia ($\sum P = 0$)**:
    *   Potencia entregada por la fuente de tensión:
        $$P_{\text{fuente}} = -V \cdot I = -12 \cdot 2 = -24 \, \text{W} \quad (\text{Activo})$$
    *   Potencia disipada en la resistencia de $4 \, \Omega$:
        $$P_{4\Omega} = I^2 \cdot R = 2^2 \cdot 4 = 16 \, \text{W}$$
    *   Potencia disipada en la resistencia de $2 \, \Omega$:
        $$P_{2\Omega} = I^2 \cdot R = 2^2 \cdot 2 = 8 \, \text{W}$$
    *   Suma de potencias:
        $$\sum P = P_{\text{fuente}} + P_{4\Omega} + P_{2\Omega} = -24 + 16 + 8 = 0 \, \text{W}$$

Se verifica perfectamente la conservación de la potencia.

---

## 5. Ejercicios Propuestos

1.  Dada una red con 3 nudos y 5 ramas, aplicar la LCK para plantear las ecuaciones independientes de corrientes necesarias para resolver el circuito.
2.  Un circuito está formado por una fuente de tensión de $5 \, \text{V}$ conectada a una resistencia de $100 \, \Omega$ en serie con un diodo LED. Si el fabricante del LED especifica que la caída de tensión en el LED es de $2 \, \text{V}$ en funcionamiento, calcular la intensidad de corriente $I$ que circula por el circuito.
3.  Describir qué es un cortocircuito a nivel físico (en términos de resistencia y corriente) y explicar por qué provoca un sobrecalentamiento instantáneo que puede dañar las pistas de cobre de un circuito integrado.


<div style="page-break-after: always;"></div>

# Tema 6: Técnicas para el Análisis de Circuitos Resistivos

Para circuitos complejos con múltiples fuentes y mallas, la aplicación directa de las leyes de Kirchhoff resulta caótica. Este tema introduce metodologías sistemáticas para reducir cualquier red a un sistema ordenado de ecuaciones lineales y analiza los teoremas de Thévenin y Norton, indispensables para simplificar etapas de hardware en diseño de sistemas.

---

## 1. Métodos Sistemáticos: Nudos y Mallas

### 1.1 Método de Análisis por Nudos (Tensiones de Nudo)
Toma como variables del circuito las tensiones de los nudos respecto a un **nudo de referencia** (masa o GND, cuyo potencial es 0V).
1.  Identificar los nudos esenciales del circuito. Elegir uno como referencia.
2.  Aplicar la Primera Ley de Kirchhoff (LCK) a cada nudo restante, expresando las corrientes de rama en función de las tensiones de nudo mediante la Ley de Ohm:
    $$I = \frac{V_{\text{origen}} - V_{\text{destino}}}{R}$$
3.  Resolver el sistema de ecuaciones para hallar las tensiones de nudo.

### 1.2 Método de Análisis por Mallas (Corrientes de Malla)
Toma como variables corrientes ficticias que circulan en lazos cerrados independientes (mallas).
1.  Identificar las mallas del circuito y asignar una corriente de malla (normalmente en sentido horario).
2.  Aplicar la Segunda Ley de Kirchhoff (LKT) a cada malla, expresando las caídas de tensión en función de las corrientes de malla. (Si una resistencia es compartida por dos mallas, la corriente neta es la diferencia de ambas corrientes).
3.  Resolver el sistema de ecuaciones.

---

## 2. Teoremas de Simplificación de Redes: Thévenin y Norton

Cualquier red lineal activa de dos terminales (A y B) puede simplificarse por un circuito equivalente muy sencillo.

```
       Circuito Lineal Activo                 Circuito Equivalente
      +------------------------+ A          +-----[ Rth ]-----+ A
      |                        |            |                 |
      |                        |          (Vth)               |
      |                        |            |                 |
      +------------------------+ B          +-----------------+ B
```

### 2.1 Teorema de Thévenin
Establece que el circuito puede sustituirse por una **fuente de tensión equivalente $V_{\text{th}}$ en serie con una resistencia equivalente $R_{\text{th}}$**:
*   $V_{\text{th}}$ (Tensión de Thévenin): Es la diferencia de potencial entre las terminales A y B en circuito abierto (sin conectar carga).
*   $R_{\text{th}}$ (Resistencia de Thévenin): Es la resistencia equivalente vista desde A y B tras apagar todas las fuentes independientes (fuentes de tensión cortocircuitadas, fuentes de corriente abiertas).

### 2.2 Teorema de Norton
Establece que el circuito puede sustituirse por una **fuente de corriente equivalente $I_{\text{N}}$ en paralelo con la misma resistencia equivalente $R_{\text{th}}$**:
*   $I_{\text{N}}$ (Corriente de Norton): Es la corriente que circula entre A y B al cortocircuitar las terminales.

Relación entre equivalentes (Transformación de fuentes):
$$V_{\text{th}} = I_{\text{N}} \cdot R_{\text{th}}$$

---

## 3. El Toque Informático

### Algoritmo SPICE de Resolución de Circuitos
Los simuladores de circuitos electrónicos (como LTspice o Multisim) se basan en el algoritmo **SPICE** (Simulation Program with Integrated Circuit Emphasis).
SPICE automatiza el análisis por nudos planteando las ecuaciones en una matriz conocida como **Análisis Modificado por Nudos (MNA)**:
$$G \cdot V = I$$
donde $G$ es la matriz de conductancias, $V$ es el vector de tensiones de nudo incógnitas e $I$ es el vector de corrientes inyectadas por las fuentes.

A continuación, implementamos en Python el planteamiento y resolución matricial de un circuito de tres nudos para simular el comportamiento de un resolvedor SPICE.

```python
import numpy as np

# Planteamos las ecuaciones del circuito mediante el método de nudos (MNA)
# Circuito con 3 nudos esenciales (más referencia GND):
# Nudo 1: Conectado a fuente de 10V respecto a GND
# Nudo 2: Conectado a nudo 1 por R=2 ohm, a nudo 3 por R=3 ohm, y a GND por R=5 ohm
# Nudo 3: Conectado a nudo 2 por R=3 ohm, y a GND por R=4 ohm y fuente de corriente de 2A (entrante)

# El sistema de ecuaciones lineal G * V = I se define como:
# Fila 1 (Nudo 1): V1 = 10
# Fila 2 (Nudo 2): (V2 - V1)/2 + V2/5 + (V2 - V3)/3 = 0  ===> -0.5*V1 + (0.5 + 0.2 + 0.333)*V2 - 0.333*V3 = 0
# Fila 3 (Nudo 3): (V3 - V2)/3 + V3/4 = 2               ===> -0.333*V2 + (0.333 + 0.25)*V3 = 2

G = np.array([
    [1.0, 0.0, 0.0],
    [-0.5, (0.5 + 0.2 + 1.0/3.0), -1.0/3.0],
    [0.0, -1.0/3.0, (1.0/3.0 + 0.25)]
])

I = np.array([10.0, 0.0, 2.0])

# Resolvemos el sistema de ecuaciones usando álgebra lineal de numpy
V_nodos = np.linalg.solve(G, I)

print("Tensiones de nudo calculadas matricialmente (estilo SPICE):")
for i, v in enumerate(V_nodos):
    print(f"Voltaje Nudo {i+1} (V{i+1}): {v:.4f} V")
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Calcular el circuito equivalente de Thévenin visto desde las terminales A y B para el circuito resistivo de la figura, que consta de una fuente de tensión independiente de $15 \, \text{V}$ en serie con una resistencia de $6 \, \Omega$, conectadas a una resistencia en paralelo de $3 \, \Omega$ entre las terminales.

```
       +---[ 6 ohm ]---+-----+ A
       |               |     |
     (15V)           [3 ohm] [ Carga ]
       |               |     |
       +---------------+-----+ B
```

**Solución:**
1.  **Calcular la tensión de Thévenin ($V_{\text{th}}$)**:
    $V_{\text{th}}$ es la tensión en circuito abierto (sin carga) entre A y B.
    El circuito se reduce a un **divisor de tensión** formado por la fuente de $15 \, \text{V}$ y las resistencias de $6 \, \Omega$ y $3 \, \Omega$:
    $$V_{\text{th}} = V_{\text{AB}} = V_{15\text{V}} \frac{R_{\text{paralelo}}}{R_{\text{serie}} + R_{\text{paralelo}}} = 15 \frac{3}{6 + 3} = 15 \frac{3}{9} = 5 \, \text{V}$$
2.  **Calcular la resistencia de Thévenin ($R_{\text{th}}$)**:
    Apagamos la fuente de tensión independiente cortocircuitándola (cable).
    Visto desde las terminales A y B, las resistencias de $6 \, \Omega$ y $3 \, \Omega$ quedan conectadas en **paralelo**:
    $$R_{\text{th}} = R_{6\Omega} \parallel R_{3\Omega} = \frac{6 \cdot 3}{6 + 3} = \frac{18}{9} = 2 \, \Omega$$
3.  **Circuito Equivalente de Thévenin**:
    Una fuente de tensión de $5 \, \text{V}$ en serie con una resistencia de $2 \, \Omega$.

---

## 5. Ejercicios Propuestos

1.  Resolver el circuito de dos mallas de la figura mediante el método de corrientes de malla y calcular la corriente que circula por la resistencia compartida de $10 \, \Omega$ si las mallas tienen fuentes de $12 \, \text{V}$ y $6 \, \text{V}$.
2.  Para el ejercicio resuelto 1, calcular cuál sería el equivalente de Norton (fuente de corriente $I_{\text{N}}$ y resistencia $R_{\text{N}}$).
3.  ¿Qué establece el Teorema de Máxima Transferencia de Potencia respecto al valor de la resistencia de carga $R_{\text{L}}$ conectada a un circuito con resistencia de Thévenin $R_{\text{th}}$? Explica su importancia en el diseño de circuitos amplificadores de audio o antenas.


<div style="page-break-after: always;"></div>

# Tema 7: Circuitos Básicos con Condensadores y Bobinas: Comportamiento Transitorio

En los temas previos hemos analizado circuitos resistivos puros, cuyas respuestas de corriente y tensión a cambios en las fuentes son instantáneas. Sin embargo, los condensadores (que almacenan energía en campos eléctricos) y las bobinas (que almacenan energía en campos magnéticos) impiden los cambios bruscos de tensión e intensidad, respectivamente. Esto da origen al **régimen transitorio**, un periodo de adaptación temporal regido por ecuaciones diferenciales.

---

## 1. El Circuito RC (Carga y Descarga)

Consideramos una resistencia $R$ en serie con un condensador $C$ alimentados por una fuente de tensión constante $V_s$ mediante un interruptor que se cierra en el instante $t = 0$.

```
       +----/ ----[ R ]----+
       |   t=0             |
     (Vs)                [ C ] v_c(t)
       |                   |
       +-------------------+
```

### 1.1 Carga del Condensador
Aplicando la Ley de Tensiones de Kirchhoff (LKT) para $t \ge 0$:
$$V_s = v_R(t) + v_C(t) = R \cdot i(t) + v_C(t)$$

Como la corriente a través del condensador es $i(t) = C \frac{dv_C(t)}{dt}$:
$$V_s = RC \frac{dv_C(t)}{dt} + v_C(t) \quad \implies \quad \frac{dv_C(t)}{dt} + \frac{1}{RC} v_C(t) = \frac{V_s}{RC}$$

Esta es una ecuación diferencial ordinaria de primer orden de coeficientes constantes. Resolviéndola bajo la condición inicial $v_C(0) = 0$ resulta:
$$v_C(t) = V_s \left( 1 - e^{-t/\tau} \right)$$
donde $\tau = RC$ es la **constante de tiempo** del circuito (medida en segundos).

*Significado de $\tau$*:
*   Para $t = \tau$: $v_C(\tau) = V_s(1 - e^{-1}) \approx 0.632 V_s$ (el condensador se ha cargado al 63.2%).
*   Para $t = 5\tau$: $v_C(5\tau) \approx 0.993 V_s$ (se considera que el transitorio ha finalizado y el circuito alcanza el **estado estacionario**).

### 1.2 Descarga del Condensador
Si cortocircuitamos el condensador previamente cargado a $V_s$ a través de $R$ en $t = 0$:
$$v_C(t) = V_s e^{-t/\tau}$$

---

## 2. El Circuito RL (Establecimiento y Extinción)

Consideramos una resistencia $R$ en serie con una bobina $L$ alimentados por $V_s$.

### 2.1 Establecimiento de Corriente
La ecuación diferencial derivada de la LKT es:
$$V_s = R \cdot i(t) + L \frac{di(t)}{dt} \quad \implies \quad \frac{di(t)}{dt} + \frac{R}{L} i(t) = \frac{V_s}{L}$$

Resolviéndola para la condición inicial $i(0) = 0$:
$$i(t) = \frac{V_s}{R} \left( 1 - e^{-t/\tau} \right)$$
donde la constante de tiempo para un circuito RL es:
$$\tau = \frac{L}{R}$$

---

## 3. El Toque Informático

### Retardos de Propagación en Buses de Datos por Capacidad Parásita
En los microprocesadores modernos que operan a frecuencias de gigahercios (GHz), las pistas físicas de cobre que conectan los transistores son muy finas y están muy juntas entre sí:
*   Las pistas presentan una resistencia óhmica pequeña pero no nula ($R_{\text{pista}}$).
*   La proximidad entre pistas y el sustrato de silicio genera una **capacitancia parásita** ($C_{\text{parásita}}$).
*   Esta combinación forma un circuito **RC de primer orden**.

Cuando el procesador cambia un bit de '0' (0V) a '1' (ej. 1.2V), la tensión no sube de forma instantánea; sigue la curva exponencial de carga.
Si la frecuencia de reloj es demasiado alta, el ciclo de reloj terminará antes de que la tensión supere el umbral lógico del receptor, provocando corrupción de datos. Este efecto de retardo de conmutación limita físicamente la velocidad máxima a la que pueden operar las CPUs modernas.

A continuación, simulamos y graficamos en Python las curvas de carga y descarga de tensión en un circuito RC.

```python
import numpy as np
import matplotlib.pyplot as plt

# Parámetros del circuito RC
R = 1000.0  # 1k ohm
C = 1e-6    # 1 uF
Vs = 5.0    # 5V

# Constante de tiempo tau = R * C
tau = R * C
print(f"Constante de tiempo (tau) calculada: {tau*1000:.2f} ms")

# Malla de tiempo (de 0 a 5 tau)
t = np.linspace(0, 5 * tau, 500)

# Fórmulas de tensión
v_carga = Vs * (1 - np.exp(-t / tau))
v_descarga = Vs * np.exp(-t / tau)

# Gráfico
plt.figure(figsize=(10, 5))
plt.plot(t * 1000, v_carga, label='Carga del Condensador', color='blue', linewidth=2)
plt.plot(t * 1000, v_descarga, label='Descarga del Condensador', color='red', linestyle='--', linewidth=2)

# Línea de referencia de tau y el 63.2% de Vs
plt.axvline(x=tau*1000, color='gray', linestyle=':', label='t = 1 tau')
plt.axhline(y=0.632*Vs, color='green', linestyle=':', label='63.2% de Vs')

plt.title("Comportamiento Transitorio en un Circuito RC")
plt.xlabel("Tiempo (milisegundos)")
plt.ylabel("Tensión (Voltios)")
plt.legend()
plt.grid(True)
plt.show()
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
En un circuito de primer orden RC con $R = 10 \, \text{k}\Omega$ y un condensador descargado de $C = 22 \, \mu\text{F}$ conectado a una fuente de $10 \, \text{V}$, calcular la constante de tiempo $\tau$ y la tensión en el condensador tras $0.1 \, \text{s}$ de haberse cerrado el interruptor.

**Solución:**
1.  **Calcular la constante de tiempo $\tau$**:
    $$\tau = R \cdot C = (10 \cdot 10^3 \, \Omega) \cdot (22 \cdot 10^{-6} \, \text{F}) = 0.22 \, \text{s}$$
2.  **Calcular la tensión en el instante $t = 0.1 \, \text{s}$ usando la ecuación de carga**:
    $$v_C(t) = V_s \left( 1 - e^{-t/\tau} \right)$$
    Sustituyendo los valores:
    $$v_C(0.1) = 10 \left( 1 - e^{-0.1 / 0.22} \right) = 10 \left( 1 - e^{-0.4545} \right)$$
    Calculamos la exponencial:
    $$e^{-0.4545} \approx 0.6348$$
    Sustituyendo de nuevo:
    $$v_C(0.1) = 10 (1 - 0.6348) = 10 (0.3652) = 3.652 \, \text{V}$$

Tras $0.1 \, \text{s}$, el condensador se ha cargado hasta alcanzar una tensión de $3.652 \, \text{V}$.

---

## 5. Ejercicios Propuestos

1.  Deducir la ecuación diferencial que rige la descarga de un circuito RL de primer orden cuando se extingue la corriente a través de una resistencia.
2.  Un circuito RL consta de una bobina de $L = 150 \, \text{mH}$ en serie con una resistencia de $50 \, \Omega$. Calcular el tiempo exacto que debe transcurrir para que la corriente alcance el 99% de su valor máximo en estado estacionario tras conectarse a una fuente.
3.  ¿Por qué un condensador se comporta como un cortocircuito en el instante inicial de la carga ($t=0^+$) y como un circuito abierto en estado estacionario ($t \to \infty$)? Explica el fenómeno físico.


<div style="page-break-after: always;"></div>

# Tema 8: Circuitos con Fuentes Senoidales: Corriente Alterna y Fasores

La corriente alterna (CA) es la forma predominante de distribución de energía eléctrica y el soporte físico de las señales analógicas de comunicaciones. Analizar circuitos con fuentes senoidales en el dominio del tiempo exige resolver ecuaciones diferenciales complejas. Sin embargo, mediante la transformación de **Fasor** y el uso de **Números Complejos**, podemos transformar estas ecuaciones diferenciales en simples ecuaciones algebraicas lineales idénticas a las del análisis resistivo.

---

## 1. Parámetros de Señales Senoidales

Una señal de tensión senoidal en el dominio del tiempo se representa como:
$$v(t) = V_m \cos(\omega t + \theta)$$

donde:
*   $V_m$: **Amplitud** o valor máximo de la señal (Voltios, V).
*   $\omega = 2\pi f$: **Frecuencia angular** (radianes por segundo, rad/s).
*   $f = 1/T$: **Frecuencia** (Hercios, Hz), donde $T$ es el periodo en segundos.
*   $\theta$: **Ángulo de fase** (radianes o grados), que indica el desplazamiento temporal respecto al origen.
*   **Valor Eficaz (o RMS)**: Es la tensión equivalente en corriente continua que produce la misma disipación de calor sobre una resistencia. Para señales senoidales puras:
    $$V_{\text{eff}} = V_{\text{RMS}} = \frac{V_m}{\sqrt{2}} \approx 0.707 V_m$$

---

## 2. Repaso de Números Complejos Aplicado a Circuitos

Un número complejo $Z$ se representa en tres formas equivalentes en el plano complejo (eje real horizontal, eje imaginario vertical):

1.  **Forma Binómica**: $Z = R + jX$ (donde $j = \sqrt{-1}$ es la unidad imaginaria en ingeniería eléctrica).
2.  **Forma Polar**: $Z = |Z|_{\phi} \quad \implies |Z| = \sqrt{R^2 + X^2}, \quad \phi = \arctan(X/R)$
3.  **Forma Exponencial**: $Z = |Z| e^{j\phi}$

Operaciones rápidas en CA:
*   **Suma/Resta** (ideal en forma binómica): $(a+jb) \pm (c+jd) = (a\pm c) + j(b\pm d)$.
*   **Multiplicación/División** (ideal en forma polar/exponencial):
    $$A_{\alpha} \cdot B_{\beta} = (A\cdot B)_{\alpha+\beta} \quad \text{y} \quad \frac{A_{\alpha}}{B_{\beta}} = \left(\frac{A}{B}\right)_{\alpha-\beta}$$

---

## 3. El Concepto de Fasor

Un **fasor** es un número complejo que representa únicamente la **amplitud** y la **fase** de una señal senoidal pura. Se denota en negrita o con una barra superior:
$$v(t) = V_m \cos(\omega t + \theta) \quad \implies \quad \mathbf{V} = V_m e^{j\theta} = V_m \angle\theta$$

La transformación al dominio fasorial (de frecuencias) nos permite omitir el término temporal $\omega t$, asumiendo que todas las fuentes del circuito operan a la misma frecuencia angular $\omega$.

---

## 4. Impedancia y Admitancia Compleja

Definimos la **Impedancia** $\mathbf{Z}$ como la relación entre el fasor de tensión $\mathbf{V}$ y el fasor de corriente $\mathbf{I}$ en un elemento de circuito:
$$\mathbf{Z} = \frac{\mathbf{V}}{\mathbf{I}} = R + jX \quad (\Omega)$$
donde $R$ es la **Resistencia** (parte real) y $X$ es la **Reactancia** (parte imaginaria).

| Elemento | Relación Temporal | Impedancia Fasorial $\mathbf{Z}$ | Comportamiento en frecuencia |
| :--- | :--- | :--- | :--- |
| **Resistencia** | $v(t) = R \cdot i(t)$ | $\mathbf{Z}_R = R$ | Independiente de la frecuencia. Tensión y corriente en fase ($\phi=0$). |
| **Bobina** | $v(t) = L \frac{di(t)}{dt}$ | $\mathbf{Z}_L = j\omega L = \omega L \angle 90^\circ$ | La reactancia crece con la frecuencia. La tensión adelanta $90^\circ$ a la corriente. |
| **Condensador** | $i(t) = C \frac{dv(t)}{dt}$ | $\mathbf{Z}_C = \frac{1}{j\omega C} = \frac{1}{\omega C} \angle -90^\circ$ | La reactancia disminuye con la frecuencia. La corriente adelanta $90^\circ$ a la tensión. |

---

## 5. El Toque Informático

### Filtros Analógicos de Frecuencia (Paso-Bajo y Paso-Alto)
En las tarjetas de sonido y de red de los computadores, es necesario filtrar el ruido de alta frecuencia de las señales analógicas antes de digitalizarlas (convertidor ADC).
*   Un filtro paso-bajo básico se construye conectando una resistencia y un condensador en serie, y tomando la tensión de salida en los extremos del condensador.
*   A bajas frecuencias, la reactancia del condensador $1/\omega C$ es enorme (circuito abierto), por lo que toda la tensión se mide a la salida.
*   A altas frecuencias, la reactancia es casi nula (cortocircuito a GND), por lo que la señal se atenúa por completo.

A continuación, implementamos en Python una simulación que calcula los fasores de tensión en un circuito RC en serie y grafica el diagrama fasorial en el plano complejo.

```python
import numpy as np
import matplotlib.pyplot as plt

# Parámetros de la señal y componentes
R = 50.0      # 50 ohm
C = 100e-6    # 100 uF
f = 50.0      # Frecuencia de red en España (50 Hz)
omega = 2 * np.pi * f
V_m, fase_v = 10.0, 0.0 # Tensión de entrada: 10V con fase 0 rad

# Fasor de tensión de entrada
V_in = V_m * np.exp(1j * fase_v)

# Impedancias complejas (Reactancia capacitiva)
Z_R = R
Z_C = 1 / (1j * omega * C)
Z_total = Z_R + Z_C

# Fasor de corriente de la malla (I = V / Z)
I = V_in / Z_total

# Fasores de tensión en los elementos
V_R = I * Z_R
V_C = I * Z_C

# Gráfico del Diagrama Fasorial
plt.figure(figsize=(7, 7))
# Trazamos los vectores en el plano complejo
plt.quiver(0, 0, np.real(V_in), np.imag(V_in), angles='xy', scale_units='xy', scale=1, color='blue', label='V_in (Entrada)')
plt.quiver(0, 0, np.real(V_R), np.imag(V_R), angles='xy', scale_units='xy', scale=1, color='green', label='V_R (Tensión R)')
plt.quiver(0, 0, np.real(V_C), np.imag(V_C), angles='xy', scale_units='xy', scale=1, color='red', label='V_C (Tensión C)')

plt.xlim(-2, 12)
plt.ylim(-10, 4)
plt.axhline(0, color='black',linewidth=1)
plt.axvline(0, color='black',linewidth=1)
plt.title("Diagrama Fasorial de Circuitos en Alterna")
plt.xlabel("Parte Real (Voltios)")
plt.ylabel("Parte Imaginaria (Voltios)")
plt.legend()
plt.grid(True)
plt.show()
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Una bobina de inductancia $L = 50 \, \text{mH}$ se conecta a una fuente de tensión alterna dada por $v(t) = 10 \cos(100t) \, \text{V}$. Calcular la impedancia compleja de la bobina y hallar la expresión temporal de la corriente $i(t)$ que circula por ella.

**Solución:**
1.  **Identificar los parámetros de la señal**:
    *   Amplitud de tensión: $V_m = 10 \, \text{V}$.
    *   Frecuencia angular: $\omega = 100 \, \text{rad/s}$.
    *   Fase de tensión: $\theta = 0^\circ \implies \mathbf{V} = 10 \angle 0^\circ \, \text{V}$.
2.  **Calcular la impedancia de la bobina ($\mathbf{Z}_L = j\omega L$)**:
    $$\mathbf{Z}_L = j \cdot 100 \cdot (50 \cdot 10^{-3}) = j \cdot 5 = 5 \angle 90^\circ \, \Omega$$
3.  **Calcular el fasor de corriente ($\mathbf{I} = \frac{\mathbf{V}}{\mathbf{Z}_L}$)**:
    $$\mathbf{I} = \frac{10 \angle 0^\circ}{5 \angle 90^\circ} = 2 \angle (0^\circ - 90^\circ) = 2 \angle -90^\circ \, \text{A}$$
4.  **Regresar al dominio del tiempo**:
    $$i(t) = 2 \cos(100t - 90^\circ) \, \text{A} = 2 \sin(100t) \, \text{A}$$

La corriente tiene una amplitud de $2 \, \text{A}$ y se encuentra retrasada $90^\circ$ respecto a la tensión.

---

## 7. Ejercicios Propuestos

1.  Una fuente de corriente alterna de $v(t) = 220\sqrt{2} \cos(2\pi \cdot 50 t) \, \text{V}$ (tensión de red en España) se conecta a una resistencia de $220 \, \Omega$. Calcular el valor eficaz (RMS) de la tensión y la potencia media disipada en la resistencia.
2.  Calcular la impedancia equivalente de un circuito en paralelo formado por una resistencia de $100 \, \Omega$ y un condensador de $10 \, \mu\text{F}$ operando a una frecuencia angular de $\omega = 1000 \, \text{rad/s}$.
3.  ¿Por qué las compañías eléctricas exigen a las industrias corregir el "factor de potencia" ($\cos\phi$) de sus instalaciones de corriente alterna y cómo se realiza esto conectando condensadores en paralelo?


<div style="page-break-after: always;"></div>

# Tema 9: Fundamentos de Instalaciones Eléctricas

Las instalaciones eléctricas constituyen el soporte vital de cualquier sistema de computación. Desde un computador personal hasta un centro de datos de gran escala, el suministro de energía limpia, eficiente y segura es un prerrequisito indispensable. Comprender el funcionamiento de la corriente alterna (CA) en su vertiente de distribución (sistemas monofásicos y trifásicos), así como las medidas de protección y seguridad eléctrica, es fundamental para evitar daños en el hardware y salvaguardar vidas humanas.

---

## 1. Sistemas Monofásicos y Trifásicos

La energía eléctrica se genera y transporta predominantemente en forma de **corriente alterna trifásica** debido a su eficiencia de transmisión y a la simplicidad de los motores eléctricos industriales.

### 1.1 Sistema Monofásico
Es el sistema común en viviendas y oficinas de baja potencia. Consta de dos conductores activos principales y uno de protección:
1.  **Fase ($L$)**: Conductor que porta la corriente alterna (habitualmente con una tensión eficaz de $230 \, \text{V}$ respecto al neutro en Europa, y frecuencia de $50 \, \text{Hz}$).
2.  **Neutro ($N$)**: Conductor de retorno que cierra el circuito. Su tensión teórica respecto a tierra es de $0 \, \text{V}$ (aunque en la práctica puede variar ligeramente).
3.  **Toma de Tierra o Conductor de Protección ($PE$)**: Conductor de seguridad que conecta las carcasas metálicas de los equipos a la tierra física de la instalación. No transporta corriente en condiciones normales.

### 1.2 Sistema Trifásico
Utilizado en la industria, grandes edificios y centros de datos. Consta de tres tensiones alternas senoidales de igual frecuencia y amplitud, pero desfasadas entre sí exactamente $120^\circ$ (o $2\pi/3$ radianes):

$$v_1(t) = V_m \cos(\omega t)$$
$$v_2(t) = V_m \cos\left(\omega t - \frac{2\pi}{3}\right)$$
$$v_3(t) = V_m \cos\left(\omega t - \frac{4\pi}{3}\right)$$

```
Tensión (V)
   ^
   |     Phase 1    Phase 2     Phase 3
   |      __--__      __--__      __--__
   |     /      \    /      \    /      \
 0 +----+--------+--+--------+--+--------+---> t
   |   /          \/          \/          \
   |  /            \          /            \
   v
```

*   **Tensión de Fase ($V_F$)**: Tensión entre cualquiera de las fases y el neutro (ej. $230 \, \text{V}$).
*   **Tensión de Línea o de Línea a Línea ($V_L$)**: Tensión medida entre dos fases cualesquiera. En una conexión en estrella (Y), la relación matemática es:
    $$V_L = \sqrt{3} \cdot V_F \approx 1.732 \cdot V_F$$
    Para $V_F = 230 \, \text{V}$, resulta una tensión de línea de $V_L \approx 400 \, \text{V}$.
*   **Ventaja clave**: La suma algebraica de las tres tensiones instantáneas en un sistema trifásico equilibrado es siempre cero ($v_1(t) + v_2(t) + v_3(t) = 0$), lo que permite que la corriente de retorno por el neutro sea nula en condiciones balanceadas, ahorrando sección de cable.

---

## 2. Distribución de Energía Eléctrica

La red de energía eléctrica consta de varias etapas ordenadas por nivel de tensión para minimizar pérdidas por efecto Joule ($P_{\text{pérdidas}} = I^2 R$):

1.  **Generación**: Alternadores en centrales eléctricas (hidroeléctricas, térmicas, nucleares, solares, eólicas) elevan la tensión de generación a niveles medios.
2.  **Transporte (Alta Tensión - AT)**: Se eleva la tensión mediante transformadores (entre $110 \, \text{kV}$ y $400 \, \text{kV}$) para reducir la corriente $I$ transmitida a grandes distancias, minimizando las pérdidas en las líneas aéreas de cobre o aluminio.
3.  **Subtransmisión y Distribución (Media Tensión - MT)**: Transformadores reductores bajan la tensión a niveles de entre $11 \, \text{kV}$ y $45 \, \text{kV}$ para la distribución regional.
4.  **Consumo final (Baja Tensión - BT)**: Las subestaciones de distribución locales reducen la tensión a los niveles nominales de utilización final: $400 \, \text{V}$ trifásico (3 fases + Neutro) y $230 \, \text{V}$ monofásico (fase + Neutro).

---

## 3. Seguridad Eléctrica y Elementos de Protección

El cuerpo humano es conductor de la electricidad. Si se cierra un circuito a través de él, se produce una corriente eléctrica cuyo efecto depende de su intensidad (desde cosquilleo, pasando por contracción muscular -tetanización-, hasta fibrilación ventricular y paro cardiaco). Por ello, los sistemas de distribución integran de forma obligatoria tres barreras de protección básicas:

### 3.1 Toma de Tierra (Protección Pasiva)
Consiste en una piqueta o red de cobre enterrada físicamente en el suelo de la estructura del edificio, conectada a los enchufes mediante el hilo verde-amarillo ($PE$).
*   **Principio de funcionamiento**: Las carcasas metálicas de los computadores, fuentes de alimentación y servidores se conectan eléctricamente al conductor de protección.
*   **Utilidad**: Si un cable activo sufre un fallo de aislamiento y toca el chasis metálico (derivación), la corriente fluirá directamente a tierra por un camino de bajísima resistencia eléctrica en lugar de quedar en el chasis esperando a que una persona lo toque.

### 3.2 Interruptor Magnetotérmico (PIA - Pequeño Interruptor Automático)
Protege la **instalación** y los cables frente a sobrecargas y cortocircuitos. Consta de dos mecanismos de disparo:
*   **Disparador Magnético (Instantáneo)**: Una bobina interna genera un campo magnético que abre el circuito al instante ante corrientes extremadamente elevadas (como un cortocircuito entre fase y neutro).
*   **Disparador Térmico (Retardado)**: Una lámina bimetálica se calienta debido a la corriente. Si la corriente supera el valor nominal durante un tiempo prolongado (sobrecarga, por conectar demasiados equipos de consumo), los coeficientes de dilatación diferentes de la lámina hacen que esta se doble y abra el interruptor.

### 3.3 Interruptor Diferencial (Protección Humana)
Protege a las **personas** contra contactos directos e indirectos midiendo la corriente residual.
*   **Principio de funcionamiento**: Monitoriza mediante un transformador toroidal la corriente que entra por la fase ($I_L$) y la que regresa por el neutro ($I_N$).
*   **Criterio de disparo**: En condiciones normales, $I_L = I_N$. Si hay una fuga de corriente a tierra (por ejemplo, corriente atravesando el chasis metálico o un cuerpo humano), entonces $I_L \neq I_N$.
*   Si la diferencia $\Delta I = |I_L - I_N|$ supera el umbral de sensibilidad de diseño del dispositivo (típicamente $30 \, \text{mA}$ para protección doméstica y de oficinas), el diferencial abre el circuito en milisegundos.

---

## 4. El Toque Informático

### Calidad de Suministro en Centros de Datos: UPS y Tierras Limpias
En las salas de servidores y centros de datos (Datacenters), un corte de energía o un pico de tensión puede corromper bases de datos o dañar costosas tarjetas de red y almacenamiento. Para mitigarlo se usan dos tecnologías fundamentales:

1.  **Sistemas de Alimentación Ininterrumpida (UPS / SAI)**:
    *   **SAI Offline**: Espera a un fallo de red para conmutar la carga a baterías por medio de un relé (retardo de unos $4\text{-}10 \, \text{ms}$, tolerable para fuentes con condensadores de retención grandes).
    *   **SAI de Doble Conversión (Online)**: Rectifica constantemente la corriente alterna de entrada a corriente continua (cargando las baterías y alimentando un inversor), y el inversor vuelve a generar CA limpia para los servidores. No hay tiempo de transferencia (retardo $0 \, \text{ms}$) y se aíslan los ruidos y picos de la red eléctrica comercial.
2.  **Tierras Limpias e Interferencias Electromagnéticas (EMI)**:
    Los racks metálicos y las mallas protectoras de los cables de datos (Ethernet blindado STP, cables de fibra con armadura) deben conectarse a una tierra de referencia muy estable libre de ruidos de conmutación de motores y ascensores. Las diferencias de potencial entre tierras de dos racks distintos pueden inducir corrientes indeseadas en los cables de red de cobre, provocando errores en la transmisión de datos.

A continuación, implementamos en Python una simulación visual de un sistema trifásico senoidal equilibrado y el cálculo de la corriente de retorno en el neutro.

```python
import numpy as np
import matplotlib.pyplot as plt

# Parámetros del sistema trifásico español
f = 50.0            # Frecuencia en Hz
omega = 2 * np.pi * f
V_rms = 230.0       # Tensión eficaz Fase-Neutro (Voltios)
V_m = V_rms * np.sqrt(2) # Amplitud máxima

# Malla de tiempo para graficar un ciclo completo (T = 1/f = 20ms)
t = np.linspace(0, 1/f, 1000)

# Ecuaciones de tensión instantánea de las tres fases
v1 = V_m * np.cos(omega * t)
v2 = V_m * np.cos(omega * t - 2 * np.pi / 3)
v3 = V_m * np.cos(omega * t - 4 * np.pi / 3)

# Suma de las tensiones en cualquier instante de tiempo
v_neutro = v1 + v2 + v3

# Gráfico
plt.figure(figsize=(10, 6))
plt.plot(t * 1000, v1, label='Fase R (L1)', color='red')
plt.plot(t * 1000, v2, label='Fase S (L2)', color='green')
plt.plot(t * 1000, v3, label='Fase T (L3)', color='blue')
plt.plot(t * 1000, v_neutro, label='Suma (Neutro)', color='black', linestyle='--', linewidth=2)

plt.title("Sistema Trifásico Equilibrado de Corriente Alterna")
plt.xlabel("Tiempo (milisegundos)")
plt.ylabel("Tensión Instantánea (Voltios)")
plt.axhline(0, color='gray', linewidth=0.8)
plt.legend()
plt.grid(True)
plt.show()
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
En un centro de datos conectado a una línea trifásica en estrella de $V_F = 230 \, \text{V}$ (Fase-Neutro), se desea conectar un sistema de refrigeración industrial diseñado para trabajar a tensión de línea (Fase-Fase). Calcular el valor de la tensión eficaz que alimentará a dicho motor de refrigeración.

**Solución:**
En una conexión trifásica en configuración de estrella (Y), la tensión de línea ($V_L$) es el resultado vectorial de la diferencia de potencial entre dos fases. Matemáticamente se calcula como:
$$V_L = \sqrt{3} \cdot V_F$$
Sustituyendo el valor eficaz de la fase:
$$V_L = 1.732 \cdot 230 \, \text{V} \approx 398.37 \, \text{V}$$

Por lo tanto, la tensión eficaz de alimentación del compresor trifásico será de aproximadamente $400 \, \text{V}$.

### Ejercicio 2
Un técnico de sistemas toca accidentalmente el chasis de un rack que tiene un fallo de aislamiento. La tensión eficaz de contacto es de $230 \, \text{V}$. Sabiendo que la resistencia del cuerpo humano húmedo en ese instante es de $2000 \, \Omega$ y que el diferencial de la línea tiene una sensibilidad de conmutación de $30 \, \text{mA}$:
1. Calcular la intensidad de corriente que atravesará al técnico.
2. Determinar si el interruptor diferencial saltará para protegerle.

**Solución:**
1.  **Cálculo de la corriente por la Ley de Ohm**:
    $$I = \frac{V}{R} = \frac{230 \, \text{V}}{2000 \, \Omega} = 0.115 \, \text{A} = 115 \, \text{mA}$$
2.  **Evaluación de disparo del diferencial**:
    El diferencial tiene un umbral de disparo de $\Delta I = 30 \, \text{mA}$. Puesto que la corriente de fuga a tierra que pasa a través del técnico es de $115 \, \text{mA}$, y $115 \, \text{mA} > 30 \, \text{mA}$, **el interruptor diferencial se disparará automáticamente** en cuestión de milisegundos, cortando el suministro eléctrico y evitando un daño severo o fatal para el técnico.

---

## 6. Ejercicios Propuestos

1.  Dibuja un diagrama esquemático mostrando el camino eléctrico seguido por la corriente cuando una lavadora con carcasa metálica sufre una fuga de corriente y dispone de toma de tierra y diferencial activos.
2.  Explica conceptualmente la diferencia entre una sobrecarga y un cortocircuito, detallando cuál de los dos elementos mecánicos internos del magnetotérmico actúa en cada caso.
3.  ¿Por qué es fundamental que la resistencia del sistema de toma de tierra física de un edificio sea muy baja (idealmente inferior a $15 \, \Omega$ o $25 \, \Omega$)? Relaciónalo con la tensión máxima permitida en las carcasas ($24 \, \text{V}$ en locales húmedos, $50 \, \text{V}$ en locales secos).


<div style="page-break-after: always;"></div>

# Tema 10: Fundamentos de Electrónica: El Diodo y el Transistor

Los circuitos resistivos, capacitivos e inductivos analizados hasta ahora son **pasivos**: no pueden amplificar potencia ni realizar conmutaciones lógicas de forma autónoma. La electrónica moderna nació con los dispositivos **semiconductores activos**, que permiten controlar el flujo de corriente mediante otra señal eléctrica de control. El diodo y el transistor (BJT y MOSFET) son los ladrillos elementales sobre los que se construyen los procesadores, las memorias y todos los sistemas digitales de computación.

---

## 1. Física de Semiconductores

El material semiconductor por excelencia es el **Silicio ($Si$)**, un elemento tetravalente (4 electrones en su capa de valencia) que forma una estructura cristalina covalente estable.

```
   |         |         |
-- Si ==--== Si ==--== Si --
   ||        ||        ||
-- Si ==--== Si ==--== Si --
   ||        ||        ||
-- Si ==--== Si ==--== Si --
   |         |         |
```

*   **Semiconductor Intrínseco**: Silicio puro. A temperatura ambiente, unos pocos electrones rompen sus enlaces térmicamente y saltan a la **banda de conducción**, dejando un hueco vacío en la **banda de valencia** (portadores de carga positiva ficticia). La conductividad es extremadamente baja.
*   **Semiconductor Extrínseco (Dopado)**: Consiste en introducir impurezas de forma controlada en la red de silicio para alterar drásticamente su conductividad:
    *   **Dopaje Tipo N (Negativo)**: Se añaden átomos pentavalentes (como Fósforo o Arsénico). Cuatro electrones se enlazan con el silicio y el quinto queda libre para la conducción. Los electrones son los **portadores mayoritarios**.
    *   **Dopaje Tipo P (Positivo)**: Se añaden átomos trivalentes (como Boro o Galio). Al faltar un electrón para completar los cuatro enlaces covalentes, se crea un **hueco** que puede aceptar un electrón de un átomo vecino. Los huecos son los **portadores mayoritarios**.

---

## 2. La Unión PN y el Diodo Rectificador

Cuando un cristal de silicio se dopa de tipo P en una mitad y de tipo N en la otra, se genera una interfaz llamada **Unión PN**.

```
      P (Huecos)          N (Electrones)
  +------------------|------------------+
  |    +    +    +   |  -    -    -     |
  |    +    +    +   |  -    -    -     |
  +------------------|------------------+
                     ^
             Zona de Depleción
```

1.  **Zona de Depleción (o de vaciado)**: En la frontera, los electrones libres de la zona N difunden hacia la zona P para recombinarse con los huecos. Esto deja iones positivos fijos en la zona N y iones negativos fijos en la zona P, creando un campo eléctrico interno que detiene la difusión posterior. Este campo da origen a una barrera de potencial ($\approx 0.7 \, \text{V}$ para el silicio).
2.  **Polarización Inversa**: Si conectamos el polo positivo de una fuente externa a la zona N y el negativo a la zona P, ensanchamos la zona de deplexión. No circula corriente (excepto una corriente de saturación inversa $I_s$ extremadamente pequeña, del orden de nanoamperios).
3.  **Polarización Directa**: Si conectamos el polo positivo a la zona P y el negativo a la zona N, vencemos la barrera de potencial interna (cuando $V > 0.7 \, \text{V}$). La zona de deplexión se estrecha y circula corriente de forma exponencial.

### La Ecuación del Diodo de Shockley
La relación corriente-tensión ($I\text{-}V$) de un diodo ideal viene dada por:
$$I = I_s \left( e^{\frac{q V}{\eta k T}} - 1 \right) = I_s \left( e^{\frac{V}{\eta V_T}} - 1 \right)$$

donde:
*   $I_s$: Corriente de saturación inversa.
*   $q$: Carga del electrón ($1.602 \times 10^{-19} \, \text{C}$).
*   $k$: Constante de Boltzmann ($1.38 \times 10^{-23} \, \text{J/K}$).
*   $T$: Temperatura absoluta en Kelvin.
*   $V_T = \frac{kT}{q} \approx 25\text{-}26 \, \text{mV}$ a temperatura ambiente ($25^\circ\text{C}$).
*   $\eta$: Factor de idealidad (generalmente entre 1 y 2).

### Aplicación: Rectificación de Corriente
Los diodos se utilizan para convertir CA en CC.
*   **Rectificador de media onda**: Bloquea el semiciclo negativo de la señal alterna.
*   **Rectificador de onda completa (puente de diodos)**: Redirige ambos semiciclos para que fluyan en la misma dirección. Al añadir un condensador en paralelo a la carga (filtro de rizado), se suaviza la salida para obtener una corriente continua estable para alimentar computadores.

---

## 3. El Transistor: BJT y MOSFET

El transistor es un dispositivo de tres terminales capaz de utilizar una pequeña señal en un terminal para controlar una corriente mucho mayor entre los otros dos terminales.

### 3.1 Transistor de Unión Bipolar (BJT)
Consta de tres capas semiconductoras dopadas alternativamente: NPN o PNP. Sus terminales son: **Base (B)**, **Colector (C)** y **Emisor (E)**.
*   **Funcionamiento**: Una pequeña corriente inyectada en la Base ($I_B$) controla una corriente mucho mayor que circula desde el Colector al Emisor ($I_C$):
    $$I_C = \beta \cdot I_B$$
    $$I_E = I_B + I_C = (\beta + 1) I_B$$
    donde $\beta$ (ganancia de corriente) típicamente oscila entre $50$ y $300$.
*   **Zonas de Operación**:
    1.  **Corte**: $I_B = 0 \implies I_C = 0$. El transistor actúa como un interruptor abierto.
    2.  **Activa**: El transistor amplifica linealmente ($I_C = \beta I_B$). Usado en amplificadores analógicos de sonido y radiofrecuencia.
    3.  **Saturación**: La corriente de base es lo bastante grande como para que el colector no pueda suministrar más corriente límite. La tensión Colector-Emisor cae a un mínimo ($V_{CE\text{,sat}} \approx 0.2 \, \text{V}$). Actúa como un interruptor cerrado.

```
       C                           D
       |                           |
    B--|  (NPN)                 G--|=[  (NMOS)
      /|                           |
     / V                           S
       |
       E
```

### 3.2 Transistor de Efecto de Campo (MOSFET)
Es el componente fundamental de la computación digital por su altísima impedancia de entrada y consumo casi nulo en estática. Sus terminales son: **Puerta (Gate, G)**, **Drenador (Drain, D)** y **Fuente (Source, S)**.
*   **Funcionamiento**: No requiere corriente de entrada en G. En su lugar, una tensión aplicada entre la Puerta y la Fuente ($V_{GS}$) genera un campo eléctrico que atrae portadores de carga bajo el óxido aislante de la puerta, creando un "canal" conductor entre el Drenador y la Fuente.
*   **Tipos principales**: NMOS (canal tipo N conductor con electrones) y PMOS (canal tipo P conductor con huecos).
*   **Zonas de Operación**:
    1.  **Corte**: $V_{GS} < V_{th}$ (tensión umbral). No hay canal, por lo que $I_D = 0$ (interruptor abierto).
    2.  **Lineal (u Ohmica)**: $V_{GS} > V_{th}$ y $V_{DS} < V_{GS} - V_{th}$. El canal está completamente abierto y se comporta como una resistencia controlada por tensión.
    3.  **Saturación**: $V_{GS} > V_{th}$ y $V_{DS} \ge V_{GS} - V_{th}$. El canal se estrangula cerca del drenador, haciendo que la corriente de drenador $I_D$ sea independiente de $V_{DS}$:
        $$I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{th})^2$$

---

## 4. El Toque Informático

### El MOSFET como Conmutador Lógico Binario
En los procesadores modernos (compuestos por miles de millones de MOSFETs), el transistor no se usa en su zona activa/saturada analógica, sino que conmutan exclusivamente entre **Corte** ('0' lógico) y **Lineal/Saturación** ('1' lógico).
*   Un transistor NMOS se "cierra" (conduce) cuando se aplica un '1' lógico en su puerta ($V_{GS} = V_{dd}$).
*   Un transistor PMOS se "cierra" cuando se aplica un '0' lógico en su puerta ($V_{GS} = 0$).

Al combinarlos adecuadamente, se crean inversores lógicos y puertas lógicas estables que no consumen energía en régimen permanente (salvo corrientes de fuga parásitas a través del óxido de puerta ultrafino, el cual representa un gran desafío de diseño térmico en las microarquitecturas modernas).

A continuación, implementamos en Python una simulación que toma una onda senoidal y modela su rectificación de onda completa mediante un puente de diodos, incluyendo la acción de filtrado de un condensador.

```python
import numpy as np
import matplotlib.pyplot as plt

# Parámetros de la simulación
f = 50.0            # 50 Hz
T = 1 / f
t = np.linspace(0, 3 * T, 1000) # 3 ciclos completos
V_m = 10.0          # Amplitud senoidal de entrada (10V)
V_in = V_m * np.sin(2 * np.pi * f * t)

# Caída de tensión en los diodos del puente (2 diodos conducen simultáneamente)
V_diodo = 0.7
V_rectificada_teorica = np.abs(V_in) - 2 * V_diodo
# La tensión no puede ser negativa
V_rect = np.maximum(0, V_rectificada_teorica)

# Simulación simplificada del efecto de filtro con condensador (carga/descarga RC)
# Constante de tiempo del filtro RC (R_carga * C_filtro)
RC = 0.05 # 50 ms (suficiente para mantener baja oscilación de rizado)
V_filtrada = np.zeros_like(V_rect)
current_v = 0.0

for i in range(len(t)):
    # Entrada rectificada instantánea
    v_instant = V_rect[i]
    if v_instant > current_v:
        # Carga del condensador instantánea (asumiendo resistencia del puente nula)
        current_v = v_instant
    else:
        # Descarga exponencial lenta del condensador sobre la resistencia de carga R
        dt = t[1] - t[0]
        current_v = current_v * np.exp(-dt / RC)
    V_filtrada[i] = current_v

# Gráfico
plt.figure(figsize=(10, 6))
plt.plot(t * 1000, V_in, label='V_in (Tensión Alterna de Entrada)', color='gray', linestyle=':')
plt.plot(t * 1000, V_rect, label='V_rect (Rectificada sin filtro)', color='red')
plt.plot(t * 1000, V_filtrada, label='V_out (Filtrada con Condensador)', color='blue', linewidth=2)

plt.title("Rectificador de Onda Completa con Filtro de Condensador")
plt.xlabel("Tiempo (milisegundos)")
plt.ylabel("Tensión (Voltios)")
plt.legend()
plt.grid(True)
plt.show()
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Calcular la corriente de drenador ($I_D$) de un transistor MOSFET NMOS que opera en zona de saturación. Datos: tensión umbral $V_{th} = 1.0 \, \text{V}$, parámetro de conductancia del proceso $\mu_n C_{ox} = 100 \, \mu\text{A/V}^2$, relación de dimensiones $W/L = 10$, y tensiones aplicadas de $V_{GS} = 3.0 \, \text{V}$ y $V_{DS} = 4.0 \, \text{V}$.

**Solución:**
1.  **Verificar la zona de operación**:
    *   $V_{GS} = 3.0 \, \text{V} > V_{th} = 1.0 \, \text{V} \implies$ El transistor está encendido.
    *   Tensión de estrangulamiento del canal: $V_{GS} - V_{th} = 3.0 - 1.0 = 2.0 \, \text{V}$.
    *   Dado que $V_{DS} = 4.0 \, \text{V} \ge 2.0 \, \text{V}$, el transistor efectivamente se encuentra en la **zona de saturación**.
2.  **Calcular la corriente usando la fórmula de saturación**:
    $$I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{th})^2$$
    Sustituyendo los valores numéricos:
    $$I_D = \frac{1}{2} \left( 100 \times 10^{-6} \, \text{A/V}^2 \right) \cdot (10) \cdot (3.0 - 1.0)^2$$
    $$I_D = \left( 50 \times 10^{-6} \right) \cdot 10 \cdot 4 = 2 \times 10^{-3} \, \text{A} = 2.0 \, \text{mA}$$

La corriente de drenador del transistor NMOS es de $2.0 \, \text{mA}$.

### Ejercicio 2
Un transistor NPN en configuración de emisor común con $\beta = 100$ se polariza con una corriente de base $I_B = 20 \, \mu\text{A}$. Si la tensión de la fuente de colector es $V_{CC} = 10 \, \text{V}$ a través de una resistencia de colector $R_C = 2 \, \text{k}\Omega$:
1. Calcular la corriente de colector teórica $I_C$ en zona activa.
2. Determinar si el transistor está realmente en zona activa o si ha entrado en saturación (asume $V_{CE\text{,sat}} = 0.2 \, \text{V}$).

**Solución:**
1.  **Corriente de colector teórica en zona activa**:
    $$I_C = \beta \cdot I_B = 100 \cdot (20 \times 10^{-6} \, \text{A}) = 2 \times 10^{-3} \, \text{A} = 2 \, \text{mA}$$
2.  **Verificación de tensiones**:
    Aplicamos la ecuación de la malla de colector:
    $$V_{CE} = V_{CC} - I_C R_C$$
    Sustituyendo los valores teóricos de zona activa:
    $$V_{CE} = 10 \, \text{V} - (2 \, \text{mA} \cdot 2 \, \text{k}\Omega) = 10 - 4 = 6 \, \text{V}$$
    Dado que $V_{CE} = 6 \, \text{V} > V_{CE\text{,sat}} = 0.2 \, \text{V}$, la suposición de que el transistor opera en **zona activa es correcta**. No está saturado.

---

## 6. Ejercicios Propuestos

1.  Explica la diferencia entre un diodo ideal y un diodo real de silicio bajo polarización directa e inversa. ¿Qué ocurre física y eléctricamente cuando superamos la tensión de ruptura inversa?
2.  Un transistor BJT se conecta con una resistencia de base de $100 \, \text{k}\Omega$ a una tensión de control digital de $5 \, \text{V}$ (asumiendo $V_{BE} = 0.7 \, \text{V}$). Si $\beta = 120$, calcula la corriente de colector si la carga permite mantener la zona activa.
3.  ¿Por qué los circuitos basados en MOSFET consumen mucha menos corriente y disipan menos energía térmica que los circuitos equivalentes basados en transistores bipolares (BJT) en reposo?


<div style="page-break-after: always;"></div>

# Tema 11: Familias Lógicas Digitales: TTL y CMOS

El álgebra de Boole proporciona el marco matemático para la computación digital, pero para ejecutarla físicamente requerimos circuitos electrónicos que traduzcan los valores abstractos '0' y '1' en rangos concretos de tensión eléctrica. Una **familia lógica** es un grupo de circuitos integrados digitales que implementan funciones lógicas utilizando una tecnología común de transistores. Las dos familias lógicas históricas y dominantes son **TTL** (basada en BJTs) y **CMOS** (basada en MOSFETs complementarios).

---

## 1. Parámetros Eléctricos de las Familias Lógicas

Para garantizar la interoperabilidad de las puertas lógicas y la inmunidad al ruido eléctrico del entorno, definimos varios parámetros críticos de corriente y tensión:

```
Tensión (V)
   ^  +-------------------------+ V_dd / V_CC (Alimentación)
   |  |   '1' Lógico de Salida  |
   |  +-------------------------+ V_OH (Mínimo de salida alta)
   |  |   Margen de Ruido Alto  |
   |  +-------------------------+ V_IH (Mínimo de entrada alta)
   |  |                         |
   |  |   Zona de Indeterminación|
   |  |                         |
   |  +-------------------------+ V_IL (Máximo de entrada baja)
   |  |   Margen de Ruido Bajo  |
   |  +-------------------------+ V_OL (Máximo de salida baja)
   |  |   '0' Lógico de Salida  |
   |  +-------------------------+ 0 V (GND / Tierra)
```

*   **Niveles de Tensión Lógica**:
    *   $V_{OH}$ (Output High Voltage): Tensión mínima garantizada por una puerta a la salida para representar un '1' lógico.
    *   $V_{OL}$ (Output Low Voltage): Tensión máxima garantizada por una puerta a la salida para representar un '0' lógico.
    *   $V_{IH}$ (Input High Voltage): Tensión mínima requerida a la entrada de una puerta para ser interpretada inequívocamente como un '1'.
    *   $V_{IL}$ (Input Low Voltage): Tensión máxima permitida a la entrada de una puerta para ser interpretada inequívocamente como un '0'.
*   **Margen de Ruido ($NM$)**: Capacidad de tolerar interferencias en el bus de conexión sin corromper el dato:
    *   Margen de Ruido Alto: $NM_H = V_{OH} - V_{IH}$
    *   Margen de Ruido Bajo: $NM_L = V_{IL} - V_{OL}$
*   **Retardo de Propagación ($t_{pd}$)**: Tiempo que tarda la salida en cambiar tras un cambio en la entrada. Limita la frecuencia de reloj máxima del procesador.
*   **Disipación de Potencia**: Calor generado por unidad de tiempo. En TTL es principalmente estático (conducción continua); en CMOS es mayormente dinámico (durante las transiciones).
*   **Fan-out (Factor de carga)**: Número máximo de entradas lógicas que la salida de una puerta puede manejar de forma fiable sin salirse de los rangos de tensión correctos.

---

## 2. Familia TTL (Transistor-Transistor Logic)

Basada en transistores bipolares (BJT) y alimentada típicamente a una tensión estricta de $V_{CC} = 5 \, \text{V}$.

### Estructura del Inversor TTL Estándar
1.  **Entrada**: Emplea un transistor NPN especial con múltiples emisores (que realiza físicamente la función lógica AND a la entrada).
2.  **Etapa Intermedia (Separadora)**: Transistor que actúa como divisor de fase para controlar los transistores de salida de forma complementaria.
3.  **Salida (Totem-Pole)**: Dos transistores NPN colocados uno encima del otro. Uno conduce para subir a '1' (Pull-Up) y el otro para bajar a '0' (Pull-Down).

*   *Características*: Consumo estático relativamente alto (corrientes constantes fluyendo por las uniones de base del BJT), velocidad alta para su época, pero baja densidad de integración térmica debido a la potencia consumida en continuo.

---

## 3. Familia CMOS (Complementary MOS)

Basada en pares complementarios de transistores de efecto de campo: un **PMOS** en la red de subida (Pull-Up) y un **NMOS** en la red de bajada (Pull-Down).

### Estructura del Inversor CMOS Estándar

```
          V_dd
           |
       +---|  (PMOS)
       |   |
  Vin--+   +--- Vout
       |   |
       +---|  (NMOS)
           |
          GND
```

*   **Funcionamiento**:
    *   Si $V_{in} = 0 \, \text{V}$ ('0'): El NMOS está en corte (abierto), el PMOS conduce (cerrado), conectando $V_{out}$ directamente a $V_{dd}$ ('1').
    *   Si $V_{in} = V_{dd}$ ('1'): El PMOS está en corte (abierto), el NMOS conduce (cerrado), drenando la carga de $V_{out}$ a GND ('0').
*   *Ventaja de Consumo Estático Nulo*: En reposo, siempre hay al menos un transistor en corte en cada camino entre $V_{dd}$ y GND, por lo que no circula corriente neta por el circuito. La única disipación ocurre en el transitorio de conmutación.

---

## 4. El Toque Informático

### Frecuencias de Reloj y Consumo Dinámico de Potencia en Procesadores
El consumo dinámico de potencia de una puerta lógica CMOS viene determinado por la carga y descarga de las capacidades parásitas de los buses y puertas de entrada vecinas ($C$):

$$P_{\text{dinámica}} = C \cdot V_{dd}^2 \cdot f$$

donde $f$ es la frecuencia de conmutación (frecuencia de reloj de la CPU).

*   **Consecuencia**: A mayor frecuencia de reloj, mayor es la disipación de calor.
*   **La Barrera Térmica**: A principios de la década de 2000, los fabricantes de procesadores (Intel, AMD) aumentaban el rendimiento elevando la frecuencia de reloj. Sin embargo, al acercarse a los $4 \, \text{GHz}$, la disipación de potencia térmica por unidad de superficie de silicio (densidad de potencia) igualó a la de un reactor nuclear.
*   **Solución**: Se abandonó la carrera de los gigahercios puros y se migró al diseño multihilo y multinúcleo (Multicore), reduciendo al mismo tiempo la tensión de alimentación $V_{dd}$ (de $5\text{V}$ a menos de $1\text{V}$) para aprovechar el impacto cuadrático de $V_{dd}^2$ en la potencia disipada.

A continuación, simulamos y comparamos la **Curva de Transferencia de Tensión (VTC)** de un inversor ideal de tecnología CMOS y un inversor clásico de tecnología TTL.

```python
import numpy as np
import matplotlib.pyplot as plt

# Generamos rampa de tensión de entrada de 0 a 5V
V_in = np.linspace(0, 5, 500)

# Inversor CMOS (Alimentado a 5V)
# Curva de conmutación simétrica y abrupta centrada en V_dd/2 (2.5V)
V_th_cmos = 2.5
V_out_cmos = 5.0 / (1.0 + np.exp(10 * (V_in - V_th_cmos)))

# Inversor TTL (Alimentado a 5V)
# Curva asimétrica de conmutación rápida alrededor de 1.4V
# V_OH de salida típico es ~3.5V y V_OL es ~0.2V
V_th_ttl = 1.4
V_out_ttl = 0.2 + (3.3 / (1.0 + np.exp(12 * (V_in - V_th_ttl))))

# Graficamos las Curvas de Transferencia (VTC)
plt.figure(figsize=(9, 6))
plt.plot(V_in, V_out_cmos, label='Inversor CMOS (Ideal)', color='blue', linewidth=2.5)
plt.plot(V_in, V_out_ttl, label='Inversor TTL (Típico)', color='red', linestyle='--', linewidth=2)

plt.axvline(V_th_cmos, color='blue', linestyle=':', alpha=0.5, label='Conmutación CMOS (2.5V)')
plt.axvline(V_th_ttl, color='red', linestyle=':', alpha=0.5, label='Conmutación TTL (1.4V)')

plt.title("Curvas de Transferencia de Tensión (V_in vs V_out)")
plt.xlabel("Tensión de Entrada V_in (Voltios)")
plt.ylabel("Tensión de Salida V_out (Voltios)")
plt.legend()
plt.grid(True)
plt.show()
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Dada una familia lógica con los siguientes parámetros eléctricos nominales:
$$V_{OH} = 2.7 \, \text{V}, \quad V_{OL} = 0.4 \, \text{V}$$
$$V_{IH} = 2.0 \, \text{V}, \quad V_{IL} = 0.8 \, \text{V}$$
Calcular los márgenes de ruido en estado alto ($NM_H$) y en estado bajo ($NM_L$).

**Solución:**
Los márgenes de ruido se definen como la diferencia absoluta entre el peor caso de tensión garantizado a la salida y el peor caso tolerado a la entrada:
1.  **Margen de Ruido Alto ($NM_H$)**:
    $$NM_H = V_{OH} - V_{IH} = 2.7 \, \text{V} - 2.0 \, \text{V} = 0.7 \, \text{V}$$
2.  **Margen de Ruido Bajo ($NM_L$)**:
    $$NM_L = V_{IL} - V_{OL} = 0.8 \, \text{V} - 0.4 \, \text{V} = 0.4 \, \text{V}$$

Por lo tanto, la familia lógica puede tolerar picos de ruido e interferencias de hasta $0.7 \, \text{V}$ en las líneas en estado lógico '1' y de hasta $0.4 \, \text{V}$ en estado lógico '0' sin inducir fallos de interpretación lógicos.

### Ejercicio 2
Un chip de procesador fabricado en tecnología CMOS opera a una frecuencia de reloj de $3.2 \, \text{GHz}$ ($3.2 \times 10^9 \, \text{Hz}$) alimentado a una tensión $V_{dd} = 1.1 \, \text{V}$. Si la capacidad parásita equivalente total de las pistas conmutando simultáneamente es de $2.5 \, \text{nF}$ ($2.5 \times 10^{-9} \, \text{F}$):
1. Calcular la potencia dinámica disipada por el chip.
2. Calcular la potencia dinámica si se reduce la tensión $V_{dd}$ a $0.9 \, \text{V}$ mediante técnicas de control dinámico de tensión (DVS).

**Solución:**
1.  **Cálculo de la potencia a $V_{dd} = 1.1 \, \text{V}$**:
    $$P_{\text{dinámica}} = C \cdot V_{dd}^2 \cdot f$$
    $$P_{\text{dinámica}} = (2.5 \times 10^{-9} \, \text{F}) \cdot (1.1 \, \text{V})^2 \cdot (3.2 \times 10^9 \, \text{Hz})$$
    $$P_{\text{dinámica}} = 2.5 \cdot 1.21 \cdot 3.2 = 9.68 \, \text{W}$$
2.  **Cálculo de la potencia a $V_{dd} = 0.9 \, \text{V}$**:
    $$P_{\text{dinámica, nueva}} = (2.5 \times 10^{-9} \, \text{F}) \cdot (0.9 \, \text{V})^2 \cdot (3.2 \times 10^9 \, \text{Hz})$$
    $$P_{\text{dinámica, nueva}} = 2.5 \cdot 0.81 \cdot 3.2 = 6.48 \, \text{W}$$

La reducción de tensión de tan solo $0.2 \, \text{V}$ disminuyó la disipación térmica dinámica en aproximadamente un 33% (ahorro de $3.2 \, \text{W}$), demostrando la eficiencia de los límites cuadráticos de tensión.

---

## 6. Ejercicios Propuestos

1.  Dibuja y explica el esquema circuital básico de una puerta NAND de dos entradas en tecnología CMOS, identificando la disposición en paralelo de los transistores PMOS y en serie de los NMOS.
2.  ¿Por qué no es recomendable conectar una salida lógica CMOS directamente a una entrada TTL sin un adaptador de niveles de tensión intermedio (Level Shifter)? Analiza la incompatibilidad de sus niveles lógicos de tensión.
3.  Explica cómo limita el *Fan-out* el número de circuitos lógicos que pueden conectarse en cascada basándote en las corrientes máximas de entrada y salida ($I_{OH}, I_{OL}, I_{IH}, I_{IL}$).


<div style="page-break-after: always;"></div>

# Glosario de Términos

*   **Campo Eléctrico**: Campo vectorial producido por cargas eléctricas que ejerce una fuerza sobre cualquier otra carga presente en él, medido en N/C o V/m.
*   **Campo Magnético**: Campo vectorial que describe la influencia magnética sobre cargas eléctricas en movimiento y materiales magnéticos, medido en teslas (T).
*   **Corriente (Intensidad)**: Flujo neto de carga eléctrica por unidad de tiempo a través de un conductor, medida en amperios (A).
*   **Dopaje**: Proceso consistente en añadir impurezas químicas de forma intencionada a un cristal semiconductor intrínseco para alterar sus propiedades conductoras (creando regiones tipo P o tipo N).
*   **Fan-out**: Número máximo de entradas lógicas que una salida digital puede gobernar simultáneamente sin comprometer sus márgenes lógicos de tensión o corriente.
*   **Fasor**: Número complejo que representa de forma simplificada la amplitud y la fase inicial de una señal senoidal pura a una frecuencia angular fija.
*   **Impedancia**: Medida de la oposición total que presenta un circuito al paso de una corriente alterna senoidal. Es un número complejo compuesto por una parte real (resistencia) y una imaginaria (reactancia).
*   **Inducción Electromagnética**: Producción de una fuerza electromotriz (tensión) a través de un conductor expuesto a un campo magnético variable en el tiempo (Ley de Faraday-Lenz).
*   **Interruptor Diferencial**: Dispositivo electromecánico de seguridad que desconecta el suministro eléctrico cuando detecta una diferencia entre la corriente de fase y de neutro, lo que indica una derivación de corriente (fuga a tierra).
*   **Interruptor Magnetotérmico**: Dispositivo automático de protección que abre el circuito para evitar daños por sobrecalentamiento térmico (sobrecargas duraderas) o fuerzas magnéticas (cortocircuitos instantáneos).
*   **Margen de Ruido**: Medida de la robustez de un circuito lógico ante interferencias eléctricas, calculada como la diferencia de rango de tensión entre el nivel garantizado a la salida y el nivel mínimo aceptado a la entrada para una misma señal digital.
*   **Tensión (Voltaje)**: Diferencia de potencial eléctrico entre dos puntos, medida en voltios (V). Representa la energía por unidad de carga necesaria para mover una carga entre ellos.
*   **Toma de Tierra**: Conexión física directa a la tierra del edificio mediante conductores de protección para desviar fugas de corriente accidentales y evitar riesgos de choque eléctrico en las carcasas metálicas de los equipos.
*   **Transistor MOSFET**: Transistor de efecto de campo metal-óxido-semiconductor que utiliza la tensión en una puerta aislada para crear un canal de conducción y controlar la corriente de drenador. Componente básico de la microelectrónica de computadores.
*   **Unión PN**: Interfaz o frontera de contacto entre un semiconductor dopado tipo P y uno tipo N, que forma la base física de los diodos y transistores.

<div style="page-break-after: always;"></div>

# Bibliografía Recomendada

1.  **Tipler, P. A., & Mosca, G. (2010).** *Física para la ciencia y la tecnología* (Volumen 2: Electricidad y magnetismo, luz). Editorial Reverté.
    *   *Nota*: Texto de referencia definitivo para la física del electromagnetismo, campo eléctrico y magnético, y ondas electromagnéticas.
2.  **Alexander, C. K., & Sadiku, M. N. (2013).** *Fundamentos de circuitos eléctricos* (5ª ed.). McGraw-Hill.
    *   *Nota*: Una obra didáctica y de gran claridad para el análisis de circuitos de corriente continua y alterna, nudos, mallas, transitorios RC/RL y teoremas de Thévenin/Norton.
3.  **Boylestad, R. L., & Nashelsky, L. (2009).** *Electrónica: Teoría de circuitos y dispositivos electrónicos* (10ª ed.). Pearson Educación.
    *   *Nota*: Manual de cabecera fundamental para comprender los principios del diodo, rectificadores de tensión, y transistores bipolares BJT y de efecto de campo MOSFET.
4.  **Weste, N. H., & Harris, D. (2011).** *CMOS VLSI Design: A Circuits and Systems Perspective* (4th ed.). Addison-Wesley.
    *   *Nota*: La guía de referencia clásica para entender el diseño integrado a gran escala en tecnología CMOS, el retardo de conmutación y la potencia dinámica en procesadores.
5.  **Floyd, T. L. (2006).** *Fundamentos de sistemas digitales* (9ª ed.). Prentice Hall.
    *   *Nota*: Excelente libro de texto introductorio para las familias lógicas digitales TTL y CMOS, márgenes de ruido e inmunidad al ruido.
