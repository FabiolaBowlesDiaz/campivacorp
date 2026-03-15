# campivacorp. Landing Page

## What This Is

A corporate landing page for campivacorp., a Bolivian agroindustrial trading company that commercializes products for human and animal consumption — raw materials and finished goods. The site showcases their 25+ years of experience, product portfolio (oils, flours, grains, sweeteners, fats, forestry derivatives, hydrocarbons), services (trading, brokerage, logistics, market analytics), and certifications (HACCP, GMP/BPM, ISO 9001/22000/14001). Built on Astro + Tailwind CSS with AstroWind template, targeting B2B clients in agro/food industries across regional markets.

## Core Value

Present campivacorp. as a credible, premium agroindustrial trading partner — the site must convey trust, expertise, and professionalism through precise brand execution and comprehensive product/service information.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Fixed navbar with SVG isotipo logo + "campivacorp." wordmark + nav links + CTA button
- [ ] Hero section with Swiper slider (3 slides, overlay, dual CTAs)
- [ ] Animated stats counter section (25+ years, 7 categories, regional markets, 5 certifications)
- [ ] "Quienes Somos" section with corporate text + brand ornaments
- [ ] Products grid (7 category cards with SVG icons)
- [ ] Services section with icons and descriptions
- [ ] Values section (Calidad, Respeto, Excelencia, Pasion)
- [ ] Certifications banner (HACCP, GMP/BPM, ISO 9001, ISO 22000, ISO 14001)
- [ ] Corporate purpose/vision section
- [ ] Contact section with form + info (email, phone, social links)
- [ ] Footer with brand, nav links, social icons, copyright
- [ ] Full brand book implementation (colors, typography, ornaments, visual style)
- [ ] Responsive design (mobile, tablet, desktop)
- [ ] AOS scroll animations throughout
- [ ] Spanish language content (all copy in Spanish)

### Out of Scope

- Blog/news section — not needed for initial corporate presence
- E-commerce/ordering — campivacorp. operates B2B via direct contact
- Multi-language (English) — future consideration, Spanish only for now
- Backend/CMS — static site, content managed in code
- Product detail pages — single-page landing, cards link to contact
- Dark mode — brand is light-focused with dark accent sections

## Context

**Tech stack (already set up):**
- Astro v5.12.9 with AstroWind template
- Tailwind CSS with DaisyUI (prefixed `daisy-`), Preline UI
- AOS (Animate On Scroll) — initialized in Layout.astro
- Swiper — CSS imported globally, JS per-component
- Google Fonts: Nunito Sans (800) for headings, Montserrat (500/700) for body

**Brand Book (MUST follow exactly):**

Colors:
- `#25272f` — base/text/navbar (carbon dark)
- `#95b444` — primary green (buttons, highlights)
- `#cbdc53` — lime green (accent, hover states)
- `#5d6f31` — dark green (alternate sections, depth)
- `#ffffff` — backgrounds and text on dark colors

Typography:
- Headings/Logo: Nunito Sans weight 800 (closest web alternative to Gotham Bold)
- Body text: Montserrat Medium (500) and Bold (700)
- Brand name always: "campivacorp." — lowercase with period

