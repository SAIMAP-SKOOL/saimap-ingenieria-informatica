# Tema 6: Contabilidad Básica: Balance y Pérdidas y Ganancias

La contabilidad es el lenguaje financiero de los negocios. Registra de forma sistemática y ordenada todos los hechos económicos que afectan al patrimonio de la empresa. Para evaluar la salud de una organización y tomar decisiones directivas racionales, es fundamental comprender y dominar los dos estados contables principales: el **Balance de Situación** (que refleja el patrimonio en un instante de tiempo) y la **Cuenta de Pérdidas y Ganancias** (que detalla cómo se generó el resultado a lo largo de un ejercicio).

---

## 1. El Patrimonio de la Empresa y la Ecuación Fundamental

El **patrimonio** es el conjunto de bienes, derechos y obligaciones de una empresa en un momento dado:
1.  **Activo (Estructura Económica)**: Bienes (propiedades, ordenadores) y derechos de cobro (facturas pendientes de clientes) controlados por la empresa. Indica *en qué se ha empleado el dinero*.
2.  **Pasivo (Estructura Financiera Externa)**: Obligaciones de pago actuales con terceros (préstamos bancarios, deudas con proveedores). Indica *la financiación ajena*.
3.  **Patrimonio Neto o Recursos Propios (Estructura Financiera Interna)**: Fondos aportados por los socios (Capital Social) y beneficios no distribuidos acumulados (Reservas). Indica *la financiación propia*.

### Ecuación Fundamental de la Contabilidad
El Activo total debe estar financiado necesariamente por fondos propios o ajenos. Por lo tanto, el Balance debe estar siempre equilibrado (cuadrado):
$$\text{Activo} = \text{Pasivo} + \text{Patrimonio Neto}$$

---

## 2. El Balance de Situación y sus Masas Patrimoniales

El Balance de Situación agrupa las cuentas del patrimonio en masas según su función y grado de liquidez (en el Activo) o exigibilidad (en el Pasivo y Neto):

```
                     BALANCE DE SITUACIÓN
 +----------------------------------+----------------------------------+
 |              ACTIVO              |     PATRIMONIO NETO Y PASIVO     |
 +----------------------------------+----------------------------------+
 | **Activo No Corriente**          | **Patrimonio Neto (No Exigible)**|
 | Bienes de larga duración.        | Capital Social, Reservas.        |
 | *Inmovilizado Material e         |                                  |
 |  Intangible (Software, PCs)*     |                                  |
 +----------------------------------+----------------------------------+
 | **Activo Corriente**             | **Pasivo No Corriente (L/P)**    |
 | Circulante (corto plazo).        | Deudas a pagar a más de 1 año.   |
 | *Existencias, Clientes, Banco*   | **Pasivo Corriente (C/P)**       |
 |                                  | Deudas a pagar a menos de 1 año. |
 +----------------------------------+----------------------------------+
```

---

## 3. La Cuenta de Pérdidas y Ganancias (Cuenta de Resultados)

Detalla los ingresos y gastos generados durante el año. Su estructura en cascada permite calcular los distintos márgenes de beneficio intermedios:

```
    Ventas / Ingresos de Explotación
  - Gastos de Explotación (Personal, Suministros)
  =============================================================
  = EBITDA (Resultado de Explotación antes de amortizaciones)
  - Amortizaciones (Depreciación del inmovilizado)
  =============================================================
  = BAII / EBIT / Resultado de Explotación
  + Ingresos Financieros
  - Gastos Financieros (Intereses de préstamos)
  =============================================================
  = BAI / EBT (Beneficio antes de Impuestos)
  - Impuesto sobre Sociedades (ej. 25%)
  =============================================================
  = Beneficio Neto / Resultado Neto (BD)
```

---

## 4. El Toque Informático

### La Activación de Gastos de Desarrollo de Software en el Activo
Una de las grandes ventajas de las empresas tecnológicas de desarrollo de software es la posibilidad de **activar los gastos de desarrollo**:
*   Si los desarrolladores de la startup trabajan durante 6 meses en crear un nuevo software propio (como una plataforma SaaS innovadora) antes de lanzarlo al mercado, los salarios de esos programadores no tienen por qué contabilizarse directamente como gastos del año (lo que daría grandes pérdidas en la cuenta de resultados).
*   Bajo ciertas condiciones legales de viabilidad técnica y comercial, la empresa puede "activar" dichos costes, registrándolos en el Activo No Corriente del Balance bajo la partida de **Inmovilizado Intangible (Aplicaciones Informáticas)**. Esto aumenta el valor patrimonial total de la startup, haciéndola más atractiva y sólida de cara a ser valorada por fondos de inversión y bancos.

A continuación, implementamos en Python una utilidad que simula la Cuenta de Pérdidas y Ganancias en cascada, calculando automáticamente todos los márgenes intermedios hasta llegar al Beneficio Neto.

