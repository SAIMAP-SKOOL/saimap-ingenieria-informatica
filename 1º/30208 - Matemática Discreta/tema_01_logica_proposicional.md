# Tema 1: Lógica Proposicional

La lógica proposicional es el sistema formal más simple para modelar y razonar sobre la veracidad de enunciados. Representa la base del diseño de hardware (puertas lógicas) y de la lógica condicional en la programación. Su objetivo principal es determinar si un razonamiento o argumento es formalmente válido abstrayéndose del contenido real del lenguaje natural.

---

## 1. Proposiciones y Conectores Lógicos

Una **proposición** es una declaración declarativa que es inequívocamente **verdadera ($V$)** o **falsa ($F$)**, pero no ambas cosas a la vez.
*   *Son proposiciones*: "El número 2 es primo", "Hoy es martes", "La memoria RAM almacena datos de forma volátil".
*   *No son proposiciones*: "¿Qué hora es?" (pregunta), "x + 2 = 5" (es una variable libre sin contexto, hasta que se defina $x$, es una función proposicional), "¡Cierra la puerta!" (orden).

### Conectores Lógicos
Para construir proposiciones complejas a partir de proposiciones simples (atómicas), se usan conectores lógicos:

| Conector | Nombre | Símbolo | Significado en lenguaje natural |
| :--- | :--- | :--- | :--- |
| **Negación** | NOT | $\neg P$ | "No $P$" o "Es falso que $P$". |
| **Conjunción** | AND | $P \land Q$ | "$P$ y $Q$". Ambos deben ser verdaderos. |
| **Disyunción** | OR | $P \lor Q$ | "$P$ o $Q$". Al menos uno debe ser verdadero. |
| **Condicional** | Implicación | $P \to Q$ | "Si $P$, entonces $Q$". $P$ es antecedente, $Q$ es consecuente. |
| **Bicondicional** | Equivalencia | $P \leftrightarrow Q$ | "$P$ si y solo si $Q$". Ambos deben tener el mismo valor de verdad. |

---

## 2. Semántica y Tablas de Verdad

La semántica de una fórmula proposicional define su valor de verdad en función de los valores de sus variables de entrada. Esto se representa mediante una **tabla de verdad**:

| $P$ | $Q$ | $\neg P$ | $P \land Q$ | $P \lor Q$ | $P \to Q$ | $P \leftrightarrow Q$ |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| $V$ | $V$ | $F$ | $V$ | $V$ | $V$ | $V$ |
| $V$ | $F$ | $F$ | $F$ | $V$ | $F$ | $F$ |
| $F$ | $V$ | $V$ | $F$ | $V$ | $V$ | $F$ |
| $F$ | $F$ | $V$ | $F$ | $F$ | $V$ | $V$ |

> [!WARNING]
> **La implicación ($P \to Q$)**: Es falsa únicamente cuando el antecedente $P$ es verdadero y el consecuente $Q$ es falso. Si $P$ es falso, la implicación siempre es verdadera (principio de "vacuidad").

---

## 3. Tautologías, Contradicciones y Contingencias

Una proposición compuesta se clasifica según los resultados de su última columna en la tabla de verdad:
1.  **Tautología**: Es verdadera para cualquier combinación de valores de verdad de sus variables atómicas. Se denota como $\mathbf{T}$ o $1$.
2.  **Contradicción**: Es falsa para cualquier combinación de valores de verdad. Se denota como $\mathbf{C}$ o $0$.
3.  **Contingencia**: Su valor de verdad depende de los valores de las variables (contiene tanto $V$ como $F$).

---

## 4. Leyes de Equivalencia Lógica

Dos fórmulas proposicionales $A$ y $B$ son **lógicamente equivalentes** ($A \equiv B$) si tienen la misma tabla de verdad. Esto nos permite simplificar fórmulas proposicionales complejas algebraicamente:

*   **Leyes de De Morgan**:
    $$\neg(P \land Q) \equiv \neg P \lor \neg Q$$
    $$\neg(P \lor Q) \equiv \neg P \land \neg Q$$
*   **Ley de la Implicación (Condicional)**:
    $$P \to Q \equiv \neg P \lor Q$$
*   **Leyes Distributivas**:
    $$P \land (Q \lor R) \equiv (P \land Q) \lor (P \land R)$$
    $$P \lor (Q \land R) \equiv (P \lor Q) \land (P \lor R)$$
*   **Leyes de Absorción**:
    $$P \land (P \lor Q) \equiv P$$
    $$P \lor (P \land Q) \equiv P$$

---

## 5. El Toque Informático

