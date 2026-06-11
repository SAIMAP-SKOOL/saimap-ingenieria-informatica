# Tema 1: Estructuras Algebraicas y Cuerpos Finitos

El estudio de las estructuras algebraicas abstractas (grupos, anillos y cuerpos) y, especialmente, de los cuerpos finitos (cuerpos de Galois) proporciona el fundamento matemático para la seguridad de la información. En ingeniería informática, estas estructuras sustentan el cifrado de datos (criptografía simétrica y asimétrica), la detección y corrección de errores en la transmisión de datos y el hashing criptográfico.

---

## 1. Grupos, Anillos y Cuerpos

Una **estructura algebraica** consiste en un conjunto no vacío dotado de una o más operaciones internas que satisfacen propiedades específicas.

### 1.1 Grupos
Un conjunto $G$ dotado de una operación binaria interna $*$ (representada como $(G, *)$) es un **grupo** si cumple las siguientes propiedades:
1.  **Asociativa**: $a * (b * c) = (a * b) * c$ para todo $a, b, c \in G$.
2.  **Elemento neutro**: Existe un único elemento $e \in G$ tal que $a * e = e * a = a$ para todo $a \in G$.
3.  **Elemento simétrico (inverso)**: Para cada $a \in G$, existe un único $a^{-1} \in G$ tal que $a * a^{-1} = a^{-1} * a = e$.

*Grupo abeliano (o conmutativo)*: Si además cumple la propiedad **conmutativa**: $a * b = b * a$ para todo $a, b \in G$.

### 1.2 Anillos
Un conjunto $A$ dotado de dos operaciones internas (generalmente suma $+$ y multiplicación $\cdot$), denotado por $(A, +, \cdot)$, es un **anillo** si:
1.  $(A, +)$ es un grupo abeliano.
2.  La multiplicación $\cdot$ es asociativa: $a \cdot (b \cdot c) = (a \cdot b) \cdot c$ para todo $a, b, c \in A$.
3.  Se cumple la propiedad **distributiva** del producto respecto a la suma:
    $$a \cdot (b + c) = a \cdot b + a \cdot c \quad \text{y} \quad (b + c) \cdot a = b \cdot a + c \cdot a$$

### 1.3 Cuerpos
Un **cuerpo** (denotado por $\mathbb{K}$) es un anillo conmutativo con elemento unitario ($1 \ne 0$) en el que todo elemento distinto del elemento neutro de la suma ($0$) tiene un inverso multiplicativo. Es decir:
*   $(\mathbb{K}, +)$ es un grupo abeliano (neutro: $0$).
*   $(\mathbb{K} \setminus \{0\}, \cdot)$ es un grupo abeliano (neutro: $1$).

Ejemplos comunes de cuerpos infinitos son los números reales $\mathbb{R}$, los números complejos $\mathbb{C}$ y los racionales $\mathbb{Q}$. Sin embargo, en informática trabajamos mayoritariamente con **cuerpos finitos** (cuerpos con un número finito de elementos).

---

## 2. Aritmética Modular y Algoritmo de Euclides Extendido

La aritmética modular opera sobre el conjunto de los restos de división por un entero positivo $m$, denotado por $\mathbb{Z}_m = \{0, 1, 2, \dots, m-1\}$.

### Relación de Congruencia
Decimos que dos enteros $a$ y $b$ son **congruentes módulo $m$**, escrito como:
$$a \equiv b \pmod m$$
si su diferencia $a-b$ es un múltiplo entero de $m$ (es decir, $m$ divide a $a-b$).

### Propiedades de $\mathbb{Z}_m$
*   $(\mathbb{Z}_m, +)$ siempre es un grupo abeliano.
*   $(\mathbb{Z}_m, +, \cdot)$ es un anillo conmutativo unitario.
*   $(\mathbb{Z}_m, +, \cdot)$ es un **cuerpo** si y solo si $m$ es un número **primo** $p$ (denotado por $\mathbb{Z}_p$ o $\mathbb{F}_p$). En este caso, cada elemento $a \ne 0$ posee un único inverso modular $a^{-1} \pmod p$ tal que $a \cdot a^{-1} \equiv 1 \pmod p$.

