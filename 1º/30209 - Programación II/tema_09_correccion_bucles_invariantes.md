# Tema 9: Demostración de Corrección en Bucles (Invariantes y Cotas)

La demostración formal de la corrección de un algoritmo nos permite asegurar matemáticamente que un programa se comporta exactamente según su especificación, sin importar los datos de entrada y sin depender de realizar pruebas empíricas. En este tema nos enfocaremos en bucles iterativos utilizando **Invariantes de Bucle** y **Funciones de Cota**.

---

## 1. Tripletas de Hoare y Corrección de Software

Para especificar y demostrar la corrección de un fragmento de código $S$, utilizamos la lógica de Hoare basada en tripletas:
$$\{Q\} \, S \, \{R\}$$

Donde:
*   **$Q$**: Precondición (estado de las variables antes de ejecutar $S$).
*   **$R$**: Postcondición (estado de las variables esperado al finalizar $S$).

Diferenciamos dos tipos de corrección:
*   **Corrección Parcial**: Si la precondición $Q$ es verdadera antes de ejecutar $S$, y **asumiendo que el programa termina**, entonces la postcondición $R$ será verdadera.
*   **Corrección Total**: Corrección Parcial + **Garantía de Terminación** (se demuestra que el programa finalizará de forma segura en un tiempo finito).

---

## 2. El Invariante de Bucle ($I$)

Un **Invariante de Bucle ($I$)** es una afirmación lógica (predicado) sobre las variables de un programa que debe ser verdadera en todas las iteraciones de un bucle. Se utiliza para demostrar la **Corrección Parcial** de un bucle.

Para demostrar que un predicado $I$ es un invariante válido, debemos verificar tres condiciones matemáticas:

1.  **Inicialización (Paso Base)**: El invariante debe ser verdadero inmediatamente antes de entrar al bucle, justo después de ejecutar las inicializaciones previas.
    $$\{Q\} \, \text{inicialización} \, \{I\}$$
2.  **Mantenimiento (Paso Inductivo)**: Si el invariante $I$ y la condición del bucle $B$ son verdaderos antes de una iteración, tras ejecutar el cuerpo del bucle $S$, el invariante $I$ debe seguir siendo verdadero.
    $$\{I \land B\} \, S \, \{I\}$$
3.  **Finalización**: Si el bucle termina (la condición $B$ se vuelve falsa) y el invariante $I$ se mantiene verdadero, la conjunción de ambos debe implicar lógicamente la postcondición final $R$ del algoritmo.
    $$(I \land \neg B) \implies R$$

---

## 3. La Función de Cota ($t$)

Para demostrar la **terminación** del bucle (pasando de corrección parcial a **corrección total**), definimos una **Función de Cota ($t$)**, que es una expresión matemática entera en función de las variables del bucle que debe cumplir dos propiedades en cada iteración:

1.  **Acotación Inferior**: Si la condición del bucle $B$ es verdadera, la función de cota debe ser estrictamente no negativa:
    $$B \implies t \ge 0$$
2.  **Decrecimiento Estricto**: La ejecución del cuerpo del bucle $S$ debe decrecer estrictamente el valor de la cota en cada iteración. Si denotamos el valor de la cota al inicio de la iteración como $t_0$ y al final como $t$:
    $$\{I \land B \land t = t_0\} \, S \, \{t < t_0\}$$

*Intuición*: Como la cota es un número entero que decrece estrictamente en cada paso y está acotada inferiormente por 0, el bucle está obligado a terminar en un número finito de iteraciones.

---

## 4. Guía Metodológica para Diseñar Invariantes y Cotas

Ante un bucle acumulador típico que procesa un array de tamaño $N$ desde un índice $i = 0$ hasta $N$:
1.  **Invariante ($I$)**: Generalmente expresa que la propiedad deseada ya se cumple de forma parcial para el subconjunto de datos procesados hasta el momento (es decir, desde el índice $0$ hasta $i-1$).
2.  **Cota ($t$)**: Mide la "distancia" o cantidad de trabajo restante para finalizar. Si el bucle termina cuando $i == N$, la cota suele ser la diferencia: $t = N - i$.

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Sea el siguiente fragmento de código Java diseñado para sumar los elementos de un array $A$ de tamaño $N \ge 0$:
```java
int suma = 0;
int i = 0;
while (i < N) {
    suma = suma + A[i];
    i = i + 1;
}
```
Demostrar formalmente la corrección total del bucle.
*   **Precondición ($Q$)**: $N \ge 0 \land A.length == N$
*   **Postcondición ($R$)**: $suma = \sum_{j=0}^{N-1} A[j]$

**Solución**:

1.  **Proponer el Invariante ($I$)**:
    El invariante indica que la variable `suma` contiene la suma acumulada de los elementos desde el índice $0$ hasta $i-1$, y que el índice $i$ está acotado:
    $$I: \left( suma = \sum_{j=0}^{i-1} A[j] \right) \quad \land \quad 0 \le i \le N$$

2.  **Verificar las Condiciones del Invariante**:
    *   **Inicialización**: Justo antes de entrar al bucle, `suma = 0` e `i = 0`.
        Sustituyendo en el invariante:
        $$suma = \sum_{j=0}^{-1} A[j] = 0 \quad \text{y} \quad 0 \le 0 \le N \quad (\text{Verdadero, pues } N \ge 0)$$
        *(La inicialización cumple el invariante).*
    *   **Mantenimiento**: Asumimos que $I$ y la condición del bucle $B: i < N$ son ciertos antes de una iteración. Tras ejecutar el cuerpo del bucle, los nuevos valores son $suma' = suma + A[i]$ e $i' = i + 1$. Comprobamos si el nuevo estado cumple el invariante:
        $$suma' = suma + A[i] = \left( \sum_{j=0}^{i-1} A[j] \right) + A[i] = \sum_{j=0}^{i} A[j] = \sum_{j=0}^{i'-1} A[j] \quad (\text{Correcto})$$
        Como $i < N \implies i + 1 \le N \implies i' \le N$, se cumple $0 \le i' \le N$.
        *(El mantenimiento es válido).*
    *   **Finalización**: El bucle termina cuando $B$ es falso, es decir, $\neg(i < N) \implies i \ge N$.
        Por el invariante sabemos que $i \le N$. Uniendo ambas: $i = N$.
        Sustituyendo $i = N$ en el invariante $I$:
        $$suma = \sum_{j=0}^{N-1} A[j] \quad (\text{Que coincide exactamente con la Postcondición } R. \text{ Demostrado}).$$

3.  **Proponer y Verificar la Función de Cota ($t$)**:
    Proponemos la cota:
    $$t = N - i$$
    *   **Acotación Inferior**: Debemos probar que si $B$ es verdadero ($i < N$), entonces $t \ge 0$:
        $$i < N \implies N - i > 0 \implies t \ge 0 \quad (\text{Cumple}).$$
    *   **Decrecimiento**: El valor de la cota tras la iteración es $t' = N - i'$.
        Como $i' = i + 1$:
        $$t' = N - (i + 1) = N - i - 1 = t - 1 < t \quad (\text{Decrece estrictamente en cada paso}).$$

*Conclusión: El bucle es formalmente correcto y tiene garantizada su terminación (Corrección Total).*
