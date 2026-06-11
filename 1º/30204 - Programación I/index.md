# Programación I: Fundamentos y Programación Estructurada en C++

Bienvenido al manual completo de **Programación I** para el Grado en Ingeniería Informática. Este recurso cubre los fundamentos lógicos y metodológicos de la programación estructurada y modular utilizando C++ como lenguaje didáctico y de ingeniería.

> [!TIP]
> También puedes acceder al **[Manual Completo en un solo archivo](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30204%20-%20Programaci%C3%B3n%20I/Manual_Completo_Programacion_I.md)** para una lectura continua o impresión directa.

---

## Estructura del Manual

El manual se compone de 8 temas principales organizados de forma incremental:

### Fundamentos y Control de Flujo
*   [Tema 1: Algoritmos y Programas](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30204%20-%20Programaci%C3%B3n%20I/tema_01_algoritmos_y_programas.md)
    *   Ciclo de vida del software, Von Neumann, compiladores e intérpretes.
*   [Tema 2: Tipos de Datos y Expresiones](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30204%20-%20Programaci%C3%B3n%20I/tema_02_tipos_de_datos_y_expresiones.md)
    *   Variables, constantes, operadores, precedencia y casting de tipos.
*   [Tema 3: Instrucciones de Control de Flujo](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30204%20-%20Programaci%C3%B3n%20I/tema_03_instrucciones_de_control_flujo.md)
    *   Estructuras secuenciales, alternativas (`if`, `switch`) e iterativas (`while`, `for`, `do-while`).

### Diseño Modular y Estructuras de Datos
*   [Tema 4: Desarrollo Modular](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30204%20-%20Programaci%C3%B3n%20I/tema_04_desarrollo_modular.md)
    *   Funciones, paso de parámetros (valor y referencia mediante `&`), precondiciones y postcondiciones.
*   [Tema 5: Vectores y Matrices (Arrays)](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30204%20-%20Programaci%C3%B3n%20I/tema_05_vectores_y_matrices.md)
    *   Arrays unidimensionales y bidimensionales. Algoritmos de búsqueda (lineal/binaria) y ordenación (burbuja, selección, inserción).
*   [Tema 6: Cadenas de Caracteres](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30204%20-%20Programaci%C3%B3n%20I/tema_06_cadenas_de_caracteres.md)
    *   C-strings, `std::string` y funciones de manipulación de texto.
*   [Tema 7: Registros (Structs)](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30204%20-%20Programaci%C3%B3n%20I/tema_07_registros_structs.md)
    *   Definición de registros, anidamiento y arrays de structs.

### Entrada/Salida y Persistencia
*   [Tema 8: Flujos y Ficheros](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30204%20-%20Programaci%C3%B3n%20I/tema_08_flujos_y_ficheros.md)
    *   E/S estándar (`cin`/`cout`) e I/O en ficheros de texto (`fstream`).

---

## Configuración del Entorno de Desarrollo (C++)

Para escribir, compilar y probar los códigos de este manual, se recomienda configurar la colección de compiladores de GNU (**GCC**):

1.  **En Windows**: Puedes descargar y configurar **MinGW-w64** (que incluye `g++` y `make`) de forma rápida usando MSYS2 o instaladores empaquetados.
2.  **En Linux (Ubuntu/Debian)**:
    ```bash
    sudo apt update
    sudo apt install build-essential
    ```
    Este comando instala el compilador `g++`, el depurador `gdb` y utilidades de automatización de compilación.
3.  **Compilación Básica**:
    Para compilar un archivo fuente C++ llamado `programa.cpp` desde la terminal:
    ```bash
    g++ -std=c++11 -Wall -O2 programa.cpp -o programa
    ```
    *   `-std=c++11`: Utiliza el estándar moderno C++11.
    *   `-Wall`: Habilita todos los avisos de advertencia del compilador (buenas prácticas).
    *   `-O2`: Activa la optimización de código de nivel 2.
    *   `-o programa`: Define el nombre del archivo binario ejecutable de salida.
