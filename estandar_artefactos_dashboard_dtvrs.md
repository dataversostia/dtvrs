# Estándar de artefactos — Dashboards Dataversos

Fuente de verdad técnica para construir dashboards interactivos como Artifact de Claude,
autocontenidos en un solo HTML. Extraído del desarrollo de `dashboard_ventas_dataversos.html`
(reporte de ventas de nutracéuticos). Úsalo como checklist y como copia-pega de tokens al
arrancar el próximo dashboard.

> Nota de ubicación: este archivo vive hoy en `0404_webapp_ventas_dtvrs/` porque nació ahí.
> Si quieren que sea la fuente compartida entre clientes (como ya prevé
> `02_dtvrs/CLAUDE.md` en la sección "Metodología de entregables"), muévanlo a
> `02_dtvrs/03_reporteria_dtvrs/` o donde definan como estándar interno — el contenido no
> depende de la carpeta.

---

## 1. Paleta de marca (tokens CSS)

Fuente: `bd_ventas_contexto_dtvrs.md` del cliente/proyecto — **siempre revisar ese archivo
primero**, la paleta cambia entre entregas (ya migramos una vez de Cinnabar/Salmon a
Blushed brick/Sweet salmon a mitad de proyecto).

```css
:root{
  --dtvrs-gunmetal:#222A35;      /* fondo oscuro principal, texto sobre fondos claros */
  --dtvrs-charcoal:#333F50;      /* superficies, texto secundario */
  --dtvrs-cool-gray:#8497B0;     /* texto tenue, bordes, deshabilitado */
  --dtvrs-cinnabar:#CB4F55;      /* ACENTO principal — botones, links, dato clave (usar con moderación) */
  --dtvrs-salmon:#F89E97;        /* acento suave — hover, fondos de énfasis tenue */
  --dtvrs-white:#FFFFFF;
  --dtvrs-offwhite:#F4F5F7;

  --status-good:     #3F9A57;    /* verde */
  --status-warning:  #e8871e;    /* ámbar */
  --status-serious:  #ec835a;
  --status-critical: #CB4F55;    /* rojo — hoy coincide con el acento, no es casualidad, revisar en cada proyecto */
  --status-neutral:  var(--dtvrs-cool-gray);

  /* Paleta categórica de gráficos (8 pasos, orden fijo, nunca ciclar) */
  --s1:#CB4F55; --s2:#333F50; --s3:#8497B0; --s4:#F89E97;
  --s5:#222A35; --s6:#FF8A7E; --s7:#6B7A91; --s8:#5F7086;
}
```

**Decisión de diseño documentada:** el skill `dataviz` recomienda por defecto una paleta
categórica validada por CVD (contraste para daltonismo). Aquí la reemplazamos deliberadamente
por los 5 colores de marca + tintes/sombras derivados, porque el cliente pidió explícitamente
"usa los colores de la paleta dtvrs para todos los gráficos". Regla: **las palabras del
usuario ganan sobre el default genérico** — pero si el próximo cliente no lo pide, usar el
validador (`scripts/validate_palette.js` del skill `dataviz`) antes de comprometerse a una
paleta de marca para charts.

### Modo oscuro — patrón de 3 estados (obligatorio, no opcional)

Cada token de color con variante oscura se define **tres veces**, nunca dos:

```css
:root{ /* claro, valores por defecto */
  --bg: var(--dtvrs-white);
  --accent: var(--dtvrs-cinnabar);
  /* ... */
}
@media (prefers-color-scheme: dark){
  :root:not([data-theme="light"]){ /* sistema en oscuro, sin override explícito */
    --bg: var(--dtvrs-gunmetal);
    /* ... mismos tokens, valores oscuros */
  }
}
:root[data-theme="dark"]{ /* usuario forzó oscuro desde el toggle de la UI */
  --bg: var(--dtvrs-gunmetal);
  /* ... idénticos a los del media query de arriba */
}
```

Motivo: el visor de Artifacts tiene **tres** estados de tema (claro explícito / oscuro
explícito / "sistema" sin atributo), no dos. Omitir el bloque `@media` dark dejaría el modo
"sistema" roto — es el bug más fácil de introducir sin darse cuenta.

### Tokens derivados útiles (no son de marca, pero simplifican mantenimiento)

```css
--nav-btn-bg: var(--dtvrs-charcoal);        /* claro */
--nav-btn-bg-hover: var(--dtvrs-gunmetal);
--nav-btn-bg-active: var(--dtvrs-cinnabar);
--nav-btn-ink: #ffffff;                      /* texto de botones de navegación */
/* en oscuro: nav-btn-bg → salmon, nav-btn-bg-active → cinnabar, nav-btn-ink → #222A35 */
```
Centralizar "color de fondo de botón de navegación" y "color de texto de botón de
navegación" en variables permite que un solo cambio de tokens (p. ej. "los textos deben ser
gunmetal en oscuro") se propague a todos los botones sin tocar cada regla CSS una por una.

