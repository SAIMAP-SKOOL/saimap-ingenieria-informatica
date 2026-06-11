# Tema 13: Mecanismo de Interrupciones y Excepciones en ARM

Para resolver las ineficiencias de la Entrada/Salida por Polling, la arquitectura de computadores introduce el mecanismo de **Interrupciones**. Este sistema permite a los periféricos notificar de forma asíncrona al procesador cuando ocurre un evento relevante, permitiendo a la CPU dedicarse a otras tareas útiles mientras el dispositivo trabaja de forma autónoma.

---

## 1. Conceptos y Diferencia entre Excepciones e Interrupciones

Aunque a nivel de hardware se gestionan de forma muy similar, conceptualmente existe una distinción clara:

*   **Excepciones (Síncronas)**: Son provocadas de forma interna por la propia ejecución de las instrucciones del programa. Ocurren en instantes de tiempo predecibles. Ejemplos:
    *   División por cero.
    *   Fallo de página de memoria.
    *   Instrucción no definida.
    *   Llamadas al sistema operativo (`SVC` - *Supervisor Call*).
*   **Interrupciones (Asíncronas)**: Son provocadas de forma externa por eventos físicos de hardware ajenos a la secuencia de instrucciones que corre la CPU. Ocurren de forma impredecible en cualquier instante de tiempo. Ejemplos:
    *   Llegada de un paquete de datos por la tarjeta de red.
    *   Pulsación de una tecla por parte del usuario.
    *   Vencimiento del temporizador de hardware (*Timer ticks*).

---

## 2. El Proceso de Atención a una Interrupción (Hardware/Software)

Cuando un periférico activa la línea física de interrupción (`IRQ` o `FIQ` en ARM), el procesador y el sistema operativo coordinan el siguiente flujo de acciones:

### 1. Fase de Hardware (CPU)
1.  La CPU finaliza la ejecución de la instrucción máquina actual.
2.  Salva el registro de estado actual copiándolo al **SPSR** (*Saved Program Status Register*) del modo correspondiente.
3.  Guarda el valor de retorno en el registro de enlace especial **LR_irq** (guardando $PC + 4$).
4.  Cambia automáticamente el modo del procesador a `IRQ Mode` y deshabilita nuevas interrupciones (pone el bit `I = 1` en el CPSR).
5.  Actualiza el contador de programa `PC` con la dirección de memoria reservada correspondiente de la **Tabla de Vectores de Interrupción (IVT)**.

### 2. Fase de Software (Sistema Operativo e ISR)
6.  La posición de la IVT contiene una instrucción de salto directo a la **Rutina de Servicio a la Interrupción (ISR)** de ese periférico.
7.  **Prólogo de la ISR**: Salva en la pila todos los registros de trabajo de la CPU que va a utilizar para no destruir el contexto del programa interrumpido.
8.  **Cuerpo de la ISR**: Procesa la información del periférico y escribe en la controladora para indicarle que el dato ya se leyó (borrar el bit de petición).
9.  **Epílogo de la ISR**: Restaura los registros guardados de la pila.
10. **Retorno de la ISR**: Ejecuta una instrucción de retorno dedicada que restaura simultáneamente el `PC` y el registro de estado `CPSR` desde el `SPSR`:
    `SUBS PC, LR, #4` (en ARM, debido al desfase del pipeline).

---

## 3. La Tabla de Vectores de Interrupción (IVT) en ARM

ARM reserva las posiciones más bajas de la memoria física para la Tabla de Vectores de Interrupción. Cada entrada de la tabla tiene un tamaño fijo de 4 bytes, suficiente para albergar una instrucción de salto (`LDR PC, [PC, #offset]`):

| Dirección Vector | Excepción / Interrupción | Descripción |
|:---:|:---|:---|
| **0x00000000** | Reset | Arranque o reinicio físico del procesador. |
| **0x00000004** | Undefined Instruction | La CPU intenta ejecutar un código de operación inválido. |
| **0x00000008** | Software Interrupt (SVC) | Llamada al sistema para invocar servicios del S.O. |
| **0x0000000C** | Prefetch Abort | Fallo al intentar buscar una instrucción en memoria. |
| **0x00000010** | Data Abort | Fallo al intentar leer/escribir datos en memoria (ej: alineación). |
| **0x00000018** | IRQ | Petición de interrupción externa normal. |
| **0x0000001C** | FIQ | Petición de interrupción externa rápida (Fast IRQ). |

---

## 4. El Toque Informático

### Simulador del Control de Flujo de Interrupciones
El siguiente script en Python simula el comportamiento de una CPU ejecutando un bucle principal de procesamiento, que es interrumpido de forma asíncrona por un temporizador de hardware. Muestra cómo se guarda el contexto y se bifurca a la tabla de vectores y a la ISR:

