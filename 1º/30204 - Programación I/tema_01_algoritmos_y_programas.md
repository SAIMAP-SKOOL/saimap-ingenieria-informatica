# Tema 1: Algoritmos y Programas

Un programa de ordenador es la plasmación física de un algoritmo mediante un lenguaje estructurado. Este tema introduce los conceptos fundamentales de la arquitectura del ordenador, el proceso de traducción de código fuente a binario ejecutable y el ciclo de vida del software para la construcción de sistemas estables y mantenibles.

---

## 1. Introducción a la Informática y Arquitectura de Computadores

El funcionamiento interno de cualquier ordenador moderno sigue el modelo propuesto en 1945 por John von Neumann.

### Arquitectura de Von Neumann
La **Arquitectura de Von Neumann** se divide en cuatro bloques principales interconectados por canales de comunicación denominados **buses**:
1.  **Unidad Central de Procesamiento (CPU)**: Es el "cerebro" y contiene:
    *   **Unidad de Control (UC)**: Decodifica y coordina la ejecución de las instrucciones de programa.
    *   **Unidad Aritmético-Lógica (ALU)**: Realiza operaciones aritméticas ($+$, $-$, $\cdot$) y lógicas ($\text{AND}$, $\text{OR}$, comparaciones).
    *   **Registros**: Memoria interna ultrarrápida de almacenamiento temporal de datos e instrucciones en ejecución.
2.  **Memoria Principal (RAM)**: Almacena de forma temporal tanto los datos de entrada/salida como las instrucciones del programa en ejecución. Es volátil (se borra al apagar el equipo).
3.  **Dispositivos de Entrada/Salida (E/S)**: Permiten la comunicación con el exterior (teclado, ratón, pantalla, discos duros).
4.  **Buses**: Vías de comunicación físicas (cables o pistas de circuito integrado) que transfieren datos, direcciones de memoria e instrucciones de control entre los componentes.

---

## 2. Traductores de Lenguaje: Compiladores vs Intérpretes

Los ordenadores solo comprenden el **lenguaje máquina** (código binario formado por secuencias de 0s y 1s que corresponden a voltajes eléctricos). Para programar a alto nivel (cerca del lenguaje humano), requerimos herramientas que traduzcan el código a binario.

### 2.1 Compiladores (Ejemplo: C++, Rust, Go)
Traducen todo el código fuente del programa de una sola vez y generan un archivo binario independiente y ejecutable (`.exe` o sin extensión en UNIX).
*   **Ventajas**: Ejecución extremadamente rápida (ya traducido al procesador) y protección del código fuente original.
*   **Desventajas**: El proceso de compilación toma tiempo y el binario generado no es portátil (un binario compilado en Windows no funciona en Linux o macOS).

### 2.2 Intérpretes (Ejemplo: Python, Ruby, PHP)
Traducen y ejecutan el código fuente línea a línea en tiempo de ejecución mediante un programa especial denominado intérprete.
*   **Ventajas**: Desarrollo rápido y código portátil (el mismo archivo `.py` corre en cualquier SO con el intérprete instalado).
*   **Desventajas**: Ejecución mucho más lenta y necesidad de distribuir el código fuente.

---

## 3. Herramientas de Desarrollo y Proceso de Compilación en C++

Para construir programas ejecutables en C++, el código pasa por cuatro etapas automáticas:

```
  Código Fuente (.cpp)  ===> [ Preprocesador ] ===> Código Expandido
                                                     ||
  Código Binario Objeto (.o) <=== [ Compilador ] <====++
            ||
            vv
     [ Enlazador / Linker ] ===> Archivo Ejecutable (.exe o binario)
```

1.  **Preprocesador**: Procesa las directivas que inician con `#` (como `#include`), copiando los archivos de cabecera al código fuente.
2.  **Compilador**: Convierte el código expandido en lenguaje de bajo nivel (ensamblador) y luego a código binario objeto (archivos `.o` o `.obj`).
3.  **Enlazador (Linker)**: Junta todos los archivos objeto generados por el compilador junto con las librerías precompiladas del sistema para crear el ejecutable definitivo.
4.  **Depurador (Debugger)**: Herramienta que permite ejecutar el programa línea a línea para inspeccionar el estado de las variables y detectar errores lógicos de ejecución.

