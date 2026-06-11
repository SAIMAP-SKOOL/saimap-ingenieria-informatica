# Tema 1: Representación de la Información y Conversión de Bases

La información que maneja un computador (textos, imágenes, sonido, programas) debe codificarse físicamente en un soporte binario. Para comprender cómo procesa y almacena datos el hardware, debemos estudiar primero los **sistemas de numeración posicionales** y los algoritmos matemáticos que nos permiten traducir cantidades entre distintas bases numéricas.

---

## 1. Sistemas de Numeración Posicionales

En un sistema posicional, un número se representa por una cadena de dígitos donde el valor de cada dígito depende de su posición respecto a la coma fraccionaria. Cada posición tiene asociado un peso que es una potencia de la **base (o radix)** $r$ del sistema.

Un número genérico $N$ en base $r$ se expresa polinómicamente como:
$$N_r = \sum_{i=-m}^{n-1} d_i \cdot r^i = d_{n-1} r^{n-1} + \dots + d_0 r^0 + d_{-1} r^{-1} + \dots + d{-m} r^{-m}$$

donde:
*   $r$: Base del sistema (número de dígitos disponibles).
*   $d_i$: Dígitos permitidos ($0 \le d_i < r$).
*   $n$: Número de dígitos de la parte entera.
*   $m$: Número de dígitos de la parte fraccionaria.

### Bases Principales en Computación

| Sistema | Base ($r$) | Dígitos permitidos |
| :--- | :---: | :--- |
| **Decimal** | 10 | $\{0, 1, 2, 3, 4, 5, 6, 7, 8, 9\}$ |
| **Binario** | 2 | $\{0, 1\}$ |
| **Octal** | 8 | $\{0, 1, 2, 3, 4, 5, 6, 7\}$ |
| **Hexadecimal** | 16 | $\{0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A=10, B=11, C=12, D=13, E=14, F=15\}$ |

---

## 2. Algoritmos de Conversión entre Bases

### 2.1 Conversión a Base 10
Para pasar un número en cualquier base $r$ a base 10, basta con desarrollar su polinomio característico sumando el producto de cada dígito por su peso decimal.

### 2.2 Conversión de Base 10 a Base $r$
El proceso se divide en dos partes:
*   **Parte Entera (Divisiones Sucesivas)**: Dividimos la parte entera entre la base $r$ de forma reiterada. Los restos de cada división (leídos en orden inverso, de la última división a la primera) conforman el número entero en base $r$.
*   **Parte Fraccionaria (Multiplicaciones Sucesivas)**: Multiplicamos la parte fraccionaria por la base $r$. La parte entera del resultado será el dígito fraccionario correspondiente. Volvemos a tomar la parte fraccionaria resultante y multiplicamos por $r$, repitiendo hasta obtener parte fraccionaria cero o alcanzar la precisión deseada.

### 2.3 Conversiones Rápidas entre Bases Binarias ($r = 2^k$)
Dado que $8 = 2^3$ y $16 = 2^4$, podemos realizar conversiones instantáneas agrupando bits:
*   **Binario a Octal**: Agrupamos los bits de 3 en 3 partiendo desde la coma hacia la izquierda y derecha. Cada grupo de 3 bits se sustituye directamente por su equivalente octal (0-7).
*   **Binario a Hexadecimal**: Agrupamos los bits de 4 en 4. Cada grupo de 4 bits se sustituye directamente por su dígito hexadecimal equivalente (0-F).

---

## 3. El Toque Informático

### ¿Por qué los Computadores usan Binario y los Programadores Hexadecimal?
*   **Binario en Hardware**: Es físicamente sencillo construir componentes electrónicos que distingan únicamente dos estados de tensión eléctrica estables: presencia de tensión (un "1" lógico, ej. 5V o 1.2V) o ausencia de tensión (un "0" lógico, GND). Usar más niveles aumentaría drásticamente los errores por ruido térmico en las líneas de datos.
*   **Hexadecimal en Software**: El binario es ilegible para humanos en secuencias largas. La memoria se organiza en **bytes (8 bits)**. Como $2^4 = 16$, un byte se puede representar exactamente con dos dígitos hexadecimales (por ejemplo, $11111111_2$ es $\text{FF}_{16}$). Esto simplifica la visualización de direcciones de memoria (punteros) y códigos de colores en programación web (ej. `#FFFFFF` para blanco).