Logo/Isotipo:
- 3 organic overlapping leaves in green gradient (#95b444 to #5d6f31 with #25272f border)
- SVG inline implementation
- Navbar: isotipo + "campiva" bold + "corp." regular + period

Visual Style:
- Corporate premium, serious, trustworthy
- Backgrounds: pure white or #25272f (carbon) — no generic grays
- Cards: subtle #95b444 border or soft shadow
- Primary buttons: #95b444 bg, white text, hover #5d6f31
- Secondary buttons: #95b444 border, #95b444 text, transparent bg
- Brand ornaments: organic leaf shapes at 5-10% opacity as background decorations
- NO: purple gradients, generic corporate blues, heavy shadows, "AI generic" design

**Company Information:**

Name: campivacorp. (always lowercase with period)

Quienes Somos (exact text):
"Somos una empresa cuyos fundadores tienen mas de 25 anos de experiencia en la industria de oleos, derivados y subproductos; nutricion humana con productos agroindustriales y nutricion animal. Enfocados en brindar soluciones de alimentacion humana y animal, con transacciones tanto locales como internacionales, atendiendo diversos mercados de la region. Nuestros socios estrategicos y nosotros cumplimos con los estandares mas exigentes de inocuidad, calidad, trazabilidad y medio ambiente; monitoreando a todos los participantes de nuestra cadena de valor desde la produccion en campo, transformacion industrial, almacenamiento y logistica, centrando al productor primario y el cliente final como el centro de nuestra estrategia corporativa."

Proposito Corporativo:
"Consolidarnos como socios estrategicos de todas las industrias que se dediquen a la industrializacion y comercializacion de bienes de consumo humano alineados a nuestro portafolio de productos, enfocados en bienestar, calidad, seguridad, inocuidad, innovacion y nutricion de precision. Asi mismo, brindar soluciones integrales para la nutricion animal con productos altamente nutritivos, estandarizados y accesibles."

Products (7 categories):
1. Aceites: Crudo de Soya, Crudo de Girasol, Refinado de Soya, Refinado de Girasol, Refinado Soya+Girasol Blend
2. Harinas y Tortas: Harina de Soya Expeller, Torta de Soya Estandar, Torta de Soya HiPro, Torta de Girasol, Harina de Carne y Hueso, Harina de Plumas
3. Granos: Soya, Maiz, Sorgo, Mani, Quinua (blanca/negra/roja), Chia, Sesamo (negro/blanco), Cacao, Cafe
4. Endulzantes: Azucar Refinada, Azucar Rubia, Miel de Abeja, Stevia
5. Grasas: Mantecas y Margarinas, Sebo Bovino, Acidos Grasos, Oleinas Vegetales, Lecitina de Soya
6. Derivados Forestales: Carbon Vegetal, Lena
7. Hidrocarburos: Gasolina, Diesel ULSD

Services:
- Trading
- Brokeraje
- Logistica local e internacional
- Analitica de mercados y Soft Landing
- Asesoramiento tecnico y comercial
- Maquilas
- Analisis de laboratorio via Surveyors

Certifications: HACCP, GMP/BPM, ISO 9001, ISO 22000, ISO 14001

Values:
- Calidad: estandares mas altos de calidad e inocuidad
- Respeto: entorno, medio ambiente, colaboradores, etica
- Excelencia: especialistas y perfeccionistas
- Pasion: agroindustria y cadena de valor alimentaria

Contact:
- Email: rcampbell@campivacorp.com
- Phone: +59169006424
- Social: Facebook, WhatsApp, Instagram, LinkedIn

**Sections to build (in order):**
1. Navbar (fixed, SVG isotipo + wordmark + links + CTA)
2. Hero (Swiper, 3 slides with overlay, dual CTAs)
3. Stats (AOS animated counters)
4. Quienes Somos (photo placeholder + text + ornaments)
5. Productos (7 category cards with SVG icons)
6. Servicios (icon + description grid)
7. Valores (4 value cards)
8. Certificaciones (banner with logos/badges)
9. Proposito Corporativo (vision section)
10. Contacto (form + info + map placeholder)
11. Footer (brand, links, social, copyright)

## Constraints

- **Tech stack**: Astro 5 + Tailwind CSS + AstroWind template — already installed, must work within this framework
- **Brand**: Exact color palette, typography, and visual style from brand book — no deviations
- **Language**: All content in Spanish
- **Performance**: Static site, must be fast — no heavy JS frameworks
- **Responsive**: Must work on mobile, tablet, and desktop
- **Libraries**: Swiper for hero slider, AOS for scroll animations, Preline for interactive components — already wired up

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| AstroWind template base | Pre-built components, Tailwind integration, good performance | — Pending |
| DaisyUI with `daisy-` prefix | Avoids class collisions with AstroWind | — Pending |
| Nunito Sans 800 for Gotham Bold | Closest free web font alternative | — Pending |
| Single-page landing (not multi-page) | Simpler for corporate showcase, all info accessible | — Pending |
| SVG inline isotipo | Scalable, color-customizable, no image dependency | — Pending |
| Placeholder images | Real photos to be added later by client | — Pending |

---
*Last updated: 2026-03-15 after initialization*
