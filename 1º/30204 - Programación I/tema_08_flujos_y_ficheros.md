# Tema 8: Flujos y Ficheros

La memoria principal (RAM) del ordenador es volátil y tiene una capacidad limitada. Al cerrar el programa o apagar el equipo, todas las variables, arrays y estructuras se pierden. Para lograr que la información se conserve a largo plazo se requiere de la **persistencia de datos** en soportes de almacenamiento secundario (discos duros y SSDs) utilizando **ficheros**. En C++, esto se modela a través del concepto de **flujo** (stream).

---

## 1. El Concepto de Flujo (Stream) en C++

Un **flujo** es una abstracción lógica que representa un canal de comunicación unidireccional para transferir datos de forma secuencial desde una fuente (productor) hasta un sumidero (consumidor).

C++ utiliza la biblioteca estándar `<iostream>` para gestionar dos flujos básicos:
*   `std::cin`: Flujo de entrada estándar (normalmente asociado al teclado).
*   `std::cout`: Flujo de salida estándar (asociado a la pantalla de la consola).

Para interactuar con ficheros físicos en disco, se utiliza la biblioteca estándar **`<fstream>`** (File Stream), que proporciona tres tipos de flujos especializados:
*   `std::ifstream`: Flujo para operaciones de **lectura** de ficheros (Input File Stream).
*   `std::ofstream`: Flujo para operaciones de **escritura** de ficheros (Output File Stream).
*   `std::fstream`: Flujo bidireccional para **lectura y escritura**.

---

## 2. Escritura de Ficheros de Texto (`ofstream`)

Para escribir datos en un fichero de texto, seguimos el siguiente protocolo secuencial:

1.  **Declarar el flujo**: Crear un objeto del tipo `std::ofstream`.
2.  **Abrir el fichero**: Vincular el flujo con un archivo en disco mediante el método `.open()`.
3.  **Verificar la apertura**: Comprobar si el archivo se abrió correctamente (por ejemplo, si tenemos permisos de escritura).
4.  **Escribir los datos**: Enviar información al flujo utilizando el operador de inserción `<<` (de forma idéntica a `cout`).
5.  **Cerrar el flujo**: Invocar al método `.close()` para liberar el descriptor del archivo y asegurar el guardado de datos.

```cpp
#include <fstream>

std::ofstream archivo;
archivo.open("datos.txt");
if (archivo.is_open()) {
    archivo << "Linea de texto a guardar" << std::endl;
    archivo.close();
}
```

---

## 3. Lectura de Ficheros de Texto (`ifstream`)

Para leer la información guardada en un fichero, el protocolo es análogo:

1.  **Declarar el flujo**: Crear un objeto del tipo `std::ifstream`.
2.  **Abrir el fichero**: Invocar `.open()`.
3.  **Verificar la apertura**: Asegurar que el archivo existe en el disco duro.
4.  **Leer los datos**: Extraer información del flujo utilizando el operador `>>` o `getline()`.
5.  **Cerrar el flujo**: Invocar `.close()`.

### Detección de Fin de Fichero (EOF)
Para leer un archivo entero de tamaño desconocido, se utiliza un bucle que comprueba si la extracción fue exitosa o si se alcanzó el fin del fichero (`eof`).
La forma más robusta es usar el propio flujo como condición del bucle:
```cpp
std::string linea;
while (std::getline(archivo, linea)) {
    // Procesar la linea leida
}
```

---

## 4. El Toque Informático

### Almacenamiento en Búfer (Buffer Flushing)
Escribir datos directamente en un disco duro o SSD físico es una operación extremadamente lenta a nivel de hardware.
Para solucionarlo, el sistema operativo acumula los datos de escritura en un área de memoria RAM intermedia ultrarrápida conocida como **Búfer (Buffer)**. Cuando el búfer se llena o el programa cierra el archivo, se vuelcan todos los datos al disco de golpe.
*   *Vaciado Manual*: Podemos forzar la transferencia física inmediata de los datos escribiendo `std::flush` o utilizando `std::endl` (que escribe un salto de línea `\n` y realiza un *flush* automático del búfer).

