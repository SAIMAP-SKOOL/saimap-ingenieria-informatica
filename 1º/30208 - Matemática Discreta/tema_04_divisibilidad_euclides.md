# Tema 4: Divisibilidad, MCD y el Algoritmo de Euclides

La teoría de números es el estudio matemático de las propiedades de los números enteros ($\mathbb{Z}$). Aunque durante siglos se consideró una disciplina puramente teórica y abstracta, hoy en día es la columna vertebral de la seguridad informática. Comprender la divisibilidad, el cálculo eficiente del máximo común divisor ($mcd$) y la resolución de ecuaciones diofánticas es un prerrequisito esencial para descifrar el funcionamiento de los algoritmos criptográficos modernos.

---

## 1. Divisibilidad y División Entera

Decimos que un entero $a$ **divide** a un entero $b$ (o que $b$ es múltiplo de $a$, y se denota como $a \mid b$) si existe un número entero $k$ tal que:
$$b = a \cdot k$$
Si $a$ no divide a $b$, escribimos $a \nmid b$.

### Teorema de la División Entera
Dados dos enteros $a$ (dividendo) y $b$ (divisor, con $b \neq 0$), existen dos únicos enteros $q$ (cociente) y $r$ (resto) tales que:
$$a = b \cdot q + r \quad \text{donde} \quad 0 \le r < |b|$$

---

## 2. Máximo Común Divisor (MCD)

El **máximo común divisor** de dos enteros $a$ y $b$ (no ambos nulos), denotado como $mcd(a, b)$ o simplemente $d$, es el mayor entero que divide simultáneamente a ambos.
*   **Coprimidad (o primos entre sí)**: Dos enteros $a$ y $b$ son coprimos si y solo si $mcd(a, b) = 1$.

### Ineficiencia de la Factorización de Primos
El método clásico para hallar el $mcd$ descomponiendo los números en sus factores primos es extremadamente lento para números grandes. Encontrar los factores primos de un entero de 1024 bits requeriría miles de años de cómputo en supercomputadoras actuales. Por ello, recurrimos al **Algoritmo de Euclides**.

---

## 3. El Algoritmo de Euclides

Este algoritmo aprovecha la siguiente propiedad recursiva de la división: si $a = b \cdot q + r$, entonces:
$$mcd(a, b) = mcd(b, r)$$

El algoritmo consiste en realizar divisiones sucesivas hasta obtener un resto igual a cero. El último resto no nulo es el $mcd(a, b)$.

```
   a = b * q_1 + r_1       (mcd(a,b) = mcd(b, r_1))
   b = r_1 * q_2 + r_2     (mcd(b,r_1) = mcd(r_1, r_2))
   r_1 = r_2 * q_3 + 0     (El resto es cero)
   --> mcd(a,b) = r_2
```

---

## 4. Algoritmo de Euclides Extendido e Identidad de Bézout

La **Identidad de Bézout** afirma que, si $d = mcd(a, b)$, existen enteros $x$ e $y$ tales que:
$$a \cdot x + b \cdot y = d$$
Los enteros $x$ e $y$ se conocen como los **coeficientes de Bézout** y pueden hallarse despejando los restos del algoritmo de Euclides en sentido inverso o mediante un esquema matricial iterativo.

---

## 5. Resolución de Ecuaciones Diofánticas Lineales

Una **ecuación diofántica lineal** es una ecuación algebraica de la forma:
$$a \cdot x + b \cdot y = c$$
donde buscamos únicamente soluciones en las que $x$ e $y$ sean números enteros.

### Teorema de Existencia
La ecuación diofántica $ax + by = c$ tiene solución entera si y solo si el máximo común divisor de los coeficientes de entrada divide al término independiente:
$$mcd(a, b) \mid c$$
Si esta condición se cumple, podemos encontrar una solución particular $(x_0, y_0)$ mediante el algoritmo extendido de Euclides y generalizar la solución como:
$$x = x_0 + k \cdot \frac{b}{d}, \quad y = y_0 - k \cdot \frac{a}{d} \quad (\text{para cualquier entero } k)$$

---

## 6. El Toque Informático

### Complejidad del Algoritmo de Euclides y Aplicación en Criptografía
El algoritmo de Euclides es uno de los algoritmos más eficientes y antiguos conocidos.
*   **Complejidad Temporal**: El teorema de Lamé demuestra que el número de divisiones necesarias para calcular el $mcd$ de dos números es, como mucho, 5 veces el número de dígitos del número menor. Su complejidad es de **$O(\log(\min(a, b)))$**.
*   **Utilidad**: En criptografía, el cálculo del algoritmo extendido de Euclides es el paso computacional fundamental utilizado para hallar el **inverso multiplicativo modular** (indispensable para calcular la clave privada de descifrado en el algoritmo RSA).

