# Octagon Labs — Sitio web

Sitio institucional inmersivo para Octagon Labs, casa independiente de investigación y desarrollo tecnológico (Monterrey, México). Estética oscura, futurista y minimalista, con un núcleo 3D construido a partir de la geometría real del isotipo.

## Formato de entrega
Se entrega como **una experiencia HTML autocontenida e interactiva** (`Octagon Labs.dc.html`) que abre directamente en el navegador: 3D real (Three.js), animaciones de scroll (GSAP/ScrollTrigger), formulario funcional, cursor personalizado, preloader, bilingüe ES/EN y responsive. No es un árbol de código Next.js ejecutable, porque este entorno renderiza HTML en vivo en lugar de correr un build de Node. Abajo se documenta cómo mapear cada pieza al stack Next.js solicitado.

## Sistema visual
- **Color:** fondo `#0A0C10` (negro azulado), tinta `#F4F6F8`. Acentos armónicos (misma luminosidad/croma, distinto tono) por proyecto: TurboShip `#7FB2FF`, Agente de Guías `#79E6A6`, Timbra Chat `#F2D479`, FanKit `#FF9D8A`. Acento base cian `#8FE3F0`.
- **Tipografía:** Helvetica Neue/Helvetica (editorial/UI, igual que el wordmark de la marca) + IBM Plex Mono (etiquetas técnicas, índices, coordenadas).
- **Geometría:** el isotipo es un octágono con las 28 conexiones vértice-a-vértice (grafo K8). Esa "malla" es la firma y se reutiliza en el núcleo 3D, favicon, OG, preloader, menú y las consolas de proyecto — no un octágono genérico.

## Uso de la marca (regla resuelta)
Los archivos originales usan convención por **tema**: `*-light.svg` = tinta negra (para fondos claros), `*-dark.svg` = tinta blanca / transparente (para fondos oscuros). Como el sitio es oscuro, en la UI se usa la variante **blanca** (`octagon-labs-*-dark.svg`). En secciones/documentos claros usar la variante negra (`octagon-labs-*-light.svg`). No se recolorea, deforma ni se le agregan sombras. Assets organizados en `public/brand/`.

## Editar contenido (bilingüe, centralizado)
Todo el texto vive en la clase lógica del `.dc.html`, método `data()`, en dos árboles: `EN` y `ES`. Editar ambos para mantener la traducción. El sitio abre en inglés; el selector EN/ES cambia todo en vivo.

### Agregar un proyecto
1. En `data()` añade una entrada a `projBase` (num, name, slug, accent, stack).
2. Añade el objeto correspondiente en `EN.projects.items` **y** `ES.projects.items` (tagline, description, cat, status, industry[]).
El layout, la consola geométrica y el cambio de acento del núcleo 3D se generan solos.

### Métricas (placeholders)
`EN/ES.metrics.items` — cifras marcadas como placeholders configurables (nota visible "◇ Configurable placeholders"). Cambia `num`, `display`, `suffix`, `label`.

### Contacto
Correo configurable en `data().cfg.email` (actualmente `hello@octagonlabs.com`, placeholder). Redes en `cfg.socials`.

## Tweaks disponibles (props del componente)
`startLang` (en/es), `accent` (color base + swatches por proyecto), `enable3D` (fallback 2D), `reduceMotion`.

## Cambiar el objeto 3D / texturas
En `initCore()`: el núcleo se genera desde 8 vértices (`R`, ángulos `π/8 + i·π/4`) y sus 28 conexiones. Ajusta `R`, la nube de partículas (`N`), el `halo`, o sustituye la geometría manteniendo los vértices del octágono. Colores del núcleo vía `THREE.Color`. Degradación: si no hay WebGL, se muestra un isotipo SVG 2D animado.

## Mapeo al stack Next.js solicitado
- Secciones → `components/sections/*`, layout → `components/layout/*`, escena → `components/three/Core.tsx` (React Three Fiber + Drei; aquí Three.js puro).
- `data()` → `data/projects.ts`, `data/metrics.ts`, `content/{en,es}.ts` (i18n).
- Animaciones → GSAP/ScrollTrigger (igual) + Framer Motion donde convenga.
- SEO → `app/layout.tsx` metadata + JSON-LD (ya incluido en el `<head>`).

## Recursos pendientes de Octagon Labs
- Capturas/UI reales de cada producto (hoy: composición geométrica de marca).
- Cifras reales de métricas (hoy placeholders) y estatus confirmados por proyecto.
- Correo definitivo, URLs de redes, año de fundación, perfiles de equipo (sección "próximamente" ya preparada).
- Dominio/canonical y una imagen OG definitiva (se generó una base en `public/brand/og-image.png`).
