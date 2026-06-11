# Tema 2: Inferencia y Deducción Lógica

La lógica proposicional no solo sirve para evaluar fórmulas estáticas, sino para construir y validar razonamientos dinámicos. Un argumento lógico es un conjunto de premisas que pretenden justificar una conclusión. Mediante las **reglas de inferencia**, podemos realizar derivaciones formales paso a paso para verificar rigurosamente la validez de un razonamiento sin recurrir a costosas tablas de verdad.

---

## 1. El Concepto de Argumento Válido

Un **argumento** es una secuencia de proposiciones compuestas por una o más **premisas** ($P_1, P_2, \dots, P_n$) y una **conclusión** ($Q$). Se representa formalmente como:
$$(P_1 \land P_2 \land \dots \land P_n) \to Q$$

*   **Validez**: Un argumento es **válido** si y solo si la implicación anterior es una tautología. En términos prácticos: si todas las premisas son verdaderas, es imposible que la conclusión sea falsa.
*   **Verdad vs. Validez**: La validez es una propiedad formal de la estructura del razonamiento, no del contenido empírico. Un argumento puede ser perfectamente válido aunque sus premisas y su conclusión sean fácticamente falsas (por ejemplo: "Todos los informáticos son marcianos. Juan es informático. Por tanto, Juan es marciano").

---

## 2. Reglas de Inferencia Básicas

Las reglas de inferencia son tautologías fundamentales expresadas de forma simplificada que nos permiten deducir proposiciones nuevas (conclusiones parciales) a partir de proposiciones conocidas (premisas).

### 2.1 Modus Ponens (Afirmación del Antecedente)
Si una implicación y su antecedente son verdaderos, el consecuente también es verdadero:
$$\frac{P \to Q, \quad P}{Q}$$

### 2.2 Modus Tollens (Negación del Consecuente)
Si una implicación es verdadera y su consecuente es falso, el antecedente es falso:
$$\frac{P \to Q, \quad \neg Q}{\neg P}$$

### 2.3 Silogismo Hipotético (Regla de Transitividad)
Permite encadenar implicaciones:
$$\frac{P \to Q, \quad Q \to R}{P \to R}$$

### 2.4 Silogismo Disyuntivo
Si tenemos una disyuntiva y uno de los términos es falso, el otro debe ser verdadero:
$$\frac{P \lor Q, \quad \neg P}{Q}$$

### 2.5 Regla de Resolución
Es la regla fundamental en la que se basan los demostradores automáticos de teoremas en inteligencia artificial:
$$\frac{P \lor Q, \quad \neg P \lor R}{Q \lor R}$$

---

## 3. Deducción Natural y Derivaciones Formales

Una **derivación formal** consiste en escribir una lista de proposiciones numeradas, donde cada una de ellas es una premisa original o se ha obtenido aplicando una regla de inferencia a las líneas anteriores. La última línea de la derivación debe ser la conclusión deseada.

---

## 4. El Toque Informático

### Programación Lógica (Prolog) y Motores de Inferencia
En ingeniería informática, las reglas de inferencia se automatizan para crear sistemas capaces de "pensar". El exponente más claro es **Prolog** (Programming in Logic), un lenguaje declarativo donde se definen hechos y reglas lógicas, y el motor de inferencia (resolución) deduce la veracidad de las consultas mediante encadenamiento hacia atrás (backtracking).

Por ejemplo, definimos en Prolog:
```prolog
humano(juan).
mortal(X) :- humano(X).
```
Si preguntamos `?- mortal(juan).`, Prolog busca en su base de conocimiento, aplica una unificación equivalente a un Modus Ponens y responde `true`.

A continuación, implementamos en Python un motor de inferencia simple basado en Modus Ponens para deducir nuevos hechos a partir de una lista de reglas conocidas.

```python
# Base de conocimientos: hechos iniciales verdaderos
hechos = {"compilador_optimiza", "código_bien_diseñado"}

# Reglas: representadas como tuplas (antecedentes_lista, consecuente)
# Si todos los antecedentes son verdaderos, deducimos el consecuente
reglas = [
    (["compilador_optimiza", "código_bien_diseñado"], "ejecución_rápida"),
    (["ejecución_rápida"], "usuario_satisfecho")
]

print("Hechos iniciales:", hechos)
nuevo_hecho_deducido = True

# Ciclo del motor de inferencia (encadenamiento hacia adelante)
while nuevo_hecho_deducido:
    nuevo_hecho_deducido = False
    for antecedentes, consecuente in reglas:
        if consecuente not in hechos:
            # Comprobamos si todos los antecedentes de la regla están en los hechos conocidos
            if all(ant in hechos for ant in antecedentes):
                print(f"Deduciendo: Como {antecedentes} son verdaderos -> {consecuente} es verdadero (Modus Ponens)")
                hechos.add(consecuente)
                nuevo_hecho_deducido = True

print("Base de conocimientos final:", hechos)
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Demostrar la validez del siguiente argumento lógico mediante deducción natural (justificando la regla y las líneas implicadas en cada paso):
*   Premisa 1: $P \to Q$
*   Premisa 2: $\neg Q$
*   Premisa 3: $\neg P \to R$
*   Conclusión: $R$

**Solución:**
Escribimos la derivación formal paso a paso:

1.  $P \to Q$ (Premisa)
2.  $\neg Q$ (Premisa)
3.  $\neg P \to R$ (Premisa)
4.  $\neg P$ (Modus Tollens aplicado a las líneas 1 y 2)
5.  $R$ (Modus Ponens aplicado a las líneas 3 y 4)

La última línea coincide con la conclusión buscada ($R$), por lo que el razonamiento queda demostrado formalmente como **válido**.

### Ejercicio 2
Analizar el siguiente razonamiento e identificar si es válido o si incurre en alguna falacia:
"Si un programa tiene un bucle infinito, entonces la CPU se calienta. La CPU se calienta. Por lo tanto, el programa tiene un bucle infinito."

**Solución:**
1.  Modelamos el argumento:
    *   $P$: El programa tiene un bucle infinito.
    *   $Q$: La CPU se calienta.
    *   Premisa 1: $P \to Q$
    *   Premisa 2: $Q$
    *   Conclusión: $P$
2.  Evaluamos la validez formal:
    El razonamiento se resume como $\frac{P \to Q, \quad Q}{P}$. Esta estructura es inválida. En lógica formal se conoce como la **Falacia de Afirmación del Consecuente**. 
    
    Físicamente, la CPU puede calentarse por múltiples razones ajenas a un bucle infinito (por ejemplo, procesamiento de renderizado 3D complejo, ventilación defectuosa o sobrecalentamiento del entorno). Por lo tanto, aunque las premisas sean verdaderas, la conclusión no es necesariamente verdadera. El razonamiento es **inválido**.

---

## 6. Ejercicios Propuestos

1.  Demuestra la validez del siguiente argumento lógico mediante derivación formal paso a paso:
    *   Premisas: $A \to \neg B$, $C \to B$, $A \land D$
    *   Conclusión: $\neg C$
2.  Dadas las premisas $P \lor Q$, $P \to R$, y $\neg Q$, deduce formalmente la conclusión $R$.
3.  Investiga y explica la diferencia en lógica computacional entre el **encadenamiento hacia adelante** (forward chaining) y el **encadenamiento hacia atrás** (backward chaining) y cómo aplican estos conceptos las bases de datos deductivas.