### Conexión con las Expresiones Booleanas y Evaluación en Cortocircuito
En lenguajes de programación como Python, C++ o Java, las expresiones condicionales (`if (P && Q)`) utilizan exactamente la lógica proposicional. Sin embargo, los compiladores aplican la **evaluación en cortocircuito** (short-circuit evaluation) para optimizar la ejecución:
*   En `P && Q` (conjunción), si `P` es falso, el programa no evalúa `Q` (pues el resultado final será falso independientemente del valor de `Q`).
*   En `P || Q` (disyunción), si `P` es verdadero, no se evalúa `Q` (pues el resultado final ya es verdadero de forma segura).

Esto evita fallos en tiempo de ejecución. Por ejemplo, en `if (ptr != nullptr && ptr->val == 5)`, si el puntero `ptr` es nulo, la primera parte es falsa y la segunda parte (que daría un error de violación de acceso de puntero nulo) nunca se ejecuta.

A continuación, implementamos en Python un generador automático de tablas de verdad para evaluar expresiones proposicionales binarias.

```python
# Definimos los conectores lógicos como funciones
def and_op(p, q): return p and q
def or_op(p, q): return p or q
def imp_op(p, q): return (not p) or q
def bic_op(p, q): return p == q

# Expresión a evaluar: ¬(P ∧ Q) ↔ (¬P ∨ ¬Q)
def evaluar_expresion(p, q):
    left = not and_op(p, q)
    right = or_op(not p, not q)
    return bic_op(left, right)

# Impresión de la Tabla de verdad
print("|  P  |  Q  | ¬(P ∧ Q) | ¬P ∨ ¬Q | Resultado Bicondicional |")
print("|-----|-----|----------|---------|-------------------------|")
for p in [True, False]:
    for q in [True, False]:
        v_left = not (p and q)
        v_right = (not p) or (not q)
        res = v_left == v_right
        
        # Mapeamos True/False a V/F para la visualización
        to_char = lambda x: 'V' if x else 'F'
        print(f"|  {to_char(p)}  |  {to_char(q)}  |    {to_char(v_left)}     |    {to_char(v_right)}    |            {to_char(res)}            |")
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Demostrar mediante álgebra lógica que la expresión $(P \land Q) \lor (P \land \neg Q)$ es equivalente a $P$.

**Solución:**
Aplicamos las leyes de equivalencia lógica paso a paso:
1.  **Ley Distributiva** (factor común $P$ con conjunción $\land$):
    $$(P \land Q) \lor (P \land \neg Q) \equiv P \land (Q \lor \neg Q)$$
2.  **Ley de la Negación (Tercero Excluido)** (sabemos que $Q \lor \neg Q$ es siempre verdadero):
    $$Q \lor \neg Q \equiv \mathbf{T}$$
3.  **Ley de Identidad** (cualquier proposición en conjunción con una tautología conserva su valor original):
    $$P \land \mathbf{T} \equiv P$$

Por lo tanto, queda demostrado algebraicamente que $(P \land Q) \lor (P \land \neg Q) \equiv P$.

### Ejercicio 2
Construir la tabla de verdad de la proposición compuesta $(P \to Q) \land (Q \to P)$ y clasificarla.

**Solución:**
Evaluamos la expresión columna a columna para las cuatro combinaciones de variables:

1.  Para $P=V, Q=V$:
    *   $P \to Q = V$, $Q \to P = V \implies (V) \land (V) = V$.
2.  Para $P=V, Q=F$:
    *   $P \to Q = F$, $Q \to P = V \implies (F) \land (V) = F$.
3.  Para $P=F, Q=V$:
    *   $P \to Q = V$, $Q \to P = F \implies (V) \land (F) = F$.
4.  Para $P=F, Q=F$:
    *   $P \to Q = V$, $Q \to P = V \implies (V) \land (V) = V$.

La última columna contiene valores $\{V, F, F, V\}$, lo que significa que la proposición es una **contingencia** (coincide con la semántica del operador bicondicional $P \leftrightarrow Q$).

---

## 7. Ejercicios Propuestos

1.  Construye la tabla de verdad de la expresión proposicional $\neg(P \lor \neg Q) \to (P \land Q)$ y determina si es una tautología, contradicción o contingencia.
2.  Simplifica la expresión lógica $\neg(\neg P \land Q) \land (P \lor Q)$ aplicando las leyes de equivalencia proposicional detallando cada ley utilizada.
3.  Escribe una expresión lógica equivalente para el operador OR exclusivo (XOR), utilizando únicamente los conectores básicos de conjunción ($\land$), disyunción ($\lor$) y negación ($\neg$).
