# Favicon dtvrs - Guía Rápida

## 📋 Qué tienes

| Archivo | Uso | Tamaño |
|---------|-----|--------|
| `favicon.svg` | Moderno, escalable, recomendado | Vectorial |
| `favicon-32x32.png` | Escritorio principal | 32x32px |
| `favicon-16x16.png` | Fallback antiguo | 16x16px |
| `favicon-64x64.png` | Dispositivos (iOS, Android) | 64x64px |
| `site.webmanifest` | Metadatos PWA (opcional) | JSON |

---

## 🚀 Implementación (3 pasos)

### 1. Copiar archivos a la raíz del proyecto
```
Tu proyecto/
├── index.html
├── favicon.svg         ← copiar aquí
├── favicon-32x32.png   ← copiar aquí
└── favicon-16x16.png   ← copiar aquí
```

### 2. Agregar al `<head>` de tu HTML

**Opción A: Completa (recomendada)**
```html
<head>
    <!-- SVG favicon (moderno) -->
    <link rel="icon" href="/favicon.svg" type="image/svg+xml">
    
    <!-- PNG fallback -->
    <link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
    <link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
    
    <!-- Apple devices -->
    <link rel="apple-touch-icon" href="/favicon-64x64.png">
    
    <!-- Metadatos -->
    <meta name="theme-color" content="#2C3E50">
</head>
```

**Opción B: Minimalista (solo SVG)**
```html
<link rel="icon" href="/favicon.svg" type="image/svg+xml">
```

### 3. Listo
Abre tu navegador → verás el isotipo en la pestaña

---

## ⚙️ Detalles técnicos

### Colores usados
- **Azul oscuro (nodos)**: `#2C3E50`
- **Gris claro (fondo)**: `#D4DEE8`

### Por qué SVG es superior
- ✅ Se escala perfectamente (16px, 32px, 200px)
- ✅ Peso mínimo (~1KB)
- ✅ Se puede animar con CSS
- ✅ Soporte en todos los navegadores modernos

### Cómo forzar recarga durante desarrollo
Si cambias el favicon y no se ve actualizado:
```html
<link rel="icon" href="/favicon.svg?v=2" type="image/svg+xml">
```
(Incrementa el número `v=2`, `v=3`, etc.)

---

## 📱 Soporta

| Plataforma | Compatible |
|----------|-----------|
| Chrome/Edge (desktop) | ✅ SVG |
| Firefox | ✅ SVG |
| Safari | ✅ SVG |
| iOS (home screen) | ✅ PNG 64x64 |
| Android | ✅ PNG/SVG |
| Navegadores antiguos | ✅ PNG 16x16 fallback |

---

## 🎨 Personalización

### Cambiar colores (editar favicon.svg)

Busca y reemplaza:
- `#2C3E50` → Tu color azul
- `#D4DEE8` → Tu color gris

### Remover fondo
En `favicon.svg`, comenta esta línea:
```svg
<!-- <circle cx="100" cy="100" r="100" fill="#D4DEE8"/> -->
```

---

## 🐛 Troubleshooting

| Problema | Solución |
|----------|----------|
| No aparece el favicon | Limpiar caché (Ctrl+Shift+R) + recargar |
| Se ve pixelado | Usar SVG en vez de PNG |
| No funciona en iOS | Asegurar `rel="apple-touch-icon"` con PNG 64x64 |
| Aparece en una pestaña pero no otra | Verificar ruta (debe ser `/favicon.svg` no `./favicon.svg`) |

---

## 📊 Peso de archivos

```
favicon.svg         ~2 KB (vectorial, mejor)
favicon-32x32.png   ~1.7 KB
favicon-16x16.png   ~717 B
favicon-64x64.png   ~3.8 KB
───────────────────────────
Total recomendado   ~2 KB (solo SVG)
```

**Recomendación**: Usar solo `favicon.svg` a menos que necesites compatibilidad con navegadores muy antiguos.
