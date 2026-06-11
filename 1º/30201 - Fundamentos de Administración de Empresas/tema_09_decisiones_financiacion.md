# Tema 9: Decisiones de Financiación: Fuentes de Financiación Empresarial

Para llevar a cabo las inversiones planificadas, la empresa necesita captar recursos financieros. Las decisiones de financiación consisten en seleccionar la combinación óptima de fuentes financieras (estructura de capital) que minimice el coste medio del capital y mantenga un nivel de riesgo financiero controlado.

---

## 1. Clasificación General de las Fuentes de Financiación

Las fuentes de financiación se pueden clasificar según diversos criterios:
*   **Según su propiedad**:
    *   **Financiación Propia**: Recursos aportados por los socios (capital social) o generados por la propia actividad de la empresa (autofinanciación). No se tienen que devolver y no tienen vencimiento exigible.
    *   **Financiación Ajena**: Recursos obtenidos de terceros (bancos, proveedores, inversores de deuda). Tienen un coste financiero explícito y deben ser devueltos en plazos pactados.
*   **Según su origen**:
    *   **Interna (Autofinanciación)**: Recursos autogenerados por la propia empresa dentro de su actividad de explotación.
    *   **Externa**: Recursos captados fuera de la empresa (bancos, ampliaciones de capital, subvenciones, etc.).
*   **Según el plazo de devolución**:
    *   **A Corto Plazo**: Vencimiento menor o igual a un año.
    *   **A Largo Plazo**: Vencimiento superior a un año o indefinido.

---

## 2. Financiación Interna o Autofinanciación

Proviene de los beneficios no distribuidos por la empresa. Se divide en dos tipos:
*   **Autofinanciación de Enriquecimiento (Reservas)**: Beneficios retenidos en la empresa para financiar nuevas inversiones de crecimiento.
    *   *Reservas Legales*: Obligatorias por ley en función de la forma jurídica.
    *   *Reservas Voluntarias*: Decididas por la junta general de accionistas.
    *   *Reservas Estatutarias*: Definidas en los estatutos de la empresa.
*   **Autofinanciación de Mantenimiento (Amortizaciones y Provisiones)**: Recursos destinados a mantener intacta la capacidad productiva de la empresa.
    *   *Amortizaciones*: Expresan la pérdida de valor sistemática del inmovilizado (depreciación física, funcional u obsolescencia tecnológica). Se contabilizan como un gasto contable, pero no suponen una salida real de caja, por lo que quedan retenidas en la empresa para la futura reposición del activo.

---

## 3. Financiación Externa a Largo Plazo

Recursos que la empresa obtiene de fuentes externas y cuya devolución se pacta a más de un año (o no tiene vencimiento en el caso del capital social).

*   **Ampliaciones de Capital**: Emisión de nuevas acciones o aumento del valor nominal de las existentes. Son recursos propios de origen externo.
*   **Préstamos a Largo Plazo**: Contratos con entidades bancarias que entregan una cantidad de dinero a cambio de su devolución periódica junto con intereses pactados.
*   **Leasing (Arrendamiento Financiero)**: Contrato de alquiler de un bien mueble o inmueble a largo plazo con una opción de compra obligatoria al finalizar el contrato.
*   **Renting (Arrendamiento Operativo)**: Alquiler a largo plazo que no incluye opción de compra obligatoria, pero incorpora en la cuota el mantenimiento, reparaciones y seguros. Muy utilizado en informática para hardware (ordenadores, servidores).
*   **Capital Riesgo (Venture Capital) y Business Angels**: Inversores privados que aportan capital propio y experiencia de gestión a startups con alto potencial de crecimiento a cambio de participaciones accionariales.

---

## 4. Financiación Externa a Corto Plazo

Recursos obtenidos con vencimiento inferior a un año para financiar el ciclo de explotación o circulante de la empresa.

*   **Crédito Comercial (Proveedores)**: Aplazamiento del pago de las materias primas o servicios contratados a los proveedores. No tiene un coste de interés explícito, pero sí un coste de oportunidad si se pierde el descuento por pronto pago.
*   **Póliza de Crédito (Línea de Crédito)**: El banco pone un límite de dinero a disposición de la empresa. Esta solo paga intereses por la cantidad de dinero efectivamente utilizada y una pequeña comisión por el saldo no dispuesto.
*   **Factoring**: Contrato por el cual la empresa vende sus facturas o derechos de cobro sobre clientes a una entidad financiera (factor) para obtener liquidez inmediata, a cambio de una comisión y un tipo de interés. Puede ser "con recurso" (el riesgo de impago lo asume la empresa) o "sin recurso" (el riesgo de impago lo asume el banco).
*   **Descuento Comercial**: El banco anticipa el importe de un efecto comercial (pagaré o letra de cambio) que la empresa tiene a su favor, descontando del importe nominal los intereses por el tiempo que falta hasta el vencimiento y comisiones.

---

## 5. El Toque Informático

### Amortización de Préstamos: El Método Francés
La estructura más común de devolución de préstamos bancarios es el **Método Francés**, caracterizado por el pago de una **cuota periódica constante** ($a$) que engloba intereses y devolución de principal.
A medida que avanza el préstamo, la porción destinada a pagar intereses disminuye (porque se calcula sobre el capital pendiente, que va reduciéndose) y la porción destinada a amortizar capital aumenta.

La fórmula matemática para calcular la cuota periódica constante es:
$$a = C_0 \cdot \frac{i \cdot (1+i)^n}{(1+i)^n - 1}$$
donde:
*   $C_0$: Capital prestado (principal).
*   $i$: Tasa de interés aplicable al periodo (anual, mensual, etc.).
*   $n$: Número total de periodos de amortización.

