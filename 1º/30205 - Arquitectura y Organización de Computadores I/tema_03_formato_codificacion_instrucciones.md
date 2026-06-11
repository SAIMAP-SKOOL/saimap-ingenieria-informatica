# Tema 3: Formato y Codificación de Instrucciones Máquina en ARM

Una de las características clave de la arquitectura RISC de ARM de 32 bits es la regularidad en el formato de sus instrucciones. Para permitir una decodificación veloz mediante hardware, todas las instrucciones se codifican en palabras de longitud fija de exactamente **32 bits** (4 bytes).

---

## 1. El Formato General de una Instrucción ARM

La Unidad de Control del procesador interpreta los bits de una instrucción dividiéndolos en campos específicos. El formato básico para las instrucciones de procesamiento de datos es:

```
 Bit  31    28 27  26 25 24    21 20 19    16 15    12 11            0
     [  cond  | 0 | 0| I|  opcode | S|   Rn   |   Rd   |   Operand2   ]
```

### Descripción Detallada de los Campos

1.  **cond (Bits 31-28)**: Campo de Condición. Permite la ejecución condicional de casi cualquier instrucción en ARM.
    *   `1110` (0xE): Ejecución Incondicional (`AL` - *Always*).
    *   `0000` (0x0): Ejecución si es Igual (`EQ` - *Equal*).
    *   `0001` (0x1): Ejecución si es Diferente (`NE` - *Not Equal*).
2.  **Tipo de Instrucción (Bits 27-26)**: Para procesamiento de datos, estos bits siempre valen `00`.
3.  **I - Immediate flag (Bit 25)**:
    *   `I = 0`: El campo `Operand2` es un registro (y opcionalmente puede ser procesado por el Barrel Shifter).
    *   `I = 1`: El campo `Operand2` representa un valor inmediato (constante numérica).
4.  **opcode (Bits 24-21)**: Define la operación específica.
    *   `0100` (0x4): `ADD` (Suma)
    *   `0010` (0x2): `SUB` (Resta)
    *   `1101` (0xD): `MOV` (Mover/Copiar)
    *   `0000` (0x0): `AND` (Y lógica)
5.  **S (Bit 20)**: *Set Condition Codes*.
    *   `S = 0`: No modifica las banderas del registro CPSR.
    *   `S = 1`: Modifica las banderas (N, Z, C, V) en función del resultado.
6.  **Rn (Bits 19-16)**: Registro del primer operando fuente (4 bits permiten codificar de R0 a R15).
7.  **Rd (Bits 15-12)**: Registro de destino. Almacena el resultado de la operación.
8.  **Operand2 (Bits 11-0)**: Operando secundario. Ocupa 12 bits y su interpretación depende del bit `I`.

---

## 2. La Codificación de Valores Inmediatos (Operand2 con I=1)

El campo `Operand2` tiene solo 12 bits libres. Esto imposibilita representar cualquier constante de 32 bits de forma directa. ARM solventa esto dividiendo los 12 bits del operando inmediato en dos subcampos:
*   **Rotación (Bits 11-8)**: 4 bits para codificar un valor de rotación par (multiplicado por 2), dando un rango de rotación de 0 a 30 posiciones a la derecha.
*   **Valor Base (Bits 7-0)**: 8 bits para almacenar una constante entre 0 y 255.

La fórmula de decodificación de la constante final es:
$$\text{Constante} = \text{Valor Base} \text{ rotado a la derecha } (2 \cdot \text{Rotación}) \text{ bits}$$

Esto permite representar constantes útiles como potencias de dos, máscaras de bits comunes o múltiplos de página de forma eficiente en solo 12 bits de espacio de instrucción física.

---

## 3. El Toque Informático

### Compilador/Codificador de Instrucciones Ensamblador ARM
El siguiente script en Python simula el trabajo del ensamblador convirtiendo una cadena de texto de código ensamblador ARM simple a su representación en código de máquina de 32 bits (tanto en binario como en hexadecimal):