### Algoritmo de Euclides Extendido (Cálculo del Inverso Modular)
Para hallar el inverso multiplicativo de $a$ módulo $m$, requerimos que $\text{mcd}(a, m) = 1$ (es decir, que sean coprimos). El **Algoritmo de Euclides Extendido** calcula enteros $x$ e $y$ (coeficientes de Bézout) tales que:
$$a \cdot x + m \cdot y = \text{mcd}(a, m) = 1$$

Aplicando aritmética modular módulo $m$:
$$a \cdot x \equiv 1 \pmod m$$
Esto significa que $x$ es el inverso modular de $a$ módulo $m$ ($a^{-1} \equiv x \pmod m$).

---

## 3. Cuerpos Finitos (Cuerpos de Galois $\text{GF}(q)$)

Un cuerpo finito existe si y solo si el número de sus elementos (orden) es de la forma:
$$q = p^m$$
donde $p$ es un número primo (la **característica** del cuerpo) y $m \ge 1$ es un entero positivo. Estos cuerpos se denotan por $\text{GF}(p^m)$.
*   Si $m = 1$: El cuerpo es $\text{GF}(p) \cong \mathbb{Z}_p$, con la aritmética modular habitual.
*   Si $m > 1$: Los elementos del cuerpo $\text{GF}(p^m)$ no son simplemente enteros modulo $p^m$. Se representan como **polinomios de grado menor que $m$** con coeficientes en $\mathbb{Z}_p$. Las operaciones de suma y multiplicación se realizan módulo un **polinomio irreducible** $P(x)$ de grado $m$.

---

## 4. El Toque Informático

### 4.1 Criptografía Asimétrica (Algoritmo RSA)
El cifrado RSA se basa en la dificultad matemática de factorizar el producto de dos números primos grandes.
1.  Se eligen dos primos grandes $p$ y $q$, y se calcula $n = p \cdot q$.
2.  Se calcula la función indicadora de Euler $\phi(n) = (p-1)(q-1)$.
3.  Se elige un entero coprimo $e$ con $\phi(n)$ ($1 < e < \phi(n)$), que actúa como clave pública.
4.  Se calcula el inverso modular $d = e^{-1} \pmod{\phi(n)}$ mediante el algoritmo de Euclides extendido. El valor $d$ es la clave privada.
5.  **Cifrado**: Para un mensaje $M$: $C = M^e \pmod n$.
6.  **Descifrado**: $M = C^d \pmod n$.

### 4.2 Criptografía Simétrica: Cuerpos de Galois en AES
El estándar de cifrado simétrico **AES** (Advanced Encryption Standard) utiliza el cuerpo finito de Galois $\text{GF}(2^8)$ para realizar operaciones de confusión y difusión de bytes. El polinomio irreducible de grado 8 utilizado en AES es:
$$P(x) = x^8 + x^4 + x^3 + x + 1$$
Cada byte se interpreta como un polinomio de grado menor a 8. La operación de sustitución S-Box de AES consiste en calcular el inverso multiplicativo de cada byte en $\text{GF}(2^8)$ módulo $P(x)$, lo cual proporciona una excelente no-linealidad frente a criptoanálisis lineal.

A continuación, implementamos en Matlab/Octave el algoritmo de Euclides extendido y simulamos el cálculo de claves RSA.

