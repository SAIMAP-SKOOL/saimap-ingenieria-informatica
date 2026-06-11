# Tema 4: Álgebra de Boole y Simplificación de Funciones

El álgebra de Boole es el marco matemático utilizado para formalizar el comportamiento de las variables lógicas binarias. Define las reglas matemáticas que gobiernan las compuertas físicas de los circuitos de los computadores. Puesto que cada compuerta lógica representa un coste físico en silicio (transistores), aprender a simplificar funciones lógicas mediante leyes algebraicas y **Mapas de Karnaugh** es crucial para optimizar el coste y rendimiento del hardware.

---

## 1. Postulados y Teoremas del Álgebra de Boole

El álgebra de Boole opera sobre un conjunto $\{0, 1\}$ con tres operaciones básicas:
1.  **Suma Lógica (OR)**: $A + B$ (se activa si $A$ o $B$ es 1).
2.  **Producto Lógico (AND)**: $A \cdot B$ o $AB$ (se activa únicamente si ambos son 1).
3.  **Inversión (NOT)**: $\bar{A}$ o $A'$ (invierte el valor de $A$).

### Leyes Fundamentales

*   **Identidad**: $A + 0 = A, \quad A \cdot 1 = A$
*   **Nulo (Dominancia)**: $A + 1 = 1, \quad A \cdot 0 = 0$
*   **Idempotencia**: $A + A = A, \quad A \cdot A = A$
*   **Complementariedad**: $A + \bar{A} = 1, \quad A \cdot \bar{A} = 0$
*   **Leyes de De Morgan**:
    $$\overline{A + B} = \bar{A} \cdot \bar{B}$$
    $$\overline{A \cdot B} = \bar{A} + \bar{B}$$

---

## 2. Formas Canónicas de una Función Lógica

Una función lógica relaciona variables de entrada binarias con una salida. Se puede representar formalmente de dos formas canónicas a partir de su tabla de verdad:

### 2.1 Primera Forma Canónica (Suma de Productos - Minterms)
Consiste en realizar una operación **OR** de los **Minterms** correspondientes a las filas donde la salida es **1**. Un minterm es un producto lógico (AND) donde aparecen todas las variables (afirmadas si valen 1, negadas si valen 0).
$$F(A,B,C) = \sum m(1, 3, 5)$$

### 2.2 Segunda Forma Canónica (Producto de Sumas - Maxterms)
Consiste en realizar una operación **AND** de los **Maxterms** correspondientes a las filas donde la salida es **0**. Un maxterm es una suma lógica (OR) de todas las variables (negadas si valen 1, afirmadas si valen 0).
$$F(A,B,C) = \prod M(0, 2, 4, 6, 7)$$

---

## 3. Puertas Lógicas Fundamentales

Las puertas lógicas son los bloques electrónicos básicos que realizan físicamente las operaciones algebraicas en el silicio.

```
       AND               OR               NOT
     +-----+           +-----+           +---+
 A --|  &  |       A --|  >1 |       A --| o |--- Y = \bar{A}
     |     |--- Y      |     |--- Y      +---+
 B --|     |       B --|     |
     +-----+           +-----+
```

*   **NAND y NOR (Puertas Universales)**:
    Las puertas NAND y NOR son de extrema importancia práctica. Cualquier circuito digital, por complejo que sea, puede implementarse utilizando exclusivamente compuertas NAND o compuertas NOR. Físicamente, en tecnología CMOS, una compuerta NAND requiere menos transistores y es más rápida que una AND pura.

---

## 4. Simplificación de Funciones mediante Mapas de Karnaugh (K-Maps)

El mapa de Karnaugh es una representación visual matricial de la tabla de verdad estructurada de forma que las celdas adyacentes difieren exactamente en un único bit (usando **Código Gray**). Esta adyacencia geométrica permite aplicar visualmente la ley de simplificación:
$$XY + X\bar{Y} = X(Y + \bar{Y}) = X$$