Un emprendedor informático que diseña su SaaS puede automatizar la generación de cuadros de amortización usando este script en Python para planificar el flujo de caja operativo:

```python
def generar_cuadro_amortizacion(principal, tin_anual, periodos_por_ano, anos):
    n = periodos_por_ano * anos
    i_periodo = tin_anual / periodos_por_ano
    
    # Cuota periódica constante (fórmula del método francés)
    cuota = principal * (i_periodo * (1 + i_periodo)**n) / ((1 + i_periodo)**n - 1)
    
    balance_pendiente = principal
    print(f"Préstamo: {principal:,.2f} € | TAE/TIN: {tin_anual*100}% | Cuota: {cuota:.2f} €")
    print("-" * 75)
    print(f"{'Periodo':8s} | {'Cuota':12s} | {'Intereses':12s} | {'Amortizado':12s} | {'Pendiente':12s}")
    print("-" * 75)
    
    for k in range(1, n + 1):
        interes_k = balance_pendiente * i_periodo
        principal_k = cuota - interes_k
        balance_pendiente -= principal_k
        # Evitar -0.00 al final por redondeos
        if abs(balance_pendiente) < 1e-4:
            balance_pendiente = 0.0
        print(f"{k:8d} | {cuota:12.2f} | {interes_k:12.2f} | {principal_k:12.2f} | {balance_pendiente:12.2f}")

# Simulación: Préstamo de 10.000 € al 6% anual a devolver en 4 cuotas anuales (periodos_por_ano = 1)
generar_cuadro_amortizacion(10000, 0.06, 1, 4)
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Una empresa de desarrollo de software solicita un préstamo bancario de $20.000 \, \text{€}$ para adquirir equipos de computación gráfica. El préstamo se pacta a devolver en 3 años mediante cuotas anuales constantes a un interés del $5\%$ anual.
1.  Calcula el importe de la cuota anual constante (método francés).
2.  Elabora el cuadro de amortización completo del préstamo.

**Solución:**

1.  **Cálculo de la cuota anual ($a$)**:
    *   $C_0 = 20.000 \, \text{€}$.
    *   $i = 0,05$ anual.
    *   $n = 3$ años.
    *   Aplicamos la fórmula:
        $$a = C_0 \cdot \frac{i \cdot (1+i)^n}{(1+i)^n - 1} = 20.000 \cdot \frac{0,05 \cdot (1,05)^3}{(1,05)^3 - 1}$$
        $$(1,05)^3 = 1,157625$$
        $$a = 20.000 \cdot \frac{0,05 \cdot 1,157625}{1,157625 - 1} = 20.000 \cdot \frac{0,05788125}{0,157625} \approx 20.000 \cdot 0,367208 = 7.344,17 \, \text{€}$$

2.  **Elaboración del cuadro de amortización**:
    *   **Año 1**:
        *   Capital pendiente inicial: $20.000 \, \text{€}$.
        *   Intereses: $20.000 \cdot 0,05 = 1.000 \, \text{€}$.
        *   Principal amortizado: $\text{Cuota} - \text{Interés} = 7.344,17 - 1.000 = 6.344,17 \, \text{€}$.
        *   Capital pendiente al final del año 1: $20.000 - 6.344,17 = 13.655,83 \, \text{€}$.
    *   **Año 2**:
        *   Capital pendiente inicial: $13.655,83 \, \text{€}$.
        *   Intereses: $13.655,83 \cdot 0,05 = 682,79 \, \text{€}$.
        *   Principal amortizado: $7.344,17 - 682,79 = 6.661,38 \, \text{€}$.
        *   Capital pendiente al final del año 2: $13.655,83 - 6.661,38 = 6.994,45 \, \text{€}$.
    *   **Año 3**:
        *   Capital pendiente inicial: $6.994,45 \, \text{€}$.
        *   Intereses: $6.994,45 \cdot 0,05 = 349,72 \, \text{€}$.
        *   Principal amortizado: $7.344,17 - 349,72 = 6.994,45 \, \text{€}$.
        *   Capital pendiente al final del año 3: $6.994,45 - 6.994,45 = 0 \, \text{€}$.

| Año | Cuota ($a$) | Intereses ($I_t$) | Principal Amortizado ($A_t$) | Capital Pendiente ($C_t$) |
|:---:|:-----------:|:-----------------:|:----------------------------:|:-------------------------:|
|  0  |      -      |         -         |               -              |       20.000,00 €         |
|  1  | 7.344,17 €  |    1.000,00 €     |          6.344,17 €          |       13.655,83 €         |
|  2  | 7.344,17 €  |     682,79 €      |          6.661,38 €          |        6.994,45 €         |
|  3  | 7.344,17 €  |     349,72 €      |          6.994,45 €          |           0,00 €          |

---

## 7. Ejercicios Propuestos

1.  Distingue de forma razonada la diferencia operativa y económica entre contratar servidores de datos mediante **Leasing** y contratarlos mediante **Renting** para una empresa de computación en la nube.
2.  Calcula la cuota mensual constante para amortizar un préstamo de $15.000 \, \text{€}$ en un plazo de 1 año (12 meses) a un interés del $12\%$ nominal anual (TIN) (pista: ajusta la tasa de interés a mensual y calcula el total de periodos $n = 12$).
3.  Una startup informática factura $80.000 \, \text{€}$ a un cliente corporativo, pero el pago se pacta a 180 días. Describe cómo el **Factoring sin recurso** puede ayudar al flujo de caja inmediato de la startup y qué riesgos asume cada parte.
