# Tema 5: Gestión de Excepciones

Un programa de software de calidad industrial debe ser tolerante a fallos. Si un fichero no existe, la red se cae, o un usuario introduce datos inválidos, el programa no debe cerrarse de forma abrupta ("romperse"). La **gestión de excepciones** es el mecanismo estructurado que proporciona Java para interceptar eventos anómalos en tiempo de ejecución, procesarlos de forma segura y garantizar el cierre correcto de los recursos del sistema.

---

## 1. Jerarquía de Excepciones en Java

En Java, todas las excepciones son objetos. La raíz de toda la estructura es la clase **`Throwable`**, que se divide en dos grandes ramas:

```
                          Throwable (Clase Raíz)
                             |
              +--------------+--------------+
              |                             |
            Error                       Exception
              |                             |
       (Fallos graves de             +------+------+
          la JVM, ej:                |             |
       OutOfMemoryError)       Unchecked        Checked
                               Exceptions      Exceptions
                                   |               |
                           (RuntimeException, (Cualquier otra,
                              ej: NullPtr,       ej: IOException,
                             ArrayIndex)          SQLException)
```

1.  **`Error`**: Representa fallos catastróficos de hardware o de la propia JVM (como `OutOfMemoryError` o `StackOverflowError`). La aplicación **no debe intentar capturarlos**, ya que la recuperación es inviable.
2.  **`Exception`**: Errores generados por el programa que sí pueden capturarse y gestionarse. Se dividen a su vez en excepciones **verificadas** y **no verificadas**.

---

## 2. Excepciones Verificadas (Checked) vs. No Verificadas (Unchecked)

### A. Excepciones Verificadas (Checked Exceptions)
Son subclases directas de `Exception` (excluyendo a `RuntimeException`).
*   **Filosofía**: Representan fallos previsibles y ajenos al control directo del programador (fallos de red, archivos no encontrados, etc.).
*   **Control de compilación**: El compilador obliga a gestionarlas mediante la regla **"Handle or Declare"**: el desarrollador debe capturarlas en un bloque `try-catch` o declarar su propagación en la firma del método mediante la cláusula `throws`.
*   *Ejemplos*: `IOException`, `FileNotFoundException`, `SQLException`.

### B. Excepciones No Verificadas (Unchecked o Runtime Exceptions)
Son subclases de `RuntimeException`.
*   **Filosofía**: Representan errores de lógica del programador o fallos en el diseño del código.
*   **Control de compilación**: El compilador no obliga a capturarlas ni a declararlas en la firma. Se asume que el desarrollador debe prevenirlas programando correctamente.
*   *Ejemplos*: `NullPointerException` (intentar usar una referencia `null`), `ArrayIndexOutOfBoundsException` (índice de array fuera de rango), `ArithmeticException` (división por cero).

---

## 3. Captura y Liberación de Recursos: `finally` vs. `try-with-resources`

### Bloque `try-catch-finally`
*   `try`: Contiene el código susceptible de lanzar excepciones.
*   `catch`: Captura un tipo específico de excepción y define la respuesta. Se pueden encadenar múltiples bloques `catch` (del más específico al más genérico).
*   `finally`: Bloque opcional. **Se ejecuta siempre**, ocurra o no una excepción. Se utiliza para liberar recursos (cerrar ficheros o conexiones a bases de datos).

```java
// Estructura clásica
BufferedReader br = null;
try {
    br = new BufferedReader(new FileReader("datos.txt"));
    System.out.println(br.readLine());
} catch (IOException e) {
    System.err.println("Error de lectura: " + e.getMessage());
} finally {
    // Liberar recurso manualmente
    if (br != null) {
        try { br.close(); } catch (IOException ex) { /* ignorar */ }
    }
}
```

### El bloque `try-with-resources` (Recomendado)
Introducido en Java 7, simplifica la liberación de recursos. Cualquier recurso que implemente la interfaz `java.lang.AutoCloseable` se declara dentro del paréntesis del `try`. La JVM garantiza el **cierre automático** del recurso al salir del bloque, ocurra o no un error, eliminando por completo la necesidad del bloque `finally` repetitivo:

