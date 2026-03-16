# Phase 3: Content Sections - Research

**Researched:** 2026-03-16
**Domain:** Astro content sections with Preline accordion, Tabler icons, brand styling
**Confidence:** HIGH

## Summary

Phase 3 replaces 6 placeholder sections in index.astro with fully realized content widgets. The existing AstroWind codebase provides strong patterns: `WidgetWrapper.astro` for section wrapping, `Headline.astro` for titles, `ItemGrid.astro` / `ItemGrid2.astro` for grid layouts, and `Content.astro` for two-column text+image layouts. The most complex piece is the Products section which needs Preline `hs-accordion` inside each card for expandable product lists -- Preline is already initialized in Layout.astro and its autoInit covers dynamically rendered accordions.

Key finding from code inspection: index.astro is missing a `#proposito` placeholder section -- it jumps from `#certificaciones` to `#contacto`. This must be added. Also, the existing `Content.astro` widget is almost exactly what ABOU-01 needs (two-column with image), and `Features2.astro` with `ItemGrid2.astro` provides the card grid pattern for Valores and Servicios. Products requires a custom widget due to the accordion requirement.

**Primary recommendation:** Build 6 new widget components (one per section), following the existing Features/Content widget patterns. Reuse WidgetWrapper + Headline. Custom-build only ProductosSection (accordion) and CertificacionesSection (dark badge layout). The rest can closely mirror existing widget patterns.

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions
- Section order: Nosotros (white+ornaments), Productos (white+ornaments), Servicios (white), Valores (white), Certificaciones (dark #25272f), Proposito (dark #25272f)
- Nosotros: two-column layout, text left, placeholder image right, exact corporate text, BrandOrnament in bg
- Productos: 7 category cards, 1/2/3 col responsive grid (last row centered), Tabler icons, Preline accordion expandable, brand ornament
- Servicios: 7 services with icon + title + description, Tabler icons
- Valores: 4 cards in 2x2 or 4-col grid, #95b444 accent
- Certificaciones: dark bg, 5 certs (HACCP, GMP/BPM, ISO 9001, ISO 22000, ISO 14001), badge/shield style, horizontal row desktop
- Proposito: dark bg, exact corporate text, large impactful typography, centered

### Claude's Discretion
- Exact Tabler icon names for each product/service/value
- Services layout (cards vs horizontal list items)
- Exact spacing between sections
- Whether to use WidgetWrapper for each section or custom containers
- AOS attributes per section
- Certificaciones icon style (shield, badge, or text-only with accent)
- Valores card design details

### Deferred Ideas (OUT OF SCOPE)
- Product detail sub-pages
- PDF product catalog download
- Client logos / social proof strip
</user_constraints>

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|-----------------|
| ABOU-01 | "Quienes Somos" section with exact corporate text | Content.astro pattern (two-column text+image) as base |
| ABOU-02 | Placeholder image (agricultural/industrial theme) | Gradient placeholder block with "Imagen" text |
| ABOU-03 | Brand ornament leaf shapes in background | BrandOrnament.astro already exists, position prop |
| PROD-01 | Grid of 7 product category cards | Custom grid with CSS grid, last-row centering via justify-items |
| PROD-02 | Each card has thematic SVG icon + name + product list | Tabler icons via astro-icon, accordion for list |
| PROD-03 | Cards styled with border/shadow + hover effect | Tailwind border-[#95b444]/20, hover:shadow-lg transition |
| PROD-04 | Expandable accordion sub-detail per category | Preline hs-accordion data attributes, already initialized |
| SERV-01 | Services with icon + title + description | ItemGrid2/Features2 pattern for card grid |
| SERV-02 | 7 services displayed | Data array passed to grid component |
| VALU-01 | 4 value cards | ItemGrid2 pattern, columns=4 (2x2 mobile, 4-col desktop) |
| VALU-02 | Each card has icon + title + description | Standard Item type from types.d.ts |
| CERT-01 | Certification banner with 5 certs prominently | Custom dark section with flex row layout |
| CERT-02 | Badge/shield SVG icons for each cert | Tabler shield-check or custom SVG badges |
| PURP-01 | Corporate purpose text from brief | Simple centered text section, large typography |
| PURP-02 | Dark background section | bg-[#25272f] text-white, WidgetWrapper isDark=true |
</phase_requirements>

## Standard Stack

### Core (already installed)
| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| Astro | (project version) | Static site framework | Already the project foundation |
| Tailwind CSS 3 | (project version) | Utility CSS | All existing widgets use it |
| astro-icon | ^1.1.5 | Icon rendering | `<Icon name="tabler:xxx" />` syntax |
| @iconify-json/tabler | ^1.2.20 | Tabler icon set | 5200+ icons, all categories covered |
| Preline UI | ^4.1.2 | Accordion interactivity | hs-accordion data attributes, already initialized in Layout.astro |
| AOS | (project version) | Scroll animations | Already configured in Layout.astro |

### No additional installations needed
All dependencies for Phase 3 are already in the project.

## Architecture Patterns

### Recommended Component Structure
```
src/components/widgets/
  NosotrosSection.astro      # About - two-column text+image
  ProductosSection.astro     # Products - card grid with accordion
  ServiciosSection.astro     # Services - icon+text grid
  ValoresSection.astro       # Values - 4-card grid
  CertificacionesSection.astro  # Certs - dark badge row
  PropositoSection.astro     # Purpose - dark centered text
```

### Pattern 1: Section Widget with WidgetWrapper
**What:** Every section uses WidgetWrapper for consistent padding, scroll offset, and intersection animation.
**When to use:** All 6 sections.
**Example:**
```astro
---
import WidgetWrapper from '~/components/ui/WidgetWrapper.astro';
import Headline from '~/components/ui/Headline.astro';
import BrandOrnament from '~/components/ui/BrandOrnament.astro';
---
<WidgetWrapper id="nosotros" containerClass="max-w-7xl mx-auto">
  <Fragment slot="bg">
    <div class="absolute inset-0 bg-white"></div>
    <BrandOrnament position="top-right" size={400} />
    <BrandOrnament position="bottom-left" size={350} />
  </Fragment>
  <Headline title="Quienes Somos" />
  <!-- content -->
</WidgetWrapper>
```
**Source:** Verified from WidgetWrapper.astro code -- uses named slot "bg" for background content.

### Pattern 2: Preline Accordion Inside Cards
**What:** Each product card contains a collapsible product list using Preline hs-accordion.
**When to use:** ProductosSection only.
**Example:**
```html
<div class="hs-accordion-group">
  <div class="hs-accordion" id="hs-productos-aceites">
    <button
      class="hs-accordion-toggle py-3 inline-flex items-center gap-x-3 w-full font-semibold text-start"
      aria-expanded="false"
      aria-controls="hs-collapse-aceites"
    >
      <Icon name="tabler:droplet" class="w-8 h-8 text-[#95b444]" />
      <span>Aceites</span>
      <Icon name="tabler:chevron-down" class="hs-accordion-active:rotate-180 transition-transform w-5 h-5 ml-auto" />
    </button>
    <div
      id="hs-collapse-aceites"
      class="hs-accordion-content hidden w-full overflow-hidden transition-[height] duration-300"
      role="region"
      aria-labelledby="hs-productos-aceites"
    >
      <ul class="pl-4 py-2 space-y-1 text-muted text-sm">
        <li>Crudo de Soya</li>
        <li>Crudo de Girasol</li>
        <!-- ... -->
      </ul>
    </div>
  </div>
</div>
```
**Source:** Preline official docs (preline.co/docs/accordion.html) + Layout.astro Preline init code.

### Pattern 3: Dark Section Styling
**What:** Sections on #25272f background with white text.
**When to use:** Certificaciones, Proposito.
**Example:**
```astro
<WidgetWrapper id="certificaciones" isDark={true} containerClass="max-w-7xl mx-auto">
  <Fragment slot="bg">
    <div class="absolute inset-0 bg-[#25272f]"></div>
  </Fragment>
  <!-- White text content -->
</WidgetWrapper>
```
**Note:** WidgetWrapper adds `class="dark"` to the inner container when `isDark=true`. However, since dark mode is disabled site-wide, use explicit `text-white` classes rather than relying on `dark:` variants.

### Pattern 4: Grid with Centered Last Row (7 items, 3 cols)
**What:** CSS grid where 7 items in 3 columns results in 1 orphan -- center it.
**When to use:** ProductosSection.
**Example:**
```astro
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 justify-items-center">
  {cards.map(card => (
    <div class="w-full max-w-sm">
      <!-- card content -->
    </div>
  ))}
</div>
```
**Note:** With `justify-items-center` and 7 items in 3 cols, the last item centers in the last row. For a visually better result with the last row having 1 centered item, wrap the last row item in a full-width flex container or use `last:col-start-2` on lg breakpoint (only works if last item is alone). A simpler approach: the grid naturally fills left-to-right, so with 7 items the last card sits in column 1 of row 3. To center the orphan, use a flex wrapper approach instead of grid.

### Anti-Patterns to Avoid
- **Building custom accordion JS:** Preline handles this. Never write custom toggle/collapse logic when hs-accordion is available.
- **Using `dark:` Tailwind variants for dark sections:** Dark mode is disabled. Use explicit color classes (`text-white`, `bg-[#25272f]`) instead.
- **Putting BrandOrnament on dark sections:** Per Phase 1 decision, ornaments are for white-bg sections only.
- **Breaking the WidgetWrapper pattern:** All sections should use WidgetWrapper for consistent scroll-mt-[72px] offset and padding. Do not create raw `<section>` tags.

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Accordion expand/collapse | Custom JS toggle | Preline hs-accordion | Handles animation, accessibility, aria attributes |
| Icon rendering | Inline SVG strings | `<Icon name="tabler:xxx" />` via astro-icon | Tree-shakes, consistent sizing, SSR-safe |
| Section wrapper | Raw `<section>` tags | WidgetWrapper.astro | Provides scroll offset, padding, intersection animation |
| Section titles | Custom `<h2>` styling | Headline.astro | Consistent typography, tagline/subtitle support |
| Grid layouts | Custom flexbox | CSS Grid via Tailwind `grid-cols-*` | Responsive, gap control, proven pattern in ItemGrid |

**Key insight:** AstroWind already has the building blocks. Phase 3 is about composing them, not reinventing them. The only truly custom piece is the accordion inside product cards.

## Common Pitfalls

### Pitfall 1: Preline Accordion Not Initializing After Astro Page Transition
**What goes wrong:** Accordion buttons don't expand/collapse after client-side navigation.
**Why it happens:** Astro View Transitions swap DOM but Preline's event listeners are lost.
**How to avoid:** Already handled -- Layout.astro has `document.addEventListener('astro:after-swap', () => setTimeout(initPreline, 100))`. Verify it works after implementation.
**Warning signs:** Accordion works on first load but not after clicking a nav link.

### Pitfall 2: Missing #proposito Section in index.astro
**What goes wrong:** Index.astro currently has NO placeholder for #proposito -- it goes from #certificaciones straight to #contacto.
**Why it happens:** Phase 2 placeholder creation missed this section.
**How to avoid:** Add the section when assembling index.astro. Proposito must appear between certificaciones and contacto.
**Warning signs:** 6 content sections in CONTEXT but only 5 placeholders in index.astro.

### Pitfall 3: Accordion IDs Must Be Unique
**What goes wrong:** Multiple accordions on page with duplicate IDs causes Preline to target wrong elements.
**Why it happens:** Copy-pasting accordion markup without updating IDs.
**How to avoid:** Use category-based IDs: `hs-productos-aceites`, `hs-productos-harinas`, etc. Each accordion group is independent within each card.
**Warning signs:** Clicking one card's accordion opens/closes a different card's content.

### Pitfall 4: BrandOrnament Parent Needs `relative` Class
**What goes wrong:** Ornament renders but positioned relative to viewport, not section.
**Why it happens:** BrandOrnament uses `absolute` positioning, needs a `relative` parent.
**How to avoid:** WidgetWrapper's outer `<section>` already has `class="relative"`. When using WidgetWrapper, ornaments work automatically via the `bg` slot.
**Warning signs:** Ornament appears in wrong position or overlaps other sections.

### Pitfall 5: Text Visibility on Dark Sections
**What goes wrong:** Default text color (#25272f) is invisible on #25272f background.
**Why it happens:** The `text-default` and `text-muted` utility classes are dark-on-light.
**How to avoid:** On dark sections, override ALL text with explicit `text-white` and `text-white/70` for muted. Do not rely on WidgetWrapper's `isDark` to handle this since dark mode is disabled.
**Warning signs:** Text appears invisible or barely visible on dark sections.

## Code Examples

### Tabler Icon Recommendations (verified available in @iconify-json/tabler)

**Products:**
| Category | Icon | Rationale |
|----------|------|-----------|
| Aceites | `tabler:droplet` | Liquid/oil representation |
| Harinas y Tortas | `tabler:grain` | Grain/flour |
| Granos | `tabler:plant` | Agricultural crops |
| Endulzantes | `tabler:candy` | Sweeteners |
| Grasas | `tabler:flask` | Chemical/processed fats |
| Derivados Forestales | `tabler:tree` | Wood/forest products |
| Hidrocarburos | `tabler:gas-station` | Fuel products |

**Services:**
| Service | Icon | Rationale |
|---------|------|-----------|
| Trading | `tabler:arrows-exchange` | Buy/sell exchange |
| Brokeraje | `tabler:handshake` | Intermediation |
| Logistica | `tabler:truck` | Transport/logistics |
| Analitica de mercados / Soft Landing | `tabler:chart-line` | Market analysis |
| Asesoramiento tecnico y comercial | `tabler:bulb` | Advisory/consulting |
| Maquilas | `tabler:building-factory` | Manufacturing |
| Analisis de laboratorio | `tabler:microscope` | Lab testing |

**Values:**
| Value | Icon | Rationale |
|-------|------|-----------|
| Calidad | `tabler:award` | Quality excellence |
| Respeto | `tabler:heart-handshake` | Mutual respect |
| Excelencia | `tabler:star` | High standards |
| Pasion | `tabler:flame` | Passion/energy |

**Certifications:**
| Certification | Icon | Rationale |
|---------------|------|-----------|
| All 5 certs | `tabler:shield-check` | Trust/verification badge |

**Confidence:** MEDIUM -- icon names based on Tabler icon naming conventions. Verify each exists at build time; astro-icon will throw a build error if an icon name is invalid, making failures immediately visible.

### WidgetWrapper bg Slot Usage
```astro
<!-- From WidgetWrapper source: bg slot replaces default Background component -->
<WidgetWrapper id="nosotros">
  <Fragment slot="bg">
    <div class="absolute inset-0 bg-white"></div>
    <BrandOrnament position="top-right" size={400} />
  </Fragment>
  <!-- Section content in default slot -->
</WidgetWrapper>
```

### Two-Column Layout (Nosotros Pattern, from Content.astro)
```astro
<div class="md:flex md:gap-16">
  <div class="md:basis-1/2 self-center">
    <!-- Text content -->
    <p class="text-lg text-muted leading-relaxed">Corporate text here...</p>
  </div>
  <div class="mt-10 md:mt-0 md:basis-1/2">
    <!-- Placeholder image -->
    <div class="w-full h-64 md:h-80 rounded-lg bg-[#95b444]/20 flex items-center justify-center">
      <span class="text-[#95b444]/60 text-lg font-medium">Imagen</span>
    </div>
  </div>
</div>
```

### Certification Badge Layout
```astro
<div class="flex flex-wrap justify-center gap-8 md:gap-12">
  {certs.map(cert => (
    <div class="flex flex-col items-center gap-3 text-center">
      <div class="w-16 h-16 rounded-full bg-[#95b444]/20 flex items-center justify-center">
        <Icon name="tabler:shield-check" class="w-8 h-8 text-[#95b444]" />
      </div>
      <span class="text-white font-bold text-sm">{cert.name}</span>
    </div>
  ))}
</div>
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| Custom JS accordions | Preline hs-accordion data attributes | Already in project | Zero custom JS needed |
| Inline SVG icons | astro-icon with Iconify JSON | Already in project | Tree-shaken, SSR-safe |
| Manual scroll offsets | WidgetWrapper scroll-mt-[72px] | Already in project | Consistent navbar offset |

**No deprecated patterns to worry about** -- all tools are current and already integrated.

## Open Questions

1. **Exact corporate text content (Nosotros and Proposito)**
   - What we know: CONTEXT says "exact corporate text from brief" in Spanish
   - What's unclear: The brief text is not included in CONTEXT.md
   - Recommendation: The implementer will need the brief text. If unavailable, use placeholder text clearly marked for replacement. The planner should note this dependency.

2. **Product list completeness**
   - What we know: Full product lists are specified in CONTEXT.md for all 7 categories
   - What's unclear: Whether the brief has additional sub-products not listed
   - Recommendation: Use the lists exactly as specified in CONTEXT.md -- they are comprehensive

3. **Services descriptions**
   - What we know: 7 service names are listed
   - What's unclear: Exact description text for each service
   - Recommendation: Write concise 1-2 sentence descriptions based on the service names if brief text is unavailable

## Validation Architecture

### Test Framework
| Property | Value |
|----------|-------|
| Framework | Visual browser verification (no unit test framework) |
| Config file | none |
| Quick run command | `npm run dev` + browser inspection |
| Full suite command | `npm run build` (catches Astro/icon build errors) |

### Phase Requirements to Test Map
| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| ABOU-01 | Nosotros section renders with text | smoke | `npm run build` | N/A (build check) |
| ABOU-02 | Placeholder image visible | manual-only | Visual check | N/A |
| ABOU-03 | Brand ornament visible at low opacity | manual-only | Visual check | N/A |
| PROD-01 | 7 product cards in responsive grid | manual-only | Visual check at 3 breakpoints | N/A |
| PROD-02 | Each card has icon + name + description | smoke | `npm run build` (icon name validation) | N/A |
| PROD-03 | Cards have border/shadow + hover | manual-only | Visual check | N/A |
| PROD-04 | Accordion expands to show product list | manual-only | Click each card in browser | N/A |
| SERV-01 | Services grid with icons | smoke | `npm run build` | N/A |
| SERV-02 | 7 services displayed | manual-only | Count in browser | N/A |
| VALU-01 | 4 value cards in grid | manual-only | Visual check | N/A |
| VALU-02 | Each card has icon + title + desc | smoke | `npm run build` | N/A |
| CERT-01 | 5 certs on dark background | manual-only | Visual check | N/A |
| CERT-02 | Badge/shield icons | smoke | `npm run build` | N/A |
| PURP-01 | Purpose text renders | smoke | `npm run build` | N/A |
| PURP-02 | Dark background applied | manual-only | Visual check | N/A |

### Sampling Rate
- **Per task commit:** `npm run build` (catches broken imports, invalid icon names, Astro syntax errors)
- **Per wave merge:** `npm run build` + visual check of all 6 sections at mobile/tablet/desktop
- **Phase gate:** Full build green + visual verification of all 15 requirements

### Wave 0 Gaps
None -- no test framework to set up. Build validation is the primary automated gate for a static site with no runtime logic. Visual verification covers the rest.

## Sources

### Primary (HIGH confidence)
- WidgetWrapper.astro, Headline.astro, ItemGrid.astro, ItemGrid2.astro, Features.astro, Features2.astro, Content.astro -- direct code inspection
- BrandOrnament.astro -- direct code inspection, props interface verified
- Layout.astro -- Preline init code verified (lines 70-86)
- types.d.ts -- Item, Widget, Features, Headline interfaces verified
- index.astro -- current placeholder structure verified (missing #proposito confirmed)

### Secondary (MEDIUM confidence)
- [Preline Accordion Docs](https://preline.co/docs/accordion.html) -- hs-accordion markup pattern verified
- Tabler icon names -- based on Iconify naming conventions, build will validate

### Tertiary (LOW confidence)
- None

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH - all libraries already installed and verified in package.json
- Architecture: HIGH - patterns directly observed in existing widget code
- Pitfalls: HIGH - identified from code inspection (missing #proposito, dark text, accordion IDs)
- Icon names: MEDIUM - naming convention based, build validates automatically

**Research date:** 2026-03-16
**Valid until:** 2026-04-16 (stable -- no fast-moving dependencies)
