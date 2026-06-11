# Tema 3: Codificación de Números Reales y Estándar IEEE 754

Representar números fraccionarios o extremadamente grandes en un computador exige un sistema que mueva la coma decimal de forma dinámica para optimizar la precisión. Mientras que la **coma fija** limita gravemente el rango de representación, el sistema de **coma flotante** (formalizado universalmente en el estándar **IEEE 754**) nos permite representar desde la distancia entre átomos hasta la distancia entre galaxias utilizando un número acotado de bits.

---

## 1. Coma Fija vs. Coma Flotante

*   **Coma Fija**: Se reserva una cantidad fija de bits para la parte entera y otra para la parte fraccionaria. Por ejemplo, en una palabra de 8 bits con 4 bits enteros y 4 fraccionarios, el rango máximo es de $[0, 15.9375]$ con un paso constante de $0.0625$. No sirve para representar valores muy grandes o muy pequeños.
*   **Coma Flotante**: Utiliza notación científica binaria. Permite representar un número $V$ mediante tres campos: un bit de signo, una mantisa (cifras significativas) y un exponente (que desplaza la coma a izquierda o derecha).

---

## 2. El Estándar IEEE 754 de Precisión Simple (32 bits)

En el formato de precisión simple de 32 bits, los bits se distribuyen en tres campos diferenciados:

```
  1 bit      8 bits                 23 bits
 +-----+----------------+---------------------------------+
 |  S  |   E (Sesgado)  |           M (Mantisa)           |
 +-----+----------------+---------------------------------+
  MSB                                                    LSB
```

1.  **Signo ($S$)**: 1 bit (bit 31). Define si el número es positivo ($S = 0$) o negativo ($S = 1$).
2.  **Exponente ($E$)**: 8 bits (bits 23 a 30). Se almacena en representación de **exceso 127** (sesgado). El exponente real $e$ es:
    $$e = E - 127$$
3.  **Mantisa ($M$)**: 23 bits (bits 0 a 22). Almacena la parte fraccionaria de una mantisa normalizada. El estándar asume un **1 implícito** (bit oculto) a la izquierda de la coma que no se almacena en memoria para ahorrar espacio. La mantisa real es:
    $$\text{Mantisa Real} = 1 + M = 1.f \quad (\text{donde } f \text{ es la fracción almacenada})$$

La fórmula general de interpretación para números normalizados es:
$$V = (-1)^S \cdot (1.M) \cdot 2^{E - 127}$$

---

## 3. Rangos Especiales de Exponente en IEEE 754

El estándar reserva los valores extremos del exponente ($E=0$ y $E=255$) para codificar casos especiales:

| Campo Exponente ($E$) | Campo Mantisa ($M$) | Interpretación / Tipo de Valor |
| :---: | :---: | :--- |
| **$0 < E < 255$** | Cualquier valor | **Número Normalizado** (aritmética estándar). |
| **$E = 0$** | $M = 0$ | **Cero** ($+0.0$ o $-0.0$, según bit de signo). |
| **$E = 0$** | $M \neq 0$ | **Número Desnormalizado** ($V = (-1)^S \cdot (0.M) \cdot 2^{-126}$). Permite una caída gradual de precisión cerca del cero. |
| **$E = 255$** | $M = 0$ | **Infinito** ($+\infty$ o $-\infty$, según bit de signo). Resultados como divisiones entre cero ($x/0$). |
| **$E = 255$** | $M \neq 0$ | **NaN (Not a Number)**. Resultados matemáticamente indeterminados (como $0/0$ o $\sqrt{-1}$). |

---

## 4. El Toque Informático

### Inestabilidad de Comparación de Reales y la Necesidad de Épsilon
En computación, la representación de coma flotante introduce errores de redondeo porque muchas fracciones decimales exactas (como $0.1_{10}$ o $0.2_{10}$) se convierten en binario en expresiones periódicas infinitas. En memoria de 32 bits, se truncan de forma irreversible.
Por este motivo:
```cpp
// ERROR CRÍTICO DE PROGRAMACIÓN
if (x == 0.3) { ... }
```
La condición anterior casi siempre será evaluada como **falsa** debido a las pequeñas diferencias de redondeo del bit menos significativo.

*   **Solución**: Las comparaciones de números reales en programación deben realizarse definiendo un margen de tolerancia llamado **Épsilon ($\epsilon$)**:
```cpp
// SOLUCIÓN CORRECTA
if (abs(x - 0.3) < 1e-6) { ... }
```

