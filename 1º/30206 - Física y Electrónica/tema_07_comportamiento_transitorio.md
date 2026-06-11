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
