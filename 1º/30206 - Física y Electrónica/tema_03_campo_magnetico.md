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
