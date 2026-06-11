# Tema 11: El Subsistema de Entrada/Salida: Registros del Controlador

Un computador no solo procesa datos internamente, también debe interactuar con el entorno exterior mediante periféricos (teclados, pantallas, sensores, tarjetas de red). El subsistema de Entrada/Salida (E/S) engloba la circuitería y protocolos necesarios para transferir información entre el procesador, la memoria y los controladores de dispositivos.

---

## 1. Métodos de Direccionamiento de Entrada/Salida

Para comunicarse con un periférico, la CPU debe poder acceder a los registros internos de su placa controladora. Existen dos enfoques de diseño:

### E/S Mapeada en Puertos (Isolated I/O)
*   **Principio**: El procesador dispone de un espacio de direcciones físicamente independiente para periféricos y utiliza instrucciones máquina específicas para acceder a él (ej. instrucciones `IN` y `OUT` en la arquitectura x86).
*   **Limitación**: Requiere buses de control adicionales y restringe el uso de las instrucciones de procesamiento de datos en periféricos.

### E/S Mapeada en Memoria (Memory-Mapped I/O - MMIO)
*   **Principio (Estándar en ARM)**: El controlador del periférico ocupa un rango de direcciones del espacio de direccionamiento general del sistema. La CPU se comunica con el dispositivo utilizando exactamente las mismas instrucciones de carga y almacenamiento empleadas con la memoria RAM (`LDR` y `STR`).
*   **Ventaja**: Es una arquitectura más limpia y permite aplicar cualquier modo de direccionamiento u operación de datos directamente sobre los registros del dispositivo.

---

## 2. Los Registros de la Placa Controladora de Dispositivo

El controlador de un periférico actúa como traductor entre los buses síncronos de alta velocidad del procesador y las señales eléctricas físicas del hardware exterior. Todo controlador expone de cara a la CPU una estructura lógica compuesta por tres tipos de registros de 8, 16 o 32 bits:

1.  **Registro de Datos (Data Register)**:
    *   *Función*: Almacena temporalmente la información enviada desde la CPU para ser transmitida al exterior (registro de salida), o la información recibida desde el exterior para ser leída por el procesador (registro de entrada).
2.  **Registro de Estado (Status Register)**:
    *   *Función (Solo Lectura para la CPU)*: Contiene banderas (*flags*) que reflejan el estado operativo actual del dispositivo. Ejemplos comunes:
        *   `Ready` (Listo para transmitir/recibir).
        *   `Busy` (Ocupado procesando).
        *   `Error` (Fallo en la comunicación física).
3.  **Registro de Control (Control Register)**:
    *   *Función (Escritura/Lectura para la CPU)*: Contiene bits de configuración escritos por el procesador para gobernar el comportamiento del periférico. Ejemplos comunes:
        *   Habilitar interrupciones del dispositivo.
        *   Configurar velocidad de transmisión (Baudrate).
        *   Iniciar o abortar una transferencia.

---

## 3. El Toque Informático

### Simulador de Hardware de un Controlador de Periférico (UART)
El siguiente script en Python simula el comportamiento lógico de una placa controladora UART (puerto serie) mapeada en memoria. Muestra cómo al escribir y leer registros simulados mediante funciones equivalentes a `LDR` y `STR`, se altera el estado interno del hardware periférico:

