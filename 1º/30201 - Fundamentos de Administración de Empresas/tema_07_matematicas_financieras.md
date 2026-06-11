# Tema 7: Matemáticas Financieras: Capitalización y Rentas

El principio básico de las finanzas afirma que **un euro hoy no vale lo mismo que un euro mañana**. El paso del tiempo altera el valor del dinero debido a tres factores fundamentales: la inflación (pérdida de poder adquisitivo), el coste de oportunidad (lo que dejamos de ganar por no invertir ese dinero) y el riesgo (la incertidumbre del cobro futuro). Las matemáticas financieras proporcionan las herramientas para homogeneizar y comparar capitales distribuidos en diferentes instantes de tiempo.

---

## 1. Capitalización Simple

Se aplica generalmente a operaciones financieras a corto plazo (menos de un año).
*   **Principio**: Los intereses generados en cada periodo no se acumulan al capital inicial para generar nuevos intereses; se retiran o permanecen inactivos.
*   **Fórmulas**:
    *   Interés acumulado ($I$):
        $$I = C_0 \cdot i \cdot n$$
    *   Capital Final ($C_n$):
        $$C_n = C_0 + I = C_0(1 + i \cdot n)$$

donde:
*   $C_0$: Capital inicial (en $t = 0$).
*   $i$: Tasa de interés anual (expresada en tanto por uno).
*   $n$: Número de años (o periodos anuales) de la operación.

---

## 2. Capitalización Compuesta

Se aplica a operaciones a largo plazo (más de un año).
*   **Principio (Interés Compuesto)**: Los intereses generados al final de cada periodo se acumulan (se reinvierten) al capital del periodo anterior, pasando a generar nuevos intereses en el periodo siguiente.
*   **Fórmulas**:
    *   Capital Final ($C_n$):
        $$C_n = C_0 (1 + i)^n$$
    *   Interés total generado ($I$):
        $$I = C_n - C_0 = C_0 \left[ (1 + i)^n - 1 \right]$$

### Tasa de Interés Nominal (TIN) vs. Tasa Anual Equivalente (TAE)
Si el interés se liquida en periodos inferiores al año (por ejemplo, de forma mensual o trimestral), la tasa nominal (TIN) no refleja el rendimiento real. Debemos calcular la TAE, que tiene en cuenta el efecto de la reinversión de intereses:
$$\text{TAE} = \left( 1 + \frac{\text{TIN}}{m} \right)^m - 1$$
donde $m$ es el número de periodos de liquidación al año (ej. $m = 12$ para liquidaciones mensuales).

---

## 3. Valoración de Rentas Financieras Constantes

Una **renta financiera** es una secuencia de cobros o pagos (llamados términos de la renta, $C$) distribuidos a lo largo del tiempo.
Para hallar el **Valor Actual ($V_0$)** de una renta de $n$ capitales constantes de valor $C$ pagados al final de cada año (renta constante temporal pospagable) a un tipo de interés $i$:
$$V_0 = \sum_{t=1}^n \frac{C}{(1+i)^t} = C \cdot \frac{1 - (1+i)^{-n}}{i}$$

---

## 4. El Toque Informático

### Análisis Financiero: Compra de Servidores Locales (On-Premise) vs. Cloud (AWS)
Al diseñar la infraestructura de una empresa tecnológica, el director de sistemas (CTO) y el director financiero (CFO) deben decidir:
*   **Opción A**: Comprar servidores físicos propios hoy por un coste inicial de $30.000 \, \text{€}$.
*   **Opción B**: Alquilar servidores en la nube (AWS) pagando una cuota constante de $11.000 \, \text{€/año}$ durante los próximos 3 años.

Aunque a simple vista $11.000 \times 3 = 33.000 \, \text{€}$ parece más caro que $30.000$, las matemáticas financieras demuestran que, debido al valor temporal del dinero, pagar en el futuro es más ventajoso. Descontando las cuotas futuras de AWS a una tasa de oportunidad del $8\%$, el valor actual de la opción Cloud es:
$$V_{0\text{, Cloud}} = 11.000 \cdot \frac{1 - (1+0.08)^{-3}}{0.08} \approx 28.348 \, \text{€}$$
Dado que $28.348 \, \text{€} < 30.000 \, \text{€}$, la opción de la nube es financieramente más barata en términos de Valor Actual.

