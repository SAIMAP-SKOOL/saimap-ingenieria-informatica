# Arquitectura y Organización de Computadores I: Programación en Ensamblador ARMv7

Bienvenido al manual completo de **Arquitectura y Organización de Computadores I** (AOC1) para el Grado en Ingeniería Informática. El objetivo de esta asignatura es comprender el procesador desde la perspectiva del programador de sistemas, estudiando cómo las instrucciones lógicas y aritméticas se traducen al nivel de arquitectura de máquina, cómo se gestiona el flujo de control, la pila de llamadas, los bloques de activación, y cómo interactúa el hardware con los periféricos mediante Entrada/Salida por consulta (polling) o interrupciones.

Esta asignatura es fundamental para que el futuro ingeniero o ingeniera aprenda a:
1.  Escribir código de bajo nivel altamente optimizado.
2.  Depurar errores de segmentación (Segmentation Fault) analizando el estado de la pila y registros del procesador.
3.  Entender cómo interoperan lenguajes de alto nivel como C con el ensamblador (AAPCS).
4.  Diseñar controladores de dispositivos a bajo nivel.

---

## Estructura del Manual

El manual se divide en 13 temas estructurados en tres bloques didácticos:

### Bloque 1: La Arquitectura de Máquina y el Repertorio (Semanas 1 a 5)
*   **[Tema 1: Conceptos fundamentales y simulación ARM](tema_01_lenguaje_maquina_ensamblador_arm.md)**: Diferencias entre código máquina, ensamblador y alto nivel. Introducción a la arquitectura ARM.
*   **[Tema 2: Banco de registros y CPSR](tema_02_banco_registros_cpsr.md)**: Registros de propósito general y registros especiales (SP, LR, PC). Registro de estado (CPSR) y banderas de condición.
*   **[Tema 3: Formato y codificación de instrucciones](tema_03_formato_codificacion_instrucciones.md)**: Codificación binaria de instrucciones ARM, opcodes y registros.
*   **[Tema 4: Modos de direccionamiento en ARM](tema_04_modos_direccionamiento_arm.md)**: Direccionamiento inmediato, indexado, pre y post-indexado con writeback.
*   **[Tema 5: Procesamiento de datos y saltos condicionales](tema_05_procesamiento_datos_saltos.md)**: Aritmética, lógica y bifurcaciones basadas en el estado del CPSR.

### Bloque 2: Programación Estructurada y Gestión de Memoria (Semanas 6 a 11)
*   **[Tema 6: Traducción de estructuras de control](tema_06_traduccion_estructuras_control.md)**: Condicionales (if-else), bucles (for, while) y vectores en ensamblador.
*   **[Tema 7: Mecanismo de llamadas y Link Register](tema_07_llamada_subrutinas_lr.md)**: Llamada y retorno con la instrucción `BL` y preservación de `LR`.
*   **[Tema 8: Uso de la pila y preservación del contexto](tema_08_uso_pila_stack.md)**: El modelo de pila Full Descending (FD), operaciones `PUSH` y `POP`.
*   **[Tema 9: Bloques de activación y Stack Frames](tema_09_bloques_activacion_stack_frames.md)**: Variables locales, direccionamiento por FP (R11) y paso de parámetros excedentes.

### Bloque 3: Interoperabilidad y Periféricos (Semanas 12 a 15)
*   **[Tema 10: Interoperabilidad C/Ensamblador y AAPCS](tema_10_interoperabilidad_c_ensamblador.md)**: El estándar de llamadas de ARM y distribución de registros.
*   **[Tema 11: Registros del controlador de E/S](tema_11_subsistema_entrada_salida.md)**: Registros de datos, estado y control en controladores de periféricos.
*   **[Tema 12: Consulta de estado (polling)](tema_12_transferencia_polling.md)**: Entrada/Salida síncrona por espera activa.
*   **[Tema 13: Interrupciones, excepciones e ISR](tema_13_interrupciones_excepciones.md)**: Rutinas de servicio a la interrupción (ISR) y la tabla de vectores (IVT).

---

## Entornos de Simulación ARM

Para probar los ejemplos prácticos de ensamblador de este manual, se recomienda usar uno de los siguientes entornos:
1.  **VisUAL (Visual ARM Simulator)**: Un simulador de interfaz gráfica excelente que permite visualizar el mapa de memoria y la ejecución paso a paso en ARMv7 de 32 bits de forma interactiva.
2.  **ARMSim#**: Simulador de bajo nivel enfocado en sistemas empotrados que permite depurar el banco de registros y el CPSR detalladamente.
3.  **GNU Toolchain & QEMU**: Para proyectos complejos con interoperabilidad C/ensamblador.