A continuación, implementamos en Python un conversor que traduce un número real de precisión simple a su representación binaria IEEE 754 de 32 bits.

```python
import struct

def float_to_ieee754(num):
    # struct.pack('f', num) empaqueta el número como float de 32 bits (C float)
    # y 'I' lo interpreta como un entero sin signo de 32 bits
    packed = struct.pack('>f', num)
    val_int = struct.unpack('>I', packed)[0]
    
    bin_str = f"{val_int:032b}"
    
    signo = bin_str[0]
    exponente = bin_str[1:9]
    mantisa = bin_str[9:]
    
    print(f"Número Real: {num}")
    print(f"  Bit de Signo (S): {signo}")
    print(f"  Exponente (E):    {exponente} (Decimal: {int(exponente, 2)})")
    print(f"  Mantisa (M):      {mantisa}")
    print(f"  Representación Hexadecimal: 0x{val_int:08X}")
    return bin_str

# Prueba
float_to_ieee754(-6.625)
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Representar el número decimal $-6.625_{10}$ en el estándar IEEE 754 de precisión simple.

**Solución:**
1.  **Determinar el bit de signo**:
    El número es negativo, por tanto: $S = 1$.
2.  **Convertir el valor absoluto a binario**:
    $6_{10} = 110_2$ y $0.625_{10} = .101_2 \implies 6.625_{10} = 110.101_2$.
3.  **Normalizar la mantisa**:
    Desplazamos la coma hacia la izquierda hasta que quede un único dígito diferente de cero a su izquierda:
    $$110.101_2 = 1.10101_2 \cdot 2^2$$
    *   La mantisa normalizada es $1.10101$.
    *   La parte fraccional a almacenar en los 23 bits de memoria es: $10101000000000000000000_2$ (completando con ceros a la derecha).
    *   El exponente real es: $e = 2$.
4.  **Calcular el exponente sesgado ($E$)**:
    $$E = e + 127 = 2 + 127 = 129_{10}$$
    Convertimos $129$ a binario de 8 bits: $129 / 2 \dots \implies 10000001_2$.
5.  **Ensamblar la palabra de 32 bits**:
    $$\text{Palabra} = \underbrace{1}_{S} \, \underbrace{10000001}_{E} \, \underbrace{10101000000000000000000}_{M}$$

En hexadecimal, agrupando de 4 en 4: $1100 \, 0000 \, 1101 \, 0100 \dots_2 = \text{C0D40000}_{16}$.

### Ejercicio 2
Decodificar la palabra hexadecimal de 32 bits `0x40B00000` para hallar su valor decimal real según IEEE 754.

**Solución:**
1.  **Convertir el hexadecimal a binario**:
    `4` = 0100, `0` = 0000, `B` = 1011, `0` = 0000...
    $$\text{Binario} = 0100 \, 0000 \, 1011 \, 0000 \dots 0000_2$$
2.  **Extraer los campos**:
    *   $S = 0$ (número positivo).
    *   $E = 10000001_2 = 129_{10}$.
    *   $M = 01100000 \dots 0_2 = 0.011_2$ (en base decimal: $0 \cdot 2^{-1} + 1 \cdot 2^{-2} + 1 \cdot 2^{-3} = 0.25 + 0.125 = 0.375$).
3.  **Calcular el exponente real ($e$)**:
    $$e = E - 127 = 129 - 127 = 2$$
4.  **Calcular el valor real**:
    $$V = (-1)^0 \cdot (1 + 0.375) \cdot 2^2 = 1.375 \cdot 4 = 5.5_{10}$$

La palabra codifica el número real $+5.5_{10}$.

---

## 6. Ejercicios Propuestos

1.  Codifica el número real $+0.75_{10}$ en el estándar IEEE 754 de precisión simple y expresa su resultado en formato binario de 32 bits y hexadecimal.
2.  Dada la palabra binaria `0xBF800000` en IEEE 754 de precisión simple, realiza la decodificación paso a paso para hallar el valor real en base 10.
3.  Investiga el formato de precisión doble del estándar IEEE 754. Detalla cuántos bits reserva para cada campo (signo, exponente, mantisa), qué valor de sesgo utiliza y cuál es su ventaja respecto a la precisión simple.
