# Matemáticas II: Álgebra Lineal y Cálculo Numérico para Ingeniería Informática

Bienvenido al manual completo de **Matemáticas II** para el Grado en Ingeniería Informática. Este recurso abarca desde las estructuras algebraicas teóricas (esenciales para criptografía y codificación) hasta los métodos numéricos lineales y la resolución computacional con software científico (Matlab/Octave).

> [!TIP]
> También puedes acceder al **[Manual Completo en un solo archivo](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30203%20-%20Matem%C3%A1ticas%20II/Manual_Completo_Matematicas_II.md)** para una lectura continua o impresión directa.

---

## Estructura del Manual

El manual se divide en dos grandes bloques temáticos y un laboratorio final de programación:

### Bloque I: Estructuras y Álgebra Lineal Teórica
*   [Tema 1: Estructuras Algebraicas y Cuerpos Finitos](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30203%20-%20Matem%C3%A1ticas%20II/tema_01_estructuras_algebraicas.md)
    *   Aritmética modular y algoritmo de Euclides extendido.
    *   Grupos, anillos y cuerpos. Cuerpos finitos ($\mathbb{Z}_p$, $\text{GF}(2^m)$).
    *   *Toque informático*: Criptografía RSA, AES y códigos correctores de errores.
*   [Tema 2: Álgebra Matricial y Sistemas de Ecuaciones](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30203%20-%20Matem%C3%A1ticas%20II/tema_02_algebra_matricial_sel.md)
    *   Matrices y determinantes. Operaciones elementales de fila y eliminación de Gauss.
    *   Clasificación de sistemas (Rouché-Frobenius).
*   [Tema 3: Espacios Vectoriales](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30203%20-%20Matem%C3%A1ticas%20II/tema_03_espacios_vectoriales.md)
    *   Dependencia lineal, subespacios, bases y dimensión.
*   [Tema 4: Aplicaciones Lineales](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30203%20-%20Matem%C3%A1ticas%20II/tema_04_aplicaciones_lineales.md)
    *   Matrices asociadas, núcleo (kernel) e imagen, y cambio de base.
    *   *Toque informático*: Matrices de proyección y rotación en gráficos computacionales 3D.
*   [Tema 5: Valores y Vectores Propios (Diagonalización)](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30203%20-%20Matem%C3%A1ticas%20II/tema_05_valores_vectores_propios.md)
    *   Polinomio característico, diagonalización y criterios.
    *   *Toque informático*: PageRank de Google y Reducción de Dimensionalidad (PCA).
*   [Tema 6: Ortogonalidad](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30203%20-%20Matem%C3%A1ticas%20II/tema_06_ortogonalidad.md)
    *   Espacios euclídeos, proyecciones y método de Gram-Schmidt.

### Bloque II: Vertiente Numérica y Computacional
*   [Tema 7: Factorización Matricial y Coste Computacional](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30203%20-%20Matem%C3%A1ticas%20II/tema_07_factorizacion_matricial.md)
    *   Descomposiciones LU, QR y Cholesky. Estrategias de pivotaje y complejidad temporal.
*   [Tema 8: Resolución Aproximada de Sistemas](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30203%20-%20Matem%C3%A1ticas%20II/tema_08_resolucion_aproximada_sistemas.md)
    *   Métodos iterativos de Jacobi y Gauss-Seidel. Método de las potencias.
*   [Tema 9: Laboratorio de Programación Matemática](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30203%20-%20Matem%C3%A1ticas%20II/tema_09_laboratorio_programacion_matematica.md)
    *   Sintaxis básica y vectorización en Matlab/Octave.
    *   Resolución numérica y simulación de los temas anteriores.

---

## Configuración de Software (Matlab/Octave)

Para ejecutar los ejemplos de este manual de forma libre y gratuita, se recomienda instalar **GNU Octave** (alternativa de código abierto compatible con Matlab):

*   **En Windows**: Puedes descargar el instalador ejecutable de Octave desde su web oficial (https://www.gnu.org/software/octave/).
*   **En Linux (Ubuntu/Debian)**:
    ```bash
    sudo apt update
    sudo apt install octave
    ```

El software científico permite realizar operaciones matriciales pesadas de forma optimizada gracias a las librerías BLAS (Basic Linear Algebra Subprograms) y LAPACK subyacentes.
