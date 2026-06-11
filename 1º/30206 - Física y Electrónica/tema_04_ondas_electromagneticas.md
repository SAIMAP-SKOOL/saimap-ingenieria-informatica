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
