# Tema 2: Codificación de Números Enteros

Para realizar cálculos aritméticos en un computador, no basta con codificar números naturales; debemos representar valores positivos y negativos. A lo largo de la historia de la informática se han diseñado varios métodos de codificación de enteros firmados (con signo). De todos ellos, el **Complemento a 2** se ha impuesto de forma universal en las Unidades Aritmético-Lógicas (ALUs) de los procesadores actuales debido a su eficiencia matemática para simplificar sumas y restas en un único circuito sumador básico.

---

## 1. Métodos de Codificación de Enteros Firmados (para $N$ bits)

Consideramos una palabra binaria de longitud fija de $N$ bits.

### 1.1 Signo-Magnitud (SM)
*   El bit más significativo (MSB, a la izquierda) representa el signo: $0$ para positivo ($+$) y $1$ para negativo ($-$).
*   Los restantes $N-1$ bits representan el valor absoluto (magnitud) en binario puro.
*   **Problemas**: Existe el doble cero ($+0 = 00000000_2$ y $-0 = 10000000_2$ para 8 bits), lo que complica las comprobaciones lógicas. La suma requiere circuitos separados para sumar y restar.
*   **Rango**: $[-2^{N-1} + 1, \quad 2^{N-1} - 1]$.

### 1.2 Complemento a 1 (C1)
*   Los números positivos se representan igual que en binario puro.
*   Los números negativos se obtienen **invirtiendo todos los bits** de su equivalente positivo (cambiando $0$ por $1$ y viceversa).
*   **Problemas**: Sigue existiendo doble representación del cero ($+0$ y $-0$).
*   **Rango**: $[-2^{N-1} + 1, \quad 2^{N-1} - 1]$.

### 1.3 Complemento a 2 (C2)
*   Los números positivos se representan igual que en binario puro.
*   Los números negativos se obtienen tomando su equivalente positivo, **invirtiendo todos sus bits (C1) y sumando 1** al bit menos significativo (LSB, a la derecha).
*   **Ventajas**:
    *   **Cero único**: El cero se representa únicamente como $00000000_2$ (para 8 bits). El acarreo sobrante al negar y sumar se descarta.
    *   **Suma unificada**: La resta $A - B$ se ejecuta físicamente como la suma $A + (-B)$.
*   **Rango**: $[-2^{N-1}, \quad 2^{N-1} - 1]$ (permite representar un número negativo extra).

### 1.4 Exceso o Representación Sesgada (Excess-$S$)
Consiste en almacenar el número sumándole un valor constante llamado **sesgo (o exceso)** $S$ de forma que todos los números almacenados sean positivos:
$$\text{Valor Codificado} = \text{Valor Real} + S$$
El sesgo habitual para $N$ bits es $S = 2^{N-1}$ o $S = 2^{N-1} - 1$. Es de gran utilidad para codificar los exponentes en coma flotante.

---

## 2. Aritmética y Detección de Desbordamiento (Overflow) en C2

Al operar con un número fijo de $N$ bits, el resultado de una suma de dos enteros firmados puede salirse del rango de representación máximo de la palabra binaria, lo que produce un **desbordamiento (overflow)** y devuelve un resultado erróneo.

### Regla de Signos
*   Si sumamos dos números de signos opuestos, **nunca** puede producirse desbordamiento.
*   Si sumamos dos números del mismo signo (ambos positivos o ambos negativos), se produce desbordamiento si y solo si el resultado tiene el **signo opuesto** al de los sumandos.

### Lógica de Compuertas en Hardware
En el interior de la ALU, el circuito detecta el desbordamiento de forma instantánea comparando los acarreos del bit de signo:
$$V = C_{in} \oplus C_{out}$$
donde $C_{in}$ es el acarreo entrante al bit de signo (MSB) y $C_{out}$ es el acarreo saliente del bit de signo. Si la operación XOR da $1$, se activa la bandera de desbordamiento (Overflow Flag).

---

## 3. El Toque Informático

### La Vulnerabilidad de Desbordamiento de Enteros en Software (Wrap-around)
En lenguajes de programación compila-directos como C o C++, el tipo `int` estándar suele codificarse en C2 usando 32 bits. El valor máximo representable es $2^{31}-1 = 2.147.483.647$.
Si sumamos 1 a este valor:
$$2.147.483.647 + 1 = -2.147.483.648$$

El valor de repente "salta" a su extremo negativo (`INT_MIN`) debido a que el bit de acarreo invade el bit de signo. Este comportamiento conocido como **wrap-around** es el origen de vulnerabilidades de seguridad críticas. Un atacante puede explotar un desbordamiento de enteros para saltarse comprobaciones de tamaño de búfer en memoria (`if (size1 + size2 < MAX_BUFFER)`), asignando un búfer minúsculo que luego provocará un desbordamiento de pila (stack overflow) al escribir.