```python
class UARTController:
    def __init__(self):
        # Direcciones físicas simuladas de los registros del controlador (MMIO)
        self.ADDR_DATA = 0x40001000
        self.ADDR_STATUS = 0x40001004
        self.ADDR_CONTROL = 0x40001008
        
        # Estado interno de los registros (32 bits)
        self.reg_data = 0
        self.reg_status = 0x00000001  # Bit 0 (READY) inicializado en 1
        self.reg_control = 0
        
        # Búfer de transmisión físico
        self.buffer_tx = []

    def leer_registro(self, addr):
        if addr == self.ADDR_DATA:
            return self.reg_data
        elif addr == self.ADDR_STATUS:
            return self.reg_status
        elif addr == self.ADDR_CONTROL:
            return self.reg_control
        return 0

    def escribir_registro(self, addr, valor):
        if addr == self.ADDR_DATA:
            # Escribir en datos inicia la transmisión física
            self.reg_data = valor
            self.reg_status &= ~0x01  # Poner READY = 0 (Ocupado transmitiendo)
            self._simular_transmision_fisica(valor)
        elif addr == self.ADDR_CONTROL:
            self.reg_control = valor
            print(f"UART Control actualizado a: 0x{valor:08X}")

    def _simular_transmision_fisica(self, caracter_ascii):
        # El hardware envía los bits físicamente
        char = chr(caracter_ascii)
        self.buffer_tx.append(char)
        print(f"[HARDWARE UART]: Transmitiendo físicamente carácter '{char}'...")
        # Transmisión terminada: Restaurar READY = 1
        self.reg_status |= 0x01

# Simulación de la CPU escribiendo en la UART
uart = UARTController()
# 1. Comprobar si está lista leyendo el estado (READY bit)
estado = uart.leer_registro(uart.ADDR_STATUS)
if estado & 0x01:
    print("La UART está lista para transmitir.")
    # 2. Escribir el carácter ASCII 'H' (72) en el registro de datos (equivale a un STR de la CPU)
    uart.escribir_registro(uart.ADDR_DATA, 72)
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Un periférico tiene sus registros de control, estado y datos mapeados en las siguientes direcciones de memoria física:
*   `STATUS_REG`: `0x40002000` (Bit 0: `READY` a 1; Bit 1: `ERROR` a 1).
*   `DATA_REG`: `0x40002004`.
Escribir una rutina en ensamblador ARM para comprobar si el bit de error del periférico está activo y, en caso afirmativo, saltar a una etiqueta de control de fallos `gestionar_error`.

**Solución:**
```assembly
        LDR R0, =0x40002000   ; Cargar dirección de STATUS_REG en R0
        LDR R1, [R0]          ; R1 = contenido de STATUS_REG
        TST R1, #0x02         ; Comprobar bit 1 (ERROR) mediante AND lógica
        BNE gestionar_error   ; Si el resultado no es cero (bit de error activo), saltar
```

### Ejercicio 2
Explicar por qué es recomendable declarar como `volatile` en lenguaje C los punteros destinados a direccionar registros de Entrada/Salida mapeados en memoria (MMIO).

**Solución:**
El compilador de C realiza optimizaciones agresivas. Si observa un bucle de lectura como:
```c
while (*status_ptr == 0); // Esperar a que cambie el bit
```
El optimizador asumirá que, como el código del programa no modifica el valor apuntado por `status_ptr` dentro del bucle, el valor es constante. Por tanto, cargará el dato una sola vez en un registro de la CPU y entrará en un bucle infinito de CPU leyendo el registro interno, ignorando la memoria.
Al declarar el puntero como `volatile`:
`volatile int *status_ptr = (int*) 0x40002000;`
Le indicamos al compilador que la dirección apuntada puede ser modificada por un hardware externo ajeno al software. Esto obliga a la CPU a realizar un acceso físico real a la memoria RAM/bus en cada iteración, permitiendo leer el estado del periférico actualizado en tiempo real.

---

## 5. Ejercicios Propuestos

1.  Distingue entre el direccionamiento de Entrada/Salida **Mapeada en Memoria (MMIO)** y **Mapeada en Puertos (Isolated I/O)** en términos de espacio de direccionamiento e instrucciones máquina requeridas.
2.  Un sensor de temperatura digital expone un registro de datos de 16 bits en la dirección `0x4000F000`. Escribe la instrucción de ensamblador ARM para cargar dicha lectura de datos sabiendo que los buses de datos de ARM son de 32 bits.
3.  Investiga el papel del bit `READY` y del bit `BUSY` en un periférico. ¿Por qué es fundamental que la Unidad de Control del dispositivo coordine estos estados de forma independiente al procesador central?
