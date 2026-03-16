# Phase 3: Content Sections - Context

**Gathered:** 2026-03-16
**Status:** Ready for planning

<domain>
## Phase Boundary

The full page body is populated — a visitor can scroll through About, Products, Services, Values, Certifications, and Corporate Purpose sections, understanding what campivacorp. does, what they sell, and why they are trustworthy. These are 6 independent content sections that slot into the existing index.astro placeholder anchors.

</domain>

<decisions>
## Implementation Decisions

### Section Order and Backgrounds
Sections appear in this order with alternating backgrounds for visual rhythm:
1. **Quiénes Somos** (#nosotros) — white bg + brand ornaments
2. **Productos** (#productos) — white bg + brand ornaments
3. **Servicios** (#servicios) — white bg
4. **Valores** (#valores) — white bg
5. **Certificaciones** (#certificaciones) — dark bg #25272f, text white (decided Phase 1)
6. **Propósito Corporativo** (#proposito) — dark bg #25272f, text white (decided Phase 1)

### Quiénes Somos (ABOU-01, ABOU-02, ABOU-03)
- Two-column layout: text left, placeholder image right (desktop), stacked on mobile
- Exact corporate text from brief (Spanish, no modifications)
- Brand ornament leaf shapes in background at 5-8% opacity (BrandOrnament.astro from Phase 1)
- Placeholder image: use a gradient placeholder or solid #95b444/20% block with text "Imagen" — structured for easy image drop-in later
- Section title: "Quiénes Somos" in Nunito Sans 800

### Productos (PROD-01, PROD-02, PROD-03, PROD-04)
- Grid of 7 category cards: responsive 1-col mobile, 2-col tablet, 3-col desktop (last row centered)
- Each card: Tabler icon (already installed via astro-icon) + category name + brief description
- Cards styled with border-[#95b444]/20 or subtle shadow, hover effect (slight scale or shadow increase)
- **Expandable sub-detail**: Use Preline accordion (hs-accordion) within each card — clicking reveals full product list for that category
- Product lists from brief:
  - Aceites: Crudo de Soya, Crudo de Girasol, Refinado de Soya, Refinado de Girasol, Refinado Soya+Girasol Blend
  - Harinas y Tortas: Harina de Soya Expeller, Torta de Soya Estándar, Torta de Soya HiPro, Torta de Girasol, Harina de Carne y Hueso, Harina de Plumas
  - Granos: Soya, Maíz, Sorgo, Maní, Quinua (blanca/negra/roja), Chía, Sésamo (negro/blanco), Cacao, Café
  - Endulzantes: Azúcar Refinada, Azúcar Rubia, Miel de Abeja, Stevia
  - Grasas: Mantecas y Margarinas, Sebo Bovino, Ácidos Grasos, Oleínas Vegetales, Lecitina de Soya
  - Derivados Forestales: Carbón Vegetal, Leña
  - Hidrocarburos: Gasolina, Diesel ULSD
- Tabler icon suggestions per category (Claude's discretion on exact icons):
  - Aceites: tabler:droplet
  - Harinas y Tortas: tabler:grain
  - Granos: tabler:plant
  - Endulzantes: tabler:candy
  - Grasas: tabler:flask
  - Derivados Forestales: tabler:tree
  - Hidrocarburos: tabler:gas-station
- Brand ornament in background

### Servicios (SERV-01, SERV-02)
- Grid or list layout with icon + title + short description per service
- 7 services from brief: Trading, Brokeraje, Logística local e internacional, Analítica de mercados y Soft Landing, Asesoramiento técnico y comercial, Maquilas, Análisis de laboratorio vía Surveyors
- Each service gets a Tabler icon (Claude's discretion)
- Clean, minimal cards or horizontal items

### Valores (VALU-01, VALU-02)
- 4 value cards in a 2x2 or 4-col grid
- Each card: icon + title + description text from brief
- Values: Calidad, Respeto, Excelencia, Pasión
- Cards with subtle #95b444 accent (top border or icon color)

### Certificaciones (CERT-01, CERT-02)
- Dark background section (#25272f)
- 5 certifications displayed prominently: HACCP, GMP/BPM, ISO 9001, ISO 22000, ISO 14001
- Badge/shield style SVG icons or Tabler certification icons
- Horizontal row on desktop, wrapping on mobile
- White text, badges can use #95b444 accent

### Propósito Corporativo (PURP-01, PURP-02)
- Dark background section (#25272f)
- Exact corporate purpose text from brief (Spanish)
- Large, impactful typography — this is the "mission statement" moment
- Centered text layout, generous padding
- Optional: brand ornament at very low opacity on dark bg (if it looks good, otherwise skip)

### Claude's Discretion
- Exact Tabler icon names for each product/service/value
- Whether services use cards or horizontal list items
- Exact spacing between sections
- Whether to use WidgetWrapper for each section or custom containers
- Animation details (AOS attributes per section)
- Whether Certificaciones uses shield icons, badge icons, or text-only with accent
- Valores card design details (icon position, border style)

</decisions>

<code_context>
## Existing Code Insights

### Reusable Assets
- `BrandOrnament.astro` — leaf decoration component with position prop (Phase 1)
- `WidgetWrapper.astro` — section wrapper with padding, scroll offset, dark mode support
- `Headline.astro` (ui) — section title component
- `ItemGrid.astro` / `ItemGrid2.astro` — grid layouts for items with icons
- `Features.astro` / `Features2.astro` / `Features3.astro` — feature showcase widgets
- Tabler icons via `astro-icon` — full set available
- Preline UI — accordion component via data attributes (hs-accordion)

### Established Patterns
- index.astro has placeholder `<section>` elements with correct IDs for all 6 sections
- Each section wraps in WidgetWrapper or custom section with `scroll-mt-[72px]` for navbar offset
- Dark sections: use `bg-[#25272f]` + `text-white` directly (no .dark class)
- AOS attributes added in Phase 4 polish, but can be included now

### Integration Points
- index.astro placeholder sections need to be replaced with actual widget imports
- Each widget is a standalone .astro component in src/components/widgets/

</code_context>

<specifics>
## Specific Ideas

- Products section should feel like a catalog — organized, scannable, professional
- Services should emphasize the VALUE each service provides, not just list names
- Certificaciones on dark bg should feel "authoritative" — like trust badges on an e-commerce site
- Propósito Corporativo should feel like a "mission statement wall" — impactful, clean
- References: camposol.com product sections, raatz.com.py product catalog style

</specifics>

<deferred>
## Deferred Ideas

- Product detail sub-pages — out of scope (single page with expandable cards)
- PDF product catalog download — v2 feature
- Client logos / social proof strip — requires client assets

</deferred>

---

*Phase: 03-content-sections*
*Context gathered: 2026-03-16*
