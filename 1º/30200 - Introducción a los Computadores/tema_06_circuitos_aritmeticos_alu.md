# Tema 6: Sistemas Combinacionales II: Circuitos Aritméticos y la ALU

La capacidad de un computador para ejecutar cálculos numéricos y lógicos reside en la **Unidad Aritmético-Lógica (ALU)**. La ALU es un sistema puramente combinacional compuesto por bloques aritméticos (sumadores, restadores, comparadores) y lógicos (puertas AND, OR, XOR) cuya salida es seleccionada dinámicamente mediante una señal de control. Estudiar los circuitos sumadores es la puerta de entrada para comprender cómo procesa físicamente la información el silicio.

---

## 1. Circuitos Sumadores Básicos

### 1.1 Semisumador (Half-Adder)
Suma dos bits de entrada ($A$ y $B$) y produce dos salidas: el bit de suma ($S$) y el acarreo de salida ($C$).

```
       Tabla de Verdad                Esquema Lógico
      A  B | S  C                  A --+-----\   XOR
      -----+-----                      |      )====== S = A \oplus B
      0  0 | 0  0                  B --|-+---/
      0  1 | 1  0                      | |
      1  0 | 1  0                  A --|-+----\  AND
      1  1 | 0  1                      |      |====== C = A \cdot B
                                   B --+------/
```

*   Ecuaciones:
    $$S = A \oplus B$$
    $$C = A \cdot B$$

### 1.2 Sumador Completo (Full-Adder)
Suma tres bits: los dos operandos ($A$ y $B$) y el acarreo proveniente de la etapa de suma anterior ($C_{in}$). Produce el bit de suma ($S$) y el acarreo de salida ($C_{out}$).

*   Ecuaciones:
    $$S = A \oplus B \oplus C_{in}$$
    $$C_{out} = A \cdot B + (A \oplus B) \cdot C_{in}$$

---

## 2. Sumadores de Múltiples Bits (Paralelos)

Para sumar palabras de $N$ bits se conectan varios sumadores completos en cascada:

### 2.1 Sumador con Propagación de Acarreo (Ripple Carry Adder - RCA)
El acarreo de salida de cada etapa se conecta directamente al acarreo de entrada de la etapa siguiente.
*   **Problema**: Retardo proporcional a la longitud de palabra ($O(N)$). Las últimas etapas deben esperar a que se calcule el acarreo de todas las anteriores, limitando la velocidad máxima del procesador.

### 2.2 Sumador con Anticipación de Acarreo (Carry Lookahead Adder - CLA)
Calcula los acarreos de todas las etapas en paralelo en tiempo constante $O(1)$ mediante dos funciones lógicas previas:
*   Generador de Acarreo: $G_i = A_i \cdot B_i$ (se genera acarreo si ambos son 1).
*   Propagador de Acarreo: $P_i = A_i \oplus B_i$ (se propaga el acarreo de entrada).

---

## 3. Estructura Interna de una ALU Elemental

Una ALU básica consta de:
1.  **Ruta de Datos**: Un sumador/restador paralelo y compuertas lógicas en paralelo.
2.  **Selector (Multiplexor)**: Toma las salidas de todos los bloques anteriores y, mediante un bus de selección de instrucción (código de operación u OPCode), redirige una de ellas hacia el bus de salida final.

---

## 4. El Toque Informático

### El Registro de Estado (FLAGS Register)
Las ALUs no solo devuelven el resultado del cálculo, sino que activan señales de estado de 1 bit llamadas **Flags** (banderas) que se guardan en el Registro de Estado de la CPU tras cada operación:
*   **Zero Flag ($Z$)**: Se pone a 1 si el resultado de la operación es exactamente cero.
*   **Sign Flag ($S$ o $N$)**: Copia el bit más significativo (MSB) del resultado, indicando si es negativo.
*   **Carry Flag ($C$)**: Se activa si la suma produce un acarreo saliente del MSB (operaciones sin signo).
*   **Overflow Flag ($V$ o $O$)**: Se activa si ocurre un desbordamiento aritmético en Complemento a 2.

Estas banderas son el corazón de la bifurcación lógica: cuando programas un `if (a == b)` en alto nivel, el compilador realiza una resta en la ALU (`a - b`). Si la resta es cero, se activa el flag $Z$. La CPU lee este flag y ejecuta un salto condicional de instrucción (`JZ` - Jump if Zero).

A continuación, implementamos en Python una simulación lógica de un Sumador por Propagación de Acarreo (RCA) de 4 bits.

