# Tema 5: Aritmética Modular y Congruencias

La aritmética modular es un sistema aritmético para números enteros donde los números "dan la vuelta" al llegar a un cierto valor límite llamado **módulo** ($m$). Se le conoce coloquialmente como "aritmética del reloj". Es el pilar fundamental del álgebra computacional moderna, proporcionando estructuras matemáticas finitas donde las operaciones aritméticas son rápidas, acotadas en memoria y perfectamente reversibles (inversos modulares).

---

## 1. Relación de Congruencia

Sean $a, b \in \mathbb{Z}$ y $m \in \mathbb{Z}^+$ (con $m > 1$). Decimos que $a$ es **congruente** con $b$ módulo $m$, y lo denotamos como:
$$a \equiv b \pmod m$$
si y solo si la diferencia $a - b$ es divisible por $m$ (es decir, $m \mid (a - b)$). Esto es equivalente a decir que $a$ y $b$ devuelven exactamente el mismo resto al dividirse entre $m$:
$$a \pmod m = b \pmod m$$

### El Conjunto de Clases de Equivalencia $\mathbb{Z}_m$
La congruencia es una relación de equivalencia (reflexiva, simétrica y transitiva). Divide a los enteros en exactamente $m$ clases de equivalencia representadas por el conjunto:
$$\mathbb{Z}_m = \{0, 1, 2, \dots, m-1\}$$

---

## 2. Aritmética en $\mathbb{Z}_m$ y Cálculo de Inversos

La aritmética modular conserva la suma y el producto:
1.  Si $a_1 \equiv b_1 \pmod m$ y $a_2 \equiv b_2 \pmod m$, entonces:
    $$(a_1 + a_2) \equiv (b_1 + b_2) \pmod m$$
    $$(a_1 \cdot a_2) \equiv (b_1 \cdot b_2) \pmod m$$

### El Inverso Modular
En la aritmética real, el inverso de $a$ es $1/a$. En aritmética modular, no existen los números decimales. Definimos el **inverso modular** de $a$ módulo $m$ como un entero $x \in \mathbb{Z}_m$ tal que:
$$a \cdot x \equiv 1 \pmod m$$

*   **Condición de Existencia**: El inverso de $a$ módulo $m$ existe si y solo si $a$ y $m$ son coprimos:
    $$mcd(a, m) = 1$$
*   **Cálculo**: Se calcula aplicando el Algoritmo de Euclides Extendido. Obtenemos los coeficientes $a \cdot x + m \cdot y = 1$. Tomando módulo $m$ en ambos lados:
    $$a \cdot x \equiv 1 \pmod m$$
    Por tanto, el coeficiente $x$ de Bézout (ajustado para ser positivo sumándole $m$ si es negativo) es el inverso modular buscado.

---

## 3. Teoremas Fundamentales

### 3.1 Teorema del Resto Chino (CRT)
Si tenemos un sistema de congruencias lineales con módulos coprimos dos a dos:
$$x \equiv a_1 \pmod{m_1}$$
$$x \equiv a_2 \pmod{m_2}$$
$$\dots$$
$$x \equiv a_k \pmod{m_k}$$
El teorema garantiza que existe una solución única para $x$ módulo $M = m_1 \cdot m_2 \dots m_k$.

### 3.2 Pequeño Teorema de Fermat
Si $p$ es un número primo y $a$ es un entero tal que $p \nmid a$, entonces:
$$a^{p-1} \equiv 1 \pmod p$$
Multiplicando por $a$, se obtiene la forma general válida para cualquier entero $a$:
$$a^p \equiv a \pmod p$$

### 3.3 Función $\phi$ de Euler y Teorema de Euler
La **función indicatriz de Euler** $\phi(n)$ define el número de enteros entre $1$ y $n$ que son coprimos con $n$.
*   Si $p$ es primo: $\phi(p) = p-1$.
*   Si $n = p \cdot q$ (con $p$ y $q$ primos distintos): $\phi(n) = (p-1)(q-1)$.
*   **Teorema de Euler**: Si $a$ y $n$ son coprimos ($mcd(a, n) = 1$), entonces:
    $$a^{\phi(n)} \equiv 1 \pmod n$$

---

## 4. El Toque Informático

### Algoritmos de Hashing, Checksums y Códigos de Control
La aritmética modular se utiliza para garantizar la integridad y distribución de los datos en informática:
1.  **Dígitos de Control (DNI, Tarjetas de Crédito, IBAN)**:
    El número de control del DNI en España se calcula tomando la parte numérica módulo 23: $\text{DNI} \pmod{23}$. El resto resultante se mapea con una letra específica. Si se introduce mal un dígito del número, el módulo no coincidirá y el sistema detectará el error de entrada instantáneamente.