---

## 2. Tipografía

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Nunito+Sans:wght@200;300;400&family=Barlow+Semi+Condensed:wght@200;500;600&display=swap" rel="stylesheet">
```

- **Cuerpo:** Nunito Sans — weight vía token `--fw-body` (300 en este proyecto; el cliente
  puede pedir otro valor, mantenerlo centralizado en la variable, no hardcodeado por regla).
- **Títulos (h1–h4):** Nunito Sans — weight vía token `--fw-heading` (400 aquí).
- **Eslogan / etiquetas / eyebrow:** Barlow Semi Condensed.
- `font-stretch:75%` en `body` es una aproximación a un requisito de "ancho condensado" —
  ni Nunito Sans ni Barlow Semi Condensed exponen un eje variable `wdth` real vía Google
  Fonts, así que el efecto es sutil/nulo según navegador. Decirlo explícitamente al cliente,
  no dejar que lo descubra solo.
- Los `<select>` y otros controles de formulario **no heredan weight por defecto** en varios
  navegadores — agregar `select{font-family:inherit;font-weight:var(--fw-body);}` explícito,
  si no los dropdowns se ven con un peso distinto al resto de la UI.

---

## 3. Estructura de layout (secciones fijas)

```
<header class="top" id="top">           ← sticky, position:sticky top:0
  título + subtítulo + tabs de navegación (botones, no <a>, mismo bloque que el título)
  logo (claro/oscuro) + selector de tema + badge "Actualizado: fecha"
</header>
<main>
  <div class="filterbar">               ← moneda, rango de fechas A/B, acciones (CSV/PDF/reset)
  <div class="kpi-section">             ← bordeada, título "Resumen ejecutivo" + botón Inicio + 5 KPI cards
  <section class="tabpanel">...</section>  ← una por pestaña, cada una con botón Inicio propio
  <footer class="site">
</main>
```

**Componentes reutilizables entre proyectos:**
- **KPI card:** label + valor + barras comparativas A/B + badge de variación (▲/▼ + monto + %).
  Sin barra lateral de color (se probó y el cliente la pidió fuera — mantiene el look limpio).
- **Botón "↑ Inicio":** uno por sección, clase `.btn-inicio` (con borde) + atributo
  `data-scroll-top`, delegado a un solo listener global. **Nunca usar
  `element.scrollIntoView()` apuntando al header** — ver Gotcha #1 abajo.
- **Tabla comparativa por dimensión:** botones (Ciudad/Canal/Marca/...) que cambian el
  `GROUP BY` de una tabla Período A vs Período B vs variación absoluta/relativa. Columnas
  redimensionables a mano (drag en un `.col-resizer` por `<th>`, con `table-layout:fixed`).
- **Sección de hallazgos:** tarjetas con borde de color izquierdo (verde/ámbar/rojo) + tag
  FAVORABLE/VIGILAR/DESFAVORABLE, generadas por código a partir de las variaciones más
  relevantes (no a mano).
- **Selector de rango de fechas:** un solo `<select>` que cambia sus opciones según el
  preset (Mes → "Julio 2026"; Trimestre → "Q3 2026"; Semestre → "H2 2026"; Personalizado →
  año + mes-desde + mes-hasta). Año fiscal = año calendario salvo que el cliente diga otra
  cosa.

---

## 4. Patrón de datos embebidos (para datasets grandes)

No embeber filas completas — usar **tablas de dimensión + array de hechos compacto**:

```js
// dataset.json
{
  "stores":   [[ciudad, canal, sub_canal, ubicacion_dms], ...],   // dimensión, sin repetir
  "products": [[marca, linea, producto, unidades], ...],           // dimensión, sin repetir
  "facts":    [[fechaSerial, storeIdx, productIdx, cantidad, precio_u, tco], ...] // hechos, numérico
}
```
Expandir a filas completas en el cliente (`app.js`) al cargar. Esto redujo el dataset de
~5-6MB a ~0.5MB en este proyecto (18.784 filas). Las fechas van como **serial de Excel**
(`Date.UTC(1899,11,30)` como época), nunca como string ISO — evita ambigüedad de timezone.

---

## 5. Proceso de build (plantilla + splice)

Archivos de trabajo (en el scratchpad de la sesión, no en el repo del proyecto):
- `dashboard_template.html` — HTML/CSS con placeholders `/*__DATASET_JSON__*/`,
  `/*__APP_JS__*/`, `__LOGO_DATA_URI__`, `__LOGO_DATA_URI_DARK__`, `__FAVICON_DATA_URI__`,
  `__GENERATED_AT__`.
- `app.js` — toda la lógica (estado, agregaciones, renderizado SVG, exportación).
- `dataset.json` — generado desde el Excel/fuente de datos.
- `build.js` — hace el splice final:

```js
// build.js — SIEMPRE usar función de reemplazo, nunca string directo
let out = template.replace('/*__DATASET_JSON__*/', ()=>dataset);
out = out.replace('/*__APP_JS__*/', ()=>appjs);
```

---

## 6. Gotchas ya encontrados (evitar repetirlos)

1. **`String.replace(placeholder, texto)` con texto que contiene `$'`, `$&`, `` $` `` o
   `$1`-`$9`** — JavaScript interpreta esos patrones como reemplazos especiales y trunca o
   corrompe el resultado silenciosamente (sin error). Nuestro `app.js` usa `'▲ $'` en varios
   sitios y esto rompió el build una vez. **Solución fija:** pasar siempre una función
   `(match) => texto` como segundo argumento de `.replace()`, nunca el string directo, en
   cualquier script de build.

