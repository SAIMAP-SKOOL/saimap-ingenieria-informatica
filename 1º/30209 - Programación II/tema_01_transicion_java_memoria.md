# Tema 1: Transición a Java y Gestión de Memoria (Stack vs. Heap)

Al programar en lenguajes como C, el desarrollador tiene el control total (y la responsabilidad) de la gestión de memoria utilizando punteros y funciones como `malloc()` y `free()`. La transición a Java introduce una capa de abstracción crítica: la **Máquina Virtual de Java (JVM)**. En Java no existen punteros explícitos ni aritmética de punteros, y la memoria se gestiona de forma automática. Para evitar errores y comprender la ejecución del software, es imprescindible entender cómo organiza la JVM la memoria en dos áreas fundamentales: el **Stack (Pila)** y el **Heap (Montículo)**.

---

## 1. Stack vs. Heap: Organización Espacial de la Memoria

La JVM divide la memoria en dos regiones con propósitos de diseño muy diferentes:

### A. El Stack (Pila de llamadas)
Es una memoria de acceso rápido estructurada bajo el principio LIFO (Last-In, First-Out).
*   **Contenido**: Almacena variables locales de tipo primitivo (como `int`, `double`, `boolean`) y las **referencias** (direcciones de memoria) a los objetos que residen en el Heap. También guarda los marcos de activación de los métodos en ejecución (parámetros y direcciones de retorno).
*   **Ciclo de vida**: Su gestión es automática y ligada al flujo de control. Al invocar un método, se crea su marco de llamada en el tope del Stack; al terminar el método, el marco se destruye de inmediato y la memoria de sus variables locales se libera.

### B. El Heap (Montículo de objetos)
Es una gran área de memoria de acceso dinámico y global.
*   **Contenido**: Almacena **todos los objetos** creados en el programa (mediante la palabra clave `new`), incluyendo sus atributos (incluso si estos atributos son primitivos) y todos los arrays.
*   **Ciclo de vida**: No está acotado por el ámbito de un método. Un objeto permanece en el Heap mientras exista al menos una referencia activa en el Stack que apunte a él.

---

## 2. Referencias y su representación gráfica

En Java, cuando declaramos una variable de tipo objeto, por ejemplo:
```java
Persona p = new Persona("Ana");
```
No estamos guardando el objeto `Persona` dentro de la variable `p`. En su lugar, la variable `p` reside en el Stack y almacena una **referencia** (un puntero implícito seguro) que apunta a la ubicación física del objeto real `Persona` creado en el Heap.

### Representación Espacial en Memoria (ASCII)

Si ejecutamos el siguiente bloque de código:
```java
Persona p1 = new Persona("Ana");
Persona p2 = p1;
Persona p3 = new Persona("Luis");
```

La distribución física de la memoria de la JVM se estructurará de la siguiente forma:

```
        STACK (Pila)                         HEAP (Montículo)
   +-------------------+                +-----------------------+
p1 |   ref @001        |--------------> |  Persona              |
   +-------------------+                |  - nombre: "Ana"      |
p2 |   ref @001        |-------------/  +-----------------------+
   +-------------------+
p3 |   ref @002        |--------------> |  Persona              |
   +-------------------+                |  - nombre: "Luis"     |
                                        +-----------------------+
```

### Consecuencias Críticas de la Copia de Referencias:
*   La asignación `p2 = p1` **no copia el objeto** en el Heap; únicamente copia la dirección de memoria en el Stack.
*   Tanto `p1` como `p2` apuntan al mismo y único objeto `"Ana"`.
*   Si modificamos el nombre del objeto a través de `p2.setNombre("Clara")`, la llamada a `p1.getNombre()` devolverá `"Clara"`, ya que el objeto subyacente es compartido.

---

## 3. Ciclo de Vida de un Objeto y Garbage Collector (GC)

