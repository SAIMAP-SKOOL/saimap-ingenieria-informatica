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
