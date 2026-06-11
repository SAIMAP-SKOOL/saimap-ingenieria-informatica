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
