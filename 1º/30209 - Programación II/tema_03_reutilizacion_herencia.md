# Tema 3: Mecanismos de Reutilización: Herencia

La herencia es uno de los pilares de la programación orientada a objetos. Permite crear una nueva clase (subclase o clase derivada) a partir de una clase existente (superclase o clase base), heredando sus atributos y métodos. Esto evita la duplicación de código y permite establecer una relación jerárquica de tipo **"es-un"** (por ejemplo, un `Estudiante` **es una** `Persona`).

---

## 1. Conceptos Fundamentales de la Herencia

En Java, la herencia se implementa mediante la palabra clave `extends`:
```java
public class Estudiante extends Persona {
    // Estudiante hereda todos los miembros no privados de Persona
}
```

### Reglas Clave en Java:
1.  **Herencia Simple**: En Java **no existe la herencia múltiple de clases**. Una clase solo puede extender directamente a una única superclase (es decir, una sola rama paterna).
2.  **La clase Object**: Si no especificamos ninguna superclase con `extends`, la clase hereda de forma automática e implícita de `java.lang.Object` (la raíz de todas las jerarquías en Java).
3.  **Constructores**: Los constructores **no se heredan**. Cada subclase debe definir sus propios constructores.

---

## 2. El Modificador de Acceso `protected`

El modificador `protected` proporciona un nivel intermedio de encapsulación:
*   Un miembro `protected` (atributo o método) es accesible por la propia clase, por las clases que estén en el **mismo paquete** y por las **subclases** (incluso si estas se encuentran en paquetes diferentes).
*   *Buenas prácticas*: Evita hacer atributos `protected` directamente si deseas un encapsulamiento fuerte. Es preferible mantener los atributos `private` en la clase base y exponer métodos de acceso `protected` si es necesario que las subclases los manipulen.

---

## 3. Sobreescritura de Métodos (Method Overriding)

La **sobreescritura** ocurre cuando una subclase define un método con la **misma firma** (nombre, tipo de retorno y parámetros) que un método de su superclase, proporcionando una implementación específica y adaptada a su comportamiento.

### La Anotación `@Override`
Es una directiva para el compilador. No cambia el comportamiento del programa, pero es fundamental por dos razones:
1.  **Seguridad**: Si cometemos un error de tipografía al sobreescribir (por ejemplo, escribir `tostring()` en lugar de `toString()`), el compilador generará un error indicando que el método no existe en la superclase.
2.  **Legibilidad**: Documenta claramente que el método está redefiniendo un comportamiento heredado.

Diferencia clave con la **sobrecarga**:
*   *Sobreescritura*: Mismo nombre, mismos parámetros, distinta clase (relación de herencia).
*   *Sobrecarga*: Mismo nombre, distintos parámetros, misma clase.

---

## 4. La Referencia `super`

La palabra clave `super` hace referencia directa a la superclase inmediata de la clase actual. Se utiliza en dos escenarios:

### A. Invocación de Constructores del Padre
En el constructor de una subclase, podemos llamar al constructor de la superclase usando `super(...)`:
```java
public class Estudiante extends Persona {
    private String matricula;

    public Estudiante(String nombre, int edad, String matricula) {
        super(nombre, edad); // Llama al constructor de Persona. DEBE SER LA PRIMERA LÍNEA.
        this.matricula = matricula;
    }
}
```
*Si no escribimos explícitamente `super(...)` en la primera línea de un constructor, el compilador inserta de forma invisible la llamada al constructor sin parámetros del padre: `super();`. Si el padre no tiene constructor sin parámetros, se producirá un error de compilación.*

### B. Invocación de Métodos del Padre
Permite invocar la versión del método que existe en la superclase, lo que resulta útil cuando queremos complementar la funcionalidad heredada en lugar de reemplazarla por completo:
```java
@Override
public void mostrarInformacion() {
    super.mostrarInformacion(); // Ejecuta la impresión básica del padre (nombre, edad)
    System.out.println("Matrícula: " + this.matricula); // Añade el campo específico
}
```

---

## 5. Orden de Construcción en Memoria

Cuando creamos una instancia de una subclase (`new Estudiante()`), la JVM ejecuta los constructores en cadena de arriba hacia abajo de la jerarquía:
1.  Se reserva memoria en el Heap para el objeto completo (que contendrá tanto los atributos de `Persona` como los de `Estudiante`).
2.  Se ejecuta el constructor de `Object`.
3.  Se ejecuta el constructor de `Persona` (inicializa atributos heredados).
4.  Se ejecuta el constructor de `Estudiante` (inicializa atributos específicos).

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Diseñar la estructura de clases para un sistema de personal universitario:
1.  Crear la clase base `Empleado` con los atributos `nombre` (String) y `sueldoBase` (double). Debe tener un método `calcularSueldo()` que simplemente retorne `sueldoBase`.
2.  Crear la subclase `Profesor` que hereda de `Empleado` y añade el atributo `horasExtras` (int) y la constante estática `PRECIO_HORA = 20.0`. Sobreescribir `calcularSueldo()` para que devuelva el sueldo base más el pago por las horas extras.
3.  Implementar los constructores correspondientes utilizando `super(...)` y la anotación `@Override`.

**Solución**:
```java
// Clase Base
public class Empleado {
    private String nombre;
    private double sueldoBase;

    public Empleado(String nombre, double sueldoBase) {
        this.nombre = nombre;
        setSueldoBase(sueldoBase);
    }

    public String getNombre() {
        return nombre;
    }

    public double getSueldoBase() {
        return sueldoBase;
    }

    public void setSueldoBase(double sueldoBase) {
        if (sueldoBase >= 0) {
            this.sueldoBase = sueldoBase;
        } else {
            throw new IllegalArgumentException("El sueldo no puede ser negativo");
        }
    }

    public double calcularSueldo() {
        return this.sueldoBase;
    }
}

// Clase Derivada
public class Profesor extends Empleado {
    private int horasExtras;
    public static final double PRECIO_HORA = 20.0;

    public Profesor(String nombre, double sueldoBase, int horasExtras) {
        super(nombre, sueldoBase); // Delega la inicialización del nombre y sueldoBase al padre
        setHorasExtras(horasExtras);
    }

    public int getHorasExtras() {
        return horasExtras;
    }

    public void setHorasExtras(int horasExtras) {
        if (horasExtras >= 0) {
            this.horasExtras = horasExtras;
        } else {
            throw new IllegalArgumentException("Las horas extras no pueden ser negativas");
        }
    }

    // Sobreescritura del método calcularSueldo
    @Override
    public double calcularSueldo() {
        // Aprovechamos el sueldo base calculado por el padre y le sumamos el extra de profesor
        return super.calcularSueldo() + (this.horasExtras * PRECIO_HORA);
    }
}
```
