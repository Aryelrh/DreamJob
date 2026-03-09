# DreamJob

Portfolio con estética de editor Neovim. HTML5 + CSS3 puro — sin JavaScript.

Referencia Figma: https://www.figma.com/community/file/1612935498248707180

---

## Cómo funciona el cambio de pestañas sin JavaScript

El sistema de tabs está completamente controlado por elementos `<input type="radio">` ocultos y el selector general de hermanos de CSS (`~`).

```html
<input type="radio" name="tab" id="r-about" class="tab-radio" />
<!-- ... más radios ... -->
<div class="nvim-window"> ... </div>
```

Cuando un radio queda marcado, CSS alcanza su hermano `.nvim-window` y activa el panel, la pestaña, el archivo en el sidebar y la etiqueta de la statusbar correspondientes:

```css
#r-about:checked ~ .nvim-window .panel-about { display: flex; }
#r-about:checked ~ .nvim-window label[for="r-about"].tab { color: var(--fg); }
```

El navegador maneja el estado; CSS maneja la presentación.

---

## Elementos HTML5 clave

| Elemento | Rol |
|---|---|
| `<header class="nvim-titlebar">` | Barra de título estilo ventana del SO |
| `<div class="nvim-tabbar" role="tablist">` | Franja de tabs de buffers |
| `<label for="r-X" class="tab">` | Tab clickeable — activa el radio al hacer clic |
| `<aside class="neotree">` | Sidebar del explorador de archivos |
| `<h2 class="neotree-header">` | Encabezado de sección del sidebar (landmark semántico) |
| `<nav>` dentro del aside | Navegación del árbol de archivos |
| `<label class="tree-file">` | Fila de archivo clickeable en el árbol — también activa el radio |
| `<main class="editor-area">` | Viewport del editor |
| `<article role="tabpanel">` | Un buffer / panel de archivo |
| `<ol class="code-lines">` | Lista ordenada de líneas de código (semántica y accesible) |
| `<section class="nvim-term">` | Split de terminal animado |
| `<header class="term-bar">` | Barra de estado de la terminal |
| `<div role="log" aria-live="polite">` | Salida de la terminal (región ARIA live para lectores de pantalla) |
| `<footer class="nvim-statusbar">` | Statusline de Neovim |

---

## Técnicas CSS clave

### Custom properties (`:root`)
Todos los colores, tamaños y espaciados viven como variables en `:root`. El esquema de colores es **Rosé Pine Moon**.

```css
:root {
  --bg:      #232136;  /* base */
  --syn-kw:  #c4a7e7;  /* iris — keywords */
  --term-h:  280px;
}
```

### Contadores CSS — números de línea automáticos
`<ol class="code-lines">` usa `counter-reset` y `counter-increment` para generar los números de línea desde CSS, sin números escritos a mano en el HTML.

```css
.code-lines { counter-reset: line; }
.code-lines > li { counter-increment: line; }
.code-lines > li::before { content: counter(line); }
```

### `@keyframes` — animación de la terminal
Tres animaciones controlan la experiencia de la terminal:

- **`term-open`** — desliza el panel de terminal hacia arriba desde `height: 0` hasta `height: var(--term-h)`.
- **`line-appear`** — revela cada línea de salida con un pequeño desplazamiento hacia arriba.
- **`blink`** — alterna la opacidad del cursor de bloque con `step-start` (cambio instantáneo, sin easing).

```css
@keyframes term-open {
  from { height: 0; opacity: 0; }
  to   { height: var(--term-h); opacity: 1; }
}
```

Las líneas se revelan de forma escalonada incrementando `animation-delay` en cada clase `.tl-N`:

```css
#r-presentation:checked ~ .nvim-window .tl-1 { animation: line-appear 0.15s ease 0.65s forwards; }
#r-presentation:checked ~ .nvim-window .tl-2 { animation: line-appear 0.15s ease 0.82s forwards; }
/* ... */
```

`animation-fill-mode: forwards` mantiene cada elemento en su estado final al terminar la animación.

### CSS Grid — layout de la ventana
`.nvim-window` usa un grid de 4 filas para fijar el chrome en alturas exactas:

```css
.nvim-window {
  display: grid;
  grid-template-rows: var(--titlebar-h) var(--tabbar-h) 1fr var(--statusbar-h);
  height: 100dvh;
}
```

La fila `1fr` es el cuerpo (sidebar + editor) y ocupa todo el espacio restante.

### Barras de habilidad con `--pct`
Cada barra lee una variable CSS local definida inline en el HTML:

```html
<span class="skill-bar" style="--pct: 80%"></span>
```

```css
.skill-bar::after {
  width: var(--pct, 0%);
}
```

### `resize: vertical` en la terminal
Una vez que la animación `term-open` termina (`forwards`), la terminal recibe `resize: vertical`, permitiendo al usuario arrastrar su borde superior para cambiar la altura — sin JavaScript.

---

## Ofertas de empleo analizadas