A continuación, implementamos un programa en C++ completo que escribe un listado de estudiantes (registros `struct`) en un archivo de texto, y posteriormente lo lee de nuevo en un array en memoria.

```cpp
#include <iostream>
#include <fstream>
#include <string>

struct Estudiante {
    std::string nombre;
    int edad;
    double nota;
};

int main() {
    const std::string nombre_fichero = "alumnos.txt";
    
    // 1. Escritura de registros en el fichero
    std::ofstream archivo_salida;
    archivo_salida.open(nombre_fichero);
    
    if (!archivo_salida.is_open()) {
        std::cerr << "Error al abrir el archivo para escritura." << std::endl;
        return 1;
    }
    
    // Escribimos dos estudiantes, uno por línea
    archivo_salida << "Juan_Perez 20 8.5" << std::endl;
    archivo_salida << "Maria_Gomez 22 9.2" << std::endl;
    
    archivo_salida.close();
    std::cout << "Datos guardados en " << nombre_fichero << " con éxito." << std::endl;
    
    // 2. Lectura y reconstrucción en memoria
    std::ifstream archivo_entrada;
    archivo_entrada.open(nombre_fichero);
    
    if (!archivo_entrada.is_open()) {
        std::cerr << "Error al abrir el archivo para lectura." << std::endl;
        return 1;
    }
    
    const int MAX_ESTUDIANTES = 5;
    Estudiante grupo[MAX_ESTUDIANTES];
    int contador = 0;
    
    // Leemos formateado directamente: nombre, edad y nota mientras no lleguemos al final
    while (archivo_entrada >> grupo[contador].nombre >> grupo[contador].edad >> grupo[contador].nota) {
        contador++;
        if (contador >= MAX_ESTUDIANTES) break; // Evitamos desbordar el array
    }
    
    archivo_entrada.close();
    
    // Imprimimos los resultados cargados desde el archivo
    std::cout << "\n--- Alumnos cargados desde fichero ---" << std::endl;
    for (int i = 0; i < contador; ++i) {
        std::cout << "Nombre: " << grupo[i].nombre 
                  << " | Edad: " << grupo[i].edad 
                  << " | Nota: " << grupo[i].nota << std::endl;
    }
    
    return 0;
}
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Escribir un programa en C++ que lea un archivo de texto llamado `numeros.txt` que contiene un número real por línea, calcule la suma y el promedio de todos ellos y muestre los resultados en pantalla.

**Solución:**
```cpp
#include <iostream>
#include <fstream>

int main() {
    std::ifstream entrada;
    entrada.open("numeros.txt");
    
    if (!entrada.is_open()) {
        std::cout << "No se pudo abrir el archivo numeros.txt. Asegurate de que existe." << std::endl;
        return 1;
    }
    
    double numero;
    double suma = 0.0;
    int cantidad = 0;
    
    // Bucle de lectura hasta el fin del fichero
    while (entrada >> numero) {
        suma += numero;
        cantidad++;
    }
    
    entrada.close();
    
    if (cantidad > 0) {
        std::cout << "Cantidad de numeros leidos: " << cantidad << std::endl;
        std::cout << "Suma total: " << suma << std::endl;
        std::cout << "Promedio: " << (suma / cantidad) << std::endl;
    } else {
        std::cout << "El archivo de números estaba vacío." << std::endl;
    }
    
    return 0;
}
```

---

## 6. Ejercicios Propuestos

1.  Escribir un programa en C++ que copie el contenido de un archivo de texto llamado `origen.txt` a otro llamado `destino.txt` carácter por carácter.
2.  ¿Qué diferencia hay entre abrir un archivo en modo texto clásico frente a abrirlo en modo de adición (`std::ios::app`) al instanciar un objeto `ofstream`?
3.  Explicar por qué es indispensable invocar el método `.close()` al finalizar el trabajo con un fichero en C++. ¿Qué ocurre con la memoria intermedia del búfer si el programa colapsa antes de cerrarlo?
