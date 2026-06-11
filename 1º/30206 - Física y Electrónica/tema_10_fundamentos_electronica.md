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