A continuación, implementamos en Python una utilidad que convierte un número decimal (con parte fraccionaria) a su representación binaria equivalente de forma analítica.

```python
def decimal_a_binario(numero_dec, precision=10):
    parte_entera = int(numero_dec)
    parte_fraccionaria = numero_dec - parte_entera
    
    # 1. Parte entera por divisiones sucesivas
    bits_enteros = []
    temp = parte_entera
    if temp == 0:
        bits_enteros.append("0")
    while temp > 0:
        bits_enteros.append(str(temp % 2))
        temp = temp // 2
    bits_enteros.reverse() # Leemos de abajo a arriba
    
    # 2. Parte fraccionaria por multiplicaciones sucesivas
    bits_fracc = []
    temp_f = parte_fraccionaria
    while temp_f > 0 and len(bits_fracc) < precision:
        temp_f *= 2
        digito = int(temp_f)
        bits_fracc.append(str(digito))
        temp_f -= digito
        
    resultado_entero = "".join(bits_enteros)
    resultado_fracc = "".join(bits_fracc)
    
    if resultado_fracc:
        return f"{resultado_entero}.{resultado_fracc}"
    return resultado_entero

# Prueba
num = 45.625
print(f"Decimal: {num}")
print(f"Binario calculado: {decimal_a_binario(num)}")
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Convertir el número decimal $45.625_{10}$ a base binaria de forma analítica.

**Solución:**
1.  **Parte entera ($45$)**:
    *   $45 / 2 = 22 \quad \implies \text{Resto } 1$
    *   $22 / 2 = 11 \quad \implies \text{Resto } 0$
    *   $11 / 2 = 5 \quad \implies \text{Resto } 1$
    *   $5 / 2 = 2 \quad \implies \text{Resto } 1$
    *   $2 / 2 = 1 \quad \implies \text{Resto } 0$
    *   $1 / 2 = 0 \quad \implies \text{Resto } 1$
    
    Leemos los restos de abajo a arriba: $101101_2$.
2.  **Parte fraccionaria ($0.625$)**:
    *   $0.625 \cdot 2 = 1.25 \quad \implies \text{Dígito } 1$ (nos queda $0.25$)
    *   $0.25 \cdot 2 = 0.50 \quad \implies \text{Dígito } 0$ (nos queda $0.5$)
    *   $0.50 \cdot 2 = 1.00 \quad \implies \text{Dígito } 1$ (nos queda $0.0$, terminamos)
    
    Leemos de arriba a abajo: $.101_2$.
3.  **Resultado**: $45.625_{10} = 101101.101_2$.

### Ejercicio 2
Convertir el número binario $1101011.011_2$ a octal y hexadecimal mediante agrupamiento.

**Solución:**
1.  **A Octal (grupos de 3 bits)**:
    *   Parte entera (desde la coma a la izquierda): $\underbrace{001}_{1} \underbrace{101}_{5} \underbrace{011}_{3}$ (añadimos ceros a la izquierda para completar).
    *   Parte fraccionaria (desde la coma a la derecha): $\underbrace{011}_{3}$.
    *   Resultado: $153.3_8$.
2.  **A Hexadecimal (grupos de 4 bits)**:
    *   Parte entera: $\underbrace{0110}_{6} \underbrace{1011}_{11=B}$ (añadimos ceros a la izquierda para completar).
    *   Parte fraccionaria: $\underbrace{0110}_{6}$ (añadimos un cero a la derecha para completar).
    *   Resultado: $6B.6_{16}$.

---

## 5. Ejercicios Propuestos

1.  Convierte el número decimal $127.3125_{10}$ a base binaria y hexadecimal detallando los pasos de la conversión de la parte entera y la fraccionaria.
2.  Halla el equivalente decimal de la cadena hexadecimal $A2C.8_{16}$ desarrollando su polinomio característico de potencias de 16.
3.  Explica por qué algunas fracciones decimales exactas (como $0.1_{10}$ o $0.2_{10}$) se convierten en fracciones periódicas infinitas en base binaria, y analiza qué problemas de precisión puede inducir esto en el software.
