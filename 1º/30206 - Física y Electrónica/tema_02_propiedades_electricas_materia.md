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
