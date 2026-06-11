# Tema 8: Selección de Inversiones: Métodos Estáticos y Dinámicos

Toda empresa, y especialmente en el sector tecnológico, se enfrenta al reto de decidir en qué proyectos invertir sus recursos limitados. Un proyecto de inversión consiste en el empleo de recursos financieros actuales con la expectativa de obtener ingresos futuros distribuidos a lo largo del tiempo. Para analizar la viabilidad de una inversión, es crucial definir y modelar correctamente los flujos de caja y aplicar metodologías cuantitativas de selección.

---

## 1. El Concepto de Flujo de Caja (Cash Flow)

En finanzas corporativas, la rentabilidad y viabilidad de un proyecto se miden en función de su liquidez, no de sus beneficios contables.
*   **Diferencia fundamental**: El beneficio contable se rige por el principio de devengo (ingresos y gastos anotados cuando nacen, independientemente de cuándo se cobren o paguen), mientras que el flujo de caja se basa en el principio de caja (entradas y salidas reales de dinero).
*   **Representación temporal**: Un proyecto de inversión se define por un desembolso inicial ($A$) en el momento $t = 0$ y una serie de cobros ($C_t$) y pagos ($P_t$) durante los periodos $t = 1, 2, \dots, n$. El flujo neto de caja del periodo $t$ ($Q_t$) es:
    $$Q_t = C_t - P_t$$

---

## 2. Métodos Estáticos de Selección de Inversiones

Los métodos estáticos no tienen en cuenta el valor del dinero en el tiempo. Operan sumando directamente cantidades de dinero de diferentes años, lo cual es teóricamente incorrecto pero útil como primera aproximación rápida.

### Plazo de Recuperación (Payback) Simple
Es el tiempo que tarda la empresa en recuperar el desembolso inicial ($A$) mediante los flujos netos de caja generados.
*   **Fórmula (cuando los flujos netos de caja son constantes, $Q_t = Q$):**
    $$\text{Payback} = \frac{A}{Q}$$
*   **Fórmula (cuando los flujos netos de caja son variables):**
    Se van acumulando los flujos neto de caja año a año hasta igualar el desembolso inicial $A$. Si la recuperación ocurre durante el año $k$:
    $$\text{Payback} = (k - 1) + \frac{A - \text{Flujo Acumulado}_{k-1}}{Q_k}$$
*   **Criterio de decisión**: Se prefieren los proyectos con menor plazo de recuperación.
*   **Limitaciones**: Ignora los flujos de caja posteriores al periodo de recuperación y no considera la diferencia de valor del dinero en el tiempo.

---

## 3. Métodos Dinámicos de Selección de Inversiones

Los métodos dinámicos corrigen las deficiencias de los métodos estáticos descontando (actualizando) los flujos futuros de caja para traerlos al momento actual ($t = 0$), aplicando una tasa de descuento o coste de oportunidad del capital ($r$).

### Valor Actual Neto (VAN)
El VAN (también conocido como NPV por sus siglas en inglés) es la diferencia entre el valor actualizado de los flujos de caja futuros y el desembolso inicial.
*   **Fórmula**:
    $$VAN = -A + \sum_{t=1}^n \frac{Q_t}{(1+r)^t} = -A + \frac{Q_1}{(1+r)^1} + \frac{Q_2}{(1+r)^2} + \dots + \frac{Q_n}{(1+r)^n}$$

*   **Criterios de aceptación del VAN**:
    *   $VAN > 0$: El proyecto es **viable**. Genera valor para la empresa por encima del coste de capital ($r$).
    *   $VAN = 0$: El proyecto es **indiferente**. Se recupera la inversión y se cubre el coste de capital, pero no se genera valor extra.
    *   $VAN < 0$: El proyecto es **no viable**. Destruye valor porque no cubre el rendimiento mínimo exigido.
    *   **Jerarquización**: Si hay varios proyectos mutuamente excluyentes, se elige el que tenga el **mayor VAN**.

### Tasa Interna de Retorno (TIR)
La TIR (o IRR por sus siglas en inglés) es la tasa de descuento ($TIR$) que hace que el Valor Actual Neto sea igual a cero. Representa la rentabilidad interna media anual que genera el proyecto.
*   **Fórmula**:
    $$0 = -A + \sum_{t=1}^n \frac{Q_t}{(1+TIR)^t}$$
