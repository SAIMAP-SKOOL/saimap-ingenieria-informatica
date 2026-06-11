# Tema 4: Teoremas Fundamentales de la Probabilidad

El análisis probabilístico estructurado requiere herramientas que permitan descomponer problemas complejos en subproblemas más sencillos. Las dos herramientas fundamentales para ello son el Teorema de la Probabilidad Total y el Teorema de Bayes, esenciales en computación para algoritmos de clasificación (como el Clasificador Naive Bayes) e inferencia probabilística.

---

## 1. Partición del Espacio Muestral

Para aplicar estos teoremas, primero debemos estructurar el espacio muestral. Decimos que una colección de sucesos $A_1, A_2, \dots, A_k$ constituye una **partición** del espacio muestral $\Omega$ si cumple dos condiciones:

1.  **Exclusividad**: Son mutuamente excluyentes entre sí (no pueden ocurrir a la vez):
    $$A_i \cap A_j = \emptyset \quad \text{para todo } i \neq j$$
2.  **Exhaustividad**: Su unión cubre completamente el espacio muestral:
    $$\bigcup_{i=1}^k A_i = \Omega$$

Bajo una partición, cualquier suceso arbitrario $B$ puede representarse como la unión de sus intersecciones con cada uno de los elementos de la partición:
$$B = (B \cap A_1) \cup (B \cap A_2) \cup \dots \cup (B \cap A_k)$$

---

## 2. Teorema de la Probabilidad Total

Si conocemos las probabilidades a priori de los sucesos de una partición, $P(A_i)$, y las probabilidades condicionadas de un suceso $B$ dado cada $A_i$, $P(B|A_i)$, podemos calcular la probabilidad absoluta de $B$ mediante el **Teorema de la Probabilidad Total**:

$$P(B) = \sum_{i=1}^k P(A_i) \cdot P(B|A_i)$$

*   **Intuición Física**: Imagina que deseas llegar a un servidor de destino y hay $k$ rutas físicas disponibles ($A_1, \dots, A_k$). Cada ruta tiene una probabilidad de ser elegida, $P(A_i)$, y una probabilidad de que el paquete se pierda en ella, $P(B|A_i)$. La probabilidad total de perder el paquete ($P(B)$) es la suma ponderada de las probabilidades de pérdida en cada ruta individual, donde los pesos son las probabilidades de elegir cada ruta.

---

## 3. Teorema de Bayes

Una vez que ha ocurrido el suceso $B$, podemos preguntarnos cuál es la probabilidad de que haya sido causado por un elemento de la partición en particular ($A_j$). El **Teorema de Bayes** nos permite actualizar nuestra creencia a priori $P(A_j)$ incorporando la nueva evidencia de que $B$ ha ocurrido:

$$P(A_j|B) = \frac{P(A_j \cap B)}{P(B)} = \frac{P(A_j) \cdot P(B|A_j)}{\sum_{i=1}^k P(A_i) \cdot P(B|A_i)}$$

*   **Intuición**: Es la **fórmula del aprendizaje**. Nos permite "invertir" la relación de condicionamiento.
    *   $P(A_j)$ es la **probabilidad a priori** (nuestra creencia inicial antes de observar el efecto).
    *   $P(B|A_j)$ es la **verosimilitud** (la probabilidad de observar el efecto $B$ si la causa fuera $A_j$).
    *   $P(A_j|B)$ es la **probabilidad a posteriori** (la creencia actualizada tras observar el efecto $B$).
    *   El denominador es la probabilidad total de observar el efecto, actuando como una constante de normalización.

### Aplicación Práctica: Filtros Bayesianos de Spam
Un filtro de spam analiza la presencia de ciertas palabras clave. Supongamos que un email contiene la palabra "Gratis" ($B$). Conocemos la probabilidad de que un email sea spam, $P(\text{Spam})$, y la probabilidad de que aparezca "Gratis" si el email es spam, $P(\text{"Gratis"|Spam})$. Aplicando Bayes, calculamos la probabilidad a posteriori de que el correo sea spam dado que contiene dicha palabra: $P(\text{Spam|"Gratis"})$.

---

## 4. Cálculo Probabilístico en R

Podemos automatizar los cálculos de Bayes y Probabilidad Total en R mediante variables estructuradas:

```R
# Supongamos tres servidores de base de datos A, B y C que atienden el tráfico.
# Probabilidades a priori de asignación de consultas:
p_servidores <- c(A = 0.50, B = 0.30, C = 0.20)

# Probabilidades de error de consulta en cada servidor:
p_error_dado_servidor <- c(A = 0.01, B = 0.03, C = 0.05)

# 1. Aplicar el Teorema de la Probabilidad Total para obtener P(Error)
p_error_total <- sum(p_servidores * p_error_dado_servidor)
print(paste("Probabilidad Total de Error:", p_error_total)) 
# Retorna 0.024 (2.4% de las consultas fallan en total)

# 2. Aplicar el Teorema de Bayes
# ¿Si una consulta falló, cuál es la probabilidad de que proceda del servidor C?
p_C_dado_error <- (p_servidores["C"] * p_error_dado_servidor["C"]) / p_error_total
print(paste("Probabilidad de que el causante sea C:", p_C_dado_error))
# Retorna 0.4166 (A pesar de procesar solo el 20% del tráfico, C causa el 41.7% de los errores)
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
En una fábrica de software, el código es desarrollado por tres programadores: Ana (escribe el 50% de las líneas de código), Carlos (30%) y Elena (20%). Por estadísticas previas, se sabe que la probabilidad de introducir un bug por cada línea escrita es de 0.02 para Ana, 0.05 para Carlos y 0.08 para Elena.
1. Si seleccionamos una línea de código al azar, calcular la probabilidad de que contenga un bug.
2. Si se detecta un bug en una línea de código durante la auditoría, calcular la probabilidad de que dicha línea haya sido escrita por Carlos.

**Solución**:
Definimos los sucesos de la partición:
*   $A$: Línea escrita por Ana ($P(A) = 0.50$).
*   $C$: Línea escrita por Carlos ($P(C) = 0.30$).
*   $E$: Línea escrita por Elena ($P(E) = 0.20$).
*   $B$: La línea contiene un bug (el efecto observado).
    *   $P(B|A) = 0.02$
    *   $P(B|C) = 0.05$
    *   $P(B|E) = 0.08$

1.  **Probabilidad Total de Bug ($P(B)$)**:
    $$P(B) = P(A) \cdot P(B|A) + P(C) \cdot P(B|C) + P(E) \cdot P(B|E)$$
    $$P(B) = (0.50 \times 0.02) + (0.30 \times 0.05) + (0.20 \times 0.08)$$
    $$P(B) = 0.010 + 0.015 + 0.016 = 0.041$$
    *(La probabilidad de encontrar un bug en una línea elegida al azar es del 4.1%).*

2.  **Teorema de Bayes para calcular $P(C|B)$**:
    $$P(C|B) = \frac{P(C) \cdot P(B|C)}{P(B)} = \frac{0.30 \times 0.05}{0.041} = \frac{0.015}{0.041} \approx 0.3659$$
    *(Si la línea contiene un bug, hay una probabilidad aproximada del 36.59% de que haya sido escrita por Carlos).*
