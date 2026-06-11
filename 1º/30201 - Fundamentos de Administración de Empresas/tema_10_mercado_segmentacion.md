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
