## Purpose & context
Artefacto para visualización de cuentas por pagar de la empresa, y gestionar programación de pagos

## Numbers & Symbols Formats 
Utiliza paréntesis para números negativos, tanto para números enteros, decimales y porcentajes
No utilices doble paréntesis en casos de que por el contexto quieras poner una cifra entre paréntesis y sea además negativa.
Utiliza el símbolo "$" para dólares estadounidenses, el símbolos "Bs" para bolivianos y los símbolos ▲$ y ▲% para variaciones en monto económico y variaciones en porcentaje

*Tools & resources*
Dile a Claude qué recordar u olvidar...

# Guía de marca — dataversos
*Transformando datos en oportunidades*

Fuente única de verdad para colores y tipografía. Pensado para pegarse en Claude Code
y en cualquier material (web, formulario de diagnóstico, documentos, presentaciones).

---

## Paleta de colores

| Color | Hex | RGB | Rol sugerido |
|---|---|---|---|
| **Gunmetal** | `#222A35` | 34, 42, 53 | Fondo oscuro principal · texto sobre fondos claros |
| **Charcoal** | `#333F50` | 51, 63, 80 | Superficies (tarjetas, barras) · texto secundario |
| **Cool gray** | `#8497B0` | 132, 151, 176 | Texto tenue · eslogan · bordes · estados deshabilitados |
| **Blushed brick** | `#CB4F55` | 254, 61, 46 | **Acento principal** · botones/CTAs · enlaces · nodo del isotipo |
| **Sweet Salmon** | `#F89E97` | 243, 117, 108 | Acento suave · hover · fondos de énfasis tenue |

**Regla de uso del acento:** Cinnabar es tu color de marca, pero úsalo con moderación
(botones, enlaces, un dato clave). Si tiñes todo de rojo, pierde fuerza. Salmon es su
compañero más suave para hovers y fondos con un toque de color.

**Neutros de apoyo** (no son colores de marca, pero los vas a necesitar en pantallas claras):
blanco `#FFFFFF` para fondos claros y un off-white `#F4F5F7` para superficies suaves.

---

## Variables CSS (listas para pegar)

```css
:root {
  /* Colores de marca */
  --dtvrs-gunmetal:      #222A35;
  --dtvrs-charcoal:      #333F50;
  --dtvrs-cool-gray:     #8497B0;
   --dtvrs-blushedbrick: #CB4F55;
  --dtvrs-sweetsalmon:   #F89E97;

  /* Neutros de apoyo */
  --dtvrs-white:     #FFFFFF;
  --dtvrs-offwhite:  #F4F5F7;

  /* Roles semánticos — TEMA CLARO (recomendado para el formulario) */
  --color-bg:          var(--dtvrs-white);
  --color-surface:     var(--dtvrs-offwhite);
  --color-text:        var(--dtvrs-gunmetal);
  --color-text-muted:  var(--dtvrs-cool-gray);
  --color-accent:      var(--dtvrs-blushedbrick);
  --color-accent-soft: var(--dtvrs-sweetsalmon);
  --color-border:      var(--dtvrs-cool-gray);
}

/* Roles semánticos — TEMA OSCURO (para landing / secciones hero) */
.tema-oscuro {
  --color-bg:         var(--dtvrs-gunmetal);
  --color-surface:    var(--dtvrs-charcoal);
  --color-text:       var(--dtvrs-white);
  --color-text-muted: var(--dtvrs-cool-gray);
  --color-accent:     var(--dtvrs-blushedbrick);
}
```

**Nota de accesibilidad:** Cinnabar (`#FE3D2E`) sobre blanco no alcanza contraste AA para
texto pequeño; úsalo para botones (texto blanco encima) y titulares, no para párrafos largos.
Para texto de cuerpo usa Gunmetal sobre blanco (contraste excelente).

---

## Tipografía

### Logo (no tocar)
El logo usa **Avenir Next LT Pro Bold** y **Avenir Next LT Pro Condensed**.
Mantén el logo como imagen exportada (SVG/PNG): la fuente queda incrustada, así que no
necesitas tenerla instalada ni licenciada para mostrarlo.

### Web, UI y documentos (recomendado)
**No usar Arial Narrow** (no es confiable en web; el condensado no se renderiza bien en
muchos dispositivos). En su lugar, un stack que usa el Avenir Next real donde exista
(dispositivos Apple lo traen de fábrica) y cae a una webfont gratuita casi idéntica:

```css
/* Texto general y titulares */
--fuente-principal: "Avenir Next", "Nunito Sans", system-ui, -apple-system, "Segoe UI", Arial, sans-serif;

/* Momentos condensados (eslogan, etiquetas, números grandes) */
--fuente-condensada: "Avenir Next Condensed", "Barlow Semi Condensed", sans-serif;
```

**Webfonts a cargar** (gratis, Google Fonts):
- **Nunito Sans** — pesos 200 (light), 400 (texto), 600 (semibold), 700 (bold). Sustituto libre de Avenir, humanista y aireada.
- **Barlow Semi Condensed** — pesos 500/600, solo para el estilo condensado (eslogan, etiquetas).

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Nunito+Sans:wght@300;400;600;700&family=Barlow+Semi+Condensed:wght@500;600&display=swap" rel="stylesheet">
```
> Si algún peso no carga, genera la línea exacta desde fonts.google.com (Nunito Sans es una
> fuente variable y su URL puede incluir ejes adicionales).

### Jerarquía sugerida
- **Titulares:** Nunito Sans **300 (Light)** — el look delgado y elegante — o 700 si quieres más contraste. Color Gunmetal (o blanco en tema oscuro).
- **Cuerpo:** Nunito Sans **400**, color Gunmetal. (No bajes de 400 en párrafos de celular: el light cansa la vista en texto pequeño.)
- **Eslogan / etiquetas:** fuente condensada (Barlow Semi Condensed), color Cool gray.
- **Botones:** Nunito Sans **600/700**, fondo Cinnabar, texto blanco.