```octave
% Función que implementa el Algoritmo de Euclides Extendido
function [d, x, y] = euclides_extendido(a, b)
    if b == 0
        d = a;
        x = 1;
        y = 0;
    else
        [d, x1, y1] = euclides_extendido(b, mod(a, b));
        x = y1;
        y = x1 - floor(a / b) * y1;
    end
end

% Script de simulación para cálculo de Claves RSA
p = 61; % Número primo
q = 53; % Número primo
n = p * q;
phi = (p - 1) * (q - 1);

e = 17; % Clave pública (debe ser coprimo con phi)

% Calculamos la clave privada d como el inverso modular de e módulo phi
[mcd, d, ~] = euclides_extendido(e, phi);

% El inverso d puede ser negativo, lo ajustamos al rango [0, phi)
d = mod(d, phi);

printf("Parámetros RSA:\n");
printf("Primos: p = %d, q = %d\n", p, q);
printf("Módulo: n = %d\n", n);
printf("Función phi(n) = %d\n", phi);
printf("Clave Pública: e = %d (mcd con phi: %d)\n", e, mcd);
printf("Clave Privada calculada: d = %d\n\n", d);

% Cifrado y descifrado de prueba
mensaje = 65; % Letra 'A' en ASCII
cifrado = mod(mensaje^e, n); % Cifrado básico

% Para el descifrado usamos la función de exponenciación modular rápida para evitar overflow
descifrado = 1;
base = mod(cifrado, n);
exp_temp = d;
while exp_temp > 0
    if mod(exp_temp, 2) == 1
        descifrado = mod(descifrado * base, n);
    end
    base = mod(base * base, n);
    exp_temp = floor(exp_temp / 2);
end

printf("Mensaje Original: %d\n", mensaje);
printf("Mensaje Cifrado:  %d\n", cifrado);
printf("Mensaje Descifrado: %d\n", descifrado);
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Encontrar el inverso multiplicativo de $13$ módulo $60$, es decir, resolver $13 \cdot x \equiv 1 \pmod{60}$.

**Solución usando el Algoritmo de Euclides Extendido:**
1.  **Fase de divisiones sucesivas (Euclides ordinario)**:
    *   $60 = 4 \cdot 13 + 8$
    *   $13 = 1 \cdot 8 + 5$
    *   $8 = 1 \cdot 5 + 3$
    *   $5 = 1 \cdot 3 + 2$
    *   $3 = 1 \cdot 2 + 1$ (el mcd es 1, confirmando la existencia de inverso).
2.  **Fase de sustitución hacia atrás (Extended)**:
    Despejamos los restos de cada ecuación:
    *   $1 = 3 - 1 \cdot 2$
    *   $2 = 5 - 1 \cdot 3$
    *   $3 = 8 - 1 \cdot 5$
    *   $5 = 13 - 1 \cdot 8$
    *   $8 = 60 - 4 \cdot 13$
    
    Ahora, sustituimos secuencialmente:
    *   $1 = 3 - 1 \cdot (5 - 1 \cdot 3) = 2 \cdot 3 - 1 \cdot 5$
    *   $1 = 2 \cdot (8 - 1 \cdot 5) - 1 \cdot 5 = 2 \cdot 8 - 3 \cdot 5$
    *   $1 = 2 \cdot 8 - 3 \cdot (13 - 1 \cdot 8) = 5 \cdot 8 - 3 \cdot 13$
    *   $1 = 5 \cdot (60 - 4 \cdot 13) - 3 \cdot 13 = 5 \cdot 60 - 23 \cdot 13$
3.  **Resultado**:
    La ecuación de Bézout obtenida es:
    $$(-23) \cdot 13 + 5 \cdot 60 = 1$$
    Por tanto, aplicando el módulo 60:
    $$-23 \cdot 13 \equiv 1 \pmod{60}$$
    Ajustando al rango positivo:
    $$x \equiv -23 + 60 = 37 \pmod{60}$$
    El inverso multiplicativo de $13$ módulo $60$ es $37$. (Comprobación: $13 \cdot 37 = 481 = 8 \cdot 60 + 1 \equiv 1 \pmod{60}$).

---

## 6. Ejercicios Propuestos

1.  Resolver la congruencia lineal $7x \equiv 3 \pmod{29}$.
2.  Definir las tablas de sumar y multiplicar del cuerpo finito $\text{GF}(3) \cong \mathbb{Z}_3$. ¿Cuáles son los inversos multiplicativos de cada uno de sus elementos no nulos?
3.  Explicar por qué el anillo $(\mathbb{Z}_8, +, \cdot)$ no es un cuerpo. Muestra al menos un elemento distinto de cero que carezca de inverso multiplicativo.
