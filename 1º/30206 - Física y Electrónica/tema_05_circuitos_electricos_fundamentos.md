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