A continuación, implementamos en Python una simulación que compara el crecimiento de un capital invertido a interés simple frente a interés compuesto a lo largo de 10 años.

```python
def simular_capitalización(c0, interes_anual, anos):
    print(f"Capital Inicial: {c0} € | Tasa Interés: {interes_anual*100}% anual")
    print("-" * 55)
    print(f"{'AÑO':5s} | {'CAPITAL SIMPLE':18s} | {'CAPITAL COMPUESTO':18s}")
    print("-" * 55)
    
    for t in range(1, anos + 1):
        # Capitalización Simple
        c_simple = c0 * (1 + interes_anual * t)
        # Capitalización Compuesta
        c_compuesta = c0 * ((1 + interes_anual) ** t)
        print(f"{t:5d} | {c_simple:16.2f} € | {c_compuesta:16.2f} €")

# Simulación de 10.000 € al 5% durante 8 años
simular_capitalización(10000, 0.05, 8)
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Invertimos un capital inicial de $10.000 \, \text{€}$ durante 5 años en un fondo que ofrece una tasa de interés compuesto del $4\%$ anual. Calcular el capital final obtenido y el interés total acumulado.

**Solución:**
1.  **Identificar parámetros**:
    *   $C_0 = 10.000 \, \text{€}$.
    *   $i = 0.04$ anual.
    *   $n = 5$ años.
2.  **Calcular Capital Final ($C_n$)**:
    $$C_n = C_0 (1 + i)^n = 10.000 \cdot (1 + 0.04)^5 = 10.000 \cdot (1.04)^5$$
    Calculamos la potencia: $(1.04)^5 \approx 1.21665$.
    $$C_n = 10.000 \cdot 1.21665 = 12.166,53 \, \text{€}$$
3.  **Calcular interés total acumulado ($I$)**:
    $$I = C_n - C_0 = 12.166,53 - 10.000 = 2.166,53 \, \text{€}$$

Al cabo de 5 años, el capital final es de $12.166,53 \, \text{€}$, habiendo generado $2.166,53 \, \text{€}$ en intereses.

### Ejercicio 2
Un banco ofrece un préstamo personal al $6\%$ de Tasa de Interés Nominal (TIN) con liquidación de intereses mensual ($m=12$). Calcular la Tasa Anual Equivalente (TAE) real de la operación.

**Solución:**
1.  **Identificar parámetros**:
    *   $\text{TIN} = 0.06$.
    *   $m = 12$ periodos anuales.
2.  **Aplicar la fórmula de la TAE**:
    $$\text{TAE} = \left( 1 + \frac{\text{TIN}}{m} \right)^m - 1 = \left( 1 + \frac{0.06}{12} \right)^{12} - 1$$
    $$\text{TAE} = (1 + 0.005)^{12} - 1 = (1.005)^{12} - 1$$
    Calculamos la potencia: $(1.005)^{12} \approx 1.06168$.
    $$\text{TAE} = 1.06168 - 1 = 0.06168 \quad \implies \quad 6.17\%$$

El préstamo tiene una TAE real del $6.17\%$, reflejando el impacto del interés compuesto mensual.

---

## 6. Ejercicios Propuestos

1.  Calcula el capital final obtenido al invertir $5.000 \, \text{€}$ a interés simple durante 9 meses a una tasa de interés del $6\%$ anual (pista: ajusta el tiempo $n$ a fracción de año).
2.  Deseamos acumular un capital de $25.000 \, \text{€}$ dentro de 4 años. Si un fondo nos ofrece una rentabilidad de interés compuesto del $5\%$ anual, ¿cuánto capital debemos depositar hoy en el banco?
3.  Calcula el Valor Actual de una renta de alquiler de servidores dedicada que nos cuesta $2.000 \, \text{€}$ al final de cada año durante los próximos 5 años, a un tipo de interés de valoración del $7\%$ anual.
