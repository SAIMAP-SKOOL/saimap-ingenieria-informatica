# Tema 4: Desarrollo Modular

A medida que los programas crecen en complejidad, escribir todo el código dentro de la función `main` se vuelve insostenible. El **desarrollo modular** consiste en dividir un problema complejo en subproblemas más pequeños y fáciles de resolver de forma aislada (metodología "divide y vencerás" o **Diseño Descendente**). Cada módulo se implementa mediante una **función** autónoma.

---

## 1. Declaración y Definición de Funciones

En C++, una función debe ser conocida por el compilador antes de poder ser invocada. Por ello, se divide en dos fases:

### 1.1 Declaración (Prototipo)
Informa al compilador sobre la existencia de la función, su nombre, qué parámetros recibe y qué tipo de dato devuelve. Se sitúa habitualmente al principio del archivo (o en archivos de cabecera `.h`):
```cpp
tipo_retorno nombre_funcion(tipo_parametro1 param1, tipo_parametro2 param2);
```

### 1.2 Definición
Contiene el cuerpo de la función con las instrucciones a ejecutar. Se suele implementar debajo de la función `main`:
```cpp
tipo_retorno nombre_funcion(tipo_parametro1 param1, tipo_parametro2 param2) {
    // Código de la función
    return valor_retorno; // Si el tipo_retorno no es void
}
```

*Funciones sin retorno (`void`)*: Si una función realiza una tarea (como imprimir un menú o inicializar un recurso) pero no devuelve ningún valor de cálculo, su tipo de retorno se declara como `void` y la instrucción `return` es opcional.

---

## 2. Paso de Parámetros: Valor vs Referencia

Los parámetros son las variables de entrada que una función necesita para operar. C++ ofrece dos mecanismos fundamentales para pasar argumentos a una función:

### 2.1 Paso por Valor
El compilador realiza una **copia** del valor de la variable original en el parámetro formal de la función.
*   **Comportamiento**: Cualquier modificación que se realice sobre el parámetro dentro de la función **no afecta** a la variable original del programa llamador.
*   **Uso**: Es seguro y el predeterminado para tipos de datos primitivos pequeños (`int`, `float`, `char`).

### 2.2 Paso por Referencia
Se pasa la **dirección de memoria** de la variable original utilizando el operador `&` en la declaración del parámetro.
*   **Comportamiento**: El parámetro formal actúa como un alias (referencia directa) de la variable original. Cualquier modificación que se realice dentro de la función **alterará directamente** la variable original.
*   **Uso**: Permite que una función devuelva múltiples resultados modificando las variables pasadas por referencia, y evita la copia ineficiente en memoria de estructuras u objetos pesados.

---

## 3. Especificación de Funciones: Precondiciones y Postcondiciones

El diseño de software robusto se basa en el **Diseño por Contrato** mediante la definición formal de precondiciones y postcondiciones:

*   **Precondiciones (Pre)**: Requisitos que deben cumplirse obligatoriamente antes de invocar la función (por ejemplo, "el divisor de una división no puede ser cero" o "el valor de entrada de una raíz cuadrada debe ser no negativo"). Es responsabilidad de quien invoca a la función asegurar que se cumplan.
*   **Postcondiciones (Post)**: Garantías que la función asegura cumplir al finalizar su ejecución, siempre y cuando se hayan satisfecho las precondiciones (por ejemplo, "la función devuelve el valor absoluto de la entrada" o "el fichero de salida ha sido guardado en disco").

---

## 4. El Toque Informático

### Gestión de Memoria: La Pila de Llamadas (Call Stack)
Cuando un programa invoca a una función, el sistema operativo reserva un bloque de memoria temporal en la **Pila de Llamadas** (Call Stack) conocido como **Marco de Activación** (Stack Frame).
El Marco de Activación contiene:
1.  Las variables locales y parámetros formales de la función.
2.  La **dirección de retorno** (dónde debe continuar el programa principal cuando termine la función).

*Rendimiento en memoria*:
*   Si pasamos una variable por **valor**, se reservan bytes adicionales en el Stack Frame para almacenar su copia. Si la estructura a copiar es grande, esto puede provocar saturación de memoria.
*   Si la pasamos por **referencia**, solo se almacena un puntero (típicamente de 4 u 8 bytes) al Stack Frame de la función llamadora, optimizando el consumo de recursos.

A continuación, implementamos el clásico algoritmo de intercambio de variables (`swap`) en C++ para ilustrar el comportamiento del paso por valor frente al paso por referencia.

```cpp
#include <iostream>

// 1. Paso por Valor: Intenta intercambiar pero falla al operar sobre copias
void swap_por_valor(int a, int b) {
    int aux = a;
    a = b;
    b = aux;
}

// 2. Paso por Referencia: Utiliza el operador & para modificar las variables originales
void swap_por_referencia(int& a, int& b) {
    int aux = a;
    a = b;
    b = aux;
}

int main() {
    int x = 10;
    int y = 20;
    
    std::cout << "Valores iniciales: x = " << x << ", y = " << y << std::endl;
    
    // Invocación por valor
    swap_por_valor(x, y);
    std::cout << "Valores tras swap_por_valor: x = " << x << ", y = " << y << " (Fallo)" << std::endl;
    
    // Invocación por referencia
    swap_por_referencia(x, y);
    std::cout << "Valores tras swap_por_referencia: x = " << x << ", y = " << y << " (Exito)" << std::endl;
    
    return 0;
}
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Dada la siguiente función, documentar sus Precondiciones y Postcondiciones y explicar qué devuelve.
```cpp
double calcular_raiz_division(double dividendo, double divisor) {
    return sqrt(dividendo / divisor);
}
```

**Solución (Especificación de Contrato):**
*   **Precondiciones**:
    1.  El `divisor` debe ser distinto de cero ($divisor \ne 0$) para evitar una división por cero en coma flotante.
    2.  El cociente de la división must ser mayor o igual a cero ($\frac{dividendo}{divisor} \ge 0$) para que la raíz cuadrada real esté matemáticamente definida.
*   **Postcondiciones**:
    1.  La función devuelve la raíz cuadrada del cociente de la división entre el dividendo y el divisor, expresada como un número real de precisión doble (`double`).
    2.  Las variables originales `dividendo` y `divisor` no sufren modificaciones en el programa llamador ya que se pasaron por valor.

---

## 6. Ejercicios Propuestos

1.  Escribir una función en C++ llamada `calcular_potencia` que tome una base real (`double`) y un exponente entero (`int`) y devuelva el resultado de elevar la base a la potencia. (Define sus firmas/declaraciones y definición).
2.  Escribir una función que reciba el radio de una esfera y devuelva tanto su área superficial como su volumen a través de paso de parámetros por referencia.
3.  ¿Qué es un desbordamiento de pila (*stack overflow*) y cómo se relaciona con la pila de llamadas al invocar funciones de forma recursiva infinita?
