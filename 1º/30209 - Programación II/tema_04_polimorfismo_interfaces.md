# Tema 4: Polimorfismo, Enlace Dinámico e Interfaces

El polimorfismo y las interfaces permiten diseñar sistemas de software altamente extensibles y desacoplados. Mediante estas herramientas, podemos escribir código que opere sobre abstracciones generales (como "FiguraGeometrica" o "ConexionBD") sin preocuparnos por las implementaciones concretas que existirán en el futuro, las cuales se resolverán dinámicamente en tiempo de ejecución.

---

## 1. Polimorfismo y Tipado (Estático vs. Dinámico)

El **polimorfismo** es la propiedad por la cual una variable de referencia puede apuntar a objetos de diferentes tipos (clases) dentro de una jerarquía de herencia.

Para comprender el polimorfismo, debemos distinguir entre dos conceptos de tipo:
1.  **Tipo Estático (o de Declaración)**: Es el tipo con el que se declara la variable de referencia en el código. Se evalúa en tiempo de compilación y determina qué métodos **está permitido invocar** sobre esa variable.
2.  **Tipo Dinámico (o de Ejecución)**: Es el tipo real del objeto creado en el Heap al que apunta la referencia. Se evalúa en tiempo de ejecución y determina qué implementación del método **se ejecutará realmente**.

---

## 2. Enlace Dinámico (Dynamic Binding)

El **enlace dinámico** es el mecanismo por el cual la JVM decide en tiempo de ejecución qué versión de un método sobreescrito debe invocar. La regla de oro es: **la JVM busca el método en base al tipo dinámico del objeto**.

### Visualización en Memoria de `Persona p = new Estudiante();` (ASCII):

Supongamos que la clase `Estudiante` sobreescribe el método `mostrarInformacion()` de `Persona`.

```
        STACK (Pila)                         HEAP (Montículo)
   +-------------------+                +-----------------------+
p  |   ref @500        |--------------> | Estudiante (Objeto)   |
   +-------------------+                | [Tipo estático: Pers] |
                                        | [Tipo dinámico: Estu] |
                                        | - nombre: "Ana"       |
                                        | - matricula: "A-45"   |
                                        +-----------------------+
```

*   **¿Es válida la declaración?** Sí, porque un `Estudiante` *es una* `Persona` (Upcasting automático).
*   **¿Qué pasa si llamamos a `p.mostrarInformacion()`?** El compilador comprueba que `mostrarInformacion()` existe en el tipo estático `Persona` (pasa la compilación). En tiempo de ejecución, la JVM sigue la referencia `@500` al Heap, detecta que el objeto real es un `Estudiante` y ejecuta la versión sobreescrita de `Estudiante`.
*   **¿Qué pasa si llamamos a `p.getMatricula()`?** Da **error de compilación**. Aunque el objeto en el Heap tiene matrícula, el tipo estático de la referencia es `Persona`, y la clase `Persona` no posee el método `getMatricula()`.

---

## 3. Conversión de Tipos (Casting) y el operador `instanceof`

Para acceder a métodos específicos del tipo dinámico, debemos cambiar el tipo estático de la referencia mediante un **Casting**:

*   **Upcasting (Ascendente)**: Convertir la referencia a una superclase. Es automático e intrínsecamente seguro:
    ```java
    Persona p = new Estudiante();
    ```
*   **Downcasting (Descendente)**: Convertir la referencia a una subclase. Debe ser explícito y puede ser peligroso:
    ```java
    Estudiante e = (Estudiante) p; // Ahora e puede llamar a getMatricula()
    ```
    Si el objeto apuntado por `p` no fuera realmente un `Estudiante` en tiempo de ejecución, el programa lanzaría un error crítico: `ClassCastException`.

### El operador `instanceof`
Para realizar un downcasting seguro, verificamos previamente la compatibilidad del tipo dinámico:
```java
if (p instanceof Estudiante) {
    Estudiante e = (Estudiante) p;
    System.out.println("Matrícula: " + e.getMatricula());
}
```

---

## 4. Clases Abstractas

