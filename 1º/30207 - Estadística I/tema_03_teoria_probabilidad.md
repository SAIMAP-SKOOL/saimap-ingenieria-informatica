# Tema 3: Teoría de la Probabilidad

La probabilidad es la base matemática sobre la que se construye la inferencia estadística y el aprendizaje automático. Nos permite modelar y cuantificar la incertidumbre inherente a los sistemas físicos, de red y de software (como la tasa de pérdida de paquetes o la probabilidad de fallo de un servidor).

---

## 1. Experimento Aleatorio, Espacio Muestral y Sucesos

*   **Experimento Aleatorio**: Aquel cuyo resultado no se puede predecir con certeza antes de realizarse, aun cuando se repita bajo las mismas condiciones (por ejemplo, el tiempo exacto que tardará en completarse una petición de red).
*   **Espacio Muestral ($\Omega$)**: Conjunto de todos los resultados posibles del experimento aleatorio.
    *   *Ejemplo*: Si lanzamos una moneda tres veces, $\Omega = \{CCC, CCX, CXC, XCC, CXX, XCX, XXC, XXX\}$.
*   **Suceso (o Evento)**: Cualquier subconjunto del espacio muestral ($A \subseteq \Omega$).
    *   **Suceso Elemental**: Formado por un único resultado posible.
    *   **Suceso Compuesto**: Formado por varios resultados.
    *   **Suceso Seguro**: Coincide con $\Omega$. Siempre ocurre.
    *   **Suceso Imposible ($\emptyset$)**: No contiene ningún elemento. Nunca ocurre.

### Álgebra de Sucesos y Operaciones
Sean $A, B \subseteq \Omega$ dos sucesos:
1.  **Unión ($A \cup B$)**: Ocurre si ocurre al menos uno de los dos sucesos ($A$ o $B$).
2.  **Intersección ($A \cap B$)**: Ocurre si ocurren simultáneamente ambos sucesos ($A$ y $B$).
3.  **Contrario o Complementario ($\bar{A}$ o $A^c$)**: Ocurre si no ocurre $A$. Se cumple que $A \cup \bar{A} = \Omega$ y $A \cap \bar{A} = \emptyset$.
4.  **Sucesos Mutuamente Excluyentes o Incompatibles**: Si no pueden ocurrir a la vez, es decir, $A \cap B = \emptyset$.

**Leyes de De Morgan**:
*   $\overline{A \cup B} = \bar{A} \cap \bar{B}$
*   $\overline{A \cap B} = \bar{A} \cup \bar{B}$

---

## 2. Definición de Probabilidad

La probabilidad $P(A)$ es una medida numérica asignada a un suceso $A$ que cuantifica la verosimilitud de su ocurrencia.

### A. Definición Clásica (Regla de Laplace)
Si todos los resultados del espacio muestral son igualmente probables (equiprobables):
$$P(A) = \frac{\text{Casos Favorables a } A}{\text{Casos Posibles en } \Omega}$$

### B. Definición Frecuentista
Si repetimos un experimento $N$ veces en las mismas condiciones, y el suceso $A$ ocurre $n_A$ veces, su frecuencia relativa es $f_N(A) = \frac{n_A}{N}$. La probabilidad es el límite cuando el número de repeticiones tiende a infinito:
$$P(A) = \lim_{N \to \infty} \frac{n_A}{N}$$
*(Esta definición da origen a la **Ley de los Grandes Números**, que establece que a mayor número de pruebas, la frecuencia observada se estabiliza en torno al valor real de la probabilidad).*

### C. Definición Axiomática de Kolmogorov
Es la definición matemática formal. Una probabilidad $P$ es una función que asocia a cada suceso $A$ un número real tal que cumple tres axiomas básicos:
1.  **Axioma 1**: $P(A) \ge 0$ (la probabilidad no puede ser negativa).
2.  **Axioma 2**: $P(\Omega) = 1$ (la certeza absoluta tiene probabilidad 1).
3.  **Axioma 3**: Si $A_1, A_2, \dots$ son sucesos mutuamente excluyentes dos a dos ($A_i \cap A_j = \emptyset$ para todo $i \neq j$), entonces:
    $$P\left( \bigcup_{i=1}^\infty A_i \right) = \sum_{i=1}^\infty P(A_i)$$

### Propiedades Derivadas
A partir de estos axiomas se deducen teoremas fundamentales:
*   $P(\bar{A}) = 1 - P(A)$
*   $P(\emptyset) = 0$
*   Si $A \subseteq B \implies P(A) \le P(B)$
*   $P(A \cup B) = P(A) + P(B) - P(A \cap B)$

