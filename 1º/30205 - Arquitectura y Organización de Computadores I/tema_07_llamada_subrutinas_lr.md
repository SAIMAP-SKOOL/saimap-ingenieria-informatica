# Tema 7: El Mecanismo de Llamada a Subrutinas y el Link Register (LR)

La modularidad es un principio básico del desarrollo de software que consiste en dividir un programa complejo en subprogramas reutilizables denominados funciones, procedimientos o subrutinas. A nivel de hardware, esto requiere de un mecanismo de salto bidireccional: la CPU debe saltar al inicio de la subrutina y, al finalizar su ejecución, debe retornar exactamente a la instrucción posterior a la llamada.

---

## 1. El Protocolo de Llamada y Retorno: La Instrucción BL y el Registro LR

Para realizar la llamada a una subrutina, ARM dispone de la instrucción dedicada **BL (Branch with Link)**.
*   **Funcionamiento de `BL subrutina`**:
    1.  El procesador guarda de forma automática en el **Link Register (LR / R14)** la dirección de retorno (la dirección de la instrucción inmediatamente posterior a la llamada `BL`).
    2.  Actualiza el **Program Counter (PC / R15)** con la dirección de la subrutina para transferir el control de ejecución.
*   **Retorno de la subrutina**:
    Para regresar, la subrutina debe transferir el valor almacenado en `LR` de vuelta al contador de programa `PC`. Se puede hacer mediante dos instrucciones equivalentes:
    *   `MOV PC, LR`
    *   `BX LR` (Branch and Exchange, preferible porque permite interconexión Thumb).

---

## 2. El Problema de las Subrutinas Anidadas

Una subrutina se denomina **"Hoja" (Leaf Subroutine)** si no realiza llamadas a ninguna otra subrutina. En este caso, puede mantener el registro `LR` intacto durante toda su ejecución y retornar directamente usándolo.

Sin embargo, cuando una subrutina llama a otra (subrutina **"No Hoja"** o llamadas anidadas), ocurre una sobreescritura de `LR`:

```
 Principal:
    BL FuncionA  --> [LR = Dirección Retorno a Principal]
                      Jumps to FuncionA...

 FuncionA:
    ...
    BL FuncionB  --> [LR = Dirección Retorno a FuncionA (¡Sobreescrito!)]
                      Jumps to FuncionB...

 FuncionB:
    ...
    BX LR        --> Retorna a FuncionA (Carga LR en PC)

 FuncionA:
    ...
    BX LR        --> ¡Bucle infinito! (Intenta retornar a FuncionA porque LR fue sobreescrito)
```

### La Solución: Preservación de LR en el Stack (Pila)
Para evitar la pérdida de la dirección de retorno a la función superior, toda subrutina no hoja debe salvar `LR` al entrar en la pila y recuperarlo al salir:

```assembly
FuncionA:
    PUSH {LR}      ; Guardar LR en la pila al entrar (Inicio de la función)
    ...
    BL FuncionB    ; LR se sobrescribe con el retorno a FuncionA
    ...
    POP {PC}       ; Extrae el valor guardado de la pila directamente a PC
                   ; (Equivale a un POP de LR y luego MOV PC, LR, ahorrando un ciclo)
```

---

## 3. El Toque Informático

### Simulador del Flujo de Ejecución y Link Register
El siguiente script en Python simula el contador de programa `PC` y el registro `LR` de una CPU durante una secuencia de llamadas anidadas, ilustrando la corrupción del retorno si no se aplica la preservación en el stack:

```python
class CallStackSimulator:
    def __init__(self):
        self.pc = 0x0100
        self.lr = 0x0000
        self.stack = []
        
    def llamar_subrutina(self, destino_addr, nombre, preservar=False):
        retorno_addr = self.pc + 4
        self.lr = retorno_addr
        if preservar:
            self.stack.append(retorno_addr)
            print(f"Llamando a {nombre}: Guardado retorno 0x{retorno_addr:04X} en la Pila.")
        else:
            print(f"Llamando a {nombre}: LR actualizado con retorno 0x{retorno_addr:04X} (SIN PILA).")
        self.pc = destino_addr
        
    def retornar_subrutina(self, usar_pila=False):
        if usar_pila:
            if len(self.stack) > 0:
                retorno = self.stack.pop()
                self.pc = retorno
                print(f"Retornando desde Pila: PC actualizado a 0x{self.pc:04X}")
            else:
                print("[ERROR] Pila vacía. Crash del sistema.")
        else:
            self.pc = self.lr
            print(f"Retornando desde LR:   PC actualizado a 0x{self.pc:04X}")

# Simulación de llamadas anidadas correctas (usando la Pila)
sim = CallStackSimulator()
print("--- SIMULACIÓN DE LLAMADAS CORRECTAS ---")
sim.llamar_subrutina(0x2000, "FuncionA", preservar=True)
sim.pc = 0x2010  # Avance de programa en FuncionA
sim.llamar_subrutina(0x3000, "FuncionB", preservar=True)
sim.pc = 0x3008  # Fin de FuncionB
sim.retornar_subrutina(usar_pila=True)  # Retorna a FuncionA
sim.pc = 0x2018  # Fin de FuncionA
sim.retornar_subrutina(usar_pila=True)  # Retorna a Principal
print("-" * 55)
```

---

## 4. Ejercicios Resueltos

### Ejercicio 1
Escribir el esqueleto básico en ensamblador ARM para una subrutina llamada `CalcularFactorial` que a su vez llama internamente a una subrutina multiplicadora `MultiplicarEnteros`, aplicando el protocolo de llamada y retorno seguro.

**Solución:**
```assembly
CalcularFactorial:
    PUSH {R4, LR}       ; Preservamos registros de trabajo y el registro LR
    ; ... cuerpo de la función ...
    ; aquí preparamos argumentos en R0 y R1
    BL MultiplicarEnteros ; Llamada interna (sobreescribe LR)
    ; R0 contiene el resultado del factorial temporal
    ; ... más procesamiento ...
    POP {R4, PC}        ; Restauramos R4 y cargamos la dirección de retorno guardada en PC
```

### Ejercicio 2
Explicar por qué es posible retornar de una subrutina hoja utilizando `BX LR` sin necesidad de realizar accesos a la pila de memoria RAM.

**Solución:**
Dado que la subrutina hoja no realiza llamadas internas a otras funciones, el hardware de la CPU no modifica el valor inicial de `LR` durante toda la ejecución del subprograma. Por lo tanto, el valor correcto de la dirección de retorno a la función llamadora permanece intacto en el registro `LR`. 
Al ser `LR` un registro interno del procesador, retornar mediante `BX LR` evita accesos costosos a la memoria RAM (lecturas del stack), ejecutando la vuelta de forma instantánea.

---

## 5. Ejercicios Propuestos

1.  Dada la instrucción `BL etiqueta`, indica en qué dirección de memoria exacta se calcula el retorno si la instrucción `BL` se sitúa en la posición `0x00008000`.
2.  Un programador escribe una subrutina recursiva. Describe detalladamente qué ocurrirá con la pila de memoria (Stack) y con el registro `LR` si se olvida de implementar la salvaguarda de `LR` en el prólogo de la función.
3.  Investiga el estándar EABI/AAPCS de ARM. ¿Qué registros se consideran volátiles (caller-saved) y cuáles deben ser obligatoriamente restaurados por la subrutina al finalizar (callee-saved)?
