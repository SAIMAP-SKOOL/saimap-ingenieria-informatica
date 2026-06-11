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
