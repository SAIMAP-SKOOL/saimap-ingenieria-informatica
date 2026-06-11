# Tema 9: Funciones Generadoras

Una **función generadora** es una herramienta matemática sumamente potente que permite codificar una sucesión infinita de números reales $(a_0, a_1, a_2, \dots)$ como los coeficientes de una serie formal de potencias. El célebre matemático Herbert Wilf la definió de forma poética: *"Una función generadora es un tendedero en el que colgamos una sucesión de números para exhibirlos"*. En informática, se emplean para modelar problemas de recuento complejos, particiones de memoria y para resolver de forma elegante relaciones de recurrencia.

---

## 1. Definición de Función Generadora Ordinaria (FGO)

Dada una sucesión $(a_n)_{n \ge 0} = (a_0, a_1, a_2, \dots)$, su **Función Generadora Ordinaria** es la serie de potencias formal:
$$A(x) = \sum_{n=0}^{\infty} a_n x^n = a_0 + a_1 x + a_2 x^2 + a_3 x^3 + \dots$$
donde $x$ es una variable auxiliar abstracta (no nos preocupa su convergencia numérica, sino la manipulación algebraica de sus coeficientes).

### Series de Potencias Fundamentales
Para trabajar con funciones generadoras, asociamos funciones racionales compactas con sus correspondientes desarrollos infinitos:

| Función Rational $A(x)$ | Serie de Potencias Asociada | Sucesión Generada $(a_n)$ |
| :---: | :---: | :---: |
| $\frac{1}{1-x}$ | $1 + x + x^2 + x^3 + \dots$ | $(1, 1, 1, 1, \dots)$ |
| $\frac{1}{1-ax}$ | $1 + ax + a^2 x^2 + a^3 x^3 + \dots$ | $(1, a, a^2, a^3, \dots)$ |
| $\frac{1}{(1-x)^2}$ | $1 + 2x + 3x^2 + 4x^3 + \dots$ | $(1, 2, 3, 4, \dots)$ |
| $(1+x)^k$ | $\sum_{n=0}^k \binom{k}{n} x^n$ | Coeficientes binomiales $\binom{k}{n}$ |

---

## 2. Operaciones Con Funciones Generadoras

Si $A(x) = \sum a_n x^n$ y $B(x) = \sum b_n x^n$:
1.  **Suma (Combinación Lineal)**:
    $$\alpha A(x) + \beta B(x) = \sum_{n=0}^{\infty} (\alpha a_n + \beta b_n) x^n$$
2.  **Desplazamiento a la derecha**:
    $$x^k A(x) = \sum_{n=0}^{\infty} a_n x^{n+k} = a_0 x^k + a_1 x^{k+1} + \dots \quad \implies \text{genera } (\underbrace{0, \dots, 0}_{k \text{ veces}}, a_0, a_1, \dots)$$
3.  **Derivación (Multiplicación por el índice)**:
    $$A'(x) = \sum_{n=1}^{\infty} n a_n x^{n-1} \quad \implies \quad x A'(x) = \sum_{n=0}^{\infty} n a_n x^n \quad \implies \text{genera } (0, a_1, 2a_2, 3a_3, \dots)$$

---

## 3. Resolución de Recurrencias mediante Funciones Generadoras

El método general consta de 4 pasos lógicos:
1.  **Definir la serie**: Expresar la función generadora $A(x) = \sum_{n=0}^{\infty} a_n x^n$.
2.  **Multiplicar y Sumar**: Multiplicar la ecuación de recurrencia por $x^n$ y sumar desde el orden de la relación hasta el infinito ($\sum_{n=k}^{\infty}$).
3.  **Sustituir y Despejar**: Expresar los sumatorios en términos de la función $A(x)$ y las condiciones iniciales, despejando algebraicamente la función $A(x)$.
4.  **Descomponer y Expandir**: Descomponer la función racional en fracciones simples y reescribirlas como series de potencias conocidas para extraer el coeficiente general $a_n$.

---

## 4. El Toque Informático

### Estructuras de Datos Balanceadas y los Números de Catalán
Un problema clásico en informática es determinar cuántos **árboles binarios de búsqueda distintos** se pueden construir utilizando exactamente $n$ nodos con claves únicas. 
*   Si llamamos $b_n$ a este número, podemos definir una relación de recurrencia en función del tamaño del subárbol izquierdo ($k$ nodos) y del derecho ($n-1-k$ nodos):
    $$b_n = \sum_{k=0}^{n-1} b_k \cdot b_{n-1-k} \quad (\text{con } b_0 = 1)$$
