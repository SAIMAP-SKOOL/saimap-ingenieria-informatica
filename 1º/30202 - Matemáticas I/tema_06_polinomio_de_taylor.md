# Tema 6: Polinomio de Taylor

Las funciones no polinómicas como las trigonométricas, exponenciales o logarítmicas (denominadas funciones trascendentes) son difíciles de evaluar directamente a nivel de hardware. El Polinomio de Taylor resuelve este problema transformando estas funciones complejas en sumas de polinomios ordinarios en el entorno de un punto. En la informática, esta técnica es la base del diseño de chips aritméticos, bibliotecas matemáticas estándar (`math.h`, `math` en Python) y métodos de diferencias finitas en modelado 3D.

---

## 1. Concepto y Construcción de la Aproximación Local

Dada una función $f(x)$ que es derivable $n$ veces en un punto $a$, queremos encontrar un polinomio $P_n(x)$ de grado $n$ tal que su comportamiento local en las cercanías del punto $a$ sea lo más parecido posible al de $f(x)$.

Para lograr la mejor aproximación local, imponemos la condición de que el polinomio $P_n(x)$ y sus primeras $n$ derivadas coincidan exactamente con la función $f(x)$ y sus primeras $n$ derivadas en el punto $x = a$:
$$P_n(a) = f(a), \quad P_n'(a) = f'(a), \quad P_n''(a) = f''(a), \quad \dots, \quad P_n^{(n)}(a) = f^{(n)}(a)$$

Bajo estas condiciones, se deduce algebraicamente la fórmula del **Polinomio de Taylor de grado $n$ para $f(x)$ centrado en $a$**:
$$P_n(x) = f(a) + f'(a)(x-a) + \frac{f''(a)}{2!}(x-a)^2 + \frac{f'''(a)}{3!}(x-a)^3 + \dots + \frac{f^{(n)}(a)}{n!}(x-a)^n$$
Lo cual se resume en notación sumatoria como:
$$P_n(x) = \sum_{k=0}^{n} \frac{f^{(k)}(a)}{k!}(x-a)^k \quad (\text{definiendo } 0! = 1 \text{ y } f^{(0)}(a) = f(a))$$

```
    y ^                  * f(x)
      |                /
      |               /  * P_2(x) (Aproximación cuadrática)
      |              /  /
      |             / /
      |            * /   P_1(x) (Recta tangente)
      |          / | 
  ----+---------+--+------------------> x
      0         a
```

### Polinomio de Maclaurin
Cuando el punto de aproximación es el origen, $a = 0$, el polinomio se denomina **Polinomio de Maclaurin**:
$$P_n(x) = f(0) + f'(0)x + \frac{f''(0)}{2!}x^2 + \frac{f''(0)}{3!}x^3 + \dots + \frac{f^{(n)}(0)}{n!}x^n$$

---

## 2. Desarrollos de Maclaurin Elementales Importantes

A continuación se presentan los polinomios de aproximación de Maclaurin ($a=0$) fundamentales:

| Función $f(x)$ | Término general del desarrollo de Maclaurin | Intervalo de validez |
| :--- | :--- | :--- |
| Exponencial: $e^x$ | $\displaystyle \sum_{k=0}^{n} \frac{x^k}{k!} = 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \dots$ | $x \in \mathbb{R}$ |
| Seno: $\sin x$ | $\displaystyle \sum_{k=0}^{n} \frac{(-1)^k x^{2k+1}}{(2k+1)!} = x - \frac{x^3}{3!} + \frac{x^5}{5!} - \dots$ | $x \in \mathbb{R}$ |
| Coseno: $\cos x$ | $\displaystyle \sum_{k=0}^{n} \frac{(-1)^k x^{2k}}{(2k)!} = 1 - \frac{x^2}{2!} + \frac{x^4}{4!} - \dots$ | $x \in \mathbb{R}$ |
| Logaritmo: $\ln(1+x)$ | $\displaystyle \sum_{k=1}^{n} \frac{(-1)^{k-1} x^k}{k} = x - \frac{x^2}{2} + \frac{x^3}{3} - \dots$ | $x \in (-1, 1]$ |

---

## 3. El Resto de la Aproximación y Estimación del Error

La aproximación polinómica comete un error. Definimos el **Resto de Taylor de orden $n$**, denotado por $R_n(x)$, como la diferencia exacta entre la función real y su aproximación polinómica:
$$f(x) = P_n(x) + R_n(x) \implies R_n(x) = f(x) - P_n(x)$$

### Resto de Lagrange
Si la función $f(x)$ es derivable $n+1$ veces en un intervalo abierto que contiene a $a$ y $x$, entonces existe un punto intermedio $c$ estrictamente entre $a$ y $x$ tal que el resto viene dado por:
$$R_n(x) = \frac{f^{(n+1)}(c)}{(n+1)!}(x-a)^{n+1}$$

### Cota de Error
Para estimar el error absoluto cometido en una aproximación local dentro de un intervalo, buscamos una cota superior para la derivada de orden $n+1$:
$$|R_n(x)| \le \frac{M}{(n+1)!}|x-a|^{n+1} \quad \text{donde } M \ge \max_{t \in [a, x]} |f^{(n+1)}(t)|$$

---

## 4. El Toque Informático

