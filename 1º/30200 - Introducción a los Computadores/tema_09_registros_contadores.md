# Tema 9: Aplicaciones Secuenciales: Registros y Contadores

Para procesar y mover datos de forma agregada (en bloques de 8, 16, 32 o 64 bits), los flip-flops individuales se agrupan formando estructuras secuenciales de propósito general. Las dos aplicaciones más importantes en la arquitectura de computadores son los **Registros de Desplazamiento** (utilizados para conversión y transmisión de datos) y los **Contadores** (utilizados para secuenciar operaciones y controlar el tiempo).

---

## 1. Registros de Almacenamiento y Desplazamiento

### 1.1 Registros de Almacenamiento
Consiste simplemente en $N$ Flip-Flops de tipo D compartiendo la misma señal de reloj ($CLK$) y una línea de habilitación común ($Enable$). Sirven para retener una palabra de datos binaria de forma temporal.

### 1.2 Registros de Desplazamiento (Shift Registers)
Son circuitos secuenciales donde los flip-flops se conectan en cascada, de forma que la salida de cada biestable es la entrada del siguiente. En cada flanco de reloj, la información se desplaza una posición a lo largo de la cadena.

```
       Registro de Desplazamiento de 3 bits (SIPO)
           +-----+       +-----+       +-----+
 Entrada --| D Q |---+---| D Q |---+---| D Q |---- Salida Serie
  Serie    |     |   |   |     |   |   |     |
           +-----+   |   +-----+   |   +-----+
              |      |      |      |      |
             Y0      +----- Y1     +----- Y2   (Salidas Paralelas)
```

Dependiendo de cómo se introduzcan y lean los datos, se clasifican en:
*   **SISO (Serial-In, Serial-Out)**: Entrada serie, salida serie.
*   **SIPO (Serial-In, Parallel-Out)**: Entrada serie, salidas paralelas.
*   **PISO (Parallel-In, Serial-Out)**: Entradas paralelas, salida serie.
*   **PIPO (Parallel-In, Parallel-Out)**: Entradas y salidas paralelas (el registro básico).

### Operación Aritmética por Desplazamiento
Los registros de desplazamiento son multiplicadores y divisores de altísima velocidad:
*   Desplazar un número binario **una posición a la izquierda** equivale a **multiplicarlo por 2**:
    $$00001010_2 (10_{10}) \ll 1 \implies 00010100_2 (20_{10})$$
*   Desplazar un número binario **una posición a la derecha** equivale a realizar una **división entera por 2**:
    $$00001010_2 (10_{10}) \gg 1 \implies 00000101_2 (5_{10})$$

---

## 2. Contadores

Un contador es un circuito secuencial que recorre una secuencia predeterminada de estados en respuesta a los pulsos de reloj. El número de estados distintos de la secuencia es el **Módulo ($M$)** del contador.

### 2.1 Contadores Asíncronos (Contadores de Rizado)
El reloj solo se conecta al primer flip-flop. Las etapas posteriores se disparan utilizando como señal de reloj la salida de la etapa anterior.
*   *Inconveniente*: Se acumulan los retardos de conmutación de cada etapa ($O(N)$). Durante el cambio, se producen estados intermedios transitorios erróneos en las salidas. No son recomendables para altas frecuencias.

### 2.2 Contadores Síncronos
Todos los flip-flops comparten exactamente la misma línea de reloj, por lo que conmutan simultáneamente en el mismo flanco. Para determinar cuándo debe cambiar cada etapa, se diseña una red lógica combinacional conectada a las entradas de control (J, K o T) de los flip-flops.

---

## 3. El Toque Informático

### La UART y los Puertos USB/COM
Los microprocesadores procesan la información de forma **paralela** en buses anchos (por ejemplo, leyendo 64 bits a la vez en su memoria caché). Sin embargo, enviar 64 cables físicos a través de una red o a un periférico externo (como una impresora o un disco externo) es físicamente costoso y produce interferencias electromagnéticas destructivas.
*   Para solucionarlo se utiliza un chip llamado **UART (Universal Asynchronous Receiver-Transmitter)**.
*   La UART toma la palabra paralela del bus del sistema, la carga en un registro de desplazamiento de tipo **PISO** y la transmite bit a bit en serie a través de un único cable (como el puerto USB).
*   En el receptor, otra UART recoge los bits serie mediante un registro de desplazamiento **SIPO**, reconstruye la palabra paralela original y la introduce en el bus del computador destino.

A continuación, implementamos en Python una simulación lógica de un registro de desplazamiento de 8 bits que demuestra el desplazamiento aritmético de multiplicación y división binaria.