*   **Resolución**: Para $n = 1$ o $n = 2$, se puede resolver algebraicamente (ecuación de primer o segundo grado). Para $n > 2$, se requiere resolver numéricamente por aproximaciones sucesivas (iteración).
*   **Criterios de aceptación de la TIR** (comparando con el coste del capital o tasa de corte $r$):
    *   $TIR > r$: El proyecto es **viable**. Su rentabilidad interna supera el coste de financiación.
    *   $TIR = r$: El proyecto es **indiferente**.
    *   $TIR < r$: El proyecto es **no viable**. Su rentabilidad interna es inferior a lo que cuesta financiarlo.
    *   **Inconsistencias**: En proyectos no simples (donde hay flujos netos negativos a mitad del proyecto), pueden existir múltiples tasas TIR o ninguna. En esos casos, el criterio del VAN es siempre preferible.

---

## 4. El Toque Informático

### Inversión en Software: Desarrollo Propio vs. Suscripción SaaS
La dirección de una startup de e-commerce analiza dos alternativas para implementar su nueva plataforma core de operaciones:

*   **Proyecto A (Desarrollo In-House)**: Desembolso inicial de $50.000 \, \text{€}$ en contratar un equipo de desarrollo freelance para programar una plataforma a medida. Los costes de mantenimiento serán bajos y se espera que ahorre en operaciones $15.000 \, \text{€}$ el año 1, $20.000 \, \text{€}$ el año 2, $25.000 \, \text{€}$ el año 3 y $30.000 \, \text{€}$ el año 4.
*   **Proyecto B (SaaS Parametrizado)**: Desembolso inicial de $15.000 \, \text{€}$ para la consultoría de integración inicial de un software SaaS ya existente. El coste de suscripción es anual, por lo que el ahorro neto operativo tras pagar la licencia es menor: $10.000 \, \text{€}$ el año 1, $12.000 \, \text{€}$ el año 2, $15.000 \, \text{€}$ el año 3 y $18.000 \, \text{€}$ el año 4.

Considerando un coste de oportunidad del capital ($r$) del $10\%$, usaremos Python para calcular el VAN, la TIR y el Payback de ambas alternativas para tomar la decisión óptima.

