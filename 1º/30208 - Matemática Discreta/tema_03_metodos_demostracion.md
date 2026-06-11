# Tema 3: Métodos de Demostración

Una demostración es un argumento lógico riguroso que establece de forma incontestable la veracidad de un teorema matemático. En las ciencias de la computación, las técnicas de demostración no solo sirven para validar enunciados matemáticos abstractos, sino para verificar formalmente que un programa de software es correcto o que un algoritmo termina y devuelve el resultado esperado para cualquier entrada posible.

---

## 1. Demostración Directa

Para demostrar una implicación $P \to Q$, la técnica más sencilla consiste en asumir que la premisa (antecedente) $P$ es verdadera y, utilizando definiciones, axiomas y teoremas ya demostrados, deducir lógicamente que la conclusión (consecuente) $Q$ también es verdadera.

### Ejemplo
Demostrar que la suma de dos números enteros pares es par:
1.  **Definición de par**: Un entero $x$ es par si existe un entero $k$ tal que $x = 2k$.
2.  Sean $a$ y $b$ dos números enteros pares. Por definición:
    $$a = 2k_1, \quad b = 2k_2 \quad (\text{para ciertos enteros } k_1, k_2)$$
3.  Calculamos la suma:
    $$a + b = 2k_1 + 2k_2 = 2(k_1 + k_2)$$
4.  Como la suma de dos enteros ($k_1 + k_2$) es otro número entero $k_3$, tenemos que:
    $$a + b = 2k_3$$
5.  Por tanto, la suma $a + b$ es par. Q.E.D. (Quod erat demonstrandum).

---

## 2. Demostración por Contrarrecíproco (Contraposición)

Esta técnica se basa en la equivalencia lógica entre una implicación y su contrarrecíproco:
$$P \to Q \equiv \neg Q \to \neg P$$

Para demostrar $P \to Q$, asumimos que $Q$ es falso ($\neg Q$) y demostramos que, bajo esa premisa, $P$ también es falso ($\neg P$). Es de gran utilidad cuando negar el consecuente ofrece propiedades algebraicas más directas para trabajar.

---

## 3. Demostración por Contradicción (Reducción al Absurdo)

Consiste en asumir que la afirmación que deseamos demostrar ($P$) es falsa ($\neg P$) y derivar de ahí una contradicción o imposibilidad lógica (un enunciado del tipo $R \land \neg R$, como por ejemplo demostrar que un número es par e impar simultáneamente o que $0 = 1$).
Si negar la hipótesis nos conduce a un absurdo inviable, la hipótesis de partida debe ser necesariamente verdadera.

---

## 4. El Principio de Inducción Matemática

Se utiliza para demostrar propiedades asociadas al conjunto de los números enteros positivos ($\mathbb{N}$). Consiste en el "efecto dominó": si demostramos que la propiedad se cumple para el primer elemento y que la veracidad de un elemento cualquiera arrastra al siguiente, entonces la propiedad es verdadera para todo el conjunto infinito de enteros.

El proceso consta de tres pasos formales:
1.  **Paso Base**: Demostrar que la propiedad $P(n)$ es verdadera para el primer elemento (típicamente $n = 1$ o $n = 0$).
2.  **Hipótesis de Inducción (H.I.)**: Asumir que la propiedad es verdadera para un número entero genérico $k$, es decir, asumimos que $P(k)$ es válido.
3.  **Paso Inductivo**: Demostrar que, asumiendo la H.I., la propiedad se cumple obligatoriamente para el elemento siguiente $k+1$, es decir, demostrar que:
    $$P(k) \to P(k+1)$$

---

## 5. El Toque Informático

### Verificación Formal de Programas y Recursión
En informática, la inducción matemática y la **recursión** son caras de la misma moneda. Una función recursiva define un caso base (paso base de la inducción) y un paso recursivo que simplifica el problema (paso inductivo). 

Además, para verificar bucles iterativos se utiliza el concepto de **Invariante de Bucle (Loop Invariant)**: una condición lógica que debe cumplirse:
1.  Antes de entrar al bucle (Paso Base / Inicialización).
2.  En cada iteración del bucle, asumiendo que se cumplía antes (Paso Inductivo / Mantenimiento).
3.  Al terminar el bucle (Finalización), permitiendo demostrar formalmente que el algoritmo ha procesado la estructura de datos correctamente sin dar lugar a errores de rango o lógica.

A continuación, implementamos en Python una comparación conceptual entre el cálculo de una potencia de forma recursiva y su verificación mediante invariantes lógicas.

