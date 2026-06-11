# Tema 2: Clases y Encapsulamiento

La programación orientada a objetos (POO) busca modelar el mundo real o conceptos lógicos mediante entidades autónomas llamadas **objetos**. En este tema estudiaremos cómo se definen estas entidades mediante las **clases**, cómo interactúan sus componentes y cómo el principio de **encapsulamiento** protege la integridad del estado interno de los objetos.

---

## 1. Clases, Objetos y Miembros

*   **Clase**: Es el plano, molde o plantilla que define las propiedades (atributos) y el comportamiento (métodos) que tendrán todos los objetos de ese tipo. Reside en la zona de metadatos de la JVM (Method Area).
*   **Objeto**: Es una instancia física concreta de una clase, creada en tiempo de ejecución en el Heap.

### Miembros de Instancia vs. Miembros Estáticos (`static`)

1.  **Miembros de Instancia (por defecto)**:
    *   Cada objeto en el Heap tiene su propia copia de los atributos.
    *   Para acceder a ellos se requiere una instancia concreta (`objeto.atributo`, `objeto.metodo()`).
2.  **Miembros Estáticos (`static`)**:
    *   Pertenecen a la clase en sí, no a ningún objeto particular.
    *   Solo existe **una única copia** en memoria para toda la aplicación, compartida por todas las instancias de esa clase.
    *   Se pueden invocar directamente mediante el nombre de la clase (`Clase.atributo`, `Clase.metodo()`).
    *   **Limitación**: Un método estático no tiene referencia implícita `this` y **no puede acceder directamente** a atributos o métodos de instancia, ya que estos últimos necesitan un objeto concreto para existir.

---

## 2. Constructores y Sobrecarga

Un **constructor** es un método especial que se invoca automáticamente al crear un objeto con la palabra clave `new`. Su función es reservar memoria e inicializar el estado del objeto.
*   Tienen el mismo nombre exacto que la clase y no especifican ningún tipo de retorno (ni siquiera `void`).
*   **Constructor por defecto**: Si no definimos ningún constructor, el compilador genera uno vacío sin parámetros de forma automática. Si definimos al menos un constructor propio, el automático se pierde.
*   **Sobrecarga de Constructores**: Podemos definir múltiples constructores con diferentes firmas (lista de parámetros).
*   **Uso de `this(...)`**: Permite que un constructor invoque a otro constructor de la misma clase para evitar duplicidad de código. Debe ser la primera instrucción de la función.

---

## 3. Modificadores de Acceso y Ocultación de Datos

Los modificadores de acceso determinan la visibilidad de los atributos y métodos de una clase desde otras partes del código:

| Modificador | Dentro de la Clase | Mismo Paquete | Subclases (Herencia) | En cualquier parte |
|---|:---:|:---:|:---:|:---:|
| `private` | Sí | No | No | No |
| *Sin modificador* | Sí | Sí | No | No |
| `protected` | Sí | Sí | Sí | No |
| `public` | Sí | Sí | Sí | Sí |

---

## 4. El Principio de Encapsulamiento

El encapsulamiento consiste en agrupar los atributos (estado) y métodos (comportamiento) que operan sobre ellos dentro de una clase, y **ocultar los detalles de implementación** al exterior.

### Regla de Oro del Encapsulamiento:
1.  **Todos los atributos deben ser privados (`private`)**. Esto evita que el código externo modifique directamente los datos de un objeto de forma descontrolada.
2.  **El acceso se gestiona mediante métodos públicos `get` (lectura) y `set` (escritura)**.
3.  **Los setters deben validar los datos**: Esto garantiza la consistencia del objeto.

### Ejemplo de Violación y Corrección de Consistencia:
*   *Violado*: Si el atributo `edad` es público, un programador externo puede hacer `persona.edad = -5;`, dejando al objeto en un estado biológicamente imposible.
*   *Corregido*:
```java
public class Persona {
    private int edad;

    public void setEdad(int edad) {
        if (edad >= 0 && edad <= 120) {
            this.edad = edad; // Asignación segura
        } else {
            throw new IllegalArgumentException("Edad no válida");
        }
    }
}
```

---

## 5. Visualización en Memoria de Miembros Estáticos

Supongamos que tenemos la clase `CuentaBancaria` con un atributo estático `tasaInteres`:
```java
CuentaBancaria c1 = new CuentaBancaria(1000);
CuentaBancaria c2 = new CuentaBancaria(2000);
```

### Mapa de Memoria en la JVM (ASCII):

```
       STACK (Pila)                    HEAP (Montículo)
   +-------------------+       +-------------------------------+
c1 |   ref @001        |-----> | CuentaBancaria                |
   +-------------------+       | - saldo: 1000                 |
c2 |   ref @002        |-----> +-------------------------------+
   +-------------------+       | CuentaBancaria                |
                               | - saldo: 2000                 |
                               +-------------------------------+
                               
                               METHOD AREA (Metadatos de Clase)
                               +-------------------------------+
                               | Clase CuentaBancaria          |
                               | [static] tasaInteres: 2.5%    |
                               +-------------------------------+
```

*   `saldo` se replica en cada objeto del Heap.
*   `tasaInteres` reside en el Method Area (única en memoria), accesible y compartida por todas las referencias.

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Diseñar una clase `SensorTemperatura` que represente un sensor físico. Debe cumplir:
1.  El atributo `temperatura` (en Celsius) debe estar encapsulado. No puede ser inferior a $-273.15^\circ\text{C}$ (cero absoluto).
2.  Debe tener un constructor que reciba la temperatura inicial y otro por defecto que la inicialice a $0^\circ\text{C}$ utilizando delegación de constructores `this(...)`.
3.  Debe existir un atributo estático `numSensores` que lleve la cuenta de cuántas instancias se han creado.

**Solución**:
```java
public class SensorTemperatura {
    // Atributos de instancia (privados para encapsular)
    private double temperatura;
    
    // Atributo estático (compartido para conteo global)
    private static int numSensores = 0;

    // Constructor con parámetros
    public SensorTemperatura(double temperaturaInicial) {
        setTemperatura(temperaturaInicial); // Valida y asigna
        numSensores++; // Incrementa contador global de instancias
    }

    // Constructor por defecto (delega en el principal)
    public SensorTemperatura() {
        this(0.0); // Invoca al constructor superior con el valor 0.0
    }

    // Getter de temperatura
    public double getTemperatura() {
        return this.temperatura;
    }

    // Setter con validación estricta de consistencia física
    public void setTemperatura(double temperatura) {
        if (temperatura >= -273.15) {
            this.temperatura = temperatura;
        } else {
            throw new IllegalArgumentException("La temperatura no puede ser menor al cero absoluto.");
        }
    }

    // Getter del contador estático (método estático porque no depende de una instancia)
    public static int getNumSensores() {
        return numSensores;
    }
}
```
*(Nota: El getter de `numSensores` se declara estático, lo que permite consultar el total de sensores creados mediante `SensorTemperatura.getNumSensores()` sin necesidad de tener un objeto creado).*