A continuación, implementamos en Python el Algoritmo de Euclides Extendido que calcula el $mcd$ y devuelve los coeficientes de la Identidad de Bézout.

```python
def euclides_extendido(a, b):
    # Caso base
    if a == 0:
        return b, 0, 1
    
    # Llamada recursiva
    mcd, x1, y1 = euclides_extendido(b % a, a)
    
    # Actualización de los coeficientes de Bézout basados en la recursión
    x = y1 - (b // a) * x1
    y = x1
    
    return mcd, x, y

# Ejemplo de prueba con a = 252 y b = 198
num1, num2 = 252, 198
d, x, y = euclides_extendido(num1, num2)

print(f"Número 1: {num1}, Número 2: {num2}")
print(f"MCD calculado: {d}")
print(f"Coeficientes de Bézout: x = {x}, y = {y}")
print(f"Verificación: {num1}*({x}) + {num2}*({y}) = {num1*x + num2*y}")
```

---

## 7. Ejercicios Resueltos

### Ejercicio 1
Calcular el $mcd(252, 198)$ y hallar su identidad de Bézout de forma analítica (despejando restos).

**Solución:**
1.  **Algoritmo de Euclides (hacia adelante)**:
    *   $252 = 198 \cdot 1 + 54 \quad \implies (\text{resto } 54)$
    *   $198 = 54 \cdot 3 + 36 \quad \implies (\text{resto } 36)$
    *   $54 = 36 \cdot 1 + 18 \quad \implies (\text{resto } 18)$
    *   $36 = 18 \cdot 2 + 0 \quad \implies (\text{resto } 0)$
    *   El último resto no nulo es **18**. Por tanto, $mcd(252, 198) = 18$.
2.  **Identidad de Bézout (despeje en retroceso)**:
    *   Despejamos el $18$ de la última ecuación no nula:
        $$18 = 54 - 36 \cdot 1$$
    *   Despejamos el $36$ de la ecuación previa y sustituimos:
        $$36 = 198 - 54 \cdot 3$$
        $$18 = 54 - (198 - 54 \cdot 3) \cdot 1 = 54 \cdot 4 - 198 \cdot 1$$
    *   Despejamos el $54$ de la primera ecuación y sustituimos:
        $$54 = 252 - 198 \cdot 1$$
        $$18 = (252 - 198 \cdot 1) \cdot 4 - 198 \cdot 1 = 252 \cdot 4 - 198 \cdot 4 - 198 \cdot 1$$
        $$18 = 252 \cdot (4) + 198 \cdot (-5)$$

Los coeficientes de Bézout son $x = 4$ e $y = -5$.

### Ejercicio 2
Resolver la ecuación diofántica lineal: $12x + 15y = 9$.

**Solución:**
1.  **Analizar existencia de solución**:
    *   Calculamos el $d = mcd(12, 15)$:
        $15 = 12 \cdot 1 + 3 \implies 12 = 3 \cdot 4 + 0 \implies mcd(12, 15) = 3$.
    *   Comprobamos si $d \mid c$: ¿$3 \mid 9$? Sí ($9 = 3 \cdot 3$). **La ecuación tiene solución**.
2.  **Hallar coeficientes de Bézout para el MCD**:
    *   De la división: $3 = 15 \cdot 1 + 12 \cdot (-1) \implies 12 \cdot (-1) + 15 \cdot (1) = 3$.
3.  **Escalar para el término independiente** ($c = 9$):
    *   Multiplicamos la ecuación previa por $3$ (pues $9 = 3 \cdot 3$):
        $$12 \cdot (-3) + 15 \cdot (3) = 9$$
    *   Una solución particular es: $x_0 = -3, \quad y_0 = 3$.
4.  **Expresar la solución general**:
    $$x = -3 + k \cdot \frac{15}{3} = -3 + 5k$$
    $$y = 3 - k \cdot \frac{12}{3} = 3 - 4k \quad (\text{para cualquier } k \in \mathbb{Z})$$

---

## 8. Ejercicios Propuestos

1.  Calcula el máximo común divisor de $323$ y $123$ mediante el algoritmo de Euclides y determina si son coprimos.
2.  Halla todas las soluciones enteras de la ecuación diofántica $21x + 14y = 35$. Si no existen soluciones enteras, justifica por qué.
3.  Demuestra que si $a$ y $b$ son enteros coprimos, entonces $mcd(a + b, a - b)$ solo puede ser igual a 1 o 2.
