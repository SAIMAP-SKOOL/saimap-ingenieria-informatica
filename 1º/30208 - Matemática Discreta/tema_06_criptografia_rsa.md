# Tema 6: Criptografía RSA

La criptografía de clave pública o asimétrica resolvió uno de los mayores problemas históricos de las comunicaciones: ¿cómo pueden dos partes intercambiar información de forma segura sin haberse transmitido previamente una clave secreta compartida? El algoritmo **RSA** (desarrollado por Rivest, Shamir y Adleman en 1977) se basa en una asimetría matemática simple: multiplicar dos números primos grandes es computacionalmente fácil y rápido, pero deshacer la operación (factorizar el producto de vuelta en sus componentes primos) es extremadamente difícil.

---

## 1. El Concepto de Criptografía Asimétrica

A diferencia de la criptografía simétrica (como AES, donde se usa la misma clave para cifrar y descifrar), en la criptografía asimétrica cada participante genera un par de claves:
1.  **Clave Pública**: Se difunde abiertamente al mundo. Cualquiera puede usar esta clave para cifrar un mensaje destinado a ti.
2.  **Clave Privada**: Se mantiene en estricto secreto. Solo tú dispones de ella y es la única que puede descifrar los mensajes cifrados con tu clave pública.

---

## 2. El Algoritmo RSA: Generación de Claves

El proceso matemático para generar el par de claves RSA se realiza siguiendo estos pasos:

1.  **Selección de primos**: Se eligen dos números primos gigantescos distintos, $p$ y $q$ (en la práctica, de más de 1024 o 2048 bits de longitud cada uno).
2.  **Módulo de cifrado**: Se calcula el producto de ambos primos:
    $$n = p \cdot q$$
    El número $n$ se hace público y define el tamaño del espacio de claves.
3.  **Función indicatriz**: Se calcula el valor de la función $\phi(n)$ de Euler:
    $$\phi(n) = (p-1)(q-1)$$
    Este valor se mantiene en absoluto secreto.
4.  **Exponente público ($e$)**: Se escoge un entero $e$ (exponente de cifrado) tal que sea menor y coprimo con $\phi(n)$:
    $$1 < e < \phi(n) \quad \text{y} \quad mcd(e, \phi(n)) = 1$$
    Un valor común estándar en la industria es $e = 65537$ ($2^{16} + 1$), por su eficiencia de cálculo.
5.  **Exponente privado ($d$)**: Se calcula el exponente de descifrado $d$ como el único inverso modular de $e$ módulo $\phi(n)$:
    $$d \cdot e \equiv 1 \pmod{\phi(n)}$$
    Este inverso se calcula de forma instantánea usando el Algoritmo de Euclides Extendido.

*   **Clave Pública**: Constituida por el par $(e, n)$.
*   **Clave Privada**: Constituida por el par $(d, n)$ (los factores primos originales $p$ y $q$ y el valor $\phi(n)$ deben ser destruidos de forma segura una vez terminado el proceso).

---

## 3. Cifrado y Descifrado RSA

Para transmitir un mensaje de texto, se traduce primero a un número entero $M$ (por ejemplo, utilizando su representación binaria/ASCII) tal que $0 \le M < n$.

### Cifrado
El emisor cifra el mensaje $M$ utilizando la clave pública del receptor $(e, n)$, obteniendo el criptograma $C$:
$$C = M^e \pmod n$$

### Descifrado
El receptor descifra el criptograma $C$ utilizando su clave privada $(d, n)$, recuperando el mensaje original $M$:
$$M = C^d \pmod n$$

> **Justificación matemática**: Según el teorema de Euler, como $d \cdot e \equiv 1 \pmod{\phi(n)}$, existe un entero $k$ tal que $d \cdot e = k \cdot \phi(n) + 1$. Por tanto:
> $$C^d \equiv (M^e)^d \equiv M^{e\cdot d} \equiv M^{k \cdot \phi(n) + 1} \equiv (M^{\phi(n)})^k \cdot M^1 \equiv (1)^k \cdot M \equiv M \pmod n$$

---

## 4. Algoritmo de Exponenciación Modular Rápida

Calcular $M^e \pmod n$ cuando $M, e, n$ tienen miles de bits es inviable si se multiplican secuencialmente. En su lugar se utiliza el método de **exponenciación rápida** (Square-and-Multiply):
1.  Se expresa el exponente $e$ en formato binario.
2.  Se procesa el exponente bit a bit de izquierda a derecha. En cada paso se eleva al cuadrado el acumulador actual módulo $n$.
3.  Si el bit analizado es un $1$, además se multiplica el acumulador por la base $M$ módulo $n$.

Este algoritmo reduce el coste de cálculo de $O(e)$ multiplicaciones a solo **$O(\log e)$** operaciones de multiplicación y módulo.

---

## 5. El Toque Informático

### Infraestructura de Clave Pública (PKI), Certificados SSL y SSH
RSA es la base que permite establecer conexiones seguras en Internet:
*   **HTTPS (SSL/TLS)**: Cuando te conectas a tu banco, tu navegador utiliza criptografía asimétrica (como RSA o curvas elípticas) para autenticar la identidad del servidor del banco y acordar de forma segura una clave temporal simétrica (mucho más rápida) para cifrar los datos de la sesión.
*   **Llaves SSH**: Los desarrolladores y administradores de sistemas utilizan llaves públicas y privadas RSA para iniciar sesión en servidores remotos de forma segura sin necesidad de contraseñas vulnerables a ataques de fuerza bruta.

A continuación, implementamos en Python una simulación completa de RSA: generación de claves con números primos pequeños, cifrado de un entero y descifrado.