*   Esta convolución cuadrática se resuelve utilizando funciones generadoras resultando en la ecuación cuadrática $x B(x)^2 - B(x) + 1 = 0$.
*   La expansión de esta función da como resultado la famosa sucesión de los **Números de Catalán**:
    $$C_n = \frac{1}{n+1} \binom{2n}{n}$$

Saber que existen $C_n$ árboles posibles permite diseñar algoritmos de optimización para equilibrar árboles AVL o Red-Black minimizando el coste de búsqueda $O(\log n)$ en memoria.

A continuación, implementamos en Python una simulación utilizando cálculo simbólico con la librería `sympy` (si está instalada, o una aproximación con derivadas numéricas) para expandir una función generadora y extraer sus coeficientes.

```python
import sympy as sp

# Definimos la variable simbólica x
x = sp.Symbol('x')

# Definimos la función generadora racional: A(x) = 1 / (1 - 3*x) + 2 / (1 - x)
A = 1 / (1 - 3*x) + 2 / (1 - x)

# Realizamos la expansión en serie de Taylor alrededor de x = 0 hasta el grado 10
expansion = sp.series(A, x, 0, 10)
print("Expansión en serie de Taylor de A(x):")
print(expansion)

# Extraemos los coeficientes individuales de la serie para verificar la sucesión
print("\nSucesión de coeficientes generada (primeros 7 términos):")
for n in range(7):
    # Extraemos el coeficiente de x^n
    coeff = A.diff(x, n).subs(x, 0) // sp.factorial(n)
    print(f"  a_{n} = {coeff}")
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Dada la función generadora:
$$A(x) = \frac{1}{1 - 3x} + \frac{2}{1 - x}$$
Determinar el término general de la sucesión $(a_n)$ que genera.

**Solución:**
1.  **Separar los términos**:
    Tenemos $A(x) = A_1(x) + A_2(x)$, donde $A_1(x) = \frac{1}{1-3x}$ y $A_2(x) = 2 \cdot \frac{1}{1-x}$.
2.  **Identificar las series elementales**:
    *   $\frac{1}{1-3x} = \sum_{n=0}^{\infty} (3^n) x^n$ (sucesión asociada: $3^n$).
    *   $2 \cdot \frac{1}{1-x} = 2 \cdot \sum_{n=0}^{\infty} (1^n) x^n = \sum_{n=0}^{\infty} 2 x^n$ (sucesión asociada: constante $2$).
3.  **Combinar coeficientes**:
    Sumando los coeficientes de las potencias correspondientes de $x^n$:
    $$a_n = 3^n + 2 \quad (\text{para } n \ge 0)$$

La sucesión generada es: $(3, 5, 11, 29, 83, \dots)$.

### Ejercicio 2
Resolver por medio de funciones generadoras la relación de recurrencia:
$$a_n = 2a_{n-1} \quad (\text{para } n \ge 1) \quad \text{con } a_0 = 1$$

**Solución:**
1.  **Definir la función generadora**:
    $$A(x) = \sum_{n=0}^{\infty} a_n x^n = a_0 + \sum_{n=1}^{\infty} a_n x^n$$
2.  **Multiplicar la relación por $x^n$ y sumar desde $n=1$**:
    $$\sum_{n=1}^{\infty} a_n x^n = \sum_{n=1}^{\infty} 2a_{n-1} x^n$$
3.  **Reescribir los términos en función de $A(x)$**:
    *   Lado izquierdo: $\sum_{n=1}^{\infty} a_n x^n = A(x) - a_0 = A(x) - 1$.
    *   Lado derecho: sacamos constantes y $x$ fuera del sumatorio para alinear los índices:
        $$\sum_{n=1}^{\infty} 2a_{n-1} x^n = 2x \sum_{n=1}^{\infty} a_{n-1} x^{n-1} = 2x \sum_{m=0}^{\infty} a_m x^m = 2x A(x)$$
4.  **Igualar y despejar $A(x)$**:
    $$A(x) - 1 = 2x A(x) \quad \implies \quad A(x)(1 - 2x) = 1 \quad \implies \quad A(x) = \frac{1}{1 - 2x}$$
5.  **Expandir de vuelta**:
    Sabemos que la expansión de $\frac{1}{1-2x}$ es $\sum_{n=0}^{\infty} 2^n x^n$.
    Extrayendo el coeficiente de $x^n$:
    $$a_n = 2^n$$

---

## 6. Ejercicios Propuestos

1.  Halla la función generadora en forma racional compacta para la sucesión $(1, 3, 9, 27, 81, \dots)$.
2.  Encuentra los primeros 4 coeficientes de la función generadora $A(x) = \frac{1}{(1-x)^3}$ mediante derivación sucesiva.
3.  Utiliza el método de funciones generadoras para resolver la relación de recurrencia $a_n = 3a_{n-1} + 2$ con condición inicial $a_0 = 0$.