---

## 3. Probabilidad Condicionada e Independencia

La probabilidad condicionada modela cómo cambia la probabilidad de un suceso cuando disponemos de información previa sobre la ocurrencia de otro.

### Probabilidad Condicionada
La probabilidad de que ocurra el suceso $A$ sabiendo que ya ha ocurrido el suceso $B$ (con $P(B) > 0$) se define como:
$$P(A|B) = \frac{P(A \cap B)}{P(B)}$$

*   **Intuición**: Al saber que $B$ ha ocurrido, nuestro espacio de posibilidades se reduce. El nuevo "universo" de sucesos posibles ya no es $\Omega$, sino únicamente $B$. La probabilidad de $A$ dentro de este nuevo universo es la porción de $A$ que también pertenece a $B$ ($A \cap B$), escalada por la medida del nuevo universo ($P(B)$).

De la definición de probabilidad condicionada se deriva la regla de la multiplicación para la intersección:
$$P(A \cap B) = P(B) \cdot P(A|B) = P(A) \cdot P(B|A)$$

### Independencia de Sucesos
Dos sucesos $A$ y $B$ son estadísticamente independientes si el hecho de que ocurra uno no afecta la probabilidad de ocurrencia del otro. Es decir, si se cumple cualquiera de las siguientes condiciones equivalentes:
1.  $P(A|B) = P(A)$
2.  $P(B|A) = P(B)$
3.  $P(A \cap B) = P(A) \cdot P(B)$

---

## 4. Simulación Frecuentista en R

Podemos usar R para demostrar la Ley de los Grandes Números simulando lanzamientos de una moneda y observando cómo la frecuencia relativa de caras converge a 0.5 a medida que aumenta el número de lanzamientos:

```R
# Simular el lanzamiento de una moneda (0 = Cruz, 1 = Cara)
# Lanzamos la moneda 10.000 veces
n_lanzamientos <- 10000
resultados <- sample(c(0, 1), size = n_lanzamientos, replace = TRUE, prob = c(0.5, 0.5))

# Calcular las frecuencias acumuladas de caras
caras_acumuladas <- cumsum(resultados)
ensayos <- 1:n_lanzamientos
frecuencia_relativa <- caras_acumuladas / ensayos

# Graficar la convergencia de la probabilidad
plot(ensayos, frecuencia_relativa, type="l", ylim=c(0.2, 0.8),
     main="Ley de los Grandes Números (Simulación en R)",
     xlab="Número de ensayos", ylab="Frecuencia relativa de Caras",
     col="darkgreen", lwd=2)
abline(h=0.5, col="red", lty=2, lwd=2) # Línea teórica en 0.5
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
En una red local corporativa, la probabilidad de que un paquete de datos sufra una colisión es $P(C) = 0.15$, la probabilidad de que sufra un retraso de cola es $P(R) = 0.25$, y la probabilidad de que ocurran ambos problemas simultáneamente es $P(C \cap R) = 0.05$.
1. Calcular la probabilidad de que un paquete sufra colisión o retraso de cola.
2. Calcular la probabilidad de que un paquete no sufra ninguno de los dos inconvenientes.
3. Determinar si los sucesos "Colisión" y "Retraso de cola" son independientes.

**Solución**:
1.  **Probabilidad de la unión $P(C \cup R)$**:
    $$P(C \cup R) = P(C) + P(R) - P(C \cap R) = 0.15 + 0.25 - 0.05 = 0.35$$
    *(La probabilidad de sufrir colisión, retraso o ambos es de 0.35 o 35%).*

2.  **Probabilidad de no sufrir ninguno**:
    El suceso "ninguno" equivale al complementario de la unión: $\overline{C \cup R}$.
    $$P(\overline{C \cup R}) = 1 - P(C \cup R) = 1 - 0.35 = 0.65$$
    *(La probabilidad de que un paquete viaje de forma limpia es de 0.65 o 65%).*

3.  **Comprobación de Independencia**:
    Dos sucesos son independientes si $P(C \cap R) = P(C) \cdot P(R)$.
    *   Por un lado tenemos: $P(C \cap R) = 0.05$.
    *   Por otro lado: $P(C) \cdot P(R) = 0.15 \times 0.25 = 0.0375$.
    Como $0.05 \neq 0.0375$, los sucesos **no son independientes** (son dependientes). El hecho de que un paquete sufra una colisión altera la probabilidad de que sufra un retraso de cola.
