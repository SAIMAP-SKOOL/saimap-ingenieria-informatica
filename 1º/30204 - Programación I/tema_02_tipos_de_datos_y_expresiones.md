# Tema 2: Tipos de Datos y Expresiones

Los ordenadores representan la información del mundo real en forma de bits estructurados. Para operar con ellos de forma segura y eficiente, los lenguajes fuertemente tipados como C++ asocian cada variable a un tipo de dato específico, lo que define cuánta memoria ocupa y qué operaciones son válidas.

---

## 1. Tipos de Datos Primitivos y Almacenamiento en Memoria

C++ clasifica sus tipos de datos básicos según el tipo de información y los bytes que ocupan en la memoria RAM:

| Tipo | Descripción | Tamaño típico (bytes) | Rango de valores |
| :--- | :--- | :--- | :--- |
| `bool` | Valor lógico booleano | 1 | `true` (1) o `false` (0) |
| `char` | Carácter individual (ASCII) | 1 | -128 a 127 o 0 a 255 |
| `int` | Número entero con signo | 4 | $-2.14 \cdot 10^9$ a $2.14 \cdot 10^9$ |
| `float` | Real en coma flotante (precisión simple) | 4 | $\pm 3.4 \cdot 10^{38}$ (6 decimales) |
| `double` | Real en coma flotante (precisión doble) | 8 | $\pm 1.7 \cdot 10^{308}$ (15 decimales) |

*El operador `sizeof`*: Permite consultar de forma dinámica cuántos bytes asigna el compilador de tu arquitectura a un determinado tipo de dato (por ejemplo, `sizeof(int)`).

---

## 2. Variables, Constantes y Ámbito (Alcance)

*   **Variables**: Espacios con nombre en la memoria RAM cuyo valor puede alterarse durante la ejecución. Deben declararse indicando su tipo y nombre:
    ```cpp
    int edad = 18;
    ```
*   **Constantes (`const`)**: Variables cuyo valor es inalterable tras su inicialización. Intentar modificar una constante produce un error de compilación:
    ```cpp
    const double PI = 3.14159265;
    ```
*   **Ámbito (Scope)**: Región del programa donde una variable es visible y accesible.
    *   *Locales*: Declaradas dentro de una función o bloque `{ }`. Solo existen durante la ejecución de ese bloque.
    *   *Globales*: Declaradas fuera de cualquier función. Son accesibles por todo el código, lo cual se considera una mala práctica porque dificulta la depuración.

---

## 3. Operadores y Precedencia

Los operadores combinan variables y constantes para evaluar expresiones:

1.  **Aritméticos**: Suma (`+`), resta (`-`), multiplicación (`*`), división (`/`), resto de división o módulo (`%` - solo para enteros).
2.  **Relacionales (Comparación)**: Igual a (`==`), diferente de (`!=`), mayor que (`>`), menor que (`<`), mayor o igual (`>=`), menor o igual (`<=`).
3.  **Lógicos**: Y lógico (`&&`), O lógico (`||`), negación lógica (`!`).
4.  **Asignación y Atajos**: Asignación ordinaria (`=`), y acumuladores combinados (`+=`, `-=`, `*=`, `/=`).
5.  **Incremento / Decremento**: Pre-incremento (`++x`), post-incremento (`x++`).

### Precedencia de Operadores
Determina el orden en que se evalúan las operaciones matemáticas cuando hay múltiples operadores en una misma línea:
1.  Paréntesis `( )` (máxima prioridad)
2.  Multiplicación, división y módulo `*`, `/`, `%`
3.  Suma y resta `+`, `-`
4.  Operadores relacionales
5.  Operadores lógicos (`&&` antes que `||`)
6.  Asignación `=` (mínima prioridad)

---

## 4. Conversión de Tipos (Casting)