```python
def ensamblar_datos(inst_asm):
    # Ejemplo simple para parsear: "ADD R1, R2, #45" o "SUB R3, R4, #12"
    partes = inst_asm.replace(",", "").split()
    
    cond = "1110"  # AL (Siempre)
    tipo = "00"
    i_flag = "1"   # Usaremos inmediato para simplificar
    
    opcodes = {"ADD": "0100", "SUB": "0010", "MOV": "1101"}
    opcode = opcodes.get(partes[0], "0000")
    
    s_flag = "1" if partes[0].endswith("S") else "0"
    # Quitar la S para mapear el opcode básico
    basic_op = partes[0].rstrip("S")
    opcode = opcodes.get(basic_op, "0000")
    
    rd = bin(int(partes[1].replace("R", "")))[2:].zfill(4)
    rn = bin(int(partes[2].replace("R", "")))[2:].zfill(4)
    
    # Inmediato de 12 bits (asumiendo rotación 0 para valores < 256)
    val_inmediato = int(partes[3].replace("#", ""))
    operand2 = bin(val_inmediato)[2:].zfill(12)
    
    inst_binaria = cond + tipo + i_flag + opcode + s_flag + rn + rd + operand2
    inst_hex = f"{int(inst_binaria, 2):08X}"
    
    print(f"Línea Ensamblador: {inst_asm}")
    print(f"Campos Binarios:   [{cond}] [{tipo}] [{i_flag}] [{opcode}] [{s_flag}] [{rn}] [{rd}] [{operand2}]")
    print(f"Instrucción Binaria: {inst_binaria}")
    print(f"Código de Máquina: 0x{inst_hex}")
    print("-" * 65)

# Probar codificación
ensamblar_datos("ADD R1, R2, #45")
ensamblar_datos("SUB R3, R4, #12")
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Dada la instrucción máquina en hexadecimal `0xE2831005`, decodificar todos sus campos e identificar la instrucción ensamblador equivalente.

**Solución:**
1.  **Convertir Hexadecimal a Binario de 32 bits**:
    `0xE2831005` $\implies$ `1110 0010 1000 0011 0001 0000 0000 0101`
2.  **Segmentar los campos**:
    *   `cond` (bits 31-28): `1110` $\implies$ `AL` (Siempre/Incondicional).
    *   `tipo` (bits 27-26): `00` $\implies$ Procesamiento de datos.
    *   `I` (bit 25): `1` $\implies$ Operando inmediato.
    *   `opcode` (bits 24-21): `0100` $\implies$ `ADD`.
    *   `S` (bit 20): `0` $\implies$ No actualiza flags.
    *   `Rn` (bits 19-16): `0011` $\implies$ Registro `R3`.
    *   `Rd` (bits 15-12): `0001` $\implies$ Registro `R1`.
    *   `Operand2` (bits 11-0): `0000 0000 0101` $\implies$ Inmediato base `5` con rotación `0`.
3.  **Ensamblar la instrucción final**:
    `ADD R1, R3, #5`

### Ejercicio 2
Explicar por qué una instrucción como `ADD R0, R1, #300` no se puede codificar con rotación cero en el operando inmediato y cómo lo resuelve el ensamblador.

**Solución:**
El campo de valor base para el inmediato en `Operand2` tiene un límite de 8 bits (números del 0 al 255). Por lo tanto, el número $300$ es demasiado grande para caber de forma directa en el byte de base.
Para solventar esto, el ensamblador realiza una búsqueda de una rotación par adecuada:
*   $300$ en binario es `00000001 00101100`.
*   Podemos representarlo como el valor base `75` (binario `01001011`) rotado a la derecha por 30 bits (que equivale a una rotación de 2 bits a la izquierda).
*   En el código máquina final, la rotación guardada será $15$ (ya que la CPU multiplica la rotación por 2: $15 \times 2 = 30$ bits de rotación derecha) y el valor base será $75$.

---

## 5. Ejercicios Propuestos

1.  Determina qué instrucción en ensamblador se obtiene a partir del código máquina en hexadecimal `0xE04F2003`.
2.  Calcula el valor decimal de la constante representada en `Operand2` si los bits de rotación son `0010` (rotación base 2, real $2 \times 2 = 4$) y el valor base es `11110000` (decimal 240).
3.  Explica la ventaja estructural de que todas las instrucciones del procesador tengan exactamente el mismo tamaño (32 bits) frente a las instrucciones de tamaño variable en la arquitectura x86.