### Ciclo de Vida
1.  **Creación**: Se reserva espacio en el Heap mediante `new`, se ejecuta el constructor y se devuelve la referencia.
2.  **Uso**: Se accede a sus métodos y atributos mediante su referencia en el Stack.
3.  **Inalcanzabilidad**: Un objeto se vuelve "inalcanzable" (huérfano) cuando no existe ninguna referencia activa en el Stack (ni en otros objetos del Heap) que apunte a él.

### El Recolector de Basura (Garbage Collector)
El Garbage Collector (GC) es un hilo de ejecución de baja prioridad de la JVM que se ejecuta en segundo plano de manera automática.
*   **Función**: Identifica y destruye los objetos inalcanzables en el Heap para liberar su espacio de memoria.
*   **Funcionamiento**: Aunque existen varios algoritmos (como *Mark & Sweep*), la premisa es que el GC recorre las raíces (variables del Stack) y marca todo lo alcanzable. Lo que quede sin marcar se barre (se libera).
*   **Invocación**: El desarrollador **no puede forzar** la recolección de basura. Llamar a `System.gc()` es simplemente una sugerencia a la JVM, la cual puede ser ignorada.

---

## 4. Ejemplo Práctico en Java y su Traza de Memoria

Analicemos el ciclo de vida y la pérdida de referencias con este programa:

```java
public class PruebaMemoria {
    public static void main(String[] args) {
        // Paso 1: Creación de dos objetos
        Persona a = new Persona("Carlos");
        Persona b = new Persona("Marta");
        
        // Paso 2: Redirección de referencia
        a = b; 
        
        // Paso 3: Pérdida total de referencias
        b = null;
    }
}
```

### Traza Paso a Paso de la Memoria:

1.  **Tras Paso 1**:
    *   **Stack**: Variable `a` guarda `ref @010`. Variable `b` guarda `ref @020`.
    *   **Heap**: Objeto `"Carlos"` en `@010`. Objeto `"Marta"` en `@020`.
2.  **Tras Paso 2 (`a = b`)**:
    *   **Stack**: Variable `a` ahora guarda `ref @020`. Variable `b` guarda `ref @020`.
    *   **Heap**: El objeto `"Carlos"` en `@010` queda **sin referencias**. Se vuelve inalcanzable. Es candidato para ser destruido por el Garbage Collector en su próxima pasada.
3.  **Tras Paso 3 (`b = null`)**:
    *   **Stack**: Variable `a` mantiene `ref @020`. Variable `b` pasa a ser `null` (no apunta a nada).
    *   **Heap**: El objeto `"Marta"` en `@020` **sigue vivo** porque la referencia `a` todavía apunta a él.

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Dado el siguiente código en Java:
```java
public class Ejercicio {
    public static void main(String[] args) {
        int[] v1 = {1, 2, 3};
        int[] v2 = v1;
        v2[0] = 99;
        System.out.println(v1[0]);
    }
}
```
1. Explicar razonadamente qué se imprimirá por consola.
2. Dibujar el mapa de memoria (Stack vs. Heap) durante la línea `v2[0] = 99;`.

**Solución**:
1.  **Explicación**: Se imprimirá `99`. En Java, los arrays son tratados como objetos y residen en el Heap. La variable `v1` en el Stack guarda la referencia al array `{1, 2, 3}` en el Heap. La línea `int[] v2 = v1;` copia dicha referencia en `v2`. Por tanto, `v1` y `v2` apuntan exactamente al mismo array en el Heap. Al modificar el primer elemento mediante `v2[0] = 99;`, estamos alterando el array compartido, cambio que es inmediatamente visible a través de `v1`.

2.  **Mapa de Memoria**:

```
        STACK (Pila)                         HEAP (Montículo)
   +-------------------+                +-----------------------+
v1 |   ref @100        |--------------> |  int[] (Array)        |
   +-------------------+                |  [0]: 99 (antes 1)    |
v2 |   ref @100        |-------------/  |  [1]: 2               |
   +-------------------+                |  [2]: 3               |
                                        +-----------------------+
```