2.  **Tablas Hash**:
    Para almacenar claves de forma uniforme en memoria minimizando colisiones, las funciones hash indexan elementos haciendo:
    $$\text{índice} = \text{clave} \pmod{\text{tamaño\_tabla}}$$
    Elegir un tamaño de tabla que sea un número primo minimiza las colisiones cuando las claves siguen patrones lógicos.

A continuación, implementamos en Python un calculador de inversos modulares utilizando el algoritmo de Euclides extendido.

```python
def modular_inverse(a, m):
    # Función auxiliar del algoritmo extendido
    def gcd_extended(a, b):
        if a == 0:
            return b, 0, 1
        g, x1, y1 = gcd_extended(b % a, a)
        return g, y1 - (b // a) * x1, x1

    g, x, y = gcd_extended(a, m)
    if g != 1:
        # Si el MCD no es 1, no existe inverso modular
        return None
    else:
        # Nos aseguramos de que el resultado sea positivo en Z_m
        return x % m

# Parámetros de prueba
a = 7
modulo = 26
inverso = modular_inverse(a, modulo)

if inverso is not None:
    print(f"El inverso de {a} mod {modulo} es: {inverso}")
    print(f"Verificación: ({a} * {inverso}) mod {modulo} = {(a * inverso) % modulo}")
else:
    print(f"No existe inverso modular para {a} mod {modulo} (no son coprimos)")
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Calcular el inverso modular de $7$ módulo $26$ de forma analítica.

**Solución:**
1.  **Verificar coprimidad**:
    Aplicamos Euclides para comprobar si $mcd(7, 26) = 1$:
    *   $26 = 7 \cdot 3 + 5$
    *   $7 = 5 \cdot 1 + 2$
    *   $5 = 2 \cdot 2 + 1 \quad \implies mcd(7, 26) = 1$. **Existe inverso**.
2.  **Despejar restos (Bézout)**:
    *   $1 = 5 - 2 \cdot 2$
    *   Sustituimos $2 = 7 - 5 \cdot 1$:
        $$1 = 5 - (7 - 5 \cdot 1) \cdot 2 = 5 \cdot 3 - 7 \cdot 2$$
    *   Sustituimos $5 = 26 - 7 \cdot 3$:
        $$1 = (26 - 7 \cdot 3) \cdot 3 - 7 \cdot 2 = 26 \cdot 3 - 7 \cdot 9 - 7 \cdot 2$$
        $$1 = 26 \cdot (3) + 7 \cdot (-11)$$
3.  **Aplicar congruencia módulo 26**:
    $$7 \cdot (-11) \equiv 1 \pmod{26}$$
    El inverso es $-11 \pmod{26}$. Lo pasamos a positivo sumándole el módulo:
    $$-11 + 26 = 15$$

Por tanto, el inverso modular de $7 \pmod{26}$ es **15**.

### Ejercicio 2
Resolver el sistema de congruencias lineales:
$$x \equiv 2 \pmod 3$$
$$x \equiv 3 \pmod 5$$

**Solución:**
Aplicamos el Teorema del Resto Chino:
1.  **Calcular el módulo total**: $M = m_1 \cdot m_2 = 3 \cdot 5 = 15$.
2.  **Calcular componentes parciales**:
    *   $M_1 = M / m_1 = 15 / 3 = 5$.
    *   $M_2 = M / m_2 = 15 / 5 = 3$.
3.  **Calcular inversos modulares de $M_i$**:
    *   Para $M_1$: $5 \cdot y_1 \equiv 1 \pmod 3 \implies 2 \cdot y_1 \equiv 1 \pmod 3 \implies y_1 = 2$.
    *   Para $M_2$: $3 \cdot y_2 \equiv 1 \pmod 5 \implies y_2 = 2$ (pues $3 \cdot 2 = 6 \equiv 1 \pmod 5$).
4.  **Construir la solución única**:
    $$x = (a_1 \cdot M_1 \cdot y_1 + a_2 \cdot M_2 \cdot y_2) \pmod M$$
    $$x = (2 \cdot 5 \cdot 2 + 3 \cdot 3 \cdot 2) \pmod{15}$$
    $$x = (20 + 18) \pmod{15} = 38 \pmod{15} = 8$$

La solución única al sistema de congruencias es $x \equiv 8 \pmod{15}$.

---

## 6. Ejercicios Propuestos

1.  Determina si existe el inverso de $12 \pmod{30}$. Si existe, calcúlalo; si no existe, justifica algebraicamente la respuesta.
2.  Resuelve el sistema de congruencias mediante el Teorema del Resto Chino:
    $$x \equiv 1 \pmod 5, \quad x \equiv 2 \pmod 7$$
3.  Utiliza el Pequeño Teorema de Fermat para simplificar y calcular rápidamente el valor de la potencia gigante $3^{202} \pmod{101}$ sin realizar multiplicaciones iteradas.
