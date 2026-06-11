# MANUAL COMPLETO DE FUNDAMENTOS DE ADMINISTRACIÓN DE EMPRESAS
### Grado en Ingeniería Informática - 1º Curso

Este documento unifica todos los temas del plan de estudio de administración de empresas, estrategia corporativa, matemáticas financieras, valoración de proyectos de inversión y marketing SaaS en un único manual para facilitar su lectura, impresión o conversión a formatos como PDF.

---

## Índice General del Manual

*   **Bloque 1: Organización, Estrategia y Entorno (Semanas 1 a 5)**
    *   [Tema 1: La Empresa como Sistema y su Entorno](#tema-1-la-empresa-como-sistema-y-su-entorno)
    *   [Tema 2: Creación de Empresas y el Plan de Negocio](#tema-2-creación-de-empresas-y-el-plan-de-negocio)
    *   [Tema 3: Análisis Estratégico: DAFO y Fuerzas de Porter](#tema-3-análisis-estratégico-dafo-y-fuerzas-de-porter)
    *   [Tema 4: Toma de Decisiones, Planificación y Control](#tema-4-toma-de-decisiones-planificación-y-control)
    *   [Tema 5: Estructuras Organizativas y Dirección de Recursos Humanos](#tema-5-estructuras-organizativas-y-dirección-de-recursos-humanos)
*   **Bloque 2: Gestión Económico-Financiera (Semanas 6 a 11)**
    *   [Tema 6: Contabilidad Básica: Balance y Cuenta de Pérdidas y Ganancias](#tema-6-contabilidad-básica-balance-y-cuenta-de-pérdidas-y-ganancias)
    *   [Tema 7: Matemáticas Financieras: Capitalización y Rentas](#tema-7-matemáticas-financieras-capitalización-y-rentas)
    *   [Tema 8: Selección de Inversiones: Payback, VAN y TIR](#tema-8-selección-de-inversiones-payback-van-y-tir)
    *   [Tema 9: Decisiones de Financiación: Fuentes Internas y Externas](#tema-9-decisiones-de-financiación-fuentes-internas-y-externas)
*   **Bloque 3: Comercialización y Marketing (Semanas 12 a 15)**
    *   [Tema 10: El Mercado, Investigación de Mercados y Segmentación](#tema-10-el-mercado-investigación-de-mercados-y-segmentación)
    *   [Tema 11: El Marketing Mix y las 4Ps en Empresas de Software y SaaS](#tema-11-el-marketing-mix-y-sus-variables-en-el-sector-tecnológico)
*   **Secciones Finales**
    *   [Glosario de Términos](#glosario-de-términos)
    *   [Bibliografía Recomendada](#bibliografía-recomendada)

<div style="page-break-after: always;"></div>

# Tema 1: La Empresa como Sistema y su Entorno

Una empresa es una unidad económica de producción que combina factores productivos (capital, trabajo y recursos naturales) bajo la dirección del empresario para producir bienes o prestar servicios que satisfagan las necesidades del mercado, con el objetivo de obtener el máximo beneficio económico o cumplir un fin social. En el ámbito moderno, la empresa se modela como un **sistema abierto** interconectado de forma constante con un entorno cambiante.

---

## 1. La Empresa como Sistema Abierto

Un sistema es un conjunto de elementos interrelacionados que buscan un objetivo común. La empresa es un **sistema abierto** porque interactúa activamente con el exterior:

```
        Entradas (Inputs)       Transformación         Salidas (Outputs)
     +--------------------+   +-------------------+   +------------------+
     | Capital, Trabajo,  |-->|  Diseño, Soporte, |-->| Software, Apps,  |
     | Servidores, APIs   |   |  Codificación     |   | Servicios SaaS   |
     +--------------------+   +-------------------+   +------------------+
               ^                                                |
               |               Feedback / Realimentación        |
               +------------------------------------------------+
                                    Ventas, Bugs, Métricas
```

*   **Entradas (Inputs)**: Recursos del entorno (dinero, materias primas, servidores, ingenieros, APIs de terceros).
*   **Proceso de Transformación**: La actividad interna que añade valor (por ejemplo, escribir líneas de código para estructurar una aplicación).
*   **Salidas (Outputs)**: Bienes o servicios terminados (licencias de software, plataformas web en producción, servicios SaaS).
*   **Realimentación (Feedback)**: Control de desviaciones (métricas de ventas, informes de errores/bugs de usuarios, datos financieros) para ajustar el proceso.

### Áreas Funcionales de la Empresa
Tradicionalmente se organiza en departamentos:
1.  **Producción / Operaciones**: Fabricación del producto o desarrollo del software.
2.  **Comercial / Marketing**: Venta del producto y relación con los clientes.
3.  **Financiera**: Captación de fondos y gestión presupuestaria.
4.  **Recursos Humanos**: Selección, formación y motivación del personal.
5.  **I+D+i (Investigación, Desarrollo e Innovación)**: Creación de nuevas tecnologías o procesos.

---

## 2. Formas Jurídicas de la Empresa (Legislación Española)

Al constituir una empresa, los fundadores deben elegir una personalidad jurídica. Las principales son:

*   **Empresario Individual (Autónomo)**:
    *   Persona física. No requiere capital mínimo de constitución.
    *   **Responsabilidad ilimitada**: El fundador responde de las deudas del negocio con todo su patrimonio presente y futuro.
*   **Sociedad de Responsabilidad Limitada (S.R.L. o S.L.)**:
    *   Persona jurídica de carácter mercantil. Capital dividido en *participaciones* sociales (no cotizables).
    *   Capital mínimo de constitución: Históricamente $3000 \, \text{€}$ (actualmente la ley permite $1 \, \text{€}$ bajo reserva de acumulación de reservas).
    *   **Responsabilidad limitada** al capital aportado por los socios.
*   **Sociedad Anónima (S.A.)**:
    *   Pensada para grandes corporaciones. Capital dividido en *acciones* libremente transmisibles que pueden cotizar en bolsa.
    *   Capital mínimo de constitución: $60.000 \, \text{€}$ (debe estar desembolsado al menos el 25% en el acto).
    *   **Responsabilidad limitada** al capital aportado.

---

## 3. El Entorno de la Empresa

Ninguna empresa opera aislada. Su rendimiento depende directamente de factores externos:

### 3.1 Entorno General (PESTEL)
Factores que afectan por igual a todas las empresas de un país o región geográfica:
*   **P**olíticos: Regulaciones gubernamentales, estabilidad política, subvenciones públicas.
*   **E**conómicos: Inflación, tipos de interés de los créditos, tasa de desempleo.
*   **S**ocio-culturales: Hábitos de consumo, nivel educativo, tendencias demográficas.
*   **T**ecnológicos: Aparición de nuevas tecnologías (IA, Cloud Computing, velocidad de redes).
*   **E**cológicos (Medioambientales): Cambio climático, leyes de reciclaje de residuos.
*   **L**egales: Ley de Protección de Datos (RGPD), leyes laborales de contratación.

### 3.2 Entorno Específico
Factores que afectan directamente a las empresas de un sector concreto: competidores directos, clientes (compradores), proveedores de tecnología y servicios, e intermediarios financieros.

---

## 4. El Toque Informático

### Elección de Forma Jurídica para Startups de Software
Al iniciar una startup tecnológica (por ejemplo, una nueva plataforma SaaS de gestión de proyectos), los fundadores optan casi siempre por constituir una **Sociedad Limitada (S.L.)**. El desarrollo de software innovador tiene un alto riesgo de fracaso comercial durante los primeros años; la S.L. protege el patrimonio personal de los programadores fundadores contra reclamaciones de acreedores (como proveedores de servidores en la nube) en caso de quiebra.

Cuando la startup tiene éxito y entra en fases avanzadas de financiación, los fondos de **Capital Riesgo (Venture Capital)** exigen la transformación de la empresa en una **Sociedad Anónima (S.A.)**. Las acciones de una S.A. son más fáciles de transferir, permitiendo que los inversores adquieran o vendan participaciones de forma instantánea, o preparen la salida de la compañía a bolsa (OPV).

A continuación, implementamos en Python una utilidad estructurada que categoriza y muestra de forma limpia los factores ambientales PESTEL de una empresa de desarrollo de software.

```python
def analizar_pestel_tech():
    pestel = {
        "Politico": "Subvenciones europeas para digitalización de pymes (fondos NextGen).",
        "Economico": "Subida de tipos de interés que reduce la financiación de capital riesgo.",
        "Sociocultural": "Aceptación masiva del teletrabajo y demanda de herramientas colaborativas.",
        "Tecnologico": "Madurez de los modelos de Inteligencia Artificial generativa y APIs en la nube.",
        "Ecologico": "Presión para reducir la huella de carbono de los grandes centros de datos (Datacenters).",
        "Legal": "Cumplimiento estricto del Reglamento General de Protección de Datos (RGPD) en la UE."
    }
    
    print("=== ANÁLISIS PESTEL PARA STARTUP DE SOFTWARE ===")
    for factor, descripcion in pestel.items():
        print(f"[{factor:13s}] : {descripcion}")

# Ejecución
analizar_pestel_tech()
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Comparar la Sociedad de Responsabilidad Limitada (S.L.) y la Sociedad Anónima (S.A.) en base a sus requisitos de capital mínimo de constitución, división de la propiedad y transferibilidad.

**Solución:**
La comparación formal se estructura en tres ejes:
1.  **Capital Mínimo de Constitución**:
    *   *S.L.*: Capital mínimo de $3.000 \, \text{€}$ (se permite la constitución con $1 \, \text{€}$ bajo condiciones especiales de retención de reservas).
    *   *S.A.*: Capital mínimo de $60.000 \, \text{€}$, requiriendo que esté desembolsado al menos el 25% en el momento de la firma notarial.
2.  **División de la Propiedad**:
    *   *S.L.*: El capital se divide en **participaciones** sociales.
    *   *S.A.*: El capital se divide en **acciones**.
3.  **Transferibilidad**:
    *   *S.L.*: Las participaciones tienen una transmisión restringida. Los socios existentes suelen tener derecho de adquisición preferente. No pueden cotizar en bolsa.
    *   *S.A.*: Las acciones son libremente transmisibles por naturaleza. Es la forma ideal si la empresa desea captar capital cotizando en los mercados financieros.

### Ejercicio 2
Clasificar los siguientes sucesos en el Entorno General o Entorno Específico de una consultora de desarrollo de aplicaciones móviles:
1. La aprobación de una nueva directiva europea sobre el uso ético de la Inteligencia Artificial.
2. La entrada de un nuevo competidor local que ofrece desarrollo de apps de bajo coste.
3. Una subida generalizada de los tipos de interés por parte del Banco Central Europeo.
4. El lanzamiento de un nuevo servicio de alojamiento web por parte de Amazon Web Services (proveedor clave de la consultora).

**Solución:**
1.  *Nueva directiva europea de IA*: **Entorno General (Factor Legal/Tecnológico)**. Afecta a todas las empresas del ámbito geográfico europeo que desarrollen software.
2.  *Entrada de competidor local*: **Entorno Específico**. Afecta directamente al nicho competitivo de la consultora de apps.
3.  *Subida de tipos de interés*: **Entorno General (Factor Económico)**. Afecta al coste del dinero de todo el tejido empresarial nacional.
4.  *Lanzamiento de AWS*: **Entorno Específico**. Afecta de forma directa a la estructura de costes y arquitectura de la consultora (relación directa con proveedores).

---

## 6. Ejercicios Propuestos

1.  Dibuja un diagrama del sistema abierto de una empresa proveedora de hosting en la nube, identificando explícitamente al menos 3 inputs, el proceso de transformación, 2 outputs y el feedback loop.
2.  Explica la diferencia entre la responsabilidad limitada y la responsabilidad ilimitada de los socios en la constitución de una empresa, detallando las consecuencias prácticas ante una quiebra.
3.  Realiza el análisis PESTEL para un nuevo buscador web enfocado en la privacidad del usuario en el escenario tecnológico actual.


<div style="page-break-after: always;"></div>

# Tema 2: Creación de Empresas y el Plan de Negocio

La creación de una empresa es un proceso de transformación de una idea innovadora en una realidad económica viable. El **Plan de Negocio (Business Plan)** es el documento estratégico fundamental que recoge de forma ordenada los análisis de mercado, técnicos, organizativos y financieros que demuestran la viabilidad del proyecto ante socios, inversores y entidades de financiación.

---

## 1. El Espíritu Emprendedor y la Startup Tecnológica

El **espíritu emprendedor** es la capacidad de identificar oportunidades de negocio, asumir el riesgo financiero y organizativo asociado, y movilizar recursos para lanzar un proyecto con éxito.
*   **Startup Tecnológica**: Organización temporal diseñada para buscar un modelo de negocio repetible y escalable, caracterizada por un crecimiento exponencial potencial y el uso intensivo de tecnologías digitales. Suelen requerir rondas de financiación externa debido a que sus flujos de caja iniciales son negativos mientras desarrollan la tecnología.

---

## 2. Estructura de un Plan de Negocio (Business Plan)

Un plan de negocio formal se divide en las siguientes secciones secuenciales:

1.  **Resumen Ejecutivo (Executive Summary)**: Breve síntesis (1-2 páginas) que resume la propuesta de valor, el modelo de negocio, el mercado objetivo y las necesidades financieras. Es la sección más crítica para captar el interés de un inversor.
2.  **Propuesta de Valor (Producto/Servicio)**: Descripción detallada de la tecnología, qué problema resuelve en el mercado y las ventajas competitivas.
3.  **Plan de Marketing**: Análisis de mercado, estrategia de segmentación, posicionamiento y las variables del Marketing Mix (producto, precio, distribución y comunicación).
4.  **Plan de Operaciones**: Infraestructura técnica y procesos requeridos para prestar el servicio o fabricar el producto.
5.  **Organización y Recursos Humanos**: Estructura del equipo fundador, roles técnicos clave y organigrama.
6.  **Plan Financiero**: Análisis cuantitativo que demuestra la viabilidad económica: balance previsional, estados de flujo de caja, cuenta de pérdidas y ganancias previsional, y cálculo del **Punto de Equilibrio**.

---

## 3. El Punto de Equilibrio (Umbral de Rentabilidad)

El **Punto de Equilibrio** o **Umbral de Rentabilidad** ($Q^*$) es el volumen de ventas (medido en unidades físicas o de suscripción) para el cual el beneficio de la empresa es exactamente cero. En este punto, los ingresos totales igualan a los costes totales:
$$\text{Ingresos Totales} = \text{Costes Totales}$$

### Deducción Matemática
1.  Ingresos Totales: $IT = P \cdot Q$ (donde $P$ es el precio unitario y $Q$ es la cantidad vendida).
2.  Costes Totales: $CT = CF + CV_u \cdot Q$ (donde $CF$ representa los Costes Fijos totales y $CV_u$ el Coste Variable unitario por producto).
3.  Buscamos el beneficio cero:
    $$P \cdot Q^* - (CF + CV_u \cdot Q^*) = 0$$
    $$P \cdot Q^* - CV_u \cdot Q^* = CF$$
    $$Q^* (P - CV_u) = CF$$
4.  Fórmula final:
    $$Q^* = \frac{CF}{P - CV_u}$$

*   **Margen de Contribución ($P - CV_u$)**: Cantidad que cada unidad vendida aporta para cubrir los costes fijos de la empresa.

---

## 4. El Toque Informático

### El Plan de Operaciones en una Startup SaaS (Software as a Service)
A diferencia de las empresas industriales tradicionales, el Plan de Operaciones de una empresa de software no contempla almacenes físicos, camiones de reparto ni maquinaria pesada de fabricación. En su lugar, detalla:
*   **Infraestructura de Hosting (Cloud Computing)**: Elección de proveedores cloud (ej. AWS, Google Cloud, Azure), arquitectura de servidores escalable (microservicios, Kubernetes) y cálculo de costes variables según tráfico.
*   **Plataforma de Integración Continua (CI/CD)**: Flujo de trabajo para subir código nuevo a producción de forma segura (GitHub, GitLab CI, Jenkins).
*   **Soporte de Pasarelas de Pago**: Integraciones de suscripciones automatizadas recurrentes (ej. Stripe, PayPal).
*   **Ciberseguridad y Copias de Seguridad**: Políticas de encriptación de datos de usuarios y copias de respaldo automatizadas en diferentes regiones geográficas.

A continuación, implementamos en Python una calculadora financiera para hallar el punto de equilibrio mensual de una startup de software basada en suscripción mensual.

```python
def calcular_punto_equilibrio(costes_fijos, precio_suscripcion, coste_variable_usuario):
    # costes_fijos: Coste mensual de servidores, oficinas y salarios
    # precio_suscripcion: Cuota mensual que paga el usuario
    # coste_variable_usuario: Coste mensual de API y hosting que consume cada usuario
    
    margen_contribucion = precio_suscripcion - coste_variable_usuario
    
    if margen_contribucion <= 0:
        print("[ERROR] El precio de suscripción no cubre el coste variable unitario.")
        return None
        
    punto_equilibrio = costes_fijos / margen_contribucion
    
    print("=== ANÁLISIS DE UMBRAL DE RENTABILIDAD ===")
    print(f"Costes Fijos Mensuales:  {costes_fijos} €")
    print(f"Precio de Suscripción:   {precio_suscripcion} €/mes")
    print(f"Coste Variable/Usuario:  {coste_variable_usuario} €/mes")
    print(f"Margen de Contribución:  {margen_contribucion} €")
    print(f"Punto de Equilibrio:     {punto_equilibrio:.2f} usuarios activos necesarios al mes.")
    return punto_equilibrio

# Ejecución de prueba
calcular_punto_equilibrio(costes_fijos=15000, precio_suscripcion=29, coste_variable_usuario=4)
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Una startup SaaS que desarrolla un software ERP en la nube estima unos costes fijos anuales de $120.000 \, \text{€}$ (salarios de desarrolladores, alquiler de oficina y amortización de equipos). El precio de la suscripción anual para las pymes clientes es de $250 \, \text{€}$ y los costes variables asociados por alojar los datos de cada cliente en los servidores son de $50 \, \text{€/año}$. Calcular:
1. El número mínimo de clientes anuales necesarios para que la empresa empiece a obtener beneficios (punto de equilibrio).
2. El beneficio que obtendrá la startup si alcanza las 800 suscripciones anuales.

**Solución:**
1.  **Cálculo del punto de equilibrio ($Q^*$)**:
    *   Costes Fijos: $CF = 120.000 \, \text{€}$.
    *   Precio Unitario: $P = 250 \, \text{€}$.
    *   Coste Variable Unitario: $CV_u = 50 \, \text{€}$.
    *   Fórmula:
        $$Q^* = \frac{CF}{P - CV_u} = \frac{120.000}{250 - 50} = \frac{120.000}{200} = 600 \text{ clientes}$$
    La empresa necesita exactamente 600 suscripciones anuales activas para cubrir todos sus costes.
2.  **Cálculo del beneficio para $Q = 800$**:
    *   Ingresos Totales: $IT = P \cdot Q = 250 \cdot 800 = 200.000 \, \text{€}$.
    *   Costes Totales: $CT = CF + CV_u \cdot Q = 120.000 + 50 \cdot 800 = 120.000 + 40.000 = 160.000 \, \text{€}$.
    *   Beneficio: $B = IT - CT = 200.000 - 160.000 = 40.000 \, \text{€}$.

Con 800 clientes, la startup obtendrá un beneficio neto anual de $40.000 \, \text{€}$.

### Ejercicio 2
Explicar la diferencia entre costes fijos y costes variables en una empresa de desarrollo de videojuegos, aportando al menos 2 ejemplos de cada uno.

**Solución:**
*   **Costes Fijos**: Son aquellos gastos que no dependen de la cantidad de videojuegos vendidos o del tráfico en el servidor; permanecen constantes en el corto plazo.
    *   *Ejemplo 1*: El alquiler mensual de la oficina de desarrollo.
    *   *Ejemplo 2*: Los salarios mensuales netos del equipo de diseñadores y programadores.
*   **Costes Variables**: Son aquellos que varían de forma proporcional al nivel de actividad comercial o producción.
    *   *Ejemplo 1*: La comisión por venta (típicamente del 30%) que se cobra en plataformas de distribución digital (Steam, Apple App Store, Google Play).
    *   *Ejemplo 2*: Los costes de computación y transferencia de datos de los servidores multijugador en la nube (AWS), que crecen a mayor volumen de usuarios concurrentes.

---

## 6. Ejercicios Propuestos

1.  Calcula el punto de equilibrio para una tienda online de venta de periféricos informáticos cuyos costes fijos anuales son de $45.000 \, \text{€}$, sabiendo que el precio medio de venta de un teclado mecánico es de $80 \, \text{€}$ y su coste de adquisición al fabricante es de $50 \, \text{€}$ por unidad.
2.  Explica en qué consiste el concepto de "escalabilidad" en un modelo de negocio de base tecnológica y por qué es más fácil de escalar que un negocio tradicional de manufactura física.
3.  Escribe un resumen ejecutivo de un plan de negocio ficticio para una aplicación móvil destinada a conectar tutores universitarios con estudiantes de primer curso.


<div style="page-break-after: always;"></div>

# Tema 3: Análisis Estratégico de la Empresa: DAFO y Porter

La dirección estratégica es el proceso mediante el cual la empresa formula, implementa y controla decisiones de largo plazo que le permiten adaptarse a su entorno y lograr una **ventaja competitiva** sostenible en el tiempo. Para tomar estas decisiones, es necesario realizar un diagnóstico tanto interno de las capacidades de la organización como externo de las fuerzas del sector del mercado.

---

## 1. Niveles de Estrategia en la Empresa

La planificación estratégica se divide en tres niveles organizativos:
1.  **Estrategia Corporativa (Global)**: Define el alcance de la empresa (en qué negocios quiere competir, adquisiciones de otras empresas, fusiones o diversificación).
2.  **Estrategia Competitiva (o de Negocio)**: Define cómo debe competir la empresa en cada uno de sus sectores específicos (por ejemplo, a través de liderazgo en costes o diferenciación).
3.  **Estrategia Funcional**: Optimiza el uso de los recursos en los departamentos diarios (finanzas, desarrollo de software, recursos humanos) para dar soporte a las estrategias superiores.

---

## 2. Diagnóstico Estratégico Interno y Externo: Matriz DAFO

La **Matriz DAFO** (SWOT por sus siglas en inglés) es la herramienta de diagnóstico estratégico más utilizada. Permite cruzar el análisis interno de la empresa con el análisis de su entorno:

| Origen | Factores Positivos | Factores Negativos |
| :--- | :--- | :--- |
| **Ámbito Interno** (Controlable por la empresa) | **Fortalezas ($F$)**: Recursos propios y capacidades que otorgan ventaja competitiva (ej. equipo técnico altamente cualificado). | **Debilidades ($D$)**: Puntos débiles internos que limitan el rendimiento (ej. falta de capital financiero). |
| **Ámbito Externo** (No controlable) | **Oportunidades ($O$)**: Tendencias ambientales positivas que la empresa puede aprovechar (ej. nueva ley que fomenta el teletrabajo). | **Amenazas ($A$)**: Tendencias externas negativas que pueden perjudicar el negocio (ej. aumento de costes de energía). |

---

## 3. Análisis Competitivo del Sector: Las 5 Fuerzas de Porter

El economista Michael Porter definió un marco de análisis para determinar el nivel de competitividad y atractivo (rentabilidad potencial) de un sector específico basándose en cinco fuerzas de presión:

```
                  Amenaza de nuevos Competidores
                                |
                                v
     Poder de                                     Poder de
   Proveedores --->  RIVALIDAD DEL SECTOR  <---   Clientes
                                ^
                                |
                  Amenaza de Prod. Sustitutivos
```

1.  **Amenaza de nuevos Competidores Entrantes**: Depende de las **barreras de entrada** (requisitos de capital, patentes, economías de escala).
2.  **Poder de negociación de los Proveedores**: Capacidad de los proveedores para imponer precios (alto si hay pocos proveedores o no hay sustitutos).
3.  **Poder de negociación de los Clientes (Compradores)**: Capacidad de los compradores para exigir bajadas de precio o mejoras de calidad (alto si hay muchos competidores).
4.  **Amenaza de Productos Sustitutivos**: Productos que satisfacen la misma necesidad de forma alternativa (alto si la relación calidad-precio del sustituto es buena).
5.  **Rivalidad entre los Competidores existentes**: Fuerza central que depende del crecimiento del sector, número de competidores y barreras de salida.

### Estrategias Competitivas Genéricas
Para hacer frente a estas fuerzas, Porter propuso tres estrategias:
*   **Liderazgo en Costes**: Ser el productor más barato del sector (ej. Ryanair).
*   **Diferenciación de Producto**: Hacer que el producto sea percibido como único por el cliente (ej. Apple).
*   **Enfoque o Segmentación (Nicho)**: Centrarse en un grupo específico de clientes o zona geográfica, especializándose.

---

## 4. El Toque Informático

### Barreras de Entrada y Costes de Cambio (Switching Costs) en Software
En el sector del desarrollo de software y servicios SaaS, los líderes tecnológicos crean barreras de entrada masivas mediante los **costes de cambio (switching costs)** para los usuarios:
*   Una empresa que almacena todas sus bases de datos en un ecosistema propietario (como Microsoft Office/Azure o Apple iOS) se enfrenta a costes de cambio gigantescos (tanto de dinero como de tiempo de formación de personal y migración técnica) si decide mudarse a Linux o Android.
*   Este "bloqueo del cliente" (Vendor Lock-in) neutraliza dos fuerzas de Porter a la vez: reduce drásticamente el poder de negociación de los clientes (que no pueden irse fácilmente) y reduce la amenaza de productos sustitutivos.

A continuación, implementamos en Python un formateador de la Matriz DAFO para estructurar las variables estratégicas de una startup de IA.

```python
def imprimir_dafo_tech(fortalezas, debilidades, oportunidades, amenazas):
    # DAFO formateado en consola
    ancho = 40
    print("+" + "-"*ancho + "+" + "-"*ancho + "+")
    print(f"| {'FORTALEZAS (Ámbito Interno)':^{ancho-2}} | {'DEBILIDADES (Ámbito Interno)':^{ancho-2}} |")
    print("+" + "-"*ancho + "+" + "-"*ancho + "+")
    for f, d in zip(fortalezas, debilidades):
        print(f"| - {f:<{ancho-4}} | - {d:<{ancho-4}} |")
    print("+" + "-"*ancho + "+" + "-"*ancho + "+")
    print(f"| {'OPORTUNIDADES (Ámbito Externo)':^{ancho-2}} | {'AMENAZAS (Ámbito Externo)':^{ancho-2}} |")
    print("+" + "-"*ancho + "+" + "-"*ancho + "+")
    for o, a in zip(oportunidades, amenazas):
        print(f"| - {o:<{ancho-4}} | - {a:<{ancho-4}} |")
    print("+" + "-"*ancho + "+" + "-"*ancho + "+")

# Variables de prueba
f = ["Algoritmo propio patentado", "Equipo de desarrollo senior"]
d = ["Falta de tracción de marketing", "Presupuesto inicial bajo"]
o = ["Aumento de demanda de ciberseguridad", "Fondos públicos para startups"]
a = ["Competidores gigantescos (Google/MS)", "Falta de perfiles de IA seniors"]

imprimir_dafo_tech(f, d, o, a)
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Elaborar la matriz DAFO cruzada para una startup de reciente creación dedicada al desarrollo de software médico de diagnóstico por imagen.

**Solución:**
Identificamos las variables internas y externas del proyecto:
1.  **Ámbito Interno (Fortalezas y Debilidades)**:
    *   *Fortaleza 1*: Equipo médico experto a bordo de la dirección del proyecto.
    *   *Fortaleza 2*: Alta precisión del algoritmo de IA en fases de prueba clínicas.
    *   *Debilidad 1*: Desconocimiento de los procesos de regulación sanitaria.
    *   *Debilidad 2*: Ningún canal de venta o contactos en hospitales públicos establecidos.
2.  **Ámbito Externo (Oportunidades y Amenazas)**:
    *   *Oportunidad 1*: Aumento del envejecimiento demográfico que satura el sistema de salud.
    *   *Oportunidad 2*: Programas estatales de subvención para incorporar IA en sanidad.
    *   *Amenaza 1*: Lentitud en los plazos de aprobación administrativa sanitaria.
    *   *Amenaza 2*: Rivalidad intensa por parte de grandes multinacionales del diagnóstico médico (General Electric Healthcare, Siemens Healthineers).

### Ejercicio 2
Analizar, utilizando el modelo de las 5 Fuerzas de Porter, la amenaza de productos sustitutivos para una empresa que ofrece una aplicación web de videoconferencias corporativas.

**Solución:**
La amenaza de productos sustitutivos evalúa la existencia de otras formas alternativas de cubrir la necesidad de comunicación:
1.  **Definir la necesidad**: Comunicación y colaboración remota en tiempo real.
2.  **Identificar competidores directos**: Zoom, Microsoft Teams, Google Meet (son rivales directos del sector, no sustitutos).
3.  **Identificar productos sustitutivos**:
    *   *Sustituto 1*: Llamadas telefónicas de voz tradicionales (cubren la comunicación de forma rápida y barata pero sin vídeo ni pantalla compartida).
    *   *Sustituto 2*: El correo electrónico corporativo y aplicaciones de chat asíncrono como Slack o Discord (permiten colaboración sin sincronización en tiempo real).
    *   *Sustituto 3 (Físico)*: Las reuniones y viajes de negocios presenciales (alto coste de tiempo y transporte, pero con mayor empatía y efectividad en negociaciones críticas).
4.  **Evaluación de la amenaza**:
    La amenaza de sustitutos es **media-alta**. Si el precio de las plataformas de videoconferencia sube de forma desmedida, las pymes volverán a recurrir de forma generalizada a llamadas y correos electrónicos combinados con chats asíncronos.

---

## 6. Ejercicios Propuestos

1.  Dibuja la matriz DAFO de tu perfil profesional o de un proyecto de fin de grado que tengas en mente.
2.  Explica cómo afectan las **economías de escala** (reducción del coste medio por unidad a medida que crece el volumen de producción) a la amenaza de nuevos competidores entrantes en un mercado según Porter.
3.  Analiza la fuerza de "Poder de negociación de los proveedores" para una startup de desarrollo de videojuegos independientes que depende de los motores Unreal Engine y Unity para crear sus productos.


<div style="page-break-after: always;"></div>

# Tema 4: Toma de Decisiones, Planificación y Control Directivo

La función directiva dentro de la empresa se compone de un ciclo continuo de cuatro fases: **Planificar** (qué hacer y cómo), **Organizar** (asignar recursos), **Dirigir** (liderar al personal) y **Controlar** (verificar desviaciones). En este tema analizamos el núcleo administrativo de este proceso: la toma de decisiones estratégicas, la estructuración de planes y la auditoría y control de los presupuestos empresariales.

---

## 1. El Proceso de Toma de Decisiones

Decidir es elegir la mejor alternativa entre varias disponibles para resolver un problema o aprovechar una oportunidad.

### Fases del Proceso Decisorio
1.  **Identificar el problema** o la oportunidad de negocio.
2.  **Identificar los criterios de decisión** (coste, tiempo, calidad, riesgos).
3.  **Desarrollar las alternativas** de solución.
4.  **Analizar las alternativas** evaluando su impacto respecto a los criterios.
5.  **Seleccionar e implementar** la mejor alternativa.
6.  **Evaluar la eficacia** de la decisión tomada.

### Clasificación de Decisiones
*   **Decisiones Programadas (Estructuradas)**: Son repetitivas y rutinarias. Existe un procedimiento preestablecido para resolverlas (ej. política de devoluciones).
*   **Decisiones No Programadas (No Estructuradas)**: Son únicas y complejas, sin precedentes claros. Requieren análisis a medida (ej. fusionarse con otra empresa, migrar toda la infraestructura a un nuevo lenguaje de programación).

---

## 2. Ambientes de Decisión

El nivel de información del que dispone el decisor define el ambiente en el que opera:
1.  **Certeza**: Se conocen con absoluta seguridad las consecuencias de cada alternativa.
2.  **Riesgo**: No se conoce el resultado exacto, pero se dispone de datos históricos para asignar **probabilidades** a las consecuencias. Se decide calculando el Valor Esperado ($VE = \sum P_i \cdot V_i$).
3.  **Incertidumbre**: No se conocen los resultados ni se dispone de datos para asignar probabilidades. Se decide aplicando criterios subjetivos de teoría de juegos:
    *   *Criterio Optimista (MaxiMax)*: Elegir la alternativa con el mejor resultado posible.
    *   *Criterio Pesimista (MaxiMin)*: Elegir el mejor resultado de entre los peores casos posibles (seguridad).
    *   *Criterio de Laplace*: Asumir la misma probabilidad para todos los casos.

---

## 3. Planificación y Control: El Cuadro de Mando Integral (CMI)

*   **Planificación**: Consiste en definir la **Misión** (razón de ser actual de la empresa), la **Visión** (qué quiere ser en el futuro) y los **Objetivos SMART** (Específicos, Medibles, Alcanzables, Relevantes y Acotados en el tiempo).
*   **Control**: Es la medición de los resultados reales frente a los planificados para detectar y corregir **desviaciones presupuestarias**.
*   **Cuadro de Mando Integral (Balanced Scorecard)**: Es una herramienta de control estratégico que monitoriza la empresa desde cuatro perspectivas equilibradas en lugar de mirar solo los indicadores financieros tradicionales:
    1.  *Perspectiva Financiera*: Rentabilidad, crecimiento de ingresos.
    2.  *Perspectiva del Cliente*: Satisfacción, retención, cuota de mercado.
    3.  *Perspectiva de Procesos Internos*: Calidad, tiempos de fabricación, innovación.
    4.  *Perspectiva de Aprendizaje y Crecimiento (RRHH)*: Formación, retención del talento, clima laboral.

---

## 4. El Toque Informático

### Cuadros de Mando (Dashboards) y KPIs en Desarrollo Ágil de Software
En el desarrollo de software moderno (bajo metodologías ágiles como Scrum), el control directivo se realiza en tiempo real de forma automatizada mediante plataformas de gestión de proyectos (Jira, GitHub Projects, Azure DevOps).
La directiva y los gestores de proyecto (Product Owners) controlan los siguientes **KPIs (Key Performance Indicators)** en un cuadro de mando:
*   **Sprint Velocity (Velocidad de Sprint)**: Puntos de historia completados en promedio por el equipo de desarrollo. Sirve para planificar entregas futuras de forma fiable.
*   **Lead Time (Tiempo de Entrega)**: Tiempo transcurrido desde que se solicita una funcionalidad hasta que se sube a producción. Mide la eficiencia del pipeline de desarrollo.
*   **Defect Density (Densidad de Bugs)**: Número de errores en producción por cada 1000 líneas de código escritas. Mide la calidad del proceso de pruebas (QA).

A continuación, implementamos en Python un script que automatiza el análisis de desviaciones presupuestarias para un proyecto de software, calculando si la desviación es favorable ($F$) o desfavorable ($D$).

```python
def analizar_desviaciones(presupuesto, gasto_real):
    # Diccionarios de costes presupuestados y gastos reales de desarrollo
    # Desviación = Presupuesto - Gasto Real
    # Si Desviación > 0, es Favorable (gastamos menos). Si < 0, es Desfavorable.
    
    total_presupuesto = 0
    total_real = 0
    
    print(f"{'PARTIDA':20s} | {'PRESUPUESTO':12s} | {'REAL':12s} | {'DESVIACIÓN':12s} | {'ESTADO':10s}")
    print("-" * 75)
    
    for partida in presupuesto.keys():
        pres = presupuesto[partida]
        real = gasto_real[partida]
        desv = pres - real
        estado = "FAVORABLE" if desv >= 0 else "DESFAVORABLE"
        
        total_presupuesto += pres
        total_real += real
        
        print(f"{partida:20s} | {pres:10d} € | {real:10d} € | {desv:10d} € | {estado:10s}")
        
    desv_total = total_presupuesto - total_real
    estado_total = "FAVORABLE" if desv_total >= 0 else "DESFAVORABLE"
    print("-" * 75)
    print(f"{'TOTAL PROYECTO':20s} | {total_presupuesto:10d} € | {total_real:10d} € | {desv_total:10d} € | {estado_total:10s}")

# Datos de prueba
pres = {"Desarrollo Backend": 40000, "Desarrollo Frontend": 35000, "QA/Testing": 15000, "Servidores Cloud": 5000}
real = {"Desarrollo Backend": 42000, "Desarrollo Frontend": 31000, "QA/Testing": 16500, "Servidores Cloud": 4800}

analizar_desviaciones(pres, real)
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Un proyecto de implantación de una base de datos corporativa tiene los siguientes datos financieros mensuales:
*   Presupuesto de personal técnico: $12.000 \, \text{€}$. Gasto real de personal técnico: $13.500 \, \text{€}$.
*   Presupuesto de licencias de software: $5.000 \, \text{€}$. Gasto real de licencias: $4.200 \, \text{€}$.
Calcular las desviaciones presupuestarias para cada partida y determinar el impacto económico total en el presupuesto mensual de la empresa.

**Solución:**
Calculamos la desviación para cada partida (Fórmula: $\text{Desviación} = \text{Presupuesto} - \text{Gasto Real}$):
1.  **Partida de Personal Técnico**:
    $$\text{Desviación} = 12.000 - 13.500 = -1.500 \, \text{€} \quad \implies \text{Desfavorable (gasto extra)}$$
2.  **Partida de Licencias**:
    $$\text{Desviación} = 5.000 - 4.200 = +800 \, \text{€} \quad \implies \text{Favorable (ahorro)}$$
3.  **Desviación Total del Proyecto**:
    $$\text{Desviación Total} = \text{Suma de desviaciones} = -1.500 + 800 = -700 \, \text{€}$$

El proyecto presenta una desviación global desfavorable de $-700 \, \text{€}$ en su presupuesto mensual, debido a que el incremento de costes de personal técnico no pudo ser compensado por el ahorro obtenido en licencias.

### Ejercicio 2
Explicar cómo se aplica la metodología de Objetivos SMART al redactar una meta comercial para una tienda online de software antivirus.

**Solución:**
Un objetivo debe cumplir los 5 filtros SMART para ser útil de cara a la planificación directiva:
*   **S (Specific - Específico)**: No vale decir "aumentar las ventas". Debe indicar qué exactamente: "Aumentar las ventas de la suscripción anual del software antivirus para hogares".
*   **M (Measurable - Medible)**: Debe contener una cifra cuantificable: "Aumentar las ventas en un 15%".
*   **A (Achievable - Alcanzable)**: Debe ser realista dadas las fuerzas del mercado (no proponer un aumento del 1000% si el mercado crece al 5%).
*   **R (Relevant - Relevante)**: Debe estar alineado con la estrategia general de la empresa (generar ingresos estables de suscripciones).
*   **T (Time-bound - Acotado en el tiempo)**: Debe indicar la fecha límite o periodo: "Durante el próximo trimestre (Q3)".

*Redacción final del Objetivo SMART*: "Aumentar las ventas de la suscripción anual de nuestro software antivirus residencial en un 15% durante el próximo trimestre (Q3) del año."

---

## 6. Ejercicios Propuestos

1.  Dada la siguiente matriz de decisión bajo ambiente de riesgo, calcula la alternativa óptima aplicando el criterio del Valor Esperado ($VE$):
    *   Alternativa A: Éxito (Probabilidad 0.6, beneficio $50.000\text{€}$), Fracaso (Probabilidad 0.4, pérdida $-10.000\text{€}$).
    *   Alternativa B: Éxito (Probabilidad 0.4, beneficio $80.000\text{€}$), Fracaso (Probabilidad 0.6, pérdida $-20.000\text{€}$).
2.  ¿Cuáles son las diferencias sustanciales entre las auditorías internas y las auditorías externas dentro de los procesos de control directivo de una corporación?
3.  Diseña dos indicadores clave de rendimiento (KPIs) para cada una de las cuatro perspectivas del Cuadro de Mando Integral aplicados a una empresa dedicada a impartir formación online de programación.


<div style="page-break-after: always;"></div>

# Tema 5: Estructuras Organizativas y Dirección de Recursos Humanos

El éxito de una empresa depende de cómo organice sus recursos y, sobre todo, del liderazgo y motivación de su capital humano. En este tema analizamos el diseño de las **estructuras organizativas** formales (los organigramas) y las teorías psicológicas de la **motivación y liderazgo** que permiten dirigir de forma eficaz los equipos de trabajo dentro del entorno corporativo.

---

## 1. Organización Formal vs. Informal

*   **Organización Formal**: Es la estructura planificada y definida de forma oficial por la dirección de la empresa. Se plasma visualmente en un **organigrama** y define las relaciones de autoridad, canales de comunicación y responsabilidades de cada puesto.
*   **Organización Informal**: Red de relaciones personales y sociales que surgen de forma espontánea y natural entre los trabajadores en el día a día. No aparece en los organigramas, pero tiene un impacto crítico en el clima laboral y en la difusión de información (los rumores).

---

## 2. Tipos de Estructuras Organizativas Formales

Las empresas diseñan sus organigramas siguiendo tres modelos básicos:

### 2.1 Estructura Lineal o Jerárquica
Basada en el principio de **unidad de mando**: cada subordinado responde ante un único jefe directo.
*   *Ventaja*: Máxima claridad y sencillez de autoridad.
*   *Inconveniente*: Rigidez extrema, lentitud de comunicación y falta de especialización de los directivos (un jefe debe saber de todo).

### 2.2 Estructura Funcional
Agrupa los puestos según la especialización de las tareas (departamento financiero, de desarrollo, de marketing).
*   *Ventaja*: Alta eficiencia por especialización del personal.
*   *Inconveniente*: Pérdida de la unidad de mando (un trabajador puede recibir órdenes contradictorias de jefes de distintas áreas).

### 2.3 Estructura Matricial
Común en empresas orientadas a proyectos (como constructoras o consultoras de desarrollo de software). Combina la estructura funcional clásica (vertical) con la organización por proyectos o productos (horizontal).
*   *Ventaja*: Alta flexibilidad para reasignar recursos según las necesidades de cada proyecto.
*   *Inconveniente*: **Doble dependencia** (un programador responde ante su Jefe de Departamento de Desarrollo y ante el Director del Proyecto específico), lo que puede generar conflictos de autoridad.

---

## 3. Dirección de Recursos Humanos y Motivación

La motivación laboral es el impulso psicológico que lleva al trabajador a realizar sus tareas de forma voluntaria y con el máximo rendimiento. Las dos teorías fundamentales son:

### 3.1 Jerarquía de Necesidades de Maslow
Las necesidades humanas se estructuran en una pirámide de cinco niveles. Solo cuando se satisfacen las necesidades inferiores (fisiológicas, de seguridad), surgen las necesidades de los niveles superiores (afiliación, reconocimiento, autorrealización).

### 3.2 Teoría Bifactorial de Herzberg
Divide los factores que influyen en el trabajo en dos grupos:
*   **Factores Higiénicos (Previenen la insatisfacción)**: Relacionados con el entorno del trabajo (salario, condiciones físicas de la oficina, seguridad laboral, relaciones con los compañeros). Si son malos, provocan insatisfacción; pero si son óptimos, no motivan activamente al empleado a largo plazo.
*   **Factores Motivacionales (Generan satisfacción activa)**: Relacionados con el contenido del puesto de trabajo (logro profesional, reconocimiento, responsabilidad asumida, crecimiento personal, autonomía). Son los que realmente generan una motivación duradera.

---

## 4. El Toque Informático

### Metodologías Ágiles (Scrum) y Motivación en Ingeniería de Software
Las empresas tecnológicas modernas (como Spotify o Netflix) han sustituido los organigramas piramidales rígidos por estructuras matriciales ágiles. Los desarrolladores se organizan en **Squads (equipos multifuncionales autogestionados)**, formados por programadores, diseñadores de UX y especialistas en pruebas (QA) que trabajan juntos en un producto.
*   **Motivación Técnica**: Fidelizar el talento de programación (un recurso escaso con alto índice de rotación laboral en el sector) exige aplicar los **factores motivacionales de Herzberg**.
*   Los programadores no se motivan únicamente con un buen sueldo (factor higiénico); exigen autonomía técnica, la capacidad de elegir las tecnologías a utilizar, asumir la responsabilidad del diseño de la arquitectura y la autorrealización personal de ver su código limpio y eficiente corriendo en producción.

A continuación, implementamos en Python una base de datos interactiva que asocia los estilos clásicos de liderazgo con sus características, ventajas y momentos óptimos de aplicación en proyectos informáticos.

```python
def obtener_estilo_liderazgo(nombre_estilo):
    estilos = {
        "Autocratico": {
            "Descripcion": "El líder toma todas las decisiones de forma centralizada sin consultar al equipo.",
            "Ventajas": "Rapidez extrema en la toma de decisiones en situaciones críticas.",
            "Uso_Optimo": "Ante incidentes graves de seguridad de sistemas (caída de servidores por hackeo)."
        },
        "Democratico": {
            "Descripcion": "El líder fomenta la participación del equipo y decide por consenso consensual.",
            "Ventajas": "Alta motivación de los miembros y mejores ideas de diseño técnico.",
            "Uso_Optimo": "Planificación y diseño de arquitectura de un nuevo software complejo."
        },
        "Laissez-Faire": {
            "Descripcion": "El líder da total libertad al equipo y solo interviene si es solicitado.",
            "Ventajas": "Fomenta la creatividad en profesionales experimentados.",
            "Uso_Optimo": "Equipos de investigación y desarrollo (R&D) y hackatones de innovación."
        }
    }
    
    info = estilos.get(nombre_estilo, None)
    if info:
        print(f"=== ESTILO DE LIDERAZGO: {nombre_estilo.upper()} ===")
        print(f"Descripción: {info['Descripcion']}")
        print(f"Ventajas:    {info['Ventajas']}")
        print(f"Uso Óptimo:  {info['Uso_Optimo']}\n")
    else:
        print("Estilo no registrado.")

# Prueba
obtener_estilo_liderazgo("Democratico")
```

---

## 5. Ejercicios Resueltos

### Ejercicio 1
Identificar qué tipo de estructura organizativa formal (Lineal, Funcional o Matricial) describe mejor cada una de las siguientes situaciones:
1. Un desarrollador de software recibe instrucciones directas del Director de I+D de la empresa y, simultáneamente, del Director del Proyecto del videojuego móvil que está desarrollando para la campaña de Navidad.
2. En una pequeña startup con 4 empleados, cada programador informa directamente y responde únicamente ante el CEO y fundador del negocio.

**Solución:**
1.  *Situación 1*: **Estructura Matricial**. Se presenta la característica fundamental de la doble dependencia jerárquica (un jefe vertical funcional -I+D- y un jefe horizontal por producto -Proyecto Videojuego-).
2.  *Situación 2*: **Estructura Lineal o Jerárquica**. Existe una línea directa única de autoridad y unidad de mando clara desde el CEO a cada subordinado directo sin departamentos intermediarios.

### Ejercicio 2
Explicar por qué una subida de sueldo de $500 \, \text{€/mes}$ a un programador de software sénior puede no traducirse en un aumento del rendimiento o la motivación a largo plazo, aplicando la Teoría Bifactorial de Herzberg.

**Solución:**
Según la teoría de Herzberg, el salario es considerado un **Factor Higiénico**, no un factor motivacional:
1.  Si el salario es insuficiente, generará insatisfacción y el programador buscará otro trabajo.
2.  Al aumentar el salario en $500 \, \text{€/mes}$, la insatisfacción desaparece (el programador está conforme con las condiciones económicas del entorno).
3.  Sin embargo, una vez alcanzado el nivel de sueldo aceptable, este no genera por sí mismo motivación activa o entusiasmo a largo plazo. Para lograr que aumente activamente su rendimiento y compromiso, la empresa debe complementar el salario con **Factores Motivacionales**, tales como asignarle proyectos retadores, reconocer su logro técnico, promoverlo a cargos de mayor responsabilidad de diseño técnico, y otorgarle autonomía laboral.

---

## 6. Ejercicios Propuestos

1.  Dibuja el organigrama de una empresa de desarrollo de software con estructura funcional que contenga al menos 5 departamentos distintos.
2.  Explica en qué consiste la Teoría X y la Teoría Y de Douglas McGregor sobre las asunciones directivas respecto a los trabajadores y cómo influyen en el estilo de liderazgo adoptado.
3.  Analiza la pirámide de necesidades de Maslow y aporta un ejemplo de cómo una empresa puede satisfacer la "necesidad de reconocimiento" y la "necesidad de autorrealización" de un desarrollador de software.


<div style="page-break-after: always;"></div>

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


<div style="page-break-after: always;"></div>

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


<div style="page-break-after: always;"></div>

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


<div style="page-break-after: always;"></div>

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


<div style="page-break-after: always;"></div>

# Tema 10: El Mercado y la Actividad Comercial: Investigación y Segmentación

La actividad comercial es el nexo entre la empresa y el exterior. Su principal función consiste en detectar las necesidades de los consumidores y satisfacerlas mediante la comercialización de bienes o servicios. Para que esta labor sea efectiva, las empresas deben analizar exhaustivamente el mercado en el que operan, comprender los patrones de conducta de los usuarios y segmentar la clientela para personalizar su propuesta de valor.

---

## 1. El Mercado y el Comportamiento del Consumidor

El **mercado** es el conjunto de compradores y vendedores de un bien o servicio. Desde el punto de vista del marketing, interesa especialmente la demanda actual y potencial de los compradores.

### El Proceso de Decisión de Compra
Para diseñar soluciones que los clientes deseen adquirir, es necesario comprender cómo deciden comprar. El proceso tradicional consta de cinco etapas:
1.  **Reconocimiento de la necesidad**: El usuario detecta una carencia (ej. falta de espacio en disco para backups).
2.  **Búsqueda de información**: Búsqueda en internet, opiniones de otros programadores, documentación.
3.  **Evaluación de alternativas**: Comparación de precios, capacidades, SLA y rendimiento de diferentes soluciones.
4.  **Decisión de compra**: Adquisición del producto o suscripción al servicio (ej. contratación de AWS S3).
5.  **Comportamiento post-compra**: Evaluación del producto (satisfacción, fidelidad, soporte técnico, recomendación). En software, el comportamiento post-compra define la tasa de renovación o cancelación de la suscripción (churn rate).

---

## 2. Investigación de Mercados

La **investigación de mercados** consiste en la obtención y análisis sistemático de datos relevantes para la toma de decisiones comerciales.

### Clasificación de las Fuentes de Información
*   **Fuentes Primarias**: Información recopilada directamente por la propia empresa para el estudio en curso.
    *   *Técnicas Cualitativas*: Entrevistas en profundidad, dinámicas de grupo (focus groups) o pruebas de usabilidad (User Testing).
    *   *Técnicas Cuantitativas*: Encuestas masivas, experimentación de campo o pruebas A/B de páginas web (A/B Testing).
*   **Fuentes Secundarias**: Información que ya existe y que ha sido recopilada previamente por otras entidades.
    *   *Internas*: Registros históricos de ventas, estadísticas de uso internas, logs del servidor.
    *   *Externas*: Informes sectoriales de consultoras de prestigio (Gartner, IDC), bases de datos de institutos de estadística nacionales (INE, Eurostat) o publicaciones científicas.

---

## 3. Segmentación del Mercado

Ninguna empresa puede satisfacer a todos los compradores por igual, ya que tienen necesidades, gustos y recursos diversos. La **segmentación** consiste en dividir el mercado en grupos de consumidores homogéneos entre sí, pero heterogéneos entre grupos, que requieren estrategias comerciales diferenciadas.

### Criterios de Segmentación Tradicionales
*   **Criterio Geográfico**: Región, país, tamaño de la ciudad, clima.
*   **Criterio Demográfico**: Edad, género, nivel de ingresos, ocupación, tamaño de la familia.
*   **Criterio Psicográfico**: Personalidad, estilo de vida, valores, clase social.
*   **Criterio Conductual**: Frecuencia de uso, nivel de fidelidad, beneficios buscados en el producto, nivel de conocimientos técnicos.

### Segmentación en el Entorno Tecnológico y SaaS
En las empresas de software, la segmentación adquiere matices específicos:
*   **Firmografía (para B2B SaaS)**: Tamaño de la empresa (SMB, Mid-Market, Enterprise), sector tecnológico y volumen de facturación.
*   **Patrones de Uso (Product-Led Growth)**:
    *   *Usuarios Gratuitos (Free Tier)*: Consumo de recursos básico, alta sensibilidad al precio.
    *   *Usuarios de Pago (Premium)*: Uso avanzado, requerimiento de integraciones mediante API y soporte prioritario.
    *   *Desarrolladores frente a Directivos*: Segmentación del mensaje técnico (documentación robusta) frente al comercial (retorno de inversión y seguridad de la información).

---

## 4. El Posicionamiento Estratégico

El **posicionamiento** es la imagen y el lugar que un producto o marca ocupa en la mente del consumidor en relación con la competencia.
*   **Estrategias comunes**: Posicionamiento por calidad/precio, por características técnicas específicas, por beneficios para el cliente o frente a un competidor directo.
*   **Mapa de Posicionamiento**: Herramienta gráfica bidimensional que representa cómo los consumidores perciben las marcas competidoras según dos atributos críticos (ej. Precio frente a Facilidad de integración).

---

## 5. El Toque Informático

### Clasificación Automática de Clientes en un SaaS
En la ingeniería de software moderna, el análisis de datos de uso (product analytics) es fundamental para segmentar a los usuarios de una aplicación en tiempo real. 
El siguiente script en Python clasifica a los usuarios de una API en tres cohortes en función del volumen de peticiones mensuales y de la tasa de fallos de integración, permitiendo al departamento comercial detectar oportunidades de venta (Upselling) o prevenir cancelaciones:

```python
def segmentar_clientes(usuarios):
    segmentos = {
        "Enterprise": [],
        "Ideal para Upselling (Mid-Market)": [],
        "Free / Casual": []
    }
    
    for u in usuarios:
        nombre = u["nombre"]
        peticiones = u["peticiones_mes"]
        pago_activo = u["pago_activo"]
        
        # Lógica de segmentación basada en comportamiento
        if peticiones > 100000 and pago_activo:
            segmentos["Enterprise"].append(nombre)
        elif peticiones > 20000 and not pago_activo:
            segmentos["Ideal para Upselling (Mid-Market)"].append(nombre)
        else:
            segmentos["Free / Casual"].append(nombre)
            
    # Mostrar resultados
    for segmento, nombres in segmentos.items():
        print(f"Segmento '{segmento}' ({len(nombres)} usuarios):")
        for n in nombres:
            print(f"  - {n}")
        print()

# Datos de muestra de uso de la plataforma API
usuarios_plataforma = [
    {"nombre": "FintechCorp S.L.", "peticiones_mes": 250000, "pago_activo": True},
    {"nombre": "DevStudio Ltd.", "peticiones_mes": 45000, "pago_activo": False},
    {"nombre": "BlogPersonal.me", "peticiones_mes": 1200, "pago_activo": False},
    {"nombre": "E-Commerce Grande S.A.", "peticiones_mes": 150000, "pago_activo": True},
    {"nombre": "SaaS Startup Beta", "peticiones_mes": 32000, "pago_activo": False}
]

segmentar_clientes(usuarios_plataforma)
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Una startup que comercializa una base de datos distribuida en formato Open Source planea definir su posicionamiento estratégico. Identifica y describe cómo podría segmentar su mercado distinguiendo entre desarrolladores (usuarios del producto) y directores de IT (compradores que autorizan el presupuesto).

**Solución:**
1.  **Segmentación del perfil de Desarrolladores**:
    *   *Tipo de segmentación*: Conductual y Psicográfica.
    *   *Necesidades clave*: Facilidad de uso, rendimiento en consultas complejas (latencia), compatibilidad con SQL estándar, documentación de API clara e integración sencilla en pipelines de CI/CD.
    *   *Canal de comunicación*: Foros técnicos (Reddit, StackOverflow), repositorios públicos (GitHub) y tutoriales prácticos de código.
2.  **Segmentación del perfil de Directores de IT (CIO/CTO)**:
    *   *Tipo de segmentación*: Firmográfica e Instrumentalista.
    *   *Necesidades clave*: Garantías de seguridad de datos (cumplimiento de GDPR, encriptación en reposo y tránsito), acuerdos de nivel de servicio (SLA) de disponibilidad, soporte técnico 24/7 y rentabilidad (reducción del coste total de propiedad - TCO).
    *   *Canal de comunicación*: Caso de éxito de otras empresas, reuniones comerciales ejecutivas e informes de viabilidad financiera (ROI).

Esta segmentación doble es habitual en el modelo de ventas "B2B corporativo", donde el decisor y el usuario final tienen incentivos y lenguajes diferentes.

---

## 7. Ejercicios Propuestos

1.  Dada una aplicación móvil de fitness que ofrece una versión gratuita con anuncios y una versión Premium de pago:
    *   Propón tres criterios de segmentación conductual para clasificar a los usuarios.
    *   Explica cómo podría utilizar la empresa el A/B Testing (fuente primaria cuantitativa) para aumentar la tasa de conversión a Premium.
2.  Dibuja mentalmente o esboza en código un mapa de posicionamiento bidimensional para el mercado de almacenamiento cloud, usando los ejes: **"Precio por Terabyte"** y **"Velocidad de Recuperación de Datos (Latencia)"**, y ubica de manera estimada los siguientes servicios: AWS S3 Standard, AWS Glacier (almacenamiento frío), Dropbox y un servidor FTP local propio.
3.  Explica la diferencia operativa y de coste entre contratar una consultora de mercado para realizar encuestas específicas (fuente primaria) frente a la compra de un informe sectorial ya publicado por Gartner (fuente secundaria).


<div style="page-break-after: always;"></div>

# Tema 11: El Marketing Mix y sus Variables en el Sector Tecnológico

El **Marketing Mix** representa el conjunto de variables y herramientas controlables que la empresa combina para influir en las respuestas del mercado objetivo. Tradicionalmente, estas herramientas se agrupan en las denominadas **4 Ps**: Producto, Precio, Distribución (Place) y Promoción/Comunicación. En la industria del software y los servicios SaaS, estas variables se han reconfigurado para responder a modelos de distribución digital, suscripciones y arquitecturas cloud.

---

## 1. Producto (Product) en el Software

El producto es el bien o servicio que la empresa ofrece al mercado para satisfacer una necesidad. En tecnología, el software ha transicionado de ser un bien tangible (CD-ROMs con licencias perpetuas) a un servicio dinámico e inmaterial (SaaS).
*   **Ciclo de Vida del Producto (CVP)**: Consta de las etapas de Introducción, Crecimiento, Madurez y Declive. En software, el ciclo se gestiona mediante actualizaciones continuas y control de versiones (versiones Alpha, Beta, General Availability o GA, y End-of-Life o EOL).
*   **Atributos de calidad de software como producto**: Interfaz de usuario (UI), Experiencia de usuario (UX), estabilidad, seguridad, escalabilidad de la base de código y la documentación de la API.

---

## 2. Precio (Price) y Modelos de Monetización

El precio es la cantidad de dinero que el cliente paga para obtener el producto. Es la única variable del marketing mix que genera ingresos; las otras tres representan costes.

### Estrategias de Fijación de Precios de Software
*   **Pago Único (Licencia Perpetua)**: El cliente compra la versión actual del software y la instala de por vida, requiriendo pagos adicionales por soporte o actualizaciones mayores.
*   **Modelo de Suscripción (SaaS)**: Pagos periódicos (mensuales o anuales) a cambio de la licencia de uso y mantenimiento continuo (ej. Microsoft 365, Spotify).
*   **Monetización por Consumo (Pay-as-you-go)**: El coste se calcula dinámicamente según los recursos de computación consumidos (ej. AWS cobra por horas de CPU o GB de transferencia de datos).
*   **Modelo Freemium**: Se ofrece un plan gratuito con funcionalidades básicas (Free tier) y se cobra una suscripción por acceder a características avanzadas o límites de uso superiores (ej. GitHub, Slack).

---

## 3. Distribución (Place) Digital

La distribución engloba las actividades necesarias para poner el producto a disposición de los clientes en el lugar y momento adecuados.
En el sector del software, los canales físicos han desaparecido en favor del canal digital:
*   **Distribución Directa**: Descarga directa desde la web del desarrollador o despliegue directo en la infraestructura cloud del cliente (On-Premise).
*   **Plataformas Intermediarias (App Stores y Marketplaces)**: Apple App Store, Google Play Store, AWS Marketplace o GitHub Marketplace. Proporcionan visibilidad y facilitan la pasarela de pagos a cambio de una comisión (típicamente entre el $15\%$ y el $30\%$).

---

## 4. Promoción y Comunicación (Promotion)

La promoción abarca los canales de comunicación orientados a dar a conocer el producto, persuadir a la compra y recordar sus beneficios.
*   **Inbound Marketing y Marketing de Contenidos**: Atraer clientes aportando valor (ej. escribir artículos técnicos en blogs de ingeniería, tutoriales en YouTube o documentación técnica abierta).
*   **Evangelismo Tecnológico (DevRel - Developer Relations)**: Profesionales dedicados a dar soporte y dinamizar la comunidad de desarrolladores de software que usan su tecnología.
*   **SEO y SEM**: Optimización en motores de búsqueda (SEO) y publicidad en buscadores y redes (SEM) para captar tráfico altamente cualificado hacia la landing page de la aplicación.

---

## 5. El Toque Informático

### Métricas de Rendimiento en Marketing Digital y SaaS
La viabilidad comercial de una startup tecnológica se mide mediante la interacción de tres métricas fundamentales:

1.  **Coste de Adquisición de Clientes (CAC)**: Dinero total invertido en marketing y ventas dividido por el número de nuevos clientes conseguidos en un periodo.
    $$CAC = \frac{\text{Gastos Totales de Marketing y Ventas}}{\text{Nuevos Clientes Adquiridos}}$$

2.  **Churn Rate (Tasa de Cancelación)**: Porcentaje de suscriptores que se dan de baja del servicio en un periodo determinado.
    $$\text{Churn Rate} = \frac{\text{Clientes Perdidos en el Periodo}}{\text{Clientes Iniciales del Periodo}}$$

3.  **Customer Lifetime Value (LTV)**: Ingresos totales estimados que un cliente generará durante todo el periodo que permanezca activo en la plataforma.
    $$LTV = \frac{\text{ARPU} \cdot \text{Margen Bruto}}{\text{Churn Rate}}$$
    donde ARPU (Average Revenue Per User) es el ingreso medio mensual por usuario.

*   **Regla de Oro en SaaS**: Para que un negocio tecnológico sea sostenible y rentable, la relación entre el LTV y el CAC debe cumplir:
    $$\frac{LTV}{CAC} \ge 3$$
    Es decir, un cliente debe generar un valor a largo plazo de al menos el triple de lo que costó captarlo.

A continuación, implementamos una calculadora comercial en Python para auditar la salud financiera y comercial de una app SaaS de software de gestión de bases de datos.

```python
def analizar_salud_saas(gastos_marketing, nuevos_clientes, arpu, margen_bruto, churn_rate):
    # Calcular CAC
    cac = gastos_marketing / nuevos_clientes
    
    # Calcular LTV
    ltv = (arpu * margen_bruto) / churn_rate
    
    # Ratio LTV/CAC
    ratio = ltv / cac
    
    print("=== AUDITORÍA MÉTRICAS COMERCIALES SAAS ===")
    print(f"Coste de Adquisición (CAC): {cac:.2f} €")
    print(f"Valor del Ciclo de Vida (LTV): {ltv:.2f} €")
    print(f"Relación LTV/CAC: {ratio:.2f}x")
    
    # Diagnóstico estratégico
    if ratio >= 3.0:
        print("Diagnóstico: Excelente. El negocio es altamente escalable y rentable.")
    elif ratio >= 1.0:
        print("Diagnóstico: Riesgo. El coste de captación es elevado para el LTV. Reduzca CAC o mejore retención.")
    else:
        print("Diagnóstico: Crítico. Destrucción de capital. Churn muy alto o marketing ineficiente.")
    print("==========================================")

# Simulación: Gastos de 12.000 € en anuncios para conseguir 150 clientes. 
# Abono mensual medio de 40 €, margen del 80% y baja del 5% mensual.
analizar_salud_saas(
    gastos_marketing=12000.0,
    nuevos_clientes=150,
    arpu=40.0,
    margen_bruto=0.80,
    churn_rate=0.05
)
```

---

## 6. Ejercicios Resueltos

### Ejercicio 1
Una plataforma de educación para programadores online invirtió el mes pasado $8.000 \, \text{€}$ en campañas de Google Ads y contrató los servicios de un creador de contenido por $2.000 \, \text{€}$. Gracias a estas acciones, obtuvieron $250$ nuevos suscriptores de pago. Sabiendo que la cuota de suscripción es de $25 \, \text{€/mes}$, con un margen bruto de operaciones de la plataforma del $90\%$ y una tasa de cancelación de usuarios (Churn) mensual estimada del $4\%$, calcula:
1.  El Coste de Adquisición de Clientes (CAC).
2.  El Customer Lifetime Value (LTV).
3.  Determina la viabilidad de la estrategia comercial mediante el ratio LTV:CAC.

**Solución:**

1.  **Cálculo del CAC**:
    *   $\text{Gastos totales de Marketing y Ventas} = 8.000 + 2.000 = 10.000 \, \text{€}$.
    *   $\text{Nuevos clientes} = 250$.
    *   $$CAC = \frac{10.000}{250} = 40 \, \text{€ por cliente}$$

2.  **Cálculo del LTV**:
    *   $\text{ARPU} = 25 \, \text{€/mes}$.
    *   $\text{Margen Bruto} = 0,90$.
    *   $\text{Churn Rate} = 0,04$ mensual.
    *   $$LTV = \frac{25 \cdot 0,90}{0,04} = \frac{22,5}{0,04} = 562,50 \, \text{€}$$

3.  **Evaluación LTV:CAC**:
    *   $$\text{Ratio} = \frac{LTV}{CAC} = \frac{562,50}{40} = 14,06$$
    *   Como el ratio es $14,06 \ge 3$, la estrategia comercial es extremadamente viable y muy rentable. Se puede considerar aumentar el gasto de captación para acelerar el crecimiento.

---

## 7. Ejercicios Propuestos

1.  Una startup tecnológica comercializa su software corporativo y decide lanzar un modelo **Freemium**. Discute los principales riesgos de esta estrategia (ej. el efecto "free rider" donde los usuarios gratuitos no convierten a planes premium) y cómo gestionar la variable "Producto" para mitigarlos.
2.  Un desarrollador independiente crea una app móvil de mapas offline y decide distribuirla a través de Google Play Store cobrando un precio de $2,99 \, \text{€}$. Sabiendo que Google cobra una comisión de marketplace del $15\%$, calcula el ingreso neto real por descarga que recibe el programador.
3.  Calcula el LTV de una aplicación SaaS si la tasa de cancelación (Churn) mensual sube del $3\%$ al $9\%$, asumiendo un ARPU constante de $50 \, \text{€}$ y un margen de contribución de servicios del $100\%$. Analiza detalladamente el impacto de la retención de usuarios en las métricas de marketing.


<div style="page-break-after: always;"></div>

# Glosario de Términos

*   **Balance de Situación**: Documento contable que refleja la situación patrimonial de una empresa (Activo, Pasivo y Patrimonio Neto) en un momento dado.
*   **Capitalización Simple**: Régimen financiero donde los intereses generados en cada período no se acumulan al capital para producir nuevos intereses.
*   **Capitalización Compuesta**: Régimen financiero donde los intereses generados se acumulan al capital inicial para producir nuevos intereses en los siguientes períodos.
*   **Customer Acquisition Cost (CAC)**: Coste total invertido en marketing y ventas para adquirir un nuevo cliente.
*   **Customer Lifetime Value (LTV)**: Valor neto estimado de los ingresos o beneficios que un cliente aportará a la empresa durante toda su relación comercial.
*   **Cuenta de Pérdidas y Ganancias (PyG)**: Documento contable que resume los ingresos y gastos generados durante un ejercicio económico, mostrando el resultado del ejercicio (beneficio o pérdida).
*   **DAFO (FODA)**: Herramienta de análisis estratégico que evalúa los factores internos (Debilidades y Fortalezas) y externos (Amenazas y Oportunidades) de una organización.
*   **Fuerzas de Porter**: Modelo estratégico que analiza la estructura competitiva de un sector basándose en cinco fuerzas: rivalidad entre competidores, amenaza de nuevos entrantes, poder de proveedores, poder de clientes y amenaza de productos sustitutivos.
*   **Plazo de Recuperación (Payback)**: Método de valoración de inversiones que calcula el tiempo necesario para recuperar el desembolso inicial mediante los flujos de caja netos.
*   **Tasa Interna de Retorno (TIR)**: Tasa de descuento o tipo de rendimiento que hace que el Valor Actual Neto (VAN) de una inversión sea exactamente igual a cero.
*   **Valor Actual Neto (VAN)**: Diferencia entre el valor actualizado de los flujos de caja esperados de una inversión y el desembolso inicial requerido, utilizando una tasa de descuento determinada.

<div style="page-break-after: always;"></div>

# Bibliografía Recomendada

1.  **Kotler, P., & Keller, K. L. (2016).** *Marketing Management* (15th ed.). Pearson.
    *   *Nota*: La obra de referencia mundial sobre marketing estratégico y táctico, aplicable a las estrategias de comercialización tecnológica.
2.  **Brealey, R. A., Myers, S. C., & Allen, F. (2019).** *Principles of Corporate Finance* (13th ed.). McGraw-Hill.
    *   *Nota*: Considerada la biblia de las finanzas corporativas, fundamental para comprender la valoración de proyectos, VAN, TIR y fuentes de financiación.
3.  **Porter, M. E. (2008).** *Competitive Strategy: Techniques for Analyzing Industries and Competitors*. Free Press.
    *   *Nota*: El texto clásico y fundacional sobre la estrategia competitiva y el modelo de las cinco fuerzas.
4.  **Osterwalder, A., & Pigneur, Y. (2010).** *Business Model Generation*. Wiley.
    *   *Nota*: Guía práctica para diseñar modelos de negocio de base tecnológica y startups de software/SaaS.