*   **Conversión Implícita (Promoción)**: El compilador promociona de forma automática tipos más pequeños a más grandes para evitar pérdidas de información (por ejemplo, sumar un `int` y un `double` resulta en un `double`).
*   **Conversión Explícita (Casting)**: Conversión forzada por el programador. En C++ moderno se realiza mediante `static_cast`:
    ```cpp
    double resultado = static_cast<double>(suma) / cantidad;
    ```
    *Atención*: Si dividimos dos enteros en C++ (por ejemplo, `5 / 2`), el compilador realiza división entera devolviendo `2`. Si queremos obtener la parte decimal (`2.5`), debemos forzar el casting de al menos uno de los operandos.

---

## 5. El Toque Informático

### 5.1 Desbordamiento Aritmético (Overflow)
Dado que las variables se almacenan en un número fijo de bits, existe un valor máximo representable. Si incrementamos una variable que ya se encuentra en su límite superior de rango, esta sufre un **desbordamiento** (overflow) y su valor se "da la vuelta" hacia el valor mínimo representable, lo que puede provocar graves fallos en programas bancarios o de control aeronáutico.

A continuación, implementamos un programa en C++ que ilustra la división entera (y cómo evitarla mediante casting explícito) junto con un desbordamiento de tipo entero de rango corto (`short`).

```cpp
#include <iostream>

int main() {
    // 1. División entera vs Casting explícito
    int a = 5;
    int b = 2;
    
    double division_entera = a / b;
    double division_correcta = static_cast<double>(a) / b;
    
    std::cout << "--- Division de enteros ---" << std::endl;
    std::cout << "Resultado de " << a << " / " << b << " (sin cast): " << division_entera << std::endl;
    std::cout << "Resultado de " << a << " / " << b << " (con cast): " << division_correcta << std::endl;

    // 2. Simulación de Desbordamiento Aritmético (Overflow)
    // El tipo 'short' tiene típicamente 16 bits (rango: -32768 a 32767)
    short max_valor = 32767;
    
    std::cout << "\n--- Simulacion de Overflow ---" << std::endl;
    std::cout << "Valor maximo de short: " << max_valor << std::endl;
    
    // Sumamos 1 al valor máximo representable
    max_valor = max_valor + 1;
    
    std::cout << "Valor de short tras sumarle 1 (overflow): " << max_valor << std::endl;
    
    return 0;
}
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Dadas las declaraciones:
```cpp
int x = 10;
int y = 3;
double z = 2.5;
```
Evaluar paso a paso la expresión: `(x / y) + z * 2`

**Solución:**
1.  **Analizar la precedencia de operaciones**:
    *   La expresión tiene paréntesis, sumas y productos: `(x / y) + z * 2`.
2.  **Paso 1: Evaluar los paréntesis `(x / y)`**:
    *   `x` e `y` son de tipo `int`. Por lo tanto, `10 / 3` realiza una **división entera**, cuyo resultado es `3` (se descarta la parte decimal).
3.  **Paso 2: Evaluar la multiplicación `z * 2`**:
    *   `z` es un `double` (`2.5`). Al multiplicarse por `2` (entero), el `2` se promociona de forma implícita a `double` (`2.0`).
    *   `2.5 * 2.0 = 5.0` (resultado tipo `double`).
4.  **Paso 3: Evaluar la suma final `3 + 5.0`**:
    *   Sumamos el entero `3` (de la primera evaluación) con el `double` `5.0`.
    *   El entero `3` se promociona a `3.0`.
    *   `3.0 + 5.0 = 8.0`.
5.  **Resultado**: El valor final evaluado de la expresión es `8.0` (tipo `double`).

---

## 7. Ejercicios Propuestos

1.  Escribir un programa en C++ que lea el radio de un círculo del teclado y calcule y muestre su área. (Usa constantes para definir el valor de $\pi = 3.14159$).
2.  ¿Qué diferencia hay en el comportamiento del operador módulo (`%`) frente a la división tradicional (`/`) en C++?
3.  Si declaramos una variable de tipo `char letra = 'A';` y luego le sumamos 1 (`letra = letra + 1;`), ¿qué valor contendrá la variable al imprimirla en pantalla como carácter? Explica la relación con la tabla ASCII.
