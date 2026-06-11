# Tema 5: Sistemas Combinacionales I: Bloques Lógicos MSI

Un sistema digital es **combinacional** si sus salidas en cualquier instante de tiempo dependen exclusivamente de los valores de sus entradas en ese mismo instante. Para facilitar el diseño de sistemas complejos, la industria agrupa compuertas básicas en bloques funcionales estándar de Mediana Escala de Integración (MSI - Medium Scale Integration). Estos bloques (codificadores, decodificadores, multiplexores) son los componentes básicos del enrutamiento de datos en el procesador.

---

## 1. Codificadores y Decodificadores

### 1.1 Codificadores
Dispositivo que realiza la operación inversa a un decodificador. Tiene $2^n$ líneas de entrada y $n$ líneas de salida. Su función es representar en binario cuál de las entradas está activa.
*   **Codificadores con Prioridad**: En un codificador básico, si dos entradas se activan a la vez, el resultado es erróneo. Los codificadores con prioridad resuelven esto asignando prioridad a la entrada con el índice más alto. Además, incluyen una salida de control $V$ (Valid) que indica si hay al menos una entrada activa.

### 1.2 Decodificadores
Dispositivo con $n$ entradas y $2^n$ salidas. Su función consiste en activar únicamente la salida correspondiente al código binario introducido en las entradas.
*   **Decodificador de 3 a 8**: Si introducimos el código $011_2$ (3 decimal) en las entradas, se activa únicamente la salida $Y_3$, quedando las demás en 0.
*   **Implementación de funciones lógicas**: Dado que cada salida de un decodificador representa un **minterm** de las variables de entrada, podemos implementar cualquier función lógica sumando (mediante una compuerta OR) las salidas correspondientes a los minterms de la función.

```
       Decodificador de 2 a 4
       +--------------------+
 A ----| I_0            Y_0 |---- \bar{A}\bar{B} (m_0)
       |                    |
 B ----| I_1            Y_1 |---- \bar{A}B       (m_1)
       |                    |
       |                Y_2 |---- A\bar{B}       (m_2)
       |                    |
       |                Y_3 |---- AB             (m_3)
       +--------------------+
```

---

## 2. Multiplexores (MUX) y Demultiplexores (DEMUX)

### 2.1 Multiplexores
Un multiplexor es un **selector de datos**. Tiene $2^n$ entradas de datos, $n$ entradas de selección (que actúan como control) y una única salida. El código binario en las entradas de selección determina cuál de las entradas de datos se conecta físicamente a la salida.

```
         Multiplexor 4 a 1
        +-----------------+
 D0 ----| In_0            |
 D1 ----| In_1            |
 D2 ----| In_2     Salida |---- Y
 D3 ----| In_3            |
        +-----------------+
             |       |
            S1      S0  (Selección)
```

*   **Implementación de funciones lógicas**: Un multiplexor de $2^n$ entradas de datos puede implementar cualquier función lógica de $n$ variables conectando las variables directamente a las entradas de selección y fijando a "1" o "0" las entradas de datos según convenga.

### 2.2 Demultiplexores
Realiza la operación inversa. Tiene una única entrada de datos, $n$ entradas de selección y $2^n$ salidas. Dirige el dato de la entrada única hacia la salida seleccionada por el bus de control.

---

## 3. El Toque Informático

### Decodificadores de Memoria y Señal Chip Select (CS)
En los computadores, la memoria principal está compuesta por múltiples chips de memoria integrados físicamente en la placa base. Cuando el procesador quiere leer o escribir un dato, envía una dirección por el bus de direcciones (Address Bus).
*   Las líneas de bits inferiores de la dirección se conectan a todos los chips para seleccionar la celda de memoria exacta (fila/columna).
*   Las líneas de bits superiores (que identifican el rango de memoria) se conectan a un **decodificador binario**.
*   La salida correspondiente del decodificador activa la patilla **Chip Select (CS)** de un único chip específico, apagando los demás para evitar colisiones de bus en las líneas de datos.