```java
// Estructura moderna y limpia con try-with-resources
try (BufferedReader br = new BufferedReader(new FileReader("datos.txt"))) {
    System.out.println(br.readLine());
} catch (IOException e) {
    System.err.println("Error de lectura: " + e.getMessage());
}
```

---

## 4. Lanzamiento (`throw`) y Propagación (`throws`)

*   **`throw`**: Se utiliza para lanzar explícitamente una instancia de una excepción. Detiene la ejecución normal del método:
    ```java
    if (saldo < cantidad) {
        throw new IllegalArgumentException("Saldo insuficiente");
    }
    ```
*   **`throws`**: Se coloca en la cabecera de un método para advertir a los invocadores de que este método puede propagar (no capturar) determinadas excepciones verificadas:
    ```java
    public void leerArchivo(String ruta) throws IOException {
        // ... código que puede lanzar IOException
    }
    ```

---

## 5. Diseño de Excepciones Personalizadas

En aplicaciones reales, es conveniente definir excepciones que representen errores de negocio específicos (por ejemplo, `SaldoInsuficienteException` o `UsuarioNoEncontradoException`).

*   **¿Checked o Unchecked?**
    *   Heredamos de `Exception` para crear una excepción checked (si queremos forzar al llamador a gestionarla).
    *   Heredamos de `RuntimeException` para crear una excepción unchecked (recomendado para la mayoría de los casos modernos, ya que no ensucia las firmas de los métodos).

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Diseñar una excepción unchecked llamada `SaldoInsuficienteException` que almacene el saldo actual y el importe fallido. Implementar una clase `Cuenta` con un método `retirar(double cantidad)` que lance dicha excepción si la cantidad supera el saldo. Crear un programa que intente realizar un cobro inválido y capture la excepción personalizada mostrando los datos del saldo.

**Solución**:
```java
// Excepción Personalizada (Unchecked)
public class SaldoInsuficienteException extends RuntimeException {
    private final double saldoActual;
    private final double importeFallido;

    public SaldoInsuficienteException(double saldoActual, double importeFallido) {
        // Llamamos al constructor de RuntimeException con un mensaje descriptivo
        super("Error: Intento de retirar " + importeFallido + " EUR con un saldo disponible de " + saldoActual + " EUR.");
        this.saldoActual = saldoActual;
        this.importeFallido = importeFallido;
    }

    public double getSaldoActual() {
        return saldoActual;
    }

    public double getImporteFallido() {
        return importeFallido;
    }
}

// Clase de Negocio
public class Cuenta {
    private double saldo;

    public Cuenta(double saldoInicial) {
        this.saldo = saldoInicial;
    }

    public void retirar(double cantidad) {
        if (cantidad <= 0) {
            throw new IllegalArgumentException("La cantidad a retirar debe ser mayor que cero.");
        }
        if (cantidad > this.saldo) {
            // Lanzamos nuestra excepción personalizada
            throw new SaldoInsuficienteException(this.saldo, cantidad);
        }
        this.saldo -= cantidad;
        System.out.println("Retirada con éxito. Nuevo saldo: " + this.saldo + " EUR.");
    }
}

// Clase Principal de Prueba
public class Main {
    public static void main(String[] args) {
        Cuenta miCuenta = new Cuenta(100.0); // Saldo inicial: 100 EUR

        try {
            miCuenta.retirar(150.0); // Intento de retirar más de lo permitido
        } catch (SaldoInsuficienteException e) {
            // Capturamos el error de negocio y accedemos a sus atributos específicos
            System.err.println("Auditoría: Transacción denegada.");
            System.err.println("Mensaje de la excepción: " + e.getMessage());
            System.err.println("Faltaban: " + (e.getImporteFallido() - e.getSaldoActual()) + " EUR para completar la operación.");
        }
    }
}
```
*(Nota: Al heredar de `RuntimeException`, el método `retirar` no necesita declarar `throws SaldoInsuficienteException` en su firma, haciendo el código más limpio, pero permitiendo igualmente la captura segura de errores mediante bloques `try-catch`).*
