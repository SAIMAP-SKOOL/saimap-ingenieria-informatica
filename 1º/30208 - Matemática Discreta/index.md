# Matemática Discreta: Lógica, Teoría de Números, Combinatoria y Grafos

Bienvenido al manual completo de **Matemática Discreta** para el Grado en Ingeniería Informática. A diferencia de las matemáticas continuas basadas en límites, derivadas e integrales, la matemática discreta estudia estructuras cuyos elementos se pueden contar uno a uno. Constituye la fundamentación teórica de las ciencias de la computación, dando soporte a la lógica de circuitos, las estructuras de datos, el análisis de algoritmos, las bases de datos y la criptografía de seguridad.

> [!TIP]
> Puedes acceder al [Manual Completo Unificado (Markdown)](Manual_Completo_Matematica_Discreta.md) que contiene todos los temas compilados junto con un índice, glosario y bibliografía en un único archivo listo para imprimir o convertir a PDF.

---

## Estructura del Manual

El manual se divide en cuatro bloques temáticos que cubren las 15 semanas de estudio:

### Bloque 1: Lógica y Técnicas de Demostración (Semanas 1 a 4)
*   [Tema 1: Lógica Proposicional](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30208%20-%20Matem%C3%A1tica%20Discreta/tema_01_logica_proposicional.md)
*   [Tema 2: Inferencia y Deducción Lógica](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30208%20-%20Matem%C3%A1tica%20Discreta/tema_02_inferencia_deduccion.md)
*   [Tema 3: Métodos de Demostración e Inducción](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30208%20-%20Matem%C3%A1tica%20Discreta/tema_03_metodos_demostracion.md)

### Bloque 2: Teoría de Números y Criptografía (Semanas 5 a 7)
*   [Tema 4: Divisibilidad y Algoritmo de Euclides](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30208%20-%20Matem%C3%A1tica%20Discreta/tema_04_divisibilidad_euclides.md)
*   [Tema 5: Aritmética Modular y Congruencias](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30208%20-%20Matem%C3%A1tica%20Discreta/tema_05_aritmetica_modular_congruencias.md)
*   [Tema 6: Criptografía RSA](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30208%20-%20Matem%C3%A1tica%20Discreta/tema_06_criptografia_rsa.md)

### Bloque 3: Combinatoria y Estructuras de Recurrencia (Semanas 8 a 11)
*   [Tema 7: Combinatoria Básica y Recuento](file:///d:/Usuario/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30208%20-%20Matem%C3%A1tica%20Discreta/tema_07_combinatoria_recuento.md)
*   [Tema 8: Relaciones de Recurrencia Lineales](file:///d:/Usuario/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30208%20-%20Matem%C3%A1tica%20Discreta/tema_08_relaciones_recurrencia.md)
*   [Tema 9: Funciones Generadoras](file:///d:/Usuario/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30208%20-%20Matem%C3%A1tica%20Discreta/tema_09_funciones_generadoras.md)

### Bloque 4: Teoría de Grafos y Algoritmia (Semanas 12 a 15)
*   [Tema 10: Conceptos Básicos de Grafos y Árboles](file:///d:/Usuario/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30208%20-%20Matem%C3%A1tica%20Discreta/tema_10_conceptos_basicos_grafos.md)
*   [Tema 11: Representación Matricial de Grafos](file:///d:/Usuario/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30208%20-%20Matem%C3%A1tica%20Discreta/tema_11_representacion_matricial.md)
*   [Tema 12: Algoritmos sobre Grafos](file:///d:/Usuario/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30208%20-%20Matem%C3%A1tica%20Discreta/tema_12_algoritmos_grafos.md)

---

## Orientación Práctica y Lenguajes de Programación

Para asimilar los conceptos abstractos de esta materia, el manual incluye código fuente listo para ejecutar:
1.  **Python 3**: Utilizado principalmente para teoría de números y álgebra modular, debido a su soporte nativo para enteros de precisión arbitraria (clave para tratar con números primos enormes en RSA).
2.  **C++**: Utilizado para la teoría de grafos por su correspondencia directa con el modelado de punteros y estructuras de datos dinámicas en memoria de bajo nivel (listas de adyacencia, colas de prioridad `std::priority_queue` para Dijkstra y estructuras de partición Union-Find para Kruskal).

### Requisitos del Sistema
Para correr las simulaciones de este manual:
*   Tener instalado Python 3.x (no requiere dependencias externas adicionales, pues usamos la biblioteca estándar).
*   Un compilador de C++ compatible con C++11 o superior (como GCC/G++ o MSVC).