---

## 4. Ciclo de Vida del Software

La ingeniería del software define el **Ciclo de Vida del Software** como el conjunto de fases por las que pasa un programa desde su concepción hasta su retirada:

1.  **Análisis de Requisitos**: Fase de recolección de las necesidades del cliente (qué debe hacer el programa).
2.  **Diseño**: Planificación técnica de la arquitectura del programa (diagramas, bases de datos y algoritmos).
3.  **Codificación (Implementación)**: Escritura del código fuente en el lenguaje de programación elegido.
4.  **Pruebas (Testing)**: Validación del código:
    *   *Pruebas Unitarias*: Verifican que funciones o módulos específicos actúan correctamente por separado.
    *   *Pruebas de Integración*: Verifican que los módulos funcionan correctamente al unirse.
5.  **Mantenimiento**: Corrección de errores tras el despliegue (bugs) y adición de nuevas funcionalidades.

---

## 5. El Toque Informático

### Algoritmos en Pseudocódigo
Un **algoritmo** es una secuencia finita, ordenada e inequívoca de pasos para resolver un problema. El **pseudocódigo** es un lenguaje de diseño algorítmico que imita las estructuras lógicas de programación sin importar la sintaxis rígida de un compilador.

A continuación, planteamos un algoritmo básico para calcular el **factorial** de un número entero no negativo $n$ ($n! = n \cdot (n-1) \dots 1$) en pseudocódigo y mostramos su transcripción directa a código funcional C++.

#### Algoritmo en Pseudocódigo:
```text
Algoritmo Factorial
    Leer n
    Si n < 0 Entonces
        Escribir "El factorial no está definido para negativos"
    Sino
        factorial <- 1
        Para i <- 1 Hasta n Con Paso 1 Hacer
            factorial <- factorial * i
        FinPara
        Escribir "El factorial es: ", factorial
    FinSi
FinAlgoritmo
```

#### Código Transcrito en C++:
```cpp
#include <iostream>

int main() {
    int n;
    std::cout << "Introduce un entero no negativo: ";
    std::cin >> n;

    if (n < 0) {
        std::cout << "El factorial no esta definido para negativos." << std::endl;
    } else {
        long long factorial = 1; // Usamos long long para evitar desbordamiento rápido
        for (int i = 1; i <= n; ++i) {
            factorial = factorial * i;
        }
        std::cout << "El factorial de " << n << " es: " << factorial << std::endl;
    }
    return 0;
}
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Describir brevemente la diferencia entre un error de compilación (sintáctico), un error de ejecución (runtime error) y un error lógico en el desarrollo de software.

**Solución:**
1.  **Error de Compilación**: Ocurre cuando el código viola las reglas gramaticales del lenguaje (por ejemplo, omitir un punto y coma `;` o declarar mal una variable). El compilador lo detecta y detiene el proceso de traducción, impidiendo la generación del ejecutable.
2.  **Error de Ejecución (Runtime Error)**: El programa compila correctamente y genera el binario, pero durante su ejecución realiza una operación prohibida por el sistema operativo (por ejemplo, intentar dividir un número por cero o acceder a una posición de memoria no asignada/puntero nulo). El programa se aborta abruptamente (crash).
3.  **Error Lógico**: El programa compila y corre hasta el final sin colapsar, pero produce resultados incorrectos (por ejemplo, calcular el factorial usando suma en lugar de multiplicación). Son los errores más difíciles de detectar y requieren de depuración (debugging) o suite de pruebas unitarias.

---

## 7. Ejercicios Propuestos

1.  Escribir el pseudocódigo de un algoritmo que lea tres números reales del usuario y calcule y muestre el promedio de los mismos.
2.  ¿Qué componente de la arquitectura de Von Neumann se encarga de realizar la suma de dos números enteros a nivel de hardware?
3.  Explicar detalladamente por qué las directivas que inician con `#include` en C++ no son procesadas por el compilador propiamente dicho, sino por el preprocesador.