```python
def calcular_payback(desembolso, flujos):
    acumulado = 0
    for i, q in enumerate(flujos):
        if acumulado + q >= desembolso:
            periodo = i + (desembolso - acumulado) / q
            return periodo
        acumulado += q
    return float('inf')  # No se recupera en los años evaluados

def calcular_van(desembolso, flujos, r):
    van = -desembolso
    for t, q in enumerate(flujos, start=1):
        van += q / ((1 + r) ** t)
    return van

def calcular_tir(desembolso, flujos):
    # Método numérico de secante/bisección para hallar la raíz de VAN = 0
    low, high = -0.99, 2.0
    for _ in range(100):
        mid = (low + high) / 2
        van = calcular_van(desembolso, flujos, mid)
        if abs(van) < 1e-6:
            return mid
        if van > 0:
            low = mid
        else:
            high = mid
    return mid

# Datos del análisis
r = 0.10
proj_A = {"nombre": "In-House Custom", "A": 50000, "Q": [15000, 20000, 25000, 30000]}
proj_B = {"nombre": "SaaS Integration", "A": 15000, "Q": [10000, 12000, 15000, 18000]}

for p in [proj_A, proj_B]:
    payback = calcular_payback(p["A"], p["Q"])
    van = calcular_van(p["A"], p["Q"], r)
    tir = calcular_tir(p["A"], p["Q"])
    print(f"Proyecto: {p['nombre']}")
    print(f"  - Payback: {payback:.2f} años")
    print(f"  - VAN (r={r*100}%): {van:.2f} €")
    print(f"  - TIR: {tir*100:.2f}%")
    print(f"  - Viabilidad: {'SÍ' if van > 0 else 'NO'}")
    print()
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Una empresa de software de realidad virtual planea adquirir nuevas licencias de diseño por un valor de $30.000 \, \text{€}$. Se estima que esta inversión generará unos flujos netos de caja de $12.000 \, \text{€}$ en el año 1, $15.000 \, \text{€}$ en el año 2 y $18.000 \, \text{€}$ en el año 3. Si el coste de capital de la empresa es del $8\%$ anual, calcula:
1.  El Plazo de Recuperación (Payback).
2.  El Valor Actual Neto (VAN).
3.  Determina si la inversión es recomendable.

**Solución:**

1.  **Cálculo del Payback**:
    *   Desembolso inicial $A = 30.000 \, \text{€}$.
    *   Flujo neto acumulado año 1: $12.000 \, \text{€}$. Falta por recuperar: $30.000 - 12.000 = 18.000 \, \text{€}$.
    *   Flujo neto acumulado año 2: $12.000 + 15.000 = 27.000 \, \text{€}$. Falta por recuperar: $30.000 - 27.000 = 3.000 \, \text{€}$.
    *   En el año 3, el flujo neto de caja es $Q_3 = 18.000 \, \text{€}$. El tiempo exacto del año 3 para recuperar $3.000 \, \text{€}$ es:
        $$\text{Fracción de año} = \frac{3.000}{18.000} \approx 0,167 \text{ años}$$
    *   $\text{Payback} = 2 + 0,167 = 2,17 \text{ años}$ (2 años y 2 meses).

2.  **Cálculo del VAN**:
    *   $A = 30.000 \, \text{€}$, $r = 0,08$, $Q_1 = 12.000$, $Q_2 = 15.000$, $Q_3 = 18.000$.
    *   Aplicamos la fórmula del VAN:
        $$VAN = -30.000 + \frac{12.000}{(1+0,08)^1} + \frac{15.000}{(1+0,08)^2} + \frac{18.000}{(1+0,08)^3}$$
        $$VAN = -30.000 + \frac{12.000}{1,08} + \frac{15.000}{1,1664} + \frac{18.000}{1,25971}$$
        $$VAN = -30.000 + 11.111,11 + 12.859,22 + 14.288,99$$
        $$VAN = -30.000 + 38.259,32 = 8.259,32 \, \text{€}$$

3.  **Decisión**: Dado que el $VAN = 8.259,32 \, \text{€} > 0$, la inversión es **viable** y altamente recomendable ya que incrementa el valor de la empresa.

### Ejercicio 2
Un proyecto de inversión presenta un desembolso inicial de $10.000 \, \text{€}$ y generará un flujo único de caja en el año 1 de $11.200 \, \text{€}$. 
1.  Calcula la Tasa Interna de Retorno (TIR).
2.  Si el coste de financiación del dinero es del $9\%$ anual, evalúa su viabilidad.

**Solución:**

1.  **Cálculo de la TIR**:
    *   Planteamos la ecuación de la TIR donde $VAN = 0$:
        $$0 = -10.000 + \frac{11.200}{1 + TIR}$$
    *   Despejamos la variable $TIR$:
        $$10.000 = \frac{11.200}{1 + TIR}$$
        $$1 + TIR = \frac{11.200}{10.000} = 1,12$$
        $$TIR = 1,12 - 1 = 0,12 \quad \implies \quad 12\%$$

2.  **Decisión**: Comparamos la TIR con el coste del capital ($r = 9\%$):
    Como $TIR = 12\% > r = 9\%$, el proyecto es **viable**, ya que su rentabilidad interna media supera el coste de financiación.

---

## 6. Ejercicios Propuestos

1.  Una startup de ciberseguridad evalúa un proyecto con un desembolso de $40.000 \, \text{€}$ y flujos netos de caja de $15.000 \, \text{€}$ anuales durante los próximos 4 años. Sabiendo que el coste de capital es del $10\%$ anual, determina el Payback y el VAN del proyecto.
2.  Compara dos proyectos mutuamente excluyentes para automatizar la integración continua en la nube:
    *   **Proyecto Alfa**: $A = 20.000 \, \text{€}$; $Q_1 = 15.000 \, \text{€}$; $Q_2 = 10.000 \, \text{€}$.
    *   **Proyecto Beta**: $A = 12.000 \, \text{€}$; $Q_1 = 8.000 \, \text{€}$; $Q_2 = 8.000 \, \text{€}$.
    Sabiendo que el coste de capital es del $7\%$ anual, calcula el VAN de ambos y selecciona cuál es la mejor alternativa.
3.  Calcula la TIR de una inversión que requiere un desembolso de $25.000 \, \text{€}$ y que genera $15.000 \, \text{€}$ al final del año 1 y $15.000 \, \text{€}$ al final del año 2.
