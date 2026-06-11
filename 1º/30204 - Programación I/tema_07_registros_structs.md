# Tema 7: Registros (Structs)

Las estructuras de datos homogéneas (como vectores y matrices) solo pueden almacenar elementos del mismo tipo. Para modelar entidades del mundo real que constan de múltiples atributos de diferente naturaleza (por ejemplo, los datos de un cliente: nombre, edad, saldo, si está activo), se recurre a las estructuras heterogéneas conocidas en programación estructurada como **Registros** o **Estructuras** (`struct` en C++).

---

## 1. Definición y Acceso a Miembros

Un **registro** es un tipo de dato personalizado que agrupa bajo un mismo nombre una colección de variables (denominadas campos o miembros) que pueden tener tipos distintos.

### Definición de un `struct`
Se define fuera de las funciones utilizando la palabra clave `struct` seguida del nombre de la estructura y de sus campos encerrados entre llaves (finalizando obligatoriamente con un punto y coma `;`):
```cpp
struct Cliente {
    std::string nombre;
    int edad;
    double saldo;
    bool activo;
};
```

### Declaración e Inicialización de Variables
Una vez definida la estructura, se puede utilizar como cualquier tipo de dato primitivo para declarar variables:
```cpp
Cliente c1;
c1.nombre = "Luis Felipe";
c1.edad = 25;
c1.saldo = 1250.50;
c1.activo = true;
```
*El operador punto (`.`)*: Se utiliza para acceder individualmente a cada uno de los miembros o campos de la estructura.

---

## 2. Anidamiento de Estructuras

Un miembro de un `struct` puede ser a su vez otra estructura previamente definida:
```cpp
struct Fecha {
    int dia;
    int mes;
    int anio;
};

struct Empleado {
    std::string nombre;
    double salario;
    Fecha fecha_contratacion; // Estructura anidada
};
```

Para acceder a los campos anidados, se encadenan los operadores punto:
```cpp
Empleado emp;
emp.nombre = "Ana";
emp.fecha_contratacion.dia = 15;
emp.fecha_contratacion.mes = 6;
emp.fecha_contratacion.anio = 2026;
```

---

## 3. Combinación de Arrays y Estructuras

La potencia de los registros se multiplica al combinarlos con arrays.

### 3.1 Estructuras con Arrays Internos
Un registro puede contener arrays como miembros (por ejemplo, un estudiante con sus calificaciones):
```cpp
struct Estudiante {
    std::string nombre;
    double notas[5]; // Array interno de notas
};
```

### 3.2 Arrays de Estructuras (Bases de Datos en Memoria)
Podemos declarar vectores cuyos elementos sean de tipo `struct` para almacenar bases de datos tabulares completas en la memoria RAM:
```cpp
const int MAX_ALUMNOS = 100;
Estudiante clase[MAX_ALUMNOS]; // Array de 100 estructuras Estudiante
```

---

## 4. El Toque Informático

### Alineación de Memoria y Padding de Estructuras
El tamaño en memoria de un `struct` no es necesariamente la suma exacta de los tamaños de sus miembros. Las CPUs modernas leen datos de la memoria RAM en bloques alineados a palabras (normalmente de 4 u 8 bytes).
Para acelerar la velocidad de acceso, el compilador introduce bytes de relleno invisibles (**padding**) entre los campos de forma que cada variable comience en una dirección de memoria múltiplo de su tamaño.
Por ejemplo:
```cpp
struct Ineficiente {
    char a;   // 1 byte
    // 3 bytes de padding para alinear el int a una dirección múltiplo de 4
    int b;    // 4 bytes
    char c;   // 1 byte
    // 3 bytes de padding al final
}; // sizeof es 12 bytes, en lugar de 6 bytes.
```
*Optimización*: Organizar los campos de mayor a menor tamaño (colocando `int` antes de `char`) minimiza el padding introducido por el compilador, optimizando el uso de la memoria RAM.

A continuación, implementamos en C++ un sistema de gestión escolar básico (base de datos en memoria) utilizando arrays de estructuras.

```cpp
#include <iostream>
#include <string>

// Definimos la estructura del Alumno
struct Alumno {
    std::string nombre;
    int matricula;
    double nota_final;
};

// Función para imprimir los datos de un alumno (paso por referencia constante para eficiencia)
void mostrar_alumno(const Alumno& al) {
    std::cout << "Nombre: " << al.nombre 
              << " | Mat: " << al.matricula 
              << " | Nota: " << al.nota_final << std::endl;
}

// Función para buscar a un alumno por su número de matrícula (búsqueda lineal)
int buscar_por_matricula(const Alumno grupo[], int size, int mat_buscada) {
    for (int i = 0; i < size; ++i) {
        if (grupo[i].matricula == mat_buscada) {
            return i; // Devolvemos la posición en el array
        }
    }
    return -1; // No encontrado
}

int main() {
    const int N = 3;
    // Declaración e inicialización directa de un array de estructuras Alumno
    Alumno clase[N] = {
        {"Juan Perez", 1001, 8.5},
        {"Maria Gomez", 1002, 9.2},
        {"Carlos Ruiz", 1003, 4.7}
    };
    
    std::cout << "--- Listado de Alumnos ---" << std::endl;
    for (int i = 0; i < N; ++i) {
        mostrar_alumno(clase[i]);
    }
    
    // Búsqueda
    int buscar_mat = 1002;
    std::cout << "\nBuscando matricula " << buscar_mat << "..." << std::endl;
    int pos = buscar_por_matricula(clase, N, buscar_mat);
    
    if (pos != -1) {
        std::cout << "¡Alumno encontrado!" << std::endl;
        mostrar_alumno(clase[pos]);
    } else {
        std::cout << "No existe ningun alumno con esa matricula." << std::endl;
    }
    
    return 0;
}
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Definir una estructura en C++ llamada `Fraccion` que represente un número fraccionario (con numerador y denominador enteros). Escribir una función que tome dos estructuras `Fraccion` y devuelva una nueva estructura `Fraccion` que corresponda a la suma de ambas.

**Solución:**
```cpp
#include <iostream>

struct Fraccion {
    int numerador;
    int denominador;
};

// Función de suma de fracciones: (a/b) + (c/d) = (ad + bc) / bd
Fraccion sumar_fracciones(const Fraccion& f1, const Fraccion& f2) {
    Fraccion resultado;
    resultado.numerador = f1.numerador * f2.denominador + f2.numerador * f1.denominador;
    resultado.denominador = f1.denominador * f2.denominador;
    return resultado;
}

int main() {
    Fraccion fr1 = {1, 2}; // 1/2
    Fraccion fr2 = {1, 3}; // 1/3
    
    Fraccion suma = sumar_fracciones(fr1, fr2);
    
    std::cout << "Suma: " << suma.numerador << "/" << suma.denominador << std::endl; // 5/6
    return 0;
}
```

---

## 6. Ejercicios Propuestos

1.  Escribir una estructura llamada `Punto2D` con coordenadas `x` e `y` (`double`), y desarrollar una función que calcule y devuelva la distancia euclidiana entre dos puntos.
2.  ¿Qué diferencia hay en el paso de un `struct` a una función por valor frente a pasarlo por referencia constante? Explica el impacto en la memoria Stack.
3.  Utilizando el operador `sizeof`, escribe un programa en C++ que compare el tamaño de una estructura vacía frente a una con campos para ver cuántos bytes le asigna el compilador.
