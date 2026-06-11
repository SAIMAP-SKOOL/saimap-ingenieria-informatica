# Física y Electrónica: Electromagnetismo, Circuitos y Dispositivos Semiconductores

Bienvenido al manual completo de **Física y Electrónica** para el Grado en Ingeniería Informática. Esta asignatura une los principios físicos que rigen los fenómenos eléctricos y magnéticos con su modelado matemático en circuitos y su aplicación digital en forma de transistores y familias lógicas.

> [!TIP]
> Puedes acceder al [Manual Completo Unificado (Markdown)](Manual_Completo_Fisica_y_Electronica.md) que contiene todos los temas compilados junto con un índice, glosario y bibliografía en un único archivo listo para imprimir o convertir a PDF.

---

## Estructura del Manual

El manual se divide en tres bloques temáticos alineados con las 15 semanas de estudio de la asignatura:

### Bloque 1: Campo Eléctrico, Magnetismo y Oscilaciones (Semanas 1 a 5)
*   [Tema 1: Electrostática y Campo Eléctrico](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30206%20-%20F%C3%ADsica%20y%20Electr%C3%B3nica/tema_01_electrostatica.md)
*   [Tema 2: Propiedades Eléctricas de la Materia](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30206%20-%20F%C3%ADsica%20y%20Electr%C3%B3nica/tema_02_propiedades_electricas_materia.md)
*   [Tema 3: Campo Magnético y Propiedades Magnéticas](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30206%20-%20F%C3%ADsica%20y%20Electr%C3%B3nica/tema_03_campo_magnetico.md)
*   [Tema 4: Ondas Electromagnéticas: Señales y Transmisión](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30206%20-%20F%C3%ADsica%20y%20Electr%C3%B3nica/tema_04_ondas_electromagneticas.md)

### Bloque 2: Teoría de Circuitos (Semanas 6 a 10)
*   [Tema 5: Circuitos Eléctricos: Fundamentos y Leyes (Kirchhoff)](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30206%20-%20F%C3%ADsica%20y%20Electr%C3%B3nica/tema_05_circuitos_electricos_fundamentos.md)
*   [Tema 6: Técnicas para el Análisis de Circuitos Resistivos](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30206%20-%20F%C3%ADsica%20y%20Electr%C3%B3nica/tema_06_tecnicas_analisis_circuitos.md)
*   [Tema 7: Circuitos Básicos con Condensadores y Bobinas (Transitorio)](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30206%20-%20F%C3%ADsica%20y%20Electr%C3%B3nica/tema_07_comportamiento_transitorio.md)
*   [Tema 8: Circuitos Resistivos con Fuentes Senoidales (Corriente Alterna)](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30206%20-%20F%C3%ADsica%20y%20Electr%C3%B3nica/tema_08_corriente_alterna_fasores.md)
*   [Tema 9: Fundamentos de Instalaciones Eléctricas](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30206%20-%20F%C3%ADsica%20y%20Electr%C3%B3nica/tema_09_instalaciones_electricas.md)

### Bloque 3: Electrónica Básica y Digital (Semanas 11 a 15)
*   [Tema 10: Fundamentos de Electrónica: Diodo y Transistor](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30206%20-%20F%C3%ADsica%20y%20Electr%C3%B3nica/tema_10_fundamentos_electronica.md)
*   [Tema 11: Familias Lógicas (TTL y CMOS)](file:///d:/Usuario/SAIMAP/SAIMAP%20-%20UNIVERSIDAD/Grado%20en%20Ingenier%C3%ADa%20Inform%C3%A1tica/1%C2%BA/30206%20-%20F%C3%ADsica%20y%20Electr%C3%B3nica/tema_11_familias_logicas.md)

---

## Repaso de Herramientas Matemáticas Previas

Para abordar con éxito la física del electromagnetismo y los circuitos, repasamos las herramientas matemáticas indispensables:

### 1. Álgebra Vectorial Básica
Un vector $\vec{A}$ en $\mathbb{R}^3$ se representa por sus componentes cartesianas: $\vec{A} = A_x\hat{i} + A_y\hat{j} + A_z\hat{k} = (A_x, A_y, A_z)$.
*   **Producto Escalar** (devuelve un escalar):
    $$\vec{A} \cdot \vec{B} = A_x B_x + A_y B_y + A_z B_z = \|\vec{A}\| \|\vec{B}\| \cos\theta$$
*   **Producto Vectorial** (devuelve un vector perpendicular):
    $$\vec{A} \times \vec{B} = \det \begin{pmatrix} \hat{i} & \hat{j} & \hat{k} \\ A_x & A_y & A_z \\ B_x & B_y & B_z \end{pmatrix} = (A_y B_z - A_z B_y)\hat{i} - (A_x B_z - A_z B_x)\hat{j} + (A_x B_y - A_y B_x)\hat{k}$$
    Su magnitud es $\|\vec{A} \times \vec{B}\| = \|\vec{A}\| \|\vec{B}\| \sin\theta$.

### 2. Derivadas e Integrales de Campo
*   **Gradiente ($\nabla f$)**: Representa la tasa de variación espacial de un campo escalar (ej. el potencial eléctrico $V$ genera el campo eléctrico $\vec{E} = -\nabla V$).
*   **Integral de Línea (Trabajo / Circulación)**: $\int_C \vec{E} \cdot d\vec{r}$.
*   **Integral de Superficie (Flujo)**: $\Phi = \iint_S \vec{E} \cdot d\vec{S}$.

---

## Configuración del Entorno de Simulación (Python)

Los temas de circuitos transitorios y fasores se apoyan en scripts de Python. Para preparar tu entorno:

```bash
pip install numpy matplotlib scipy
```
