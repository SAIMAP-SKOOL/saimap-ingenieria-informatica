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