### Reglas de Agrupamiento
1.  Los grupos de celdas con valor "1" deben ser de tamaño potencia de dos: $1, 2, 4, 8 \text{ o } 16$ celdas.
2.  Debemos hacer los grupos **lo más grandes posibles** y el **menor número posible** de ellos.
3.  El mapa se "enrolla" de forma que los bordes izquierdo y derecho son adyacentes, así como el superior e inferior.
4.  **Términos Indiferentes (Don't Care, "X")**: Representan estados de entrada imposibles o salidas que no importan. Se pueden tratar como 1 o 0 de forma conveniente para hacer grupos más grandes.

---

## 5. El Toque Informático

### Optimización de Área y Consumo de Silicio en Microchips
Cada compuerta lógica implementada en silicio tiene asociado un tamaño de área y consume energía disipada en calor.
*   Una función lógica compleja sin simplificar puede requerir 10 compuertas lógicas (unos 40 transistores).
*   Tras simplificarla mediante mapas de Karnaugh, la misma función exacta puede requerir solo 2 compuertas (8 transistores).

Simplificar circuitos ahorra área en la oblea de silicio (permitiendo fabricar más microchips por lote), reduce el consumo de energía (alargando la batería de los dispositivos móviles) y disminuye el **retardo de propagación** (permitiendo que el procesador funcione a una frecuencia de reloj superior).

A continuación, implementamos en Python una simulación que genera la tabla de verdad y los minterms de una función de votación mayoritaria de tres variables.

```python
# Función lógica de votación mayoritaria: 1 si al menos dos entradas son 1
def votacion_mayoritaria(a, b, c):
    return (a and b) or (a and c) or (b and c)

# Impresión de la Tabla de Verdad e identificación de Minterms
print("| A | B | C | Salida (Y) | Minterm Asociado |")
print("|---|---|---|------------|------------------|")

minterms = []
for a in [0, 1]:
    for b in [0, 1]:
        for c in [0, 1]:
            y = int(votacion_mayoritaria(a, b, c))
            min_str = ""
            if y == 1:
                # Construimos el formato del minterm
                char_a = 'A' if a else 'A\''
                char_b = 'B' if b else 'B\''
                char_c = 'C' if c else 'C\''
                min_str = f"{char_a}{char_b}{char_c}"
                min_idx = a*4 + b*2 + c
                minterms.append(f"m{min_idx}")
            print(f"| {a} | {b} | {c} |     {y}      |      {min_str:8s}    |")

print(f"\nPrimera Forma Canónica (Suma de Productos): Y = " + " + ".join(minterms))
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Dada una función lógica de tres variables $F(A,B,C)$ definida por los minterms $F = \sum m(1, 3, 5, 7)$, escribir su primera forma canónica y simplificar la expresión mediante álgebra booleana.

**Solución:**
1.  **Primera forma canónica (Minterms)**:
    Escribimos la suma de productos correspondiente a los índices indicados:
    *   $m1 (001) \implies \bar{A}\bar{B}C$
    *   $m3 (011) \implies \bar{A}BC$
    *   $m5 (101) \implies A\bar{B}C$
    *   $m7 (111) \implies ABC$
    $$F = \bar{A}\bar{B}C + \bar{A}BC + A\bar{B}C + ABC$$
2.  **Simplificación Algebraica**:
    *   Agrupamos de dos en dos y sacamos factor común:
        $$F = \bar{A}C(\bar{B} + B) + AC(\bar{B} + B)$$
    *   Como $B + \bar{B} = 1$:
        $$F = \bar{A}C(1) + AC(1) = \bar{A}C + AC$$
    *   Volvemos a sacar factor común $C$:
        $$F = C(\bar{A} + A)$$
    *   Como $A + \bar{A} = 1$:
        $$F = C(1) = C$$

La función simplificada equivale a la variable de entrada $C$.

### Ejercicio 2
Simplificar mediante un Mapa de Karnaugh la función de 3 variables: $F(A,B,C) = \sum m(3, 4, 5, 7)$.

**Solución:**
1.  **Construir el Mapa de Karnaugh (Gray Code)**:
    Filas representan $A$ ($0, 1$). Columnas representan $BC$ ($00, 01, 11, 10$).

    ```
            BC
         00  01  11  10
       +---+---+---+---+
    A 0| 0 | 0 | 1 | 0 |   (Fila A=0: m0, m1, m3, m2)
       +---+---+---+---+
      1| 1 | 1 | 1 | 0 |   (Fila A=1: m4, m5, m7, m6)
       +---+---+---+---+
    ```
2.  **Agrupar los 1s**:
    *   **Grupo 1 (Tamaño 2)**: Celda $(A=0, BC=11)$ y celda $(A=1, BC=11)$. Corresponde a los minterms $m3$ y $m7$. En este grupo, la variable $A$ cambia de $0$ a $1$ (se elimina), mientras que $B$ y $C$ permanecen constantes en $1$. El término obtenido es **$BC$**.
    *   **Grupo 2 (Tamaño 2)**: Celdas de la fila $A=1$, columnas $BC=00$ y $BC=01$. Corresponde a los minterms $m4$ y $m5$. En este grupo, $A$ es constante en $1$. $B$ permanece constante en $0$ y $C$ cambia de $0$ a $1$ (se elimina). El término obtenido es **$A\bar{B}$**.
3.  **Resultado**: Sumamos los términos de los grupos óptimos:
    $$F_{\text{simplificada}} = BC + A\bar{B}$$

---

## 7. Ejercicios Propuestos

1.  Dibuja la tabla de verdad y obtén la primera forma canónica (Minterms) de una puerta lógica XOR de dos entradas.
2.  Simplifica mediante un Mapa de Karnaugh la siguiente función lógica de 4 variables:
    $$F(A,B,C,D) = \sum m(0, 2, 8, 10, 5, 7, 13, 15)$$
3.  Demuestra mediante compuertas NAND elementales cómo construir un circuito equivalente a: (a) un inversor NOT, (b) una puerta AND de dos entradas.
