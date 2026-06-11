# Tema 14: Tipos Abstractos de Datos (TADs) Lineales

Un Tipo Abstracto de Datos (TAD) es una especificación matemática de un conjunto de datos junto con las operaciones permitidas sobre ellos, definida de manera totalmente independiente de cómo se codifiquen o almacenen físicamente en memoria. Estudiaremos las estructuras lineales clásicas (**Pilas**, **Colas** y **Listas**) analizando sus interfaces y sus implementaciones eficientes en Java mediante arrays y nodos dinámicos.

---

## 1. Abstracción en Estructuras de Datos

En Java, la separación entre la definición de un TAD y su implementación física se modela de forma natural:
*   **La Interfaz (`interface`)**: Define la especificación abstracta de los métodos (qué hace el TAD).
*   **La Clase (`class`)**: Define la estructura de datos física y codifica los métodos (cómo lo hace).

Las tres estructuras lineales clásicas son:
1.  **Pila (Stack)**: Estructura LIFO (Last-In, First-Out). El último elemento en entrar es el primero en salir. (Ej: historial de navegación, deshacer en editores, pila de llamadas de la JVM).
2.  **Cola (Queue)**: Estructura FIFO (First-In, First-Out). El primer elemento en entrar es el primero en salir. (Ej: cola de impresión, buffer de paquetes de red).
3.  **Lista (List)**: Colección secuencial ordenada donde se pueden insertar y eliminar elementos en cualquier posición de la secuencia.

---

## 2. Alternativas de Implementación: Estática vs. Dinámica

Para cualquier TAD lineal, existen dos estrategias básicas de representación en memoria:

### A. Implementación Estática (Basada en Arrays)
*   **Estructura**: Los elementos se guardan en posiciones contiguas de un array de tamaño fijo.
*   **Ventajas**: Acceso aleatorio directo muy rápido por índice ($\Theta(1)$). Menor consumo de memoria al no requerir punteros de enlace.
*   **Desventajas**: Capacidad limitada fija al crear la estructura. Si el array se llena, se debe crear uno nuevo de mayor tamaño y copiar todos los elementos ($\Theta(N)$).

### B. Implementación Dinámica (Basada en Nodos Enlazados)
*   **Estructura**: Los elementos se guardan en objetos independientes (nodos) dispersos en el Heap. Cada nodo almacena el dato útil y una **referencia (puntero)** al siguiente nodo.
*   **Ventajas**: Capacidad ilimitada y dinámica. La inserción o eliminación al inicio de la estructura es extremadamente eficiente ($\Theta(1)$) y no requiere desplazar elementos en memoria.
*   **Desventajas**: No permite acceso directo; para acceder al elemento $i$-ésimo es necesario recorrer la cadena de nodos desde el primero ($\Theta(N)$). Consume más memoria debido a las referencias de enlace.

---

## 3. Implementación Completa de una Pila Dinámica Genérica

Definiremos el contrato del TAD Pila y su correspondiente implementación dinámica utilizando nodos enlazados en Java.

### A. La Interfaz del TAD Pila (`PilaTAD`)
```java
public interface PilaTAD<T> {
    // Inserta un elemento en el tope de la pila
    void apilar(T elemento);

    // Elimina y devuelve el elemento del tope de la pila. 
    // Precondición: la pila no debe estar vacía.
    T desapilar();

    // Devuelve el elemento del tope sin eliminarlo.
    // Precondición: la pila no debe estar vacía.
    T cima();

    // Retorna true si la pila no contiene elementos
    boolean estaVacia();
}
```

### B. La Implementación Dinámica (`PilaEnlazada`)
```java
import java.util.EmptyStackException;

public class PilaEnlazada<T> implements PilaTAD<T> {

    // Clase interna privada que representa el nodo de enlace
    private static class Nodo<E> {
        E dato;
        Nodo<E> siguiente;

        Nodo(E dato, Nodo<E> siguiente) {
            this.dato = dato;
            this.siguiente = siguiente;
        }
    }

    // Atributo: referencia al nodo que está en el tope de la pila
    private Nodo<T> tope;

    public PilaEnlazada() {
        this.tope = null; // Pila inicialmente vacía
    }

    @Override
    public void apilar(T elemento) {
        // Creamos un nuevo nodo cuyo siguiente es el antiguo tope, 
        // y actualizamos el tope de la pila. Coste: Theta(1).
        this.tope = new Nodo<>(elemento, this.tope);
    }

    @Override
    public T desapilar() {
        // Precondición del contrato
        if (estaVacia()) {
            throw new EmptyStackException();
        }
        T datoRecuperado = this.tope.dato;
        this.tope = this.tope.siguiente; // Movemos el puntero al siguiente nodo. Coste: Theta(1).
        return datoRecuperado;
    }

    @Override
    public T cima() {
        // Precondición del contrato
        if (estaVacia()) {
            throw new EmptyStackException();
        }
        return this.tope.dato; // Coste: Theta(1).
    }

    @Override
    public boolean estaVacia() {
        return this.tope == null;
    }
}
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Utilizar el TAD Pila para implementar un algoritmo que compruebe si una cadena de texto que contiene paréntesis, llaves y corchetes está balanceada correctamente. Por ejemplo:
*   `"{[()]}"` -> Válida (retorna `true`).
*   `"{[(])}"` -> Inválida (retorna `false`).

**Solución**:
```java
public class BalanceadorSimbólico {

    public static boolean estaBalanceado(String expresion) {
        PilaTAD<Character> pila = new PilaEnlazada<>();

        for (int i = 0; i < expresion.length(); i++) {
            char c = expresion.charAt(i);

            // Si es un símbolo de apertura, lo apilamos
            if (c == '(' || c == '{' || c == '[') {
                pila.apilar(c);
            } 
            // Si es un símbolo de cierre, comprobamos el tope
            else if (c == ')' || c == '}' || c == ']') {
                if (pila.estaVacia()) {
                    return false; // Cierre sin apertura previa
                }
                char tope = pila.desapilar();
                if (!sonPareja(tope, c)) {
                    return false; // Los símbolos no coinciden
                }
            }
        }
        // La cadena está balanceada si no quedan símbolos pendientes en la pila
        return pila.estaVacia();
    }

    private static boolean sonPareja(char apertura, char cierre) {
        return (apertura == '(' && cierre == ')') ||
               (apertura == '{' && cierre == '}') ||
               (apertura == '[' && cierre == ']');
    }

    public static void main(String[] args) {
        System.out.println(estaBalanceado("{[()]}")); // Imprime true
        System.out.println(estaBalanceado("{[(])}")); // Imprime false
        System.out.println(estaBalanceado("((())"));   // Imprime false
    }
}
```
*(Nota: Este algoritmo tiene un coste temporal lineal $\Theta(N)$ y espacial $O(N)$ en el peor caso, lo cual es óptimo para la resolución del problema de balanceo de paréntesis).*
