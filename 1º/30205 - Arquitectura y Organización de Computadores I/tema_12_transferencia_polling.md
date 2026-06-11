# Tema 12: Métodos de Transferencia: Polling o Consulta de Estado

Una vez que el procesador conoce la estructura de registros de un periférico, debe aplicar un método de sincronización para transferir datos de forma ordenada. El método más simple y directo es la **consulta de estado o espera activa (Polling)**.

---

## 1. El Algoritmo de Polling (Espera Activa)

En la Entrada/Salida por Polling, el procesador asume todo el control de la sincronización. La CPU ejecuta de forma continuada un bucle cerrado de lectura del registro de estado del controlador del dispositivo para comprobar si este se encuentra listo para transferir un dato.

### Flujo de Operación para Transmisión
1.  **Lectura del Estado**: La CPU lee la dirección de memoria del registro de estado (`STATUS_REG`).
2.  **Máscara y Comprobación**: Aísla el bit `READY` (o el equivalente de transmisión vacía `TxEmpty`) mediante una operación lógica `AND`.
3.  **Bucle de Espera Activa (Busy Waiting)**:
    *   Si `READY = 0`: El periférico está ocupado. La CPU salta de vuelta al paso 1, entrando en un bucle de espera continua.
    *   Si `READY = 1`: El periférico está listo para recibir un nuevo dato.
4.  **Escritura del Dato**: La CPU escribe el dato correspondiente en el registro de datos (`DATA_REG`), lo que inicia la transferencia física y hace que el controlador ponga el bit `READY = 0`.

### Esquema de código en Ensamblador ARM
Asumiendo: `R0` = Dirección de `STATUS_REG`, `R1` = Dirección de `DATA_REG`, `R2` = Dato a transmitir.
```assembly
espera_activa:
        LDR R3, [R0]        ; 1. Leer STATUS_REG
        TST R3, #0x01       ; 2. Comprobar bit 0 (READY)
        BEQ espera_activa   ; 3. Si READY == 0, seguir esperando en bucle
        STR R2, [R1]        ; 4. Si READY == 1, escribir dato en DATA_REG
```

---

## 2. Análisis de Rendimiento y Sobrecarga de la CPU

Aunque la implementación por Polling es conceptualmente sencilla y requiere muy poca complejidad de hardware, presenta serias deficiencias de eficiencia en sistemas multitarea:

### Ventajas
*   **Mínima latencia de respuesta**: Como el procesador está monitorizando continuamente el periférico, detecta el cambio de estado de forma casi instantánea y transfiere el dato sin apenas retardo.
*   **Sencillez de diseño**: No requiere circuitería especial de interrupción en la placa base ni controladores de interrupciones complejos.

### Desventajas
*   **Inutilización total de la CPU**: Mientras espera que un periférico lento (como un teclado o un puerto de red) esté listo, la CPU consume el $100\%$ de sus ciclos de reloj ejecutando un bucle infructuoso, impidiendo realizar cualquier otro procesamiento útil de fondo.
*   **Ineficiencia a escala**: Si el sistema tiene múltiples periféricos, realizar Polling a cada uno de ellos (round-robin polling) añade latencia y dificulta cumplir los requisitos de tiempo real de los dispositivos.

---

## 3. El Toque Informático

### Simulación de un Bucle de Polling para Envío de Cadenas
El siguiente script en Python simula el bucle completo de espera activa de la CPU mientras transmite una cadena de texto a través del controlador de un puerto serie (UART). Muestra cómo se consume tiempo de procesamiento en espera de que el hardware esté disponible:

```python
import time

class UARTPeriferico:
    def __init__(self):
        self.ready = True
        
    def transmitir_byte(self, char):
        # Simular retardo físico de transmisión por hardware
        self.ready = False
        print(f"\n[Hardware]: Iniciando transmisión de '{char}'...")
        time.sleep(0.1)  # Retardo físico
        self.ready = True
        print(f"[Hardware]: Transmisión completa de '{char}'. UART lista.")

# Simulación de la CPU haciendo Polling
uart_dispositivo = UARTPeriferico()
mensaje = "ARM"

print("Iniciando transmisión de cadena mediante Polling...")
for caracter in mensaje:
    ciclos_de_espera = 0
    # Bucle de Espera Activa (Polling)
    while not uart_dispositivo.ready:
        ciclos_de_espera += 1
        # La CPU está bloqueada aquí consumiendo ciclos
        pass
        
    print(f"[CPU]: UART lista detectada (esperó {ciclos_de_espera} ciclos de software).")
    # Enviar el dato
    uart_dispositivo.transmitir_byte(caracter)
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Diseñar una subrutina en ensamblador ARM llamada `enviar_cadena` que transmita una secuencia de caracteres (cadena de caracteres terminada en cero, formato ASCIIZ) cuya dirección de memoria inicial está en `R2`, utilizando Polling. Las direcciones de los registros son:
*   `STATUS_REG` = `0x40003000` (Bit 0: `READY`).
*   `DATA_REG`   = `0x40003004`.

**Solución:**
```assembly
        .text
        .global enviar_cadena

enviar_cadena:
        PUSH {R4, R5, LR}     ; Guardar contexto y retorno
        LDR R4, =0x40003000   ; R4 = Dirección de STATUS_REG
        LDR R5, =0x40003004   ; R5 = Dirección de DATA_REG

bucle_caracter:
        LDRB R0, [R2], #1     ; Cargar carácter actual y avanzar puntero de cadena
        CMP R0, #0            ; Comprobar si es el fin de la cadena (carácter nulo)
        BEQ fin_transmision   ; Si R0 == 0, terminar

esperar_uart:
        LDR R3, [R4]          ; Leer STATUS_REG
        TST R3, #0x01         ; Comprobar bit 0 (READY)
        BEQ esperar_uart      ; Si es 0 (ocupado), seguir haciendo polling
        
        STRB R0, [R5]         ; Si es 1 (listo), escribir carácter en DATA_REG
        B bucle_caracter      ; Procesar siguiente carácter

fin_transmision:
        POP {R4, R5, PC}      ; Restaurar registros y retornar
```

---

## 5. Ejercicios Propuestos

1.  Dada una tasa de transmisión de red de $10.000$ bytes por segundo. Si la CPU ejecuta un bucle de Polling donde cada lectura de estado tarda $1 \, \mu\text{s}$, calcula cuántos ciclos de consulta estériles realiza la CPU en promedio entre la llegada de cada byte de datos.
2.  Propón una alternativa de diseño de hardware o software para evitar que la CPU quede bloqueada en un bucle de espera activa mientras aguarda a que un usuario pulse una tecla (dispositivo extremadamente lento).
3.  Explica qué es el efecto de *Live-lock* en E/S por consulta de estado y en qué condiciones puede llegar a congelar por completo la ejecución de procesos prioritarios del sistema operativo.