2. **`elemento.scrollIntoView()` sobre un header con `position:sticky`** no hace nada,
   porque el navegador considera que el elemento ya está "visible" en su posición fija
   aunque la página esté scrolleada. Los botones "Inicio" deben usar
   `window.scrollTo({top:0, behavior:'smooth'})`, nunca `scrollIntoView` sobre el header.

3. **El navegador de esta herramienta (Claude Browser pane) renderiza en blanco los
   screenshots de contenido scrolleado** aunque el DOM/CSS estén correctos — es una
   limitación del tool, no del artefacto. Verificar secciones fuera del primer viewport con
   `javascript_exec`/`get_page_text` (conteos de elementos, `getComputedStyle`,
   `getBoundingClientRect`), no confiar en el screenshot para eso. Además, `behavior:'smooth'`
   no anima mientras el pane está oculto (Chrome throttlea rAF en pestañas en segundo plano)
   — probar el salto instantáneo (`scrollTo(0,0)` sin smooth) para confirmar que el mecanismo
   funciona, y no interpretar la falta de animación en el pane como un bug real.

4. **Editar archivos UTF-8 (con tildes/ñ/símbolos) vía PowerShell `Get-Content`/
   `Set-Content` sin `-Encoding utf8` explícito** corrompe los caracteres no-ASCII
   (mojibake), y algunos símbolos multibyte (↺, →, ‹, ›, ↗) se pierden de forma
   irreversible (se convierten en el carácter de reemplazo `�` durante la decodificación
   incorrecta, no solo se ven mal). Para cualquier find/replace masivo en un archivo con
   texto en español, usar **Node.js** (`fs.readFileSync(p,'utf8')` / `writeFileSync(p, x,
   'utf8')`), nunca PowerShell `-replace` sobre el archivo completo. Si ya se corrompió pero
   fue un mojibake simple (sin pérdida), se puede revertir con
   `Buffer.from(textoCorrupto, 'latin1').toString('utf8')`.

5. **Exportación de PDF vía `window.claude.downloads`**: el payload binario debe pasarse
   como `Uint8Array`, nunca como `string` — un string se codifica como UTF-8 y corrompe
   cualquier byte >127 del PDF. Este proyecto usa un generador de PDF mínimo hecho a mano
   (sin librerías, porque el sandbox del Artifact bloquea CDNs externos como jsPDF) — ver
   `pdfEscape`/`buildSimplePdf` en `app.js` como referencia si se necesita replicar.

6. **Logo dual claro/oscuro**: mejor resuelto con **dos `<img>`** (uno `.logo-light`, otro
   `.logo-dark`) y CSS `display:none/block` siguiendo el mismo patrón de 3 estados de tema
   (§1), que con JS cambiando el `src`. Cero listeners adicionales, funciona también con
   "sistema" sin código extra.

---

## 7. Publicación como Artifact

- `capabilities: {"downloads": true}` — necesario para los botones de exportar CSV/PDF.
- El parámetro `favicon` del tool `Artifact` solo acepta 1-2 **emoji** (ej. `"📊"`) — es el
  ícono de pestaña dentro de claude.ai, no un favicon real. Si el HTML se va a hospedar
  también fuera de claude.ai (GitHub Pages, etc.), agregar además un
  `<link rel="icon" type="image/png" href="data:image/png;base64,...">` real en el `<head>`
  del HTML, con el isotipo de marca (no el emoji).
- Antes de publicar, probar localmente sirviendo el HTML con `preview_start` +
  `.claude/launch.json` apuntando a `npx -y serve -l <puerto> <carpeta>` — el Browser pane
  de esta sesión no está autenticado contra claude.ai y no puede abrir el Artifact privado
  directamente.

---

## 8. Publicación en GitHub

Ver memoria de sesión `github_dataversos_setup` para los detalles de la organización, el
repo contenedor `dtvrs`, la convención de branches y el flujo de autenticación que funciona
en esta máquina. Resumen rápido:
- Org: `dataversostia` (no "dataversos" a secas).
- Repo contenedor: `dataversostia/dtvrs`, una rama por caso de uso, nombrada igual que la
  carpeta del proyecto.
- GitHub Pages gratis requiere repo **público** — evaluar con el cliente antes de cambiar
  visibilidad, el HTML embebe datos (aunque sean de ejemplo/business case).