A continuación, implementamos en Python una simulación del comportamiento de la aritmética en Complemento a 2 para 8 bits, incluyendo la detección de desbordamientos.

```python
def int_to_c2(val, bits=8):
    # Rango en C2
    lim_inf = - (2 ** (bits - 1))
    lim_sup = (2 ** (bits - 1)) - 1
    
    if val < lim_inf or val > lim_sup:
        raise ValueError(f"Valor fuera de rango para {bits} bits.")
        
    if val >= 0:
        return f"{val:0{bits}b}"
    else:
        # Para negativos, hacemos el equivalente a módulo 2^bits
        return f"{(2**bits) + val:0{bits}b}"

# Simulación de suma en C2 de 8 bits: A + B
def sumar_c2(a, b, bits=8):
    lim_inf = - (2 ** (bits - 1))
    lim_sup = (2 ** (bits - 1)) - 1
    
    suma_real = a + b
    overflow = suma_real < lim_inf or suma_real > lim_sup
    
    # Simulación de truncamiento a 'bits' de longitud
    val_truncado = (a + b) % (2 ** bits)
    if val_truncado >= (2 ** (bits - 1)):
        val_final_firmado = val_truncado - (2 ** bits)
    else:
        val_final_firmado = val_truncado
        
    print(f"Operación: {a} + {b}")
    print(f"  Binario A: {int_to_c2(a, bits)}")
    print(f"  Binario B: {int_to_c2(b, bits)}")
    print(f"  Resultado obtenido: {int_to_c2(val_final_firmado, bits)} (Equivale a {val_final_firmado} en base 10)")
    print(f"  ¿Ocurrió desbordamiento (Overflow)?: {'SÍ' if overflow else 'NO'}\n")

# Pruebas
sumar_c2(85, 20)  # Sin desbordamiento
sumar_c2(85, 64)  # Produce desbordamiento positivo (> 127)
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Representar el número entero $-18_{10}$ utilizando 8 bits en los formatos de Signo-Magnitud, Complemento a 1 y Complemento a 2.

**Solución:**
1.  **Representamos el número positivo $+18_{10}$ en binario puro (8 bits)**:
    *   $18 = 16 + 2 = 00010010_2$.
2.  **Signo-Magnitud (SM)**:
    *   Cambiamos únicamente el bit más significativo (MSB) a $1$ para indicar signo negativo:
        $$\text{SM} = 10010010_2$$
3.  **Complemento a 1 (C1)**:
    *   Invertimos todos los bits de $+18$:
        $$\text{C1} = 11101101_2$$
4.  **Complemento a 2 (C2)**:
    *   Tomamos la representación de C1 y sumamos 1:
        $$11101101_2 + 1 = 11101110_2$$

El resultado para $-18_{10}$ en C2 de 8 bits es $11101110_2$.

### Ejercicio 2
Realizar en C2 de 8 bits la suma de los enteros $85_{10} + 64_{10}$. Determinar el resultado binario obtenido y analizar si se produce desbordamiento.

**Solución:**
1.  **Codificar los sumandos a binario (C2)**:
    *   $85_{10} = 01010101_2$
    *   $64_{10} = 01000000_2$
2.  **Realizar la suma binaria**:
    ```
       01010101   (85)
     + 01000000   (64)
     ----------
       10010101
    ```
3.  **Verificación del resultado y desbordamiento**:
    *   Los sumandos son de signo positivo (el bit de signo de ambos es $0$).
    *   El resultado binario obtenido es $10010101_2$. El bit de signo de este resultado es **1** (negativo).
    *   Como la suma de dos positivos dio como resultado un número negativo, **se ha producido desbordamiento (overflow)**.
    *   El valor decimal interpretado erróneamente en C2 de 8 bits para $10010101_2$ es:
        $$10010101_2 \implies (\text{Restamos } 1) = 10010100_2 \implies (\text{Invertimos}) = -01101011_2 = -107_{10}$$

El circuito de la ALU devuelve el valor erróneo $-107_{10}$ en lugar de $+149_{10}$ debido a que $+149$ supera el límite superior de 8 bits en C2 ($+127$).

---

## 5. Ejercicios Propuestos

1.  Codifica el número decimal $-57_{10}$ en Complemento a 2 utilizando una longitud de palabra de 8 bits.
2.  Dada la palabra binaria de 8 bits $10110011_2$, calcula su valor decimal equivalente asumiendo que está representada en: (a) Binario sin signo, (b) Signo-Magnitud, (c) Complemento a 2.
3.  Realiza la operación de resta $34 - 72$ en C2 utilizando 8 bits (representándola como $34 + (-72)$) e indica el resultado binario obtenido comprobando si existe desbordamiento.