```python
# Función para calcular el MCD de forma recursiva
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

# Algoritmo de Euclides Extendido para hallar el inverso modular
def mod_inverse(e, phi):
    def gcd_extended(a, b):
        if a == 0:
            return b, 0, 1
        g, x1, y1 = gcd_extended(b % a, a)
        return g, y1 - (b // a) * x1, x1
    
    g, x, y = gcd_extended(e, phi)
    return x % phi

# Simulación de Exponenciación Modular Rápida (Square-and-Multiply)
def exp_modular(base, exp, mod):
    res = 1
    base = base % mod
    while exp > 0:
        # Si el bit más a la derecha es 1 (impar)
        if (exp % 2) == 1:
            res = (res * base) % mod
        # Desplazamos exponente e indicamos cuadrado
        exp = exp // 2
        base = (base * base) % mod
    return res

# 1. Generación de claves con primos pequeños
p, q = 61, 53       # Primos de prueba
n = p * q           # Módulo: 3233
phi = (p - 1) * (q - 1) # phi(n): 3120

# Escogemos un exponente e coprimo con 3120
e = 17
assert gcd(e, phi) == 1, "e debe ser coprimo con phi"

# Calculamos d (inverso modular de e)
d = mod_inverse(e, phi) # d = 2753

# 2. Cifrado
mensaje_original = 65  # Carácter 'A' en ASCII
criptograma = exp_modular(mensaje_original, e, n)

# 3. Descifrado
mensaje_descifrado = exp_modular(criptograma, d, n)

print(f"Módulo n: {n}, Función phi(n): {phi}")
print(f"Clave Pública (e, n): ({e}, {n})")
print(f"Clave Privada (d, n): ({d}, {n})")
print(f"Mensaje original: {mensaje_original}")
print(f"Mensaje cifrado (criptograma enviado por la red): {criptograma}")
print(f"Mensaje descifrado: {mensaje_descifrado}")
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Simular el algoritmo RSA a mano para los siguientes parámetros: primos $p = 3$, $q = 11$, y exponente de cifrado $e = 3$. Cifrar y descifrar el mensaje $M = 4$.

**Solución:**
1.  **Cálculo de parámetros**:
    *   $n = p \cdot q = 3 \cdot 11 = 33$.
    *   $\phi(n) = (p - 1)(q - 1) = 2 \cdot 10 = 20$.
2.  **Calcular el exponente privado $d$**:
    *   Queremos $d \cdot e \equiv 1 \pmod{\phi(n)} \implies 3 \cdot d \equiv 1 \pmod{20}$.
    *   Probamos múltiplos: $3 \cdot d = 21 \equiv 1 \pmod{20} \implies d = 7$.
3.  **Cifrar el mensaje $M = 4$**:
    *   $C = M^e \pmod n = 4^3 \pmod{33} = 64 \pmod{33}$.
    *   $64 = 33 \cdot 1 + 31 \implies C = 31$.
4.  **Descifrar el criptograma $C = 31$**:
    *   $M = C^d \pmod n = 31^7 \pmod{33}$.
    *   Dado que $31 \equiv -2 \pmod{33}$ (para simplificar los cálculos a mano):
        $$31^7 \equiv (-2)^7 \equiv -128 \pmod{33}$$
    *   Buscamos la congruencia positiva para $-128$:
        $$-128 = 33 \cdot (-4) + 4 \implies M = 4$$

El mensaje descifrado coincide con el mensaje original, verificando la corrección del proceso.

### Ejercicio 2
Calcular de forma eficiente $5^{13} \pmod{11}$ utilizando el algoritmo de exponenciación rápida.

**Solución:**
1.  **Expresar el exponente en binario**:
    *   $13$ en binario es $1101_2$ ($13 = 8 + 4 + 1$).
2.  **Ejecutar el algoritmo paso a paso** (inicializamos el acumulador $R = 1$ y la base actual $B = 5$):
    *   Bit 1 (peso 1, a la derecha): es $1 \implies R = (R \cdot B) \pmod{11} = (1 \cdot 5) \pmod{11} = 5$.
        *   Elevamos la base al cuadrado: $B = B^2 \pmod{11} = 25 \pmod{11} = 3$.
    *   Bit 2 (peso 2): es $0 \implies R$ no cambia ($R = 5$).
        *   Elevamos la base al cuadrado: $B = B^2 \pmod{11} = 3^2 \pmod{11} = 9$.
    *   Bit 3 (peso 4): es $1 \implies R = (R \cdot B) \pmod{11} = (5 \cdot 9) \pmod{11} = 45 \pmod{11} = 1$.
        *   Elevamos la base al cuadrado: $B = B^2 \pmod{11} = 9^2 \pmod{11} = 81 \pmod{11} = 4$.
    *   Bit 4 (peso 8): es $1 \implies R = (R \cdot B) \pmod{11} = (1 \cdot 4) \pmod{11} = 4$.

Por lo tanto, $5^{13} \equiv 4 \pmod{11}$.

---

## 7. Ejercicios Propuestos

1.  Dada la clave pública RSA $(e = 7, n = 55)$, calcula la clave privada correspondiente (el exponente $d$) sabiendo que $n$ se descompone en los primos $p=5$ y $q=11$.
2.  Cifra el mensaje $M = 2$ utilizando la clave pública del ejercicio anterior y descífralo paso a paso para comprobar el funcionamiento.
3.  Explica por qué es fundamental que un atacante no pueda conocer el valor de la función $\phi(n)$ y cómo, si lograra factorizar el módulo $n$ en sus componentes $p$ y $q$, podría romper inmediatamente la seguridad del cifrado RSA.
