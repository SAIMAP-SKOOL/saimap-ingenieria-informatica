# Tema 1: Conceptos Fundamentales: Lenguaje Máquina vs. Ensamblador ARM

Para comprender la ejecución de un programa en un computador, es imprescindible estudiar los niveles de abstracción más cercanos al hardware físico. La CPU no entiende lenguajes de programación de alto nivel como Java, C++ o Python; procesa únicamente secuencias binarias de unos y ceros en la memoria.

---

## 1. Niveles de Abstracción: Alto Nivel, Ensamblador y Lenguaje Máquina

*   **Lenguaje de Alto Nivel**: Diseñado para ser legible por humanos. Abstrae la memoria física mediante variables, objetos y estructuras de control dinámicas.
*   **Lenguaje Ensamblador (Assembly)**: Representación de bajo nivel basada en nemotécnicos (abreviaturas como `ADD`, `SUB`, `MOV`) que tienen una correspondencia directa (generalmente 1 a 1) con las instrucciones de la máquina. Depende estrictamente de la arquitectura del procesador (ARM, x86, MIPS).
*   **Lenguaje Máquina**: Código binario ejecutable interpretado directamente por la Unidad de Control del procesador. Se representa frecuentemente en hexadecimal para facilitar su lectura en herramientas de depuración.

### El Proceso de Construcción del Software (Pipeline de Compilación)
1.  **Compilador**: Traduce el código fuente en alto nivel (ej. `programa.c`) a código ensamblador (`programa.s`).
2.  **Ensamblador**: Convierte el código ensamblador en un fichero objeto binario (`programa.o`) que contiene instrucciones máquina no enlazadas y tablas de símbolos.
3.  **Enlazador (Linker)**: Combina múltiples ficheros objeto y bibliotecas de sistema, resolviendo las direcciones de memoria de variables y funciones globales, para generar el fichero ejecutable binario final (ej. `programa.elf` o `programa.exe`).

---

## 2. Filosofía RISC y la Arquitectura ARM

ARM (Advanced RISC Machines) es la arquitectura de procesadores más difundida en el mundo de los dispositivos móviles, consolas portátiles y sistemas empotrados. Su diseño se rige bajo los principios **RISC (Reduced Instruction Set Computer)**:

*   **Instrucciones de longitud fija**: Todas las instrucciones de ARM de 32 bits ocupan exactamente 32 bits (4 bytes) de memoria, lo que simplifica la decodificación por hardware de la Unidad de Control.
*   **Arquitectura Load/Store (Carga/Almacenamiento)**: El procesador no puede realizar operaciones lógicas o aritméticas directamente sobre posiciones de la memoria RAM. Los datos deben ser cargados en los registros del procesador (`LDR`), procesados en el banco de registros (ej. `ADD`) y luego almacenados de vuelta en la memoria (`STR`).
*   **Repertorio de instrucciones ortogonal**: Instrucciones simples que se ejecutan habitualmente en un solo ciclo de reloj.
*   **Banco de registros amplio**: En lugar de usar acumuladores o variables de memoria intermedias, RISC dispone de múltiples registros de propósito general veloces dentro de la propia CPU.

---

## 3. Entornos de Simulación y Laboratorio

El aprendizaje práctico de la programación en ensamblador ARM se facilita mediante simuladores que permiten observar la CPU por dentro de forma interactiva:
*   **VisUAL (Visual ARM Simulator)**: Un simulador de código abierto especialmente diseñado para estudiantes de ingeniería. Permite ejecutar programas de procesamiento de datos en ARMv7, ver la evolución temporal del banco de registros, los bits del registro de estado CPSR y simular memoria RAM direccionable.

---

## 4. El Toque Informático

### Simulación de un Micro-Ensamblador/Desensamblador
Como ingenieros informáticos, podemos programar herramientas que faciliten la traducción de bajo nivel. El siguiente script de Python actúa como un desensamblador rudimentario de instrucciones máquina binarias simuladas para ARM, abstrayendo cómo la CPU analiza los bits del opcode:

