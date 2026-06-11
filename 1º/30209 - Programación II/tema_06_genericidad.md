# Tema 6: Genericidad

Antes de la introducción de la genericidad en Java 5, para crear colecciones que admitiesen cualquier tipo de dato se utilizaba la clase base `Object`. Sin embargo, esto obligaba a realizar conversiones de tipo (castings) explícitas y manuales al recuperar los elementos, lo que solía provocar errores `ClassCastException` en tiempo de ejecución. La **genericidad** permite parametrizar clases, interfaces y métodos con tipos de datos definidos por el cliente en tiempo de compilación, garantizando la seguridad de tipos (*type-safety*).

---

## 1. Clases e Interfaces Genéricas

Una clase genérica se declara especificando uno o más parámetros de tipo entre caracteres de menor y mayor que (`<T>`). Por convención de Java, se utilizan letras mayúsculas como `T` (Type), `E` (Element), `K` (Key) o `V` (Value).

```java
// Definición de una clase genérica
public class Caja<T> {
    private T contenido;

    public void guardar(T objeto) {
        this.contenido = objeto;
    }

    public T obtener() {
        return this.contenido;
    }
}
```

Al instanciar la clase, el cliente especifica el tipo concreto:
```java
Caja<String> cajaTexto = new Caja<>(); // Caja específica para Strings
cajaTexto.guardar("Hola Mundo");
String texto = cajaTexto.obtener(); // No requiere casting explícito

Caja<Integer> cajaEntero = new Caja<>(); // Caja específica para Integers
cajaEntero.guardar(42);
```

---

## 2. Métodos Genéricos

Un método genérico define sus propios parámetros de tipo, que son independientes de los parámetros de tipo de la clase. El parámetro de tipo se declara **antes del tipo de retorno** del método:

```java
public class Utilidades {
    // Método genérico para intercambiar elementos en un array
    public static <E> void intercambiar(E[] array, int i, int j) {
        E temporal = array[i];
        array[i] = array[j];
        array[j] = temporal;
    }
}
```

---

## 3. Restricciones y Comodines (Wildcards)

### El Comodín Indefinido (`?`)
Representa un tipo desconocido. Útil cuando solo leemos información de la estructura utilizando métodos heredados de `Object`.

### Comodines Acotados (Bounded Wildcards)
Permiten delimitar qué tipos de datos se aceptan como parámetro genérico en base a una jerarquía de clases:

1.  **Cota Superior (Upper Bound): `? extends T`**
    *   Acepta la clase `T` o cualquier subclase de `T` (relación hacia abajo en la jerarquía).
    *   *Uso*: Principalmente para **lectura** de datos. Garantiza que cualquier elemento recuperado es al menos de tipo `T`.
2.  **Cota Inferior (Lower Bound): `? super T`**
    *   Acepta la clase `T` o cualquier superclase de `T` (relación hacia arriba en la jerarquía).
    *   *Uso*: Principalmente para **escritura** de datos. Garantiza que podemos insertar de forma segura elementos de tipo `T` en la colección.

### Regla Mnemotécnica: PECS (Producer Extends, Consumer Super)
*   Si la estructura de datos actúa como **productora** (de la que solo leemos elementos), usa `extends`.
*   Si la estructura de datos actúa como **consumidora** (en la que solo escribimos/añadimos elementos), usa `super`.

---

## 4. Borrado de Tipos (Type Erasure)

La JVM no tiene soporte nativo para genéricos. Para asegurar la compatibilidad hacia atrás con el código Java antiguo, el compilador aplica el proceso de **Borrado de Tipos (Type Erasure)** durante la compilación:
1.  Elimina toda la información de tipos genéricos (los parámetros `<T>`).
2.  Sustituye el tipo genérico por `Object` (o por la cota especificada si existía, por ejemplo, `T extends Number` se sustituye por `Number`).
3.  Inserta castings implícitos donde sea necesario para el cliente del código.

### Limitaciones Críticas derivadas del Borrado de Tipos:
Debido a que en tiempo de ejecución la JVM no sabe qué es `T`, se imponen las siguientes limitaciones de diseño:
1.  **No se pueden usar tipos primitivos**: No se puede hacer `Caja<int>`, se debe usar la clase envolvente (`Caja<Integer>`).
2.  **No se pueden crear instancias de `T`**: `T obj = new T();` da error de compilación.
3.  **No se pueden crear arrays genéricos**: `T[] array = new T[10];` da error de compilación.
4.  **No se puede hacer casting en base a genéricos en ejecución**: `if (obj instanceof Caja<String>)` da error. Solo se puede comprobar la forma cruda: `if (obj instanceof Caja)`.

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Diseñar una clase genérica `AlmacenLimitado<T extends Number>` que represente un almacén numérico que solo acepte tipos numéricos (como `Integer`, `Double`, etc.). La clase debe cumplir:
1.  Almacenar una lista de elementos (usando `java.util.ArrayList`).
2.  Un método `agregar(T elemento)` que añada el elemento.
3.  Un método `sumarTodos()` que devuelva la suma de todos los números como un valor double.
4.  Implementar un método estático polimórfico `imprimirAlmacen(AlmacenLimitado<?> almacen)` que muestre los datos de cualquier almacén.

**Solución**:
```java
import java.util.ArrayList;
import java.util.List;

// Clase genérica acotada superiormente (solo acepta números)
public class AlmacenLimitado<T extends Number> {
    private List<T> elementos;

    public AlmacenLimitado() {
        this.elementos = new ArrayList<>();
    }

    public void agregar(T elemento) {
        this.elementos.add(elemento);
    }

    // Calcula la suma de todos los números en formato double
    public double sumarTodos() {
        double suma = 0.0;
        for (T elemento : elementos) {
            // Como T extiende de Number, tenemos garantizado el acceso al método doubleValue()
            suma += elemento.doubleValue(); 
        }
        return suma;
    }

    // Método estático genérico con comodín de lectura
    public static void imprimirAlmacen(AlmacenLimitado<?> almacen) {
        System.out.println("Suma del almacén: " + almacen.sumarTodos());
    }

    public static void main(String[] args) {
        // Válido: Integer extiende de Number
        AlmacenLimitado<Integer> almacenEnteros = new AlmacenLimitado<>();
        almacenEnteros.agregar(10);
        almacenEnteros.agregar(20);
        
        // Válido: Double extiende de Number
        AlmacenLimitado<Double> almacenReales = new AlmacenLimitado<>();
        almacenReales.agregar(5.5);
        almacenReales.agregar(4.5);

        // Error de compilación: String NO extiende de Number
        // AlmacenLimitado<String> almacenTextos = new AlmacenLimitado<>();

        System.out.println("Suma enteros:");
        imprimirAlmacen(almacenEnteros); // Imprime 30.0

        System.out.println("Suma reales:");
        imprimirAlmacen(almacenReales);  // Imprime 10.0
    }
}
```
