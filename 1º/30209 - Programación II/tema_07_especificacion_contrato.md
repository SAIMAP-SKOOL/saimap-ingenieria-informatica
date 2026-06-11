# Tema 7: Especificación Formal de Código y Diseño por Contrato

En ingeniería del software, escribir código que funcione no es suficiente; debemos garantizar que el código se comporta exactamente según lo especificado. La **especificación formal** describe qué debe hacer un módulo de software de manera precisa y abstracta, separándolo de cómo se implementa internamente. Estudiaremos la metodología del **Diseño por Contrato (DbC)** y el uso de **Invariantes** para asegurar el correcto funcionamiento del software.

---

## 1. La Filosofía del Diseño por Contrato (DbC)

El Diseño por Contrato, formulado por Bertrand Meyer, establece que la interacción entre dos componentes de software (el cliente que invoca un método y el proveedor que lo implementa) se rige por un **contrato formal** que describe sus derechos y obligaciones mutuos.

Este contrato se define formalmente mediante dos elementos clave:

### A. Precondición (Pre)
Es una condición lógica que **el cliente debe garantizar** antes de invocar el método.
*   **Obligación del cliente**: Satisfacer la precondición.
*   **Derecho del proveedor**: Asumir que la precondición es verdadera al comenzar a ejecutar el método. No está obligado a controlar qué pasa si la precondición es falsa.
*   *En Java*: Si se viola una precondición, la práctica recomendada es abortar la ejecución inmediatamente lanzando una excepción de tipo unchecked (por ejemplo, `IllegalArgumentException`).

### B. Postcondición (Post)
Es una condición lógica que **el proveedor garantiza** que se cumplirá una vez finalizada la ejecución del método, siempre y cuando el cliente haya cumplido la precondición inicial.
*   **Obligación del proveedor**: Cumplir la postcondición.
*   **Derecho del cliente**: Confiar en que el resultado final del método satisface la postcondición.

---

## 2. El Invariante de Representación (IR)

El **Invariante de Representación (IR)** de una clase es una condición lógica (un predicado booleano) sobre los atributos internos de un objeto que **debe cumplirse obligatoriamente durante todo su ciclo de vida**.

### Reglas del IR:
1.  Debe ser establecido por los **constructores** al crear el objeto.
2.  Debe preservarse por cualquier **método público** de la clase (puede violarse temporalmente a mitad de la ejecución del método, pero al finalizar y devolver el control, el IR debe volver a ser verdadero).
3.  Determina si el estado interno del objeto es **consistente** y válido.

### Ejemplo conceptual de una Fracción:
Si tenemos la clase `Fraccion` con atributos `numerador` y `denominador`:
$$\text{Invariante de Representación (IR): } \text{denominador} \neq 0$$

---

## 3. Implementación Práctica: Programación Defensiva y `checkRep()`

Para asegurar que un objeto nunca entre en un estado inválido (violación de su IR) debido a un bug en el propio código del programador, se acostumbra a crear un método de validación interno llamado **`checkRep()`** (Check Representation).

Este método comprueba de forma asertiva que el IR es verdadero. Se invoca al final del constructor y al final de cualquier método mutador (setters o métodos que alteren atributos):

```java
public class Fraccion {
    private int numerador;
    private int denominador;

    // Invariante de Representación (IR): denominador != 0
    private void checkRep() {
        if (this.denominador == 0) {
            throw new IllegalStateException("Violación de Invariante: El denominador no puede ser cero.");
        }
    }

    public Fraccion(int numerador, int denominador) {
        // Precondición: denominador != 0
        if (denominador == 0) {
            throw new IllegalArgumentException("El denominador inicial no puede ser cero.");
        }
        this.numerador = numerador;
        this.denominador = denominador;
        checkRep(); // Validar consistencia al crear
    }

    public void setDenominador(int denominador) {
        // Precondición: denominador != 0
        if (denominador == 0) {
            throw new IllegalArgumentException("El denominador no puede ser cero.");
        }
        this.denominador = denominador;
        checkRep(); // Validar consistencia tras mutación
    }
}
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Diseñar la especificación formal e implementación de una clase `Fecha` que almacene `dia` (int) y `mes` (int). No consideraremos años bisiestos (asumiremos siempre un año común de 365 días).
1. Definir formalmente el Invariante de Representación (IR) de la clase en notación matemática.
2. Especificar formalmente (Precondición y Postcondición) un método `avanzarDia()`.
3. Implementar la clase en Java, incluyendo el método `checkRep()`.

**Solución**:
1.  **Invariante de Representación (IR)**:
    $$\text{IR: } 1 \le \text{mes} \le 12 \quad \land \quad 1 \le \text{dia} \le \text{limiteDias(mes)}$$
    donde $\text{limiteDias(mes)}$ es la función que determina la cantidad de días del mes: 31 para enero, 28 para febrero, etc.

2.  **Especificación formal de `avanzarDia()`**:
    *   **Precondición**: El objeto cumple su Invariante de Representación (la fecha actual es válida).
        $$\text{Pre: } \text{objeto cumple IR}$$
    *   **Postcondición**: La fecha se incrementa en exactamente un día. Si el día excede el límite del mes, el día pasa a 1 y el mes se incrementa. Si el mes supera 12, pasa a 1 (cambio de año). El objeto resultante sigue cumpliendo su IR.
        $$\text{Post: } \text{fecha resultante es la fecha del día siguiente} \quad \land \quad \text{objeto cumple IR}$$

3.  **Implementación en Java**:
```java
public class Fecha {
    private int dia;
    private int mes;

    // Retorna la cantidad de días teóricos de un mes específico (año no bisiesto)
    private static int limiteDias(int mes) {
        switch (mes) {
            case 2: return 28;
            case 4: case 6: case 9: case 11: return 30;
            default: return 31;
        }
    }

    // Invariante de Representación
    private void checkRep() {
        if (this.mes < 1 || this.mes > 12) {
            throw new IllegalStateException("Violación de Invariante: Mes fuera de rango [1, 12].");
        }
        int maxDias = limiteDias(this.mes);
        if (this.dia < 1 || this.dia > maxDias) {
            throw new IllegalStateException("Violación de Invariante: Día fuera de rango para el mes especificado.");
        }
    }

    public Fecha(int dia, int mes) {
        // Precondición: parámetros válidos
        if (mes < 1 || mes > 12 || dia < 1 || dia > limiteDias(mes)) {
            throw new IllegalArgumentException("Fecha inicial inválida.");
        }
        this.dia = dia;
        this.mes = mes;
        checkRep(); // Comprobamos que el constructor establece el IR correctamente
    }

    // Pre: objeto cumple IR (garantizado por checkRep en llamadas previas)
    // Post: la fecha avanza en 1 día y el objeto sigue cumpliendo el IR
    public void avanzarDia() {
        this.dia++;
        if (this.dia > limiteDias(this.mes)) {
            this.dia = 1;
            this.mes++;
            if (this.mes > 12) {
                this.mes = 1;
            }
        }
        checkRep(); // Validamos que el estado final sigue siendo consistente
    }

    public int getDia() { return dia; }
    public int getMes() { return mes; }
}
```