```python
class RegistroDesplazamiento8Bits:
    def __init__(self, valor_inicial=0):
        # Almacenamos el registro como una lista de 8 bits (MSB a la izquierda)
        self.bits = [(valor_inicial >> i) & 1 for i in reversed(range(8))]
        
    def shift_left(self, bit_entrada=0):
        # Desplazamiento a la izquierda (Multiplicación por 2)
        # El bit que sale a la izquierda (MSB) se descarta, el bit_entrada entra por la derecha (LSB)
        self.bits.pop(0)
        self.bits.append(bit_entrada)
        return self.obtener_valor()
        
    def shift_right(self, bit_entrada=0):
        # Desplazamiento a la derecha (División por 2)
        # El bit que sale a la derecha (LSB) se descarta, el bit_entrada entra por la izquierda (MSB)
        self.bits.pop()
        self.bits.insert(0, bit_entrada)
        return self.obtener_valor()
        
    def obtener_valor(self):
        val = 0
        for b in self.bits:
            val = (val << 1) | b
        return val
        
    def __str__(self):
        return "".join(map(str, self.bits))

# Simulación
reg = RegistroDesplazamiento8Bits(10) # Inicializamos con 10 decimal (00001010)
print(f"Estado inicial:       {reg} (Decimal: {reg.obtener_val = reg.obtener_value if hasattr(reg, 'obtener_value') else reg.obtener_valor()})")

reg.shift_left(0)
print(f"Desplazamiento Izq:   {reg} (Decimal: {reg.obtener_valor()}) -> Multiplicado por 2")

reg.shift_right(0)
reg.shift_right(0)
print(f"Dos Desplazamientos Der: {reg} (Decimal: {reg.obtener_valor()}) -> Dividido por 4")
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Diseñar un contador síncrono Gray de 2 bits ($00 \to 01 \to 11 \to 10 \to 00 \dots$) utilizando Flip-Flops D.

**Solución:**
1.  **Tabla de Transición (Siguiente Estado)**:

    | Estado Actual $Q_1Q_0$ | Estado Siguiente $Q_{1\text{,next}}Q_{0\text{,next}}$ | Entradas $D_1D_0$ |
    | :---: | :---: | :---: |
    | 00 | 01 | 01 |
    | 01 | 11 | 11 |
    | 11 | 10 | 10 |
    | 10 | 00 | 00 |

2.  **Deducir ecuaciones lógicas para las entradas $D_1$ y $D_0$**:
    *   **Para $D_1$** (filas con salida 1): se activa para $Q_1Q_0 = 01$ y $Q_1Q_0 = 11$.
        $$D_1 = \bar{Q}_1Q_0 + Q_1Q_0 = Q_0(\bar{Q}_1 + Q_1) = Q_0$$
    *   **Para $D_0$** (filas con salida 1): se activa para $Q_1Q_0 = 00$ y $Q_1Q_0 = 01$.
        $$D_0 = \bar{Q}_1\bar{Q}_0 + \bar{Q}_1Q_0 = \bar{Q}_1(\bar{Q}_0 + Q_0) = \bar{Q}_1$$
3.  **Resultado**:
    El circuito consta de 2 Flip-Flops D:
    *   Conectamos la salida $Q_0$ del segundo flip-flop directamente a la entrada $D_1$ del primero.
    *   Conectamos la salida negada $\bar{Q}_1$ del primer flip-flop a la entrada $D_0$ del segundo.

El circuito recorrerá la secuencia Gray síncronamente al ritmo del reloj.

### Ejercicio 2
Explicar el funcionamiento de un contador Johnson de 3 bits y listar la secuencia de estados que recorre si parte del estado inicial $000_2$.

**Solución:**
Un contador Johnson se construye conectando la salida negada del último flip-flop a la entrada $D$ del primer flip-flop de un registro de desplazamiento:
$$D_0 = \bar{Q}_2, \quad D_1 = Q_0, \quad D_2 = Q_1$$

1.  Estado inicial: $000$ (salida negada del último es $1$).
2.  Flanco 1: entra $1 \implies 100$.
3.  Flanco 2: entra $1 \implies 110$.
4.  Flanco 3: entra $1 \implies 111$ (ahora la salida del último es 1, por lo que entra su negada $0$).
5.  Flanco 4: entra $0 \implies 011$.
6.  Flanco 5: entra $0 \implies 001$.
7.  Flanco 6: entra $0 \implies 000$ (vuelve al estado inicial).

La secuencia consta de 6 estados distintos ($2N$ estados para $N$ bits).

---

## 5. Ejercicios Propuestos

1.  Dibuja el esquema lógico de un contador asíncrono binario de 3 bits utilizando Flip-Flops JK configurados en modo conmutación ($J=K=1$).
2.  Diseña la lógica de reinicio (Reset) para convertir un contador binario síncrono de 4 bits en un contador de módulo 10 (contador BCD, de $0$ a $9$).
3.  Explica cómo realiza un registro de desplazamiento bidireccional la selección entre desplazamiento a la izquierda y a la derecha utilizando multiplexores en sus entradas $D$.