```python
class CPUSimulator:
    def __init__(self):
        self.pc = 0x8000  # Programa principal
        self.registers = {"R0": 42, "R1": 99}
        self.cpsr = "MODO_USUARIO"
        self.spsr = ""
        self.lr_irq = 0
        
    def bucle_principal(self):
        print(f"[CPU]: Ejecutando código principal. PC = 0x{self.pc:04X} | R0 = {self.registers['R0']}")
        self.pc += 4

    def simular_peticion_irq(self):
        print("\n!!! [HARDWARE]: Petición IRQ (Timer Tick) activa !!!")
        # 1. Salvar contexto de hardware
        self.spsr = self.cpsr
        self.lr_irq = self.pc + 4  # Dirección de retorno
        self.cpsr = "MODO_IRQ"
        
        # 2. Saltar al Vector de IRQ en la IVT (Dirección 0x18)
        self.pc = 0x0018
        print(f"[CPU]: Modo cambiado a {self.cpsr}. LR_irq = 0x{self.lr_irq:04X}. PC saltando a IVT [0x{self.pc:02X}].")
        
        # 3. Simular la ejecución de la ISR
        self._ejecutar_isr()

    def _ejecutar_isr(self):
        print(f"[IVT 0x0018]: Redirigiendo a ISR (LDR PC, =0x9000)")
        self.pc = 0x9000
        print(f"[ISR 0x{self.pc:04X}]: Guardando R0 y R1 en la pila.")
        # Procesamiento simulado
        print("[ISR]: Procesando tick del reloj del sistema...")
        print(f"[ISR 0x{self.pc:04X}]: Restaurando R0 y R1. Retornando.")
        
        # Retorno: Restaurar PC y CPSR (SUBS PC, LR, #4)
        self.pc = self.lr_irq - 4
        self.cpsr = self.spsr
        print(f"[CPU]: Retornado con éxito al programa principal. PC = 0x{self.pc:04X} | Modo = {self.cpsr}\n")

# Ejecución
cpu = CPUSimulator()
cpu.bucle_principal()
cpu.bucle_principal()
cpu.simular_peticion_irq()
cpu.bucle_principal()
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Explicar por qué al retornar de una rutina de servicio a la interrupción de tipo IRQ en un procesador ARM, se debe restar 4 al registro de enlace `LR` antes de copiarlo a `PC` (`SUBS PC, LR, #4`), en lugar de retornar directamente con `MOV PC, LR`.

**Solución:**
ARM utiliza una arquitectura con **cauce segmentado (pipelined)** de 3 etapas (Búsqueda, Decodificación y Ejecución). Debido a esto:
*   El Program Counter `PC` siempre apunta 8 bytes (dos instrucciones) por delante de la instrucción que se está ejecutando físicamente.
*   Cuando se activa la señal física de interrupción `IRQ`, el procesador guarda en `LR_irq` la dirección de la instrucción que estaba decodificando en ese momento ($PC + 4$).
*   Sin embargo, la instrucción que estaba en fase de decodificación no llegó a completarse (la CPU solo termina la que estaba en fase de ejecución).
*   Por lo tanto, al retornar de la ISR, debemos volver a ejecutar esa instrucción no completada. Para ello, debemos retroceder 4 bytes respecto a la dirección guardada en `LR`:
    $$\text{Dirección de Retorno Real} = LR - 4$$
La instrucción máquina `SUBS PC, LR, #4` resta 4 a `LR`, lo escribe en `PC` y restaura el estado del `CPSR` de forma atómica en un único ciclo de reloj.

### Ejercicio 2
Identificar la diferencia entre las interrupciones `IRQ` y `FIQ` de la arquitectura ARM. ¿Qué optimizaciones físicas implementa `FIQ` para ser más rápida?

**Solución:**
*   **IRQ (Interrupt Request)**: Es la línea de interrupción estándar de propósito general.
*   **FIQ (Fast Interrupt Request)**: Es la línea de interrupción de alta velocidad para dispositivos críticos.
*   **Optimizaciones de FIQ**:
    1.  **Banco de registros duplicado (*Register Shadowing*)**: FIQ tiene sus propios registros físicos dedicados `R8_fiq` a `R14_fiq`. Esto permite que la ISR de FIQ use estos registros directamente sin necesidad de apilarlos en la pila de memoria (ahorrando múltiples ciclos de lectura/escritura en RAM).
    2.  **Ubicación en la IVT**: El vector de FIQ está en la dirección `0x0000001C`, que es la última posición física de la IVT. Al no haber más vectores debajo, la rutina de servicio a la interrupción (ISR) de FIQ puede colocarse directamente a partir de la dirección `0x1C` sin necesidad de realizar una instrucción de salto intermedia.

---

## 6. Ejercicios Propuestos

1.  Distingue de forma detallada entre una **excepción síncrona** y una **interrupción asíncrona**, aportando dos ejemplos reales de cada una que afecten a la ejecución de un sistema informático.
2.  Un programador diseña una rutina de servicio a la interrupción (ISR) pero olvida guardar el registro `R0` en la pila al inicio. Describe las posibles consecuencias y fallos colaterales en el programa principal interrumpido.
3.  Investiga el papel del **Controlador Vectorizado de Interrupciones (VIC o NVIC)** en la arquitectura ARM. ¿Cómo ayuda a gestionar las prioridades cuando ocurren múltiples interrupciones de hardware simultáneamente?