Una clase declarada con la palabra clave `abstract` representa un concepto incompleto.
*   **Instanciación**: No se pueden crear objetos de una clase abstracta (`new ClaseAbstracta()` produce un error de compilación).
*   **Métodos Abstractos**: Pueden declararse sin cuerpo (`public abstract void dibujar();`). Las subclases no abstractas (concretas) están obligadas a sobreescribir e implementar todos los métodos abstractos heredados.
*   Pueden contener atributos de instancia y métodos normales ya implementados.

---

## 5. Interfaces

Una **interfaz** (`interface`) representa un contrato de diseño puro. Define un conjunto de operaciones que una clase "se compromete" a realizar.

### Características en Java:
1.  **Sin Atributos de Instancia**: Solo pueden definir constantes públicas, implícitamente `public static final`.
2.  **Métodos**: Todos los métodos son implícitamente públicos y abstractos (`public abstract`), excepto los métodos con implementación por defecto (`default`) o estáticos (`static`) introducidos en Java 8+.
3.  **Herencia Múltiple**: Una clase en Java solo puede extender de una clase base, pero **puede implementar múltiples interfaces** (`implements InterfazA, InterfazB`). Esto permite heredar comportamientos sin los problemas de ambigüedad de la herencia múltiple de clases.

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Diseñar un sistema de pagos utilizando interfaces y polimorfismo:
1.  Definir la interfaz `MetodoPago` con un método `procesarPago(double importe)`.
2.  Implementar la clase `TarjetaCredito` que implementa `MetodoPago` y almacena el número de tarjeta y la comisión ($1.5\%$).
3.  Implementar la clase `Paypal` que implementa `MetodoPago` y almacena el correo electrónico.
4.  Crear una clase `ProcesadorPagos` con un método estático `realizarTransaccion(MetodoPago metodo, double importe)` que ilustre el polimorfismo invocando el cobro.

**Solución**:
```java
// Interfaz (Contrato)
public interface MetodoPago {
    void procesarPago(double importe);
}

// Implementación 1
public class TarjetaCredito implements MetodoPago {
    private String numeroTarjeta;
    private static final double COMISION = 0.015; // 1.5%

    public TarjetaCredito(String numeroTarjeta) {
        this.numeroTarjeta = numeroTarjeta;
    }

    @Override
    public void procesarPago(double importe) {
        double total = importe * (1 + COMISION);
        System.out.println("Pago de " + total + " EUR procesado con Tarjeta " + numeroTarjeta + " (comisión de " + (importe * COMISION) + " EUR incluida).");
    }
}

// Implementación 2
public class Paypal implements MetodoPago {
    private String email;

    public Paypal(String email) {
        this.email = email;
    }

    @Override
    public void procesarPago(double importe) {
        System.out.println("Pago de " + importe + " EUR procesado a través de PayPal para el usuario " + email + ".");
    }
}

// Procesador que aprovecha el Polimorfismo
public class ProcesadorPagos {
    // El primer parámetro es polimórfico: acepta CUALQUIER objeto que implemente MetodoPago
    public static void realizarTransaccion(MetodoPago metodo, double importe) {
        System.out.println("Iniciando transacción...");
        
        // Enlace Dinámico: la JVM decidirá si invoca la versión de TarjetaCredito o de Paypal
        // en base al tipo dinámico del objeto que reciba en el parámetro 'metodo'
        metodo.procesarPago(importe); 
        
        System.out.println("Transacción finalizada.\n");
    }

    public static void main(String[] args) {
        MetodoPago tarjeta = new TarjetaCredito("1234-5678-9012-3456");
        MetodoPago cuentaPaypal = new Paypal("usuario@correo.com");

        // Realizamos cobros de 100 EUR con diferentes métodos
        realizarTransaccion(tarjeta, 100.0);
        realizarTransaccion(cuentaPaypal, 100.0);
    }
}
```
*(Nota: El método `realizarTransaccion` está totalmente desacoplado de las implementaciones concretas de tarjetas o cuentas de PayPal. Si en el futuro añadimos una clase `Criptomoneda` que implemente `MetodoPago`, funcionará de inmediato con el procesador de pagos existente sin cambiar una sola línea de código).*