Este portfolio fue construido teniendo en mente un objetivo concreto: aplicar a empresas reales con posiciones reales. Las ofertas seleccionadas a continuación son todas de empresas reconocidas a nivel mundial, con presencia en América Latina o acceso remoto desde Costa Rica. La razón de elegirlas es doble: disfruto más el desarrollo backend — sistemas, APIs, datos, infraestructura — y estas empresas representan metas ambiciosas y con buen futuro profesional.

---

### IBM — Fullstack Developer Junior (Data)
**Ubicación:** Heredia, Costa Rica (Híbrido) · [Ver oferta](https://www.linkedin.com/jobs/view/4372622512)

Posición de entrada en IBM Consulting enfocada en desarrollo fullstack con orientación a datos y AI. Se trabaja con Snowflake, bases de datos relacionales y soluciones cloud. El componente backend (APIs REST, queries SQL, integración de datos) es el núcleo del rol.

| Stack requerido | Stack preferido |
|---|---|
| JavaScript / TypeScript, Python o Java | Snowflake o Databricks |
| SQL y bases de datos relacionales | AWS o Azure |
| RESTful APIs | CI/CD, Docker |
| Git | React, Vue.js o Angular |
| Agile (Scrum / Kanban) | Terraform, CloudFormation |

---

### Microsoft — Software Engineer II / Senior
**Ubicación:** Costa Rica (Remoto) · [Ver oferta](https://www.linkedin.com/jobs/view/4342060553)

Ingeniería de software para Microsoft 365 (Exchange, Teams, SharePoint). Servicios distribuidos de alta escala, baja latencia y alta disponibilidad. Rol senior con mentoría a juniors y participación en on-call. Requiere al menos 3 años de experiencia.

| Stack requerido | Stack preferido |
|---|---|
| C++, C#, Java, Python o C | Azure |
| Diseño de servicios backend distribuidos | DevOps para servicios en producción |
| Inglés fluido | Master's en CS o Ingeniería |
| 3+ años de experiencia | Agile / iterativo |

---

### HP — Software Developer Internship
**Ubicación:** Spring, TX (Híbrido) · [Ver oferta](https://www.linkedin.com/jobs/view/4382915716)

Pasantía de verano 2026 de 12 semanas con posibilidad de conversión a tiempo completo. Cubre múltiples especializaciones: backend, cloud, data, firmware, ciberseguridad. Requiere elegibilidad para trabajar en EE. UU.

| Stack requerido | Stack preferido (según área) |
|---|---|
| Java, Python, C++ | Go, AWS (EKS, Lambda, RDS) |
| RESTful y GraphQL APIs | Apache Kafka, RabbitMQ |
| Git | Splunk, Prometheus, Grafana |
| Linux | Docker, CI/CD |

---

### Moody's — Data Engineer
**Ubicación:** Heredia, Costa Rica · [Ver oferta](https://cr.indeed.com/viewjob?jk=6190e7d01a45cfba)

Ingeniería de datos en la plataforma Databricks Lakehouse de Moody's Analytics. Pipelines de datos, Delta Live Tables, orquestación con Airflow y arquitectura Data Mesh. El rol es fuertemente backend/data con algo de ML.

| Stack requerido | Stack preferido |
|---|---|
| Python, SQL | Spark, dbt |
| Databricks / Delta Lake | AWS MWAA (Airflow) |
| CI/CD para pipelines de datos | Snowflake, BigQuery |
| Principios de DataOps | Conceptos de ML / LLMs |

---

### Twilio — Software Engineer Intern
**Ubicación:** Estados Unidos (Remoto) · [Ver oferta](https://www.linkedin.com/jobs/view/4371046380)

Pasantía de 3 meses en ingeniería de software. Trabajo en sistemas distribuidos, procesamiento de audio en tiempo real (DSP), mensajería y cloud. Se espera que el intern entregue proyectos reales, no tareas auxiliares.

| Stack requerido | Stack preferido |
|---|---|
| Python, Java, JavaScript, Go, C o C++ | Hadoop, Spark, Scala |
| Sistemas distribuidos | AWS |
| Pruebas unitarias e integración | Proyectos open source |

---

## Denominador común

| Aspecto | Detalle |
|---|---|
| **Lenguajes dominantes** | Python, Java, JavaScript/TypeScript, C++ |
| **Bases de datos** | SQL (Postgres, DB2), Snowflake, Databricks, MongoDB |
| **APIs** | RESTful en todos los roles; GraphQL en algunos |
| **Cloud** | AWS y/o Azure presentes en todas las ofertas |
| **Control de versiones** | Git es requisito universal |
| **Metodología** | Agile / Scrum en todos los equipos |
| **Orientación principal** | **Backend** — servicios, pipelines, APIs, datos |
| **Inglés** | Requerido o muy valorado en todas las posiciones |

Todas las posiciones son predominantemente **backend o data engineering**. Ninguna es puramente frontend. Eso refuerza la dirección que quiero tomar: construir sistemas, no interfaces.