```python
def full_adder(a, b, c_in):
    # Sumador completo de 1 bit
    s = a ^ b ^ c_in
    c_out = (a & b) | ((a ^ b) & c_in)
    return s, c_out

def ripple_carry_adder_4bits(bin_a, bin_b):
    # bin_a y bin_b son listas de 4 bits, ej. [0, 1, 0, 1] (MSB a la izquierda)
    # Volteamos para procesar desde el LSB (derecha) al MSB (izquierda)
    a = list(reversed(bin_a))
    b = list(reversed(bin_b))
    
    suma = []
    c = 0 # Acarreo inicial
    
    for i in range(4):
        s, c = full_adder(a[i], b[i], c)
        suma.append(s)
        
    suma.reverse() # Volvemos a colocar en orden MSB -> LSB
    return suma, c

# Suma: 5 (0101) + 6 (0110) = 11 (1011)
op1 = [0, 1, 0, 1]
op2 = [0, 1, 1, 0]
res, c_out = ripple_carry_adder_4bits(op1, op2)

print(f"Suma: {op1} + {op2}")
print(f"  Resultado de la suma (S): {res}")
print(f"  Acarreo de salida final (C_out): {c_out}")
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Deducir las ecuaciones del sumador completo (Full-Adder) a partir de su tabla de verdad por Minterms.

**Solución:**
1.  **Dibujamos la tabla de verdad**:

    | $A$ | $B$ | $C_{in}$ | $S$ | $C_{out}$ | Minterm |
    | :---: | :---: | :---: | :---: | :---: | :--- |
    | 0 | 0 | 0 | 0 | 0 | - |
    | 0 | 0 | 1 | 1 | 0 | $m_1 = \bar{A}\bar{B}C_{in}$ |
    | 0 | 1 | 0 | 1 | 0 | $m_2 = \bar{A}B\bar{C}_{in}$ |
    | 0 | 1 | 1 | 0 | 1 | $m_3 = \bar{A}BC_{in}$ |
    | 1 | 0 | 0 | 1 | 0 | $m_4 = A\bar{B}\bar{C}_{in}$ |
    | 1 | 0 | 1 | 0 | 1 | $m_5 = A\bar{B}C_{in}$ |
    | 1 | 1 | 0 | 0 | 1 | $m_6 = AB\bar{C}_{in}$ |
    | 1 | 1 | 1 | 1 | 1 | $m_7 = ABC_{in}$ |

2.  **Ecuación para $S$ (Suma)**:
    $$S = \bar{A}\bar{B}C_{in} + \bar{A}B\bar{C}_{in} + A\bar{B}\bar{C}_{in} + ABC_{in}$$
    Simplificamos:
    $$S = \bar{A}(\bar{B}C_{in} + B\bar{C}_{in}) + A(\bar{B}\bar{C}_{in} + BC_{in})$$
    Identificamos el XOR ($\oplus$) y XNOR ($\odot$):
    $$S = \bar{A}(B \oplus C_{in}) + A(\overline{B \oplus C_{in}}) = A \oplus B \oplus C_{in}$$
3.  **Ecuación para $C_{out}$ (Acarreo)**:
    $$C_{out} = \bar{A}BC_{in} + A\bar{B}C_{in} + AB\bar{C}_{in} + ABC_{in}$$
    Simplificamos:
    $$C_{out} = C_{in}(\bar{A}B + A\bar{B}) + AB(\bar{C}_{in} + C_{in}) = (A \oplus B)C_{in} + AB$$

Quedan demostradas las ecuaciones de diseño del sumador completo.

### Ejercicio 2
Esbozar el bloque lógico de control de una ALU de 1 bit con dos líneas de selección ($S_1, S_0$) para realizar las operaciones: $00 \to \text{AND}$, $01 \to \text{OR}$, $10 \to \text{Suma}$, $11 \to \text{Resta}$.

**Solución:**
El circuito consta de los siguientes bloques conectados en paralelo:
1.  Compuerta AND (entrada $A, B$).
2.  Compuerta OR (entrada $A, B$).
3.  Sumador completo (entradas $A, B, C_{in}$).
4.  Para la resta ($A - B$), invertimos la entrada $B$ mediante una puerta NOT si la señal es de resta ($S_1S_0 = 11$). Físicamente se implementa alimentando $B$ a una compuerta XOR controlada por $S_0$ (si $S_0=1$, $B$ se invierte a $\bar{B}$).
5.  Las salidas de la compuerta AND, OR, y el bit de suma se conectan a un multiplexor de 4 a 1.
6.  Las líneas de selección del multiplexor se conectan a $S_1$ y $S_0$. La salida seleccionada del multiplexor será la salida de la ALU.

---

## 6. Ejercicios Propuestos

1.  Dibuja el diagrama de bloques de un sumador Ripple Carry de 4 bits interconectando 4 bloques de sumadores completos elementales.
2.  Explica cómo se puede construir un circuito restador completo de 1 bit a partir de la modificación de los acarreos del sumador completo (restador con préstamo o Borrow).
3.  ¿Cómo reduce el sumador con anticipación de acarreo (Carry Lookahead) el tiempo de retraso de propagación en ALUs grandes de 32 o 64 bits? Detalla el coste de transistores asociado.