```python
# Algoritmo de exponenciación recursiva rápida: potencia(x, n) = x^n
# Caso base: n = 0 -> x^0 = 1
# Paso inductivo/recursivo: x^n = x * x^(n-1)
def potencia(x, n):
    assert n >= 0, "n debe ser un entero no negativo"
    if n == 0:
        return 1  # Caso Base
    else:
        return x * potencia(x, n - 1)  # Paso Recursivo (Inductivo)

# Verificación de invariante mediante testing unitario inducido
x_base = 3
for exponente in range(10):
    resultado = potencia(x_base, exponente)
    resultado_esperado = x_base ** exponente
    
    # Comprobación de correctitud
    assert resultado == resultado_esperado, f"Error para exponente {exponente}"
    print(f"Verificado: {x_base}^{exponente} = {resultado} (Correcto)")

print("\n¡Algoritmo verificado formalmente mediante equivalencia recursiva!")
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Demostrar por contradicción (reducción al absurdo) que el número $\sqrt{2}$ es irracional.

**Solución:**
1.  **Hipótesis de partida**: Asumimos la negación del enunciado, es decir, asumimos que $\sqrt{2}$ **es un número racional**.
2.  Por definición de racional, podemos expresar $\sqrt{2}$ como una fracción irreducible de dos enteros $a$ y $b$ (donde $\text{mcd}(a, b) = 1$, es decir, no tienen factores comunes):
    $$\sqrt{2} = \frac{a}{b}$$
3.  Elevamos ambos lados al cuadrado:
    $$2 = \frac{a^2}{b^2} \quad \implies \quad a^2 = 2b^2$$
4.  Puesto que $a^2$ es igual a $2$ por un entero ($b^2$), concluimos que $a^2$ es un número par. Si el cuadrado de un número es par, el propio número también debe ser par (se demuestra fácilmente por contraposición). Por tanto, escribimos:
    $$a = 2k \quad (\text{para algún entero } k)$$
5.  Sustituimos $a = 2k$ en la ecuación del paso 3:
    $$(2k)^2 = 2b^2 \quad \implies \quad 4k^2 = 2b^2 \quad \implies \quad 2k^2 = b^2$$
6.  Dado que $b^2 = 2k^2$, deducimos que $b^2$ también es par, y por ende $b$ también debe ser par.
7.  **Llegada al absurdo**: Hemos demostrado que $a$ es par y $b$ es par. Esto significa que ambos son divisibles por $2$, lo cual entra en **contradicción directa** con la premisa inicial de que la fracción $\frac{a}{b}$ era irreducible ($\text{mcd}(a, b) = 1$).
8.  Al conducir la negación a una contradicción matemática insoluble, la hipótesis del absurdo debe descartarse: por lo tanto, $\sqrt{2}$ es irracional. Q.E.D.

### Ejercicio 2
Demostrar por inducción matemática que para todo entero $n \ge 1$ se cumple:
$$1 + 2 + 3 + \dots + n = \frac{n(n+1)}{2}$$

**Solución:**
Definimos la propiedad $P(n) \equiv \sum_{i=1}^n i = \frac{n(n+1)}{2}$.

1.  **Paso Base**: Evaluamos para $n = 1$:
    *   Lado izquierdo: $1$.
    *   Lado derecho: $\frac{1(1+1)}{2} = \frac{2}{2} = 1$.
    *   Como $1 = 1$, el paso base es verdadero.
2.  **Hipótesis de Inducción (H.I.)**: Asumimos que $P(k)$ es verdadero para un entero $k \ge 1$:
    $$1 + 2 + 3 + \dots + k = \frac{k(k+1)}{2}$$
3.  **Paso Inductivo**: Debemos demostrar que $P(k+1)$ se cumple, es decir, que:
    $$1 + 2 + 3 + \dots + k + (k+1) = \frac{(k+1)((k+1)+1)}{2} = \frac{(k+1)(k+2)}{2}$$
    Desarrollamos el lado izquierdo usando la H.I. para sustituir la suma de los primeros $k$ términos:
    $$\underbrace{1 + 2 + 3 + \dots + k}_{\text{Sustituimos por H.I.}} + (k+1) = \frac{k(k+1)}{2} + (k+1)$$
    Sacamos factor común $(k+1)$:
    $$(k+1) \left( \frac{k}{2} + 1 \right) = (k+1) \left( \frac{k+2}{2} \right) = \frac{(k+1)(k+2)}{2}$$
    El resultado obtenido coincide exactamente con la expresión de $P(k+1)$. Queda demostrado por inducción que la propiedad se cumple para todo entero $n \ge 1$.

---

## 7. Ejercicios Propuestos

1.  Demuestra por contrarrecíproco el siguiente enunciado sobre enteros: "Si $3n+2$ es un número impar, entonces $n$ es impar".
2.  Demuestra por inducción matemática simple que para todo $n \ge 1$, la suma de los primeros $n$ números impares es igual a $n^2$:
    $$1 + 3 + 5 + \dots + (2n-1) = n^2$$
3.  Investiga el **Principio de Inducción Fuerte** y explica en qué se diferencia del principio de inducción simple. Pon un ejemplo de aplicación (como demostrar que todo entero mayor que 1 se puede descomponer en producto de primos).
