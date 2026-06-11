# Programación II: Orientación a Objetos, Rigor Formal y Estructuras de Datos

Bienvenido al manual completo de **Programación II** (Programación Orientada a Objetos Robusta, Rigor Formal y Complejidad Algorítmica) para el Grado en Ingeniería Informática. Esta asignatura marca el salto desde la programación imperativa y estructurada básica a la construcción de sistemas de software robustos, modulares y formalmente correctos.

Este manual está estructurado bajo dos filosofías críticas:
1.  **Dibuja la memoria**: Comprender de forma espacial y visual dónde residen los objetos en ejecución (Stack vs. Heap), resolviendo las dudas comunes sobre referencias, polimorfismo y enlace dinámico.
2.  **Perder el miedo al formalismo**: Proporcionar una guía metodológica práctica y mecánica para superar las demostraciones formales de corrección en bucles (invariantes y funciones de cota) e inducción recursiva.

---

## Estructura del Manual

El manual consta de 14 temas distribuidos en tres bloques temáticos fundamentales:

### Bloque 1: Programación Orientada a Objetos Robusta (Semanas 1 a 6)
*   **[Tema 1: Transición a Java y gestión de memoria](tema_01_transicion_java_memoria.md)**: El modelo de la JVM (Stack vs. Heap), referencias de objetos, ciclo de vida y recolección de basura.
*   **[Tema 2: Clases y Encapsulamiento](tema_02_clases_encapsulamiento.md)**: Estructura de clases, atributos/métodos de instancia y estáticos, constructores, y modificadores de acceso (`public`, `private`).
*   **[Tema 3: Mecanismos de reutilización: Herencia](tema_03_reutilizacion_herencia.md)**: Jerarquía de clases (`extends`), visibilidad `protected`, sobreescritura de métodos (`@Override`) y la palabra clave `super`.
*   **[Tema 4: Polimorfismo, Enlace Dinámico e Interfaces](tema_04_polimorfismo_interfaces.md)**: Enlace dinámico en tiempo de ejecución, clases abstractas e interfaces como contratos de diseño.
*   **[Tema 5: Gestión de excepciones](tema_05_gestion_excepciones.md)**: Captura y lanzamiento de excepciones, excepciones de tipo Checked vs. Unchecked, try-with-resources y excepciones personalizadas.
*   **[Tema 6: Genericidad](tema_06_genericidad.md)**: Parametrización de tipos en clases, interfaces y métodos. Uso de comodines (`?`) y borrado de tipos (Type Erasure).

### Bloque 2: Rigor Formal y Recursividad (Semanas 7 a 11)
*   **[Tema 7: Especificación formal y Diseño por Contrato](tema_07_especificacion_contrato.md)**: Diseño por Contrato (DbC): Precondiciones, Postcondiciones e Invariante de Representación (IR).
*   **[Tema 8: Algoritmos recursivos](tema_08_algoritmos_recursivos.md)**: Concepto de recursividad, caso base y caso inductivo. Recursión lineal, binaria, mutua y recursión de cola.
*   **[Tema 9: Demostración de corrección en bucles](tema_09_correccion_bucles_invariantes.md)**: Tripletas de Hoare, construcción de Invariantes de bucle ($I$) y Funciones de cota ($t$) para demostrar terminación y corrección.
*   **[Tema 10: Demostración de corrección en recursividad](tema_10_correccion_recursividad.md)**: Inducción matemática fuerte aplicada a la demostración formal de algoritmos recursivos.

### Bloque 3: Eficiencia y Estructuras de Datos (Semanas 12 a 15)
*   **[Tema 11: Complejidad y coste algorítmico](tema_11_complejidad_algoritmica.md)**: Tiempo y espacio asintóticos. Notación asintótica ($O$, $\Omega$, $\Theta$). Análisis del peor, mejor y caso medio.
*   **[Tema 12: Análisis de coste en estructuras iterativas](tema_12_coste_iterativas.md)**: Reglas operativas en estructuras secuenciales, condicionales y bucles anidados independientes y dependientes.
*   **[Tema 13: Análisis de coste en algoritmos recursivos](tema_13_coste_recursivas.md)**: Ecuaciones de recurrencia, resolución por sustitución/expansión y aplicación del Teorema Máster.
*   **[Tema 14: Introducción a los TADs lineales](tema_14_tad_lineales.md)**: Tipos Abstractos de Datos. Implementación estática y dinámica de Pilas, Colas y Listas Enlazadas.

---

## Software y Entorno de Trabajo

Para compilar y ejecutar los ejemplos Java de este manual, se requiere:
1.  **Java SE Development Kit (JDK)**: Versión 11 o superior ([OpenJDK](https://openjdk.org/) o Oracle JDK).
2.  **IDE Recomendado**: IntelliJ IDEA Community Edition, Eclipse, o VS Code equipado con el paquete de extensión para Java.
