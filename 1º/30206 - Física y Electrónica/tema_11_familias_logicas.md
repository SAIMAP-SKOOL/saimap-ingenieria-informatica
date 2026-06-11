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