```python
def calcular_pérdidas_y_ganancias(ventas, personal, servidores, amortización, gastos_fin, tasa_impuesto=0.25):
    # Gastos de explotación
    gastos_explotacion = personal + servidores
    
    # 1. EBITDA
    ebitda = ventas - gastos_explotacion
    
    # 2. EBIT / BAII
    ebit = ebitda - amortización
    
    # 3. EBT / BAI
    ebt = ebit - gastos_fin
    
    # 4. Impuestos
    impuestos = max(0, ebt * tasa_impuesto) # Solo pagamos si hay beneficio
    
    # 5. Beneficio Neto (BD)
    beneficio_neto = ebt - impuestos
    
    print("=== CUENTA DE RESULTADOS (PÉRDIDAS Y GANANCIAS) ===")
    print(f"Ventas / Ingresos:       {ventas:10d} €")
    print(f"(-) Gastos Explotación:  {-gastos_explotacion:10d} €")
    print(f"(=) EBITDA:              {ebitda:10d} €")
    print(f"(-) Amortizaciones:      {-amortización:10d} €")
    print(f"(=) EBIT (BAII):         {ebit:10d} €")
    print(f"(-) Gastos Financieros:  {-gastos_fin:10d} €")
    print(f"(=) EBT (BAI):           {ebt:10d} €")
    print(f"(-) Impuestos ({int(tasa_impuesto*100)}%):    {-int(impuestos):10d} €")
    print(f"(=) BENEFICIO NETO:      {int(beneficio_neto):10d} €")
    return beneficio_neto

# Ejecución
calcular_pérdidas_y_ganancias(
    ventas=150000, personal=80000, servidores=15000, amortización=10000, gastos_fin=5000
)
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Clasificar los siguientes elementos patrimoniales en Activo No Corriente, Activo Corriente, Patrimonio Neto, Pasivo No Corriente o Pasivo Corriente:
1. Dinero en la cuenta corriente del banco ($12.000 \, \text{€}$).
2. Equipos informáticos (ordenadores y servidores de desarrollo) ($15.000 \, \text{€}$).
3. Facturas pendientes de cobro de clientes ($8.000 \, \text{€}$).
4. Préstamo bancario a devolver dentro de 5 años ($30.000 \, \text{€}$).
5. Aportación de capital de los socios fundadores ($20.000 \, \text{€}$).
6. Deuda de corto plazo con la Seguridad Social ($1.500 \, \text{€}$).

**Solución:**
1.  *Dinero en banco*: **Activo Corriente** (Tesorería de máxima liquidez).
2.  *Equipos informáticos*: **Activo No Corriente** (Inmovilizado material de larga duración).
3.  *Facturas pendientes de clientes*: **Activo Corriente** (Derechos de cobro a corto plazo).
4.  *Préstamo a 5 años*: **Pasivo No Corriente** (Obligación de pago exigible a largo plazo).
5.  *Aportación de capital*: **Patrimonio Neto** (Recursos propios no exigibles).
6.  *Deuda a corto plazo con Seguridad Social*: **Pasivo Corriente** (Obligación exigible a corto plazo).

### Ejercicio 2
Un estudio de diseño web reporta los siguientes datos anuales: Ventas = $90.000 \, \text{€}$, Salarios del personal = $45.000 \, \text{€}$, Licencias de software = $5.000 \, \text{€}$, Amortización de ordenadores = $4.000 \, \text{€}$, Gastos de intereses bancarios = $2.000 \, \text{€}$. Sabiendo que la tasa del impuesto de sociedades es del 25%, calcular el EBITDA, el EBIT (Resultado de Explotación) y el Beneficio Neto final de la empresa.

**Solución:**
Realizamos el cálculo en cascada:
1.  **Cálculo de EBITDA**:
    *   Ingresos de Explotación (Ventas) = $90.000 \, \text{€}$.
    *   Gastos de Explotación (Salarios + Licencias) = $45.000 + 5.000 = 50.000 \, \text{€}$.
    *   $$\text{EBITDA} = 90.000 - 50.000 = 40.000 \, \text{€}$$
2.  **Cálculo de EBIT (BAII)**:
    *   $$\text{EBIT} = \text{EBITDA} - \text{Amortización} = 40.000 - 4.000 = 36.000 \, \text{€}$$
3.  **Cálculo de EBT (BAI)**:
    *   $$\text{EBT} = \text{EBIT} - \text{Gastos Financieros} = 36.000 - 2.000 = 34.000 \, \text{€}$$
4.  **Cálculo de Impuestos y Beneficio Neto**:
    *   Impuestos (25%) = $34.000 \cdot 0.25 = 8.500 \, \text{€}$.
    *   $$\text{Beneficio Neto} = \text{EBT} - \text{Impuestos} = 34.000 - 8.500 = 25.500 \, \text{€}$$

La empresa obtiene un EBITDA de $40.000 \, \text{€}$, un Resultado de Explotación (EBIT) de $36.000 \, \text{€}$ y un Beneficio Neto de $25.500 \, \text{€}$.

---

## 6. Ejercicios Propuestos

1.  Dada la siguiente lista de cuentas patrimoniales, redacta un Balance de Situación ordenado en masas patrimoniales y comprueba que se cumple la ecuación fundamental:
    *   Dinero en caja = $5.000 \, \text{€}$
    *   Ordenadores = $10.000 \, \text{€}$
    *   Capital Social = $12.000 \, \text{€}$
    *   Reservas = $3.000 \, \text{€}$
    *   Préstamos bancarios a largo plazo = $4.000 \, \text{€}$
    *   Deudas con proveedores = $2.000 \, \text{€}$
    *   Existencias en almacén = $6.000 \, \text{€}$
2.  Explica la diferencia entre el concepto contable de "Gasto" e "Inversión" y aporta un ejemplo de cada uno en una empresa de desarrollo de páginas web.
3.  ¿Qué es la amortización contable (o depreciación) de un activo y cómo afecta al cálculo del impuesto de sociedades de la empresa?
