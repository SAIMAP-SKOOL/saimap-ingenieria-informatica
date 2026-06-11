# Tema 10: Demostración de Corrección en Algoritmos Recursivos

La estructura de un algoritmo recursivo (Caso Base e Inductivo) se corresponde de forma directa con la estructura lógica del **Principio de Inducción Matemática**. Por tanto, la inducción es la herramienta natural y formal para demostrar que una función recursiva es correcta para cualquier valor de entrada.

---

## 1. El Principio de Inducción Matemática (Repaso)

Para demostrar que una propiedad o predicado $P(n)$ es verdadero para todo número entero $n \ge n_0$, seguimos dos pasos:

1.  **Caso Base**: Demostrar que la propiedad es verdadera para el valor inicial, es decir, verificar $P(n_0)$.
2.  **Paso Inductivo**:
    *   **Hipótesis de Inducción (H.I.)**: Asumimos que la propiedad es verdadera para cualquier valor menor que $n$, es decir, asumimos que $P(k)$ es cierto para todo $k < n$.
    *   **Tesis Inductiva**: Demostramos que, bajo la asunción de la H.I., la propiedad también es verdadera para el elemento actual $n$, es decir, demostramos $P(n)$.

Si se cumplen ambos pasos, la propiedad $P(n)$ queda demostrada para todo $n \ge n_0$.

---

## 2. Metodología de Demostración para Funciones Recursivas

Dada una función recursiva en Java, el procedimiento formal consta de:

1.  **Definir el Predicado de Corrección $P(n)$**:
    Expresar formalmente qué significa que la función sea correcta para un valor de entrada $n$.
    *   *Ejemplo*: $P(n): \text{potencia}(a, n) = a^n$.
2.  **Verificar el Caso Base**:
    Analizar la rama del código que no realiza llamadas recursivas (caso de parada) y comprobar que el valor devuelto coincide con el resultado matemático esperado.
3.  **Demostrar el Paso Inductivo**:
    *   Plantear la **H.I.**: Suponer que la función devuelve el resultado correcto para entradas más pequeñas (ej: $n-1$).
    *   Analizar la rama recursiva del código para la entrada $n$. Sustituir la llamada recursiva interna por su valor teórico (justificado por la H.I.) y operar algebraicamente hasta llegar al resultado teórico esperado para $n$.

---

## 3. Ejemplo Práctico de Demostración Formal

Consideremos el siguiente método recursivo en Java diseñado para calcular la potencia entera no negativa $a^n$ (con $a \neq 0$):

```java
public static double potencia(double a, int n) {
    if (n == 0) {
        return 1.0;
    }
    return a * potencia(a, n - 1);
}
```

Queremos demostrar la corrección del algoritmo para todo $n \ge 0$.

### Demostración por Inducción sobre $n$:

1.  **Definición del Predicado de Corrección $P(n)$**:
    $$P(n): \text{La llamada } \text{potencia}(a, n) \text{ termina y devuelve el valor } a^n$$

2.  **Caso Base ($n = 0$)**:
    *   Evaluamos el código para $n = 0$.
    *   La condición `n == 0` es verdadera, por lo que el programa entra en el bloque `if` y ejecuta `return 1.0;`.
    *   Sabemos matemáticamente que $a^0 = 1.0$ (para $a \neq 0$).
    *   Por lo tanto, la función devuelve $1.0$, que coincide con $a^0$. El caso base $P(0)$ es **verdadero**.

3.  **Paso Inductivo ($n > 0$)**:
    *   **Hipótesis de Inducción (H.I.)**: Asumimos que la función es correcta para $n-1$, es decir, asumimos que $P(n-1)$ es verdadero:
        $$\text{potencia}(a, n - 1) = a^{n-1}$$
    *   **Tesis**: Debemos demostrar que entonces $P(n)$ es verdadero (la llamada `potencia(a, n)` devuelve $a^n$).
    *   Evaluamos el código para $n > 0$.
    *   Como $n \neq 0$, se salta el bloque `if` y se ejecuta la instrucción:
        $$\text{retorno} = a \times \text{potencia}(a, n - 1)$$
    *   Por la Hipótesis de Inducción (H.I.), podemos sustituir la llamada recursiva `potencia(a, n-1)` por su resultado asumido $a^{n-1}$:
        $$\text{retorno} = a \times a^{n-1}$$
    *   Operando algebraicamente:
        $$\text{retorno} = a^1 \times a^{n-1} = a^{1 + (n-1)} = a^n$$
    *   El valor devuelto es exactamente $a^n$, lo que demuestra la tesis $P(n)$.

*Conclusión: Por el principio de inducción matemática, queda demostrado que el algoritmo recursivo calcula correctamente la potencia para todo $n \ge 0$.*

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Dada la siguiente función recursiva que calcula la suma de los primeros $n$ números enteros positivos:
```java
public static int sumaN(int n) {
    if (n == 1) {
        return 1;
    }
    return n + sumaN(n - 1);
}
```
Demostrar por inducción matemática sobre $n$ que el algoritmo es correcto para todo $n \ge 1$, es decir, que devuelve:
$$\sum_{i=1}^n i = \frac{n(n+1)}{2}$$

**Solución**:

1.  **Definir el Predicado de Corrección $P(n)$**:
    $$P(n): \text{sumaN}(n) = \frac{n(n+1)}{2}$$

2.  **Caso Base ($n = 1$)**:
    *   Evaluamos el código para $n = 1$.
    *   La condición `n == 1` se cumple, por lo que el método ejecuta `return 1;`.
    *   Sustituimos $n = 1$ en la fórmula teórica:
        $$\frac{1(1+1)}{2} = \frac{2}{2} = 1$$
    *   Como el valor devuelto ($1$) coincide con la fórmula teórica, $P(1)$ es **verdadero**.

3.  **Paso Inductivo ($n > 1$)**:
    *   **Hipótesis de Inducción (H.I.)**: Asumimos que $P(n-1)$ es verdadero:
        $$\text{sumaN}(n - 1) = \frac{(n-1)((n-1)+1)}{2} = \frac{(n-1)n}{2}$$
    *   **Tesis**: Demostrar que $\text{sumaN}(n) = \frac{n(n+1)}{2}$.
    *   Evaluamos el código para $n > 1$. Como $n \neq 1$, el método ejecuta:
        $$\text{retorno} = n + \text{sumaN}(n - 1)$$
    *   Aplicamos la H.I. sustituyendo `sumaN(n - 1)` por su valor asumido:
        $$\text{retorno} = n + \frac{(n-1)n}{2}$$
    *   Operamos algebraicamente para unificar el denominador:
        $$\text{retorno} = \frac{2n + n(n-1)}{2} = \frac{2n + n^2 - n}{2} = \frac{n^2 + n}{2} = \frac{n(n+1)}{2}$$
    *   El valor resultante coincide exactamente con la fórmula teórica de la tesis $P(n)$.

*Conclusión: Queda demostrado por inducción formal que el método es correcto para todo $n \ge 1$.*