A continuación, implementamos en Python una simulación lógica de un Codificador con Prioridad de 4 a 2 con salida de validación.

```python
def codificador_prioridad_4_a_2(entradas):
    # entradas es una lista de 4 booleanos: [I3, I2, I1, I0]
    # Retorna (Y1, Y0, V)
    
    if entradas[0]:    # Mayor prioridad para I3
        return 1, 1, 1
    elif entradas[1]:  # I2
        return 1, 0, 1
    elif entradas[2]:  # I1
        return 0, 1, 1
    elif entradas[3]:  # I0
        return 0, 0, 1
    else:              # Ninguna activa
        return 0, 0, 0

# Pruebas de simulación
entradas_test = [
    [0, 0, 0, 0], # Ninguna activa
    [0, 0, 1, 0], # Solo I1 activa
    [1, 1, 0, 0]  # I3 e I2 activas a la vez (debe ganar I3 por prioridad)
]

for ent in entradas_test:
    y1, y0, v = codificador_prioridad_4_a_2(ent)
    print(f"Entradas [I3, I2, I1, I0]: {ent} -> Salidas Y1Y0: {y1}{y0}, V (Válido): {v}")
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Implementar la función lógica $F(A,B,C) = \sum m(1, 2, 4, 7)$ utilizando un decodificador de 3 a 8 y una puerta OR externa.

**Solución:**
1.  **Analizar el decodificador**:
    El decodificador de 3 a 8 tiene tres entradas ($A, B, C$) y 8 salidas ($Y_0$ a $Y_7$). Cada salida $Y_i$ se activa únicamente cuando se introduce el minterm correspondiente a su índice en binario.
2.  **Identificar salidas a conectar**:
    La función se activa para los minterms 1, 2, 4 y 7. Por tanto, conectamos las salidas $Y_1, Y_2, Y_4$ e $Y_7$ del decodificador a las entradas de una compuerta OR de 4 entradas.
3.  **Esquema circuital (conceptual)**:
    *   Entradas $A, B, C \to$ Conectadas a los pines de selección del decodificador.
    *   $F = Y_1 + Y_2 + Y_4 + Y_7$.

Cuando el código de entrada coincide con uno de los minterms indicados, la correspondiente salida del decodificador pasa a 1, activando la salida final de la puerta OR a 1.

### Ejercicio 2
Implementar la misma función lógica $F(A,B,C) = \sum m(1, 2, 4, 7)$ utilizando un multiplexor de 8 a 1.

**Solución:**
1.  **Analizar el multiplexor de 8 a 1**:
    Tiene 8 entradas de datos ($D_0$ a $D_7$), 3 entradas de selección ($S_2, S_1, S_0$) y 1 salida ($Y$).
2.  **Conectar las variables**:
    Conectamos las tres variables del problema ($A, B, C$) a las líneas de selección:
    *   $S_2 = A$, $S_1 = B$, $S_0 = C$.
3.  **Configurar las entradas de datos**:
    Fijamos los valores de las entradas de datos de forma estática según la tabla de verdad de la función:
    *   Para los minterms presentes (1, 2, 4, 7), conectamos las correspondientes entradas a "1" lógico ($V_{cc}$):
        $$D_1 = D_2 = D_4 = D_7 = 1$$
    *   Para los minterms ausentes (0, 3, 5, 6), conectamos las correspondientes entradas a "0" lógico (GND):
        $$D_0 = D_3 = D_5 = D_6 = 0$$

---

## 5. Ejercicios Propuestos

1.  Dibuja el esquema lógico de un decodificador de 2 a 4 utilizando compuertas básicas AND y NOT.
2.  Explica conceptualmente cómo se puede implementar una función lógica de 4 variables utilizando únicamente un multiplexor de 8 a 1 (pista: conectando una de las variables a las entradas de datos en lugar de fijarlas a valores constantes, método del árbol de selección).
3.  ¿Cuál es la función de las entradas de habilitación (Enable) en los decodificadores MSI comerciales y cómo se usan para conectar múltiples chips en cascada?
