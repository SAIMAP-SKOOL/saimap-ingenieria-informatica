# Tema 6: Cadenas de Caracteres

La manipulación de texto es uno de los campos de aplicación más extendidos de la informática. Una **cadena de caracteres** es una secuencia ordenada de caracteres. En C++, coexisten dos formas de representar cadenas: las tradicionales cadenas al estilo del lenguaje C (C-strings) y los objetos modernos `std::string` de la biblioteca estándar de C++.

---

## 1. C-Strings (Cadenas al estilo de C)

En el lenguaje C, una cadena de caracteres no es un tipo de dato nativo, sino un **array de tipo `char` que finaliza obligatoriamente con el carácter nulo (`'\0'`)**. 

```
  Cadena: "H" "o" "l" "a" "\0"
  Índice:  0   1   2   3   4
```

El compilador reserva un byte adicional automáticamente para almacenar el carácter terminador `'\0'`, que indica el fin de la lectura de la cadena.

### Funciones de la biblioteca estándar `<cstring>`
Para operar con C-strings, se deben utilizar funciones de la librería de C:
*   `strlen(cadena)`: Devuelve la longitud real del texto (sin contar el `'\0'`).
*   `strcpy(destino, origen)`: Copia el contenido de una cadena en otra.
*   `strcat(destino, origen)`: Concatena (une) la cadena de origen al final del destino.
*   `strcmp(cad1, cad2)`: Compara alfabéticamente dos cadenas. Devuelve `0` si son idénticas, un valor negativo si `cad1 < cad2` y positivo en caso contrario.

> [!CAUTION]
> **Vulnerabilidad de Buffer Overflow (Desbordamiento de Búfer):**
> Funciones como `strcpy` o `strcat` no comprueban si el array de destino tiene espacio suficiente para albergar la nueva cadena. Si es más grande, se sobrescribirán posiciones de memoria contiguas a la variable. Esto constituye una de las mayores brechas de seguridad informática históricas, explotada por malware para inyectar código malicioso en el sistema.

---

## 2. La clase `std::string` de C++ Moderno

Para evitar los riesgos y la rigidez de las C-strings, C++ proporciona la clase **`std::string`** dentro de la biblioteca estándar `<string>`. Este tipo de datos gestiona de forma dinámica y automática la memoria necesaria.

### 2.1 Declaración y Operaciones Básicas
*   **Asignación directa**:
    ```cpp
    std::string mensaje = "Hola Mundo";
    ```
*   **Concatenación mediante operador `+`**:
    ```cpp
    std::string saludo = "Hola " + nombre;
    ```
*   **Comparación directa**: Se usan los operadores relacionales estándar (`==`, `!=`, `<`, `>`).
    ```cpp
    if (cad1 == cad2) { /* ... */ }
    ```

### 2.2 Métodos Útiles de `std::string`
*   `size()` o `length()`: Devuelven el número de caracteres de la cadena.
*   `empty()`: Devuelve `true` si la cadena está vacía.
*   `clear()`: Vacía la cadena.
*   `substr(posicion, longitud)`: Devuelve una subcadena a partir de la posición indicada y con el tamaño solicitado.
*   `find(subcadena)`: Busca la primera aparición de una subcadena y devuelve su índice; si no la encuentra, devuelve la constante `std::string::npos`.

---

## 3. El Toque Informático

### Lectura Robusta de Cadenas con Espacios
El operador de extracción estándar `cin >> cadena` lee texto de la consola, pero **se detiene al encontrar el primer espacio en blanco, tabulador o salto de línea**.
Si queremos leer una frase completa con espacios (por ejemplo, un nombre completo o dirección):
*   Para `std::string`, usamos la función **`getline(cin, cadena)`**.
*   Para C-strings, usamos **`cin.getline(array, tamaño)`**.

A continuación, implementamos en C++ un programa que procesa una cadena de texto (calculando estadísticas e identificando palabras) empleando las facilidades del tipo `std::string`.

```cpp
#include <iostream>
#include <string>

int main() {
    std::string nombre_completo;
    
    std::cout << "Introduce tu nombre completo: ";
    // Leemos la linea entera incluyendo los espacios
    std::getline(std::cin, nombre_completo);
    
    std::cout << "\nEstadisticas del nombre:" << std::endl;
    std::cout << "Texto completo: " << nombre_completo << std::endl;
    std::cout << "Numero de caracteres (con espacios): " << nombre_completo.length() << std::endl;
    std::cout << "¿Esta vacio?: " << (nombre_completo.empty() ? "Si" : "No") << std::endl;
    
    // Buscar la presencia de un espacio en blanco para separar el primer nombre
    size_t indice_espacio = nombre_completo.find(' ');
    
    if (indice_espacio != std::string::npos) {
        // Extraemos la subcadena que va desde la posición 0 hasta el espacio
        std::string primer_nombre = nombre_completo.substr(0, indice_espacio);
        std::cout << "Primer nombre extraido: " << primer_nombre << std::endl;
    } else {
        std::cout << "No se han detectado espacios. Nombre de una sola palabra." << std::endl;
    }
    
    return 0;
}
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Escribir una función en C++ llamada `es_palindromo` que tome un `std::string` y devuelva un valor booleano indicando si la palabra se lee igual de izquierda a derecha que de derecha a izquierda (ignorando mayúsculas/minúsculas).

**Solución:**
```cpp
#include <iostream>
#include <string>
#include <cctype> // Contiene la función tolower

bool es_palindromo(const std::string& cad) {
    int izq = 0;
    int der = cad.length() - 1;
    
    while (izq < der) {
        // Convertimos a minúsculas antes de comparar para ser tolerantes al caso
        if (std::tolower(cad[izq]) != std::tolower(cad[der])) {
            return false; // No coincide, no es palíndromo
        }
        izq++;
        der--;
    }
    return true; // Ha coincidido toda la palabra
}

int main() {
    std::string palabra = "Radar"; // 'R' y 'r' coincidirán gracias a tolower
    
    if (es_palindromo(palabra)) {
        std::cout << palabra << " es un palindromo." << std::endl;
    } else {
        std::cout << palabra << " no es un palindromo." << std::endl;
    }
    
    return 0;
}
```

---

## 5. Ejercicios Propuestos

1.  Escribir un programa en C++ que lea una frase del usuario y cuente el número de vocales (a, e, i, o, u) presentes en ella.
2.  ¿Qué diferencia hay en el comportamiento en memoria del paso de un `std::string` por valor frente a pasarlo por referencia constante (`const std::string&`) en las funciones?
3.  Describir qué es el carácter de terminación nula `'\0'`, y explicar qué ocurriría si un array de caracteres que representa a "Hola" careciera de este terminador al imprimirse con `std::cout`.
