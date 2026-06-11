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