```python
def desensamblar_instruccion(codigo_hex):
    # Convertir código hexadecimal a binario de 32 bits relleno con ceros a la izquierda
    inst_bin = bin(int(codigo_hex, 16))[2:].zfill(32)
    
    # Simular la decodificación de campos de una instrucción de procesamiento de datos ARM
    condicion = inst_bin[0:4]
    opcode = inst_bin[4:8]
    rd = int(inst_bin[8:12], 2)
    rn = int(inst_bin[12:16], 2)
    operando2 = int(inst_bin[16:32], 2)
    
    # Mapeo básico de opcodes
    opcodes = {
        "0100": "ADD",
        "0010": "SUB",
        "1101": "MOV",
        "0000": "AND"
    }
    
    # Mapeo de condiciones
    condiciones = {
        "1110": "AL (Siempre)",
        "0000": "EQ (Igual)",
        "0001": "NE (No igual)"
    }
    
    op_name = opcodes.get(opcode, "UNKNOWN")
    cond_name = condiciones.get(condicion, "UNKNOWN")
    
    print(f"Código Hex: 0x{codigo_hex}")
    print(f"  - Binario:    {inst_bin}")
    print(f"  - Condición:  {condicion} ({cond_name})")
    print(f"  - Opcode:     {opcode} ({op_name})")
    print(f"  - Registro D: R{rd}")
    print(f"  - Registro N: R{rn}")
    print(f"  - Operando 2: {operando2} (en decimal)")
    print(f"Traducción ASM sugerida: {op_name} R{rd}, R{rn}, #{operando2}")
    print("-" * 50)

# Simulación: Decodificar una instrucción ADD R1, R2, #45
# Representada con condición AL (1110), opcode ADD (0100), Rd (R1: 0001), Rn (R2: 0010), Operand2 (45: 0000000000101101)
# 1110 0100 0001 0010 0000000000101101 en binario -> E412002D en Hexadecimal
desensamblar_instruccion("E412002D")
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Diferenciar entre un compilador, un ensamblador y un enlazador en el ciclo de vida del desarrollo de software.

**Solución:**
*   **Compilador**: Recibe un archivo escrito en lenguaje de alto nivel (como C) y lo traduce a código ensamblador (legible para el programador de bajo nivel, con nemotécnicos de la arquitectura).
*   **Ensamblador**: Recibe el archivo en ensamblador y traduce sus instrucciones a binario puro (código máquina), generando un archivo objeto (`.o`).
*   **Enlazador**: Une los distintos archivos objeto del proyecto y resuelve las direcciones absolutas de las variables y funciones externas y librerías, empaquetando todo en un único fichero ejecutable por el sistema operativo.

### Ejercicio 2
Explicar por qué en una arquitectura RISC como ARM no es posible realizar una suma directa de una variable de memoria RAM con el contenido de un registro del procesador.

**Solución:**
ARM sigue estrictamente la arquitectura **Load/Store**. El bus de datos y la ALU (Unidad Aritmético-Lógica) están integrados dentro de la CPU para operar únicamente con operandos situados en los registros internos. La memoria RAM es un subsistema externo lento. Para realizar la operación, se debe seguir la siguiente secuencia obligatoria:
1.  Cargar el dato de la memoria RAM a un registro temporal (ej. `LDR R0, [R1]`).
2.  Realizar la suma sobre el registro (ej. `ADD R0, R0, R2`).
3.  Almacenar el resultado de vuelta en la memoria RAM (ej. `STR R0, [R1]`).

---

## 6. Ejercicios Propuestos

1.  Dada una instrucción con el código hexadecimal `0xE021000A` en el simulador, explica el significado de codificar la información en hexadecimal en lugar de binario directo para el programador.
2.  Investiga el papel de los nemotécnicos en ensamblador y determina si existe una traducción siempre directa de 1 a 1 entre una línea de código ensamblador y una instrucción máquina de 32 bits de ARM.
3.  Describe brevemente las diferencias operativas entre la arquitectura x86 (típica de ordenadores personales) y ARM (RISC). ¿Por qué ARM consume significativamente menos energía que x86?
