# Phase 2: Page Shell + Above the Fold - Context

**Gathered:** 2026-03-16
**Status:** Ready for planning

<domain>
## Phase Boundary

A visitor landing on the site sees a professional fixed navbar with the campivacorp. logo, a full-screen hero slider with three branded slides, and animated stat counters -- the complete first impression. This phase rewrites navigation.ts, customizes Header.astro, creates a Swiper hero component, and adds animated stats.

</domain>

<decisions>
## Implementation Decisions

### Navbar
- Rewrite navigation.ts completely -- replace all AstroWind demo links with anchor links (#inicio, #nosotros, #productos, #servicios, #valores, #certificaciones, #contacto)
- Remove all dropdown menus -- flat nav links only (single-page, no sub-pages)
- CTA button: "Contáctanos" linking to #contacto, styled with bg-[#95b444] text-white hover:bg-[#5d6f31]
- Logo: uses existing Logo.astro (PNG from brand book) -- already done in Phase 1
- Header.astro: set isSticky=true, remove showToggleTheme, remove showRssFeed
- Navbar bg: transparent on top, solid white with subtle shadow on scroll (AstroWind's existing scroll behavior)
- Mobile: AstroWind's ToggleMenu component handles hamburger -- keep it, just update the links

### Hero Slider
- Swiper component with 3 full-height slides (100vh or min-h-[600px])
- Each slide: background image with #25272f overlay at 60% opacity
- Background images: use high-quality placeholder images (Unsplash-style) -- agricultural fields, products, global commerce
- Store placeholder images in src/assets/images/hero/
- Text on each slide: large heading (Nunito Sans 800, white), subheading (Montserrat, white/muted)
  - Slide 1: "Soluciones agroindustriales para el mundo"
  - Slide 2: "Calidad certificada en cada transacción"
  - Slide 3: "25 años conectando mercados"
- Dual CTAs centered below text:
  - Primary: "Ver Productos" → #productos (bg-[#95b444] text-white)
  - Secondary: "Contactar" → #contacto (border-white text-white, transparent bg)
- Swiper config: autoplay 5s, fade or slide effect, pagination dots (white), navigation arrows on desktop only
- Import Swiper JS modules individually (Navigation, Pagination, Autoplay, EffectFade) per research recommendation

### Stats Section
- Dark background section (#25272f) immediately after hero
- 4 stat counters in a row (responsive: 2x2 on mobile, 4-col on desktop)
- Stats:
  - "25+" with label "Años de experiencia"
  - "7" with label "Categorías de productos"
  - "5" with label "Certificaciones internacionales"
  - "100%" with label "Compromiso con la calidad"
- Numbers animate counting up from 0 when section enters viewport
- Use Intersection Observer for trigger (not AOS -- custom counter animation)
- Numbers in Nunito Sans 800, large size (3-4rem), white
- Labels in Montserrat 500, smaller, white/muted

### Index Page
- Rewrite src/pages/index.astro as single-page layout with section anchors
- Remove all AstroWind demo content
- Structure: Hero → Stats → (placeholder slots for Phase 3 sections) → (placeholder for Phase 4)
- Each section gets an id attribute for anchor navigation

### Claude's Discretion
- Exact Swiper effect choice (fade vs slide)
- Hero image placeholders (can use solid color gradients if images unavailable)
- Stats animation timing and easing
- Exact responsive breakpoints for stats grid
- Whether to remove AstroWind demo pages (about, pricing, services, etc.) now or defer
- Navigation scroll offset (accounting for fixed header height)

</decisions>

<code_context>
## Existing Code Insights

### Reusable Assets
- `Header.astro`: Full navbar component with sticky support, dropdown menus, CTA buttons, mobile toggle. Needs data update, minimal structural changes.
- `ToggleMenu.astro`: Mobile hamburger menu -- reuse as-is
- `Button.astro`: CTA button component -- reuse for navbar and hero CTAs
- `WidgetWrapper.astro`: Section wrapper with padding, scroll offset, intersection animations
- `Logo.astro`: Already updated with campivacorp. PNG logo

### Established Patterns
- Navigation data centralized in `src/navigation.ts` -- single file controls all nav/footer links
- Header accepts links[] and actions[] arrays from page props
- AstroWind pages pass headerData from navigation.ts to PageLayout which passes to Header
- Swiper CSS already imported globally in Layout.astro (from Phase 1 setup)

### Integration Points
- `src/navigation.ts` → headerData/footerData → consumed by PageLayout → Header/Footer
- `src/pages/index.astro` → imports widgets, passes data to each
- Layout.astro already has AOS init + Preline init scripts
- Swiper JS needs to be imported in the hero component's `<script>` tag (not frontmatter)

</code_context>

<specifics>
## Specific Ideas

- Hero placeholder images: if no real photos available, use solid gradient backgrounds (#25272f to #5d6f31) with subtle brand ornament overlay -- still looks professional
- Stats section should feel impactful -- large numbers, clean typography, dark bg contrasting with the hero
- Navbar should feel minimal and corporate -- no cluttered dropdowns, just clean flat links
- References: camposol.com hero slider style, raatz.com.py navbar simplicity

</specifics>

<deferred>
## Deferred Ideas

- Remove AstroWind demo pages (about.astro, pricing.astro, services.astro, homes/, landing/) -- can be done in Phase 2 or deferred to cleanup
- Footer data in navigation.ts -- Phase 4 handles footer

</deferred>

---

*Phase: 02-page-shell-above-the-fold*
*Context gathered: 2026-03-16*