### ¿Cómo calculan los ordenadores funciones trascendentes?
Una CPU o GPU no dispone de un mecanismo directo de hardware para resolver $\cos(x)$. En su lugar, utilizan algoritmos de aproximación polinómica optimizados de Taylor-Maclaurin, el método CORDIC o aproximaciones de Chebyshev combinados con **reducción de rango**.

Por ejemplo, para evaluar $\cos(x)$ para cualquier $x \in \mathbb{R}$:
1.  **Reducción de rango**: Aprovechando la periodicidad del coseno, se mapea $x$ al intervalo básico $[-\pi, \pi]$ o $[0, \pi/2]$.
2.  **Evaluación polinómica**: Se calcula el polinomio de Taylor de grado truncado adecuado a ese intervalo para garantizar un error inferior a la precisión de coma flotante de la máquina (por ejemplo, $10^{-16}$ para una variable `double` de 64 bits).

A continuación, implementamos una aproximación de $\cos(x)$ usando Maclaurin en Python, controlando de manera dinámica el número de términos para observar cómo se reduce el error frente a la función estándar de la librería nativa de Python `math`.

```python
import math
import numpy as np
import matplotlib.pyplot as plt

def aproximar_coseno(x, n_terminos):
    # Reducción de rango: mapeamos x a [-pi, pi]
    x_reducido = x % (2 * math.pi)
    if x_reducido > math.pi:
        x_reducido -= 2 * math.pi
        
    suma = 0.0
    for k in range(n_terminos):
        coeficiente = (-1)**k
        numerador = x_reducido ** (2*k)
        denominador = math.factorial(2*k)
        
        suma += coeficiente * (numerador / denominador)
        
    return suma

# Ejemplo numérico
x_prueba = 10.5 # Aproximadamente 3.34 radianes
exacto = math.cos(x_prueba)

print(f"Cálculo de cos({x_prueba})")
print(f"Valor exacto (math.cos): {exacto:.16f}\n")

print(f"{'Términos':<10}{'Aproximación':<20}{'Error Absoluto':<20}")
for n in range(1, 10):
    aprox = aproximar_coseno(x_prueba, n)
    error = abs(exacto - aprox)
    print(f"{n:<10}{aprox:<20.16f}{error:<20.2e}")

# Visualización del error conforme nos alejamos del origen para diferentes grados
x_vals = np.linspace(-6, 6, 300)
y_cos = np.cos(x_vals)

plt.figure(figsize=(9, 5))
plt.plot(x_vals, y_cos, label="cos(x) real", color="black", linewidth=2)

for n in [2, 4, 6]:
    # Para graficar, usamos la fórmula de Taylor directamente sin reducción de rango
    y_taylor = np.zeros_like(x_vals)
    for k in range(n):
        y_taylor += ((-1)**k) * (x_vals**(2*k)) / math.factorial(2*k)
    plt.plot(x_vals, y_taylor, label=f"Taylor Grado {2*n-2}", linestyle="--")

plt.ylim(-2, 2)
plt.title("Aproximación de cos(x) mediante Polinomios de Taylor en el origen")
plt.xlabel("x (radianes)")
plt.ylabel("y")
plt.legend()
plt.grid(True)
plt.show()
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Aproximar el número $e^{0.1}$ usando el polinomio de Maclaurin de grado 3 de $f(x) = e^x$, y acotar el error de la aproximación usando el resto de Lagrange.

**Solución:**
1.  **Desarrollo de Maclaurin de $f(x) = e^x$ de grado 3**:
    El polinomio es:
    $$P_3(x) = 1 + x + \frac{x^2}{2} + \frac{x^3}{6}$$
2.  **Evaluar en $x = 0.1$**:
    $$P_3(0.1) = 1 + 0.1 + \frac{(0.1)^2}{2} + \frac{(0.1)^3}{6} = 1 + 0.1 + 0.005 + 0.00016667 = 1.10516667$$
3.  **Acotar el error cometido utilizando el Resto de Lagrange**:
    El resto $R_3(x)$ para $n=3$ es:
    $$R_3(x) = \frac{f^{(4)}(c)}{4!}x^4 = \frac{e^c}{24}x^4 \quad \text{donde } c \in (0, 0.1)$$
    Como $c \in (0, 0.1)$, sabemos que $e^c$ es estrictamente creciente. Por tanto, podemos acotar superiormente $e^c < e^{0.1} < e^1 < 3$.
    Sustituyendo el valor $x = 0.1$:
    $$|R_3(0.1)| \le \frac{3}{24}(0.1)^4 = 0.125 \cdot 10^{-4} = 1.25 \cdot 10^{-5}$$
    
    El valor aproximado calculado es $1.10516667$, con un error absoluto garantizado menor que $0.0000125$. (De hecho, el valor real es $e^{0.1} \approx 1.105170918$, con un error real de $\approx 4.25 \cdot 10^{-6}$, lo cual cumple holgadamente con la cota teórica calculada).

---

## 6. Ejercicios Propuestos

1.  Hallar el polinomio de Taylor de grado 3 de la función $f(x) = \ln(x)$ centrado en el punto $a = 1$.
2.  Utilizar el polinomio de Maclaurin de grado 2 para aproximar $\sqrt{1.2}$ usando la función generalizada de potencia $(1+x)^{1/2}$, y estimar una cota superior para el error absoluto.
3.  Calcular los límites siguientes utilizando desarrollos de Taylor de Maclaurin adecuados:
    $$\lim_{x\to 0} \frac{\cos x - 1 + \frac{x^2}{2}}{x^4}$$
