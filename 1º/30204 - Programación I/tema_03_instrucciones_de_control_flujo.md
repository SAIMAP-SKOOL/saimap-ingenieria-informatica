# Tema 3: Instrucciones de Control de Flujo

Por defecto, los programas ejecutan sus instrucciones en un flujo secuencial estricto (una tras otra en el orden en que están escritas). Las instrucciones de control de flujo permiten romper esta secuencialidad para tomar decisiones en base a condiciones variables y repetir bloques de instrucciones sin duplicar código.

---

## 1. Estructuras Condicionales (Selección)

Las condicionales desvían el flujo del programa por diferentes ramificaciones de ejecución basándose en la evaluación de expresiones booleanas (`true` o `false`).

### 1.1 Estructura `if` / `if-else`
Permite tomar una bifurcación binaria.
```cpp
if (condicion) {
    // Código ejecutado si la condición es verdadera (true)
} else {
    // Código ejecutado si la condición es falsa (false)
}
```

*Anidamiento*: Se pueden encadenar condiciones sucesivas mediante `else if`:
```cpp
if (nota >= 9) {
    std::cout << "Sobresaliente";
} else if (nota >= 7) {
    std::cout << "Notable";
} else {
    std::cout << "Aprobado/Suspenso";
}
```

### 1.2 Estructura `switch`
Es una alternativa más legible a múltiples condicionales anidadas cuando evaluamos una única variable de tipo entero, carácter o enumerado frente a valores constantes (casos):
```cpp
switch (opcion) {
    case 1:
        // Código para caso 1
        break;
    case 2:
        // Código para caso 2
        break;
    default:
        // Código si no coincide con ningún caso previo
}
```
> [!WARNING]
> El uso de la instrucción **`break`** al final de cada caso es fundamental. Si se omite, la ejecución continuará en los siguientes casos de forma secuencial (fenómeno conocido como *fallthrough*), lo cual suele generar errores de lógica indeseados.

---

## 2. Estructuras Iterativas (Bucles)

Los bucles permiten repetir la ejecución de un bloque de código mientras se cumpla una condición dada.

### 2.1 Bucle `while` (Bucle con Pre-condición)
Evalúa la condición **antes** de ejecutar el cuerpo del bucle. Si la condición inicial es falsa, el bucle nunca se llega a ejecutar:
```cpp
while (condicion) {
    // Código a repetir
    // (Debe alterar alguna variable de la condición para evitar un bucle infinito)
}
```

### 2.2 Bucle `do-while` (Bucle con Post-condición)
Ejecuta el cuerpo del bucle **al menos una vez**, y luego evalúa la condición de repetición al final:
```cpp
do {
    // Código ejecutado al menos una vez y repetido mientras se cumpla la condición
} while (condicion);
```
Es ideal para validación de entradas del usuario y menús de consola.

### 2.3 Bucle `for` (Bucle Controlado por Contador)
Es el más estructurado y compacto para repetir un bloque de código un número conocido de veces:
```cpp
for (inicializacion; condicion; incremento) {
    // Código a repetir
}
```
*Equivalencia*: Un bucle `for` se puede escribir equivalentemente como un bucle `while`:
```cpp
inicializacion;
while (condicion) {
    // Código
    incremento;
}
```

---

## 3. Instrucciones de Control de Bucles: `break` y `continue`

C++ proporciona dos palabras clave para alterar el comportamiento ordinario de los bucles:
*   **`break`**: Finaliza inmediatamente la ejecución del bucle, saltando a la primera instrucción que haya fuera de este.
*   **`continue`**: Omite el resto de la iteración actual y salta de inmediato a la siguiente evaluación de la condición del bucle (o incremento en un bucle `for`).

---

## 4. El Toque Informático

### Eficiencia, Anidamiento e Inestabilidad de Bucles
1.  **Bucle Infinito**: Si la condición de un bucle nunca pasa a ser falsa (por ejemplo, `while(true)` sin un `break` interno, o decremento erróneo en una condición de parada), el programa entrará en un bucle que consumirá el 100% de uso de la CPU, congelando el proceso.
2.  **Anidamiento y Complejidad**: Anidar bucles (un bucle dentro de otro) eleva drásticamente la cantidad de operaciones. Si un bucle exterior hace $n$ repeticiones y el interior hace $n$ repeticiones, el código del interior se ejecutará $n^2$ veces (complejidad cuadrática $O(n^2)$). Los programadores deben intentar limitar la anidación profunda para mantener los programas rápidos y optimizados.

A continuación, implementamos un programa interactivo en C++ que valida la entrada del usuario mediante un menú interactivo usando un bucle `do-while` y un selector `switch`.

```cpp
#include <iostream>

int main() {
    int opcion;
    
    // Bucle do-while para mantener el menú activo hasta que se elija salir (opción 3)
    do {
        std::cout << "\n=== MENU INTERACTIVO ===" << std::endl;
        std::cout << "1. Mostrar saludo" << std::endl;
        std::cout << "2. Mostrar consejo de programacion" << std::endl;
        std::cout << "3. Salir del programa" << std::endl;
        std::cout << "Elige una opcion (1-3): ";
        std::cin >> opcion;
        
        // Estructura condicional múltiple switch
        switch (opcion) {
            case 1:
                std::cout << "¡Hola! Bienvenido al manual de Programacion I." << std::endl;
                break;
            case 2:
                std::cout << "Consejo: Evita anidar demasiados bucles para mantener tu codigo eficiente." << std::endl;
                break;
            case 3:
                std::cout << "Saliendo del programa..." << std::endl;
                break;
            default:
                std::cout << "Opcion no valida. Por favor, introduce un numero del 1 al 3." << std::endl;
        }
    } while (opcion != 3);
    
    return 0;
}
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Escribir un programa en C++ que sume los números del 1 al 100 usando un bucle `for` y muestre el resultado por pantalla.

**Solución:**
```cpp
#include <iostream>

int main() {
    int suma = 0; // Acumulador inicializado en cero
    
    // El bucle inicializa i en 1, repite mientras i <= 100 e incrementa i en 1 en cada paso
    for (int i = 1; i <= 100; ++i) {
        suma += i; // Equivalente a: suma = suma + i
    }
    
    std::cout << "La suma de los numeros del 1 al 100 es: " << suma << std::endl;
    return 0;
}
```
*(Nota matemática: La suma se podría calcular de forma directa y más eficiente en O(1) operaciones mediante la fórmula de Gauss: $S = \frac{n(n+1)}{2} = \frac{100 \cdot 101}{2} = 5050$).*

---

## 6. Ejercicios Propuestos

1.  Escribir un programa en C++ que solicite una calificación numérica (0 a 10) al usuario y muestre por pantalla si está "Aprobado" (nota $\ge$ 5) o "Suspenso" (nota < 5) utilizando tanto un `if` como un operador ternario (`? :`).
2.  Escribir un programa en C++ que imprima en la consola los primeros $n$ números primos, donde $n$ lo introduce el usuario.
3.  ¿Qué ocurre si se omite el incremento en un bucle `for`? ¿Produce esto un error de compilación o de ejecución?
