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
