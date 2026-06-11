# Tema 8: Algoritmos Recursivos

La recursividad es una técnica de programación que permite resolver un problema definiéndolo en términos de subproblemas más sencillos de sí mismo. En lugar de utilizar bucles iterativos (`for` o `while`), una función recursiva se invoca a sí misma de forma directa o indirecta para computar el resultado final.

---

## 1. Estructura de una Función Recursiva

Para que un algoritmo recursivo sea correcto y no entre en una ejecución infinita que sature la memoria, debe estructurarse obligatoriamente en dos partes:

1.  **Caso Base (o Condición de Parada)**:
    *   Es la versión más simple y pequeña del problema, cuya solución se calcula de forma directa sin necesidad de realizar nuevas llamadas recursivas.
    *   *Importancia*: Garantiza que la recursión se detiene.
2.  **Caso Inductivo (o Llamada Recursiva)**:
    *   Es la llamada a la propia función pero pasándole un parámetro simplificado, reducido o modificado que esté **garantizado que se acerca al caso base**.

### Ejemplo Clásico: El Factorial
Matemáticamente, el factorial de $n$ ($n!$) se define como:
$$n! = \begin{cases} 
  1 & \text{si } n = 0 \quad (\text{Caso Base}) \\ 
  n \times (n-1)! & \text{si } n > 0 \quad (\text{Caso Inductivo}) 
\end{cases}$$

Implementación directa en Java:
```java
public static int factorial(int n) {
    if (n == 0) {
        return 1; // Caso Base
    }
    return n * factorial(n - 1); // Caso Inductivo
}
```

---

## 2. Tipos de Recursión

Clasificamos los algoritmos recursivos según la cantidad y la estructura de las llamadas internas:

### A. Recursión Lineal
Ocurre cuando cada ejecución de la función genera, como máximo, **una única llamada recursiva** (por ejemplo, el factorial anterior o la búsqueda lineal en un array).

### B. Recursión No Lineal (ej: Recursión Binaria)
Ocurre cuando una llamada genera **múltiples llamadas recursivas independientes**, ramificando la ejecución en forma de árbol.
*   *Ejemplo*: El cálculo del término de Fibonacci:
    $$F(n) = F(n-1) + F(n-2)$$
*   *Problema*: Su coste temporal suele crecer de manera exponencial ($O(2^n)$) si no se optimiza, ya que repite el cálculo de los mismos subproblemas muchas veces.

### C. Recursión Mutua o Cruzada
Ocurre cuando dos o más funciones se llaman entre sí de forma cíclica. Por ejemplo, la función `esPar(n)` invoca a `esImpar(n-1)` y viceversa.

### D. Recursión de Cola (Tail Recursion)
Una función es recursiva de cola si la llamada recursiva es **la última instrucción exacta** que ejecuta la función, y su resultado se devuelve directamente sin realizar ninguna operación aritmética posterior.
*   *En Java*: A diferencia de los lenguajes de programación funcional (como Haskell o Scala) o compiladores de C/C++, **la JVM no optimiza la recursión de cola**. Cada llamada añade un nuevo marco de activación en el Stack, por lo que una recursión muy profunda (ej. $100.000$ llamadas) provocará un error de desbordamiento de pila: **`StackOverflowError`**.

---

## 3. Funcionamiento de la Pila de Llamadas (Call Stack)

La recursividad hace un uso intensivo del Stack. Cada llamada recursiva pausa la ejecución del método actual y añade un nuevo marco de activación en el tope del Stack, almacenando el estado de sus variables locales y la dirección de retorno.

Si calculamos `factorial(3)`:

```
Pila de Llamadas (Stack) en ejecución ascendente:
   +------------------------------------+
3º | factorial(1): n=1, espera retorno  |  --> retorna 1
   +------------------------------------+
2º | factorial(2): n=2, espera return 2*|  --> retorna 2 * 1 = 2
   +------------------------------------+
1º | factorial(3): n=3, espera return 3*|  --> retorna 3 * 2 = 6
   +------------------------------------+
```

Una vez que se alcanza el caso base (`factorial(1)`), la pila se empieza a desapilar de arriba a abajo, resolviendo las multiplicaciones pendientes y liberando la memoria.

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Implementar un algoritmo recursivo mutuo en Java para determinar si un número entero no negativo es par o impar en base a las siguientes reglas:
*   Un número $n$ es par si $n-1$ es impar. El número $0$ es par.
*   Un número $n$ es impar si $n-1$ es par. El número $0$ no es impar.

**Solución**:
```java
public class RecursionMutua {

    // Caso Base: 0 es par.
    // Caso Inductivo: n es par si n-1 es impar.
    public static boolean esPar(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("El número debe ser no negativo.");
        }
        if (n == 0) {
            return true; // Caso Base
        }
        return esImpar(n - 1); // Llamada cruzada a esImpar (Caso Inductivo)
    }

    // Caso Base: 0 no es impar (retorna false).
    // Caso Inductivo: n es impar si n-1 es par.
    public static boolean esImpar(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("El número debe ser no negativo.");
        }
        if (n == 0) {
            return false; // Caso Base
        }
        return esPar(n - 1); // Llamada cruzada a esPar (Caso Inductivo)
    }

    public static void main(String[] args) {
        System.out.println("¿Es 4 par?: " + esPar(4));     // Retorna true
        System.out.println("¿Es 5 par?: " + esPar(5));     // Retorna false
        System.out.println("¿Es 3 impar?: " + esImpar(3)); // Retorna true
    }
}
```
*(Nota: Aunque este algoritmo es muy didáctico para entender la recursión mutua, su uso práctico para comprobar paridad es ineficiente en comparación con el operador módulo `n % 2 == 0` de coste constante).*
