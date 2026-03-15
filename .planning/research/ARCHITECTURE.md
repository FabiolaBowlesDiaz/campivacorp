# Architecture Patterns

**Domain:** Agroindustrial corporate landing page (Astro + AstroWind)
**Researched:** 2026-03-15
**Confidence:** HIGH (based on direct inspection of the actual codebase)

## Recommended Architecture

### Overview

AstroWind follows a strict **Layout > Page > Widget > UI primitive** hierarchy. The campivacorp. site is a single-page landing (`src/pages/index.astro`) that composes 11 section widgets inside a page layout. No routing beyond the index page is needed.

```
Layout.astro (HTML shell, meta, scripts: AOS + Preline + Swiper CSS)
  |
  PageLayout.astro (adds Header + main + Footer from navigation.ts)
    |
    index.astro (composes all 11 sections as widget calls with props/slots)
      |
      +-- HeroSwiper (CUSTOM - replaces Hero.astro)
      +-- Stats (EXISTING widget, needs counter animation)
      +-- QuienesSomos (CUSTOM - Content-style with ornaments)
      +-- Productos (CUSTOM - 7-card grid with SVG icons)
      +-- Servicios (CUSTOM or Features variant)
      +-- Valores (CUSTOM - 4 value cards)
      +-- Certificaciones (EXISTING Brands widget, adapted)
      +-- Proposito (CUSTOM - Content-style section)
      +-- Contacto (EXISTING Contact + custom info panel)
      |
      Each widget uses:
        WidgetWrapper.astro (section wrapper, bg, dark mode, scroll-mt)
        Headline.astro (title/subtitle/tagline pattern)
        UI primitives (Button, ItemGrid, Image, Icon, Form)
```

### Component Boundaries

| Component | Responsibility | Location | Communicates With |
|-----------|---------------|----------|-------------------|
| **Layout.astro** | HTML document, `<head>`, global CSS imports (tailwind, AOS, Swiper), AOS init script, Preline init, View Transitions | `src/layouts/Layout.astro` | Receives `metadata` prop from pages |
| **PageLayout.astro** | Wraps pages with Header + `<main>` + Footer; imports navigation data | `src/layouts/PageLayout.astro` | Imports `headerData`/`footerData` from `navigation.ts` |
| **CustomStyles.astro** | CSS custom properties (colors, fonts) -- **the brand theming hub** | `src/components/CustomStyles.astro` | Read by all components via `var(--aw-color-*)` and `var(--aw-font-*)` |
| **navigation.ts** | Navigation links, CTA actions, footer links, social links -- **single data source for nav/footer** | `src/navigation.ts` | Consumed by PageLayout, Header, Footer |
| **config.yaml** | Site-wide config: name, SEO metadata, i18n, blog settings, analytics, theme | `src/config.yaml` | Consumed by vendor integration, metadata components |
| **Header.astro** | Fixed navbar with logo, nav links, mobile toggle, CTA button | `src/components/widgets/Header.astro` | Reads `links` and `actions` from props (via navigation.ts) |
| **Footer.astro** | Footer with brand, link columns, social icons, copyright | `src/components/widgets/Footer.astro` | Reads `links`, `socialLinks`, `footNote` from props |
| **WidgetWrapper.astro** | Universal section wrapper: handles `id`, `isDark`, background slot, responsive padding, intersection animation | `src/components/ui/WidgetWrapper.astro` | Used by ALL widget components |
| **Headline.astro** | Section title/subtitle/tagline rendering | `src/components/ui/Headline.astro` | Used by most widgets |
| **index.astro** | **The page itself** -- composes all 11 sections, passes all content as props/slots | `src/pages/index.astro` | Imports and calls each widget component |

### Data Flow

```
1. STATIC DATA (no API, no CMS)
   - All content lives as props/slots in index.astro
   - Navigation data in navigation.ts
   - Site config in config.yaml
   - Brand variables in CustomStyles.astro

2. PROP FLOW (top-down, no state management)
   index.astro
     -> passes title, subtitle, items[], image, id, isDark, bg, actions
        -> Widget receives props, destructures with defaults
           -> Widget calls WidgetWrapper(id, isDark, bg)
           -> Widget calls Headline(title, subtitle, tagline)
           -> Widget renders content-specific markup

3. STYLING FLOW
   CustomStyles.astro defines CSS vars (--aw-color-primary, --aw-font-heading, etc.)
     -> tailwind.config.js maps vars to Tailwind classes (text-primary, font-heading, etc.)
       -> Components use Tailwind classes
       -> tailwind.css defines .btn, .btn-primary component classes
       -> DaisyUI available with daisy- prefix for interactive elements

4. ANIMATION FLOW
   Layout.astro initializes AOS globally (duration: 800, once: true)
     -> Components use data-aos="fade-up" attributes
     -> WidgetWrapper has built-in intersection-based fade animation
     -> Swiper JS initialized per-component (not global)

5. ANCHOR NAVIGATION (single-page)
   navigation.ts defines links as #section-id anchors
     -> Header renders anchor links
     -> WidgetWrapper applies scroll-mt-[72px] for fixed header offset
     -> Each section gets unique id prop
```

## Component Plan: What to Reuse vs. Build Custom

### Reuse As-Is (minor prop changes only)

| Existing Widget | Maps To Section | Adaptation Needed |
|----------------|-----------------|-------------------|
| `Header.astro` | Navbar | Replace Logo.astro content with SVG isotipo + wordmark. Update navigation.ts with anchor links and Spanish labels. Set `isSticky=true`. |
| `Footer.astro` | Footer | Update navigation.ts `footerData` with campivacorp. links, social icons (FB, WhatsApp, IG, LinkedIn), copyright text. |
| `Brands.astro` | Certificaciones | Pass certification logos/icons as `images[]` or `icons[]`. Add title "Certificaciones". |
| `Contact.astro` | Contacto (form part) | Configure `inputs`, `textarea`, `button` props. Will need a companion info panel alongside. |

### Reuse with Significant Modification

| Existing Widget | Maps To Section | What Changes |
|----------------|-----------------|--------------|
| `Stats.astro` | Stats counters | Add animated counter JS (AOS triggers count-up). The existing widget renders static text -- need `data-count` attribute + JS counter script. |
| `Features.astro` / `Features2.astro` | Servicios | Can use the grid + icon pattern. Needs custom styling to match brand (green icons, card borders). |

### Build Custom (no suitable existing widget)

| New Component | Maps To Section | Reason |
|--------------|-----------------|--------|
| `HeroSwiper.astro` | Hero | Existing Hero.astro is single-image centered text. Need Swiper slider with 3 slides, overlay gradient, dual CTAs per slide. Completely different structure. |
| `QuienesSomos.astro` | Quienes Somos | Text + photo layout with brand ornament background decorations. Content.astro is close but ornaments need custom implementation. |
| `Productos.astro` | Products grid | 7 category cards with custom SVG icons, hover effects, brand-colored borders. No existing widget matches this card grid pattern. |
| `Valores.astro` | Values | 4 value cards with icons. Could extend Features2 but the visual design (brand ornaments, specific layout) warrants custom. |
| `Proposito.astro` | Proposito Corporativo | Vision/mission text section with distinctive styling. Simple enough to be custom. |
| `ContactoInfo.astro` | Contacto (info panel) | Email, phone, social links panel alongside the form. Contact.astro only has the form. |

## Patterns to Follow

### Pattern 1: Widget Component Structure

Every section widget follows the same skeleton. This is the AstroWind convention and must be maintained.

```astro
---
import WidgetWrapper from '~/components/ui/WidgetWrapper.astro';
import Headline from '~/components/ui/Headline.astro';
// other imports

// Props with type + defaults
const {
  title = await Astro.slots.render('title'),
  subtitle = await Astro.slots.render('subtitle'),
  tagline,
  items = [],
  id,
  isDark = false,
  classes = {},
  bg = await Astro.slots.render('bg'),
} = Astro.props;
---

<WidgetWrapper id={id} isDark={isDark} containerClass={`max-w-6xl mx-auto ${classes?.container ?? ''}`} bg={bg}>
  <Headline title={title} subtitle={subtitle} tagline={tagline} />
  <!-- Section-specific content here -->
</WidgetWrapper>
```

**Why this matters:** WidgetWrapper provides consistent padding, scroll offset for fixed header (`scroll-mt-[72px]`), background slot, dark mode support, and intersection animation. Breaking this pattern means losing all of those features.

### Pattern 2: Section ID + Anchor Navigation

Each section receives an `id` prop in index.astro that matches the navigation anchors.

```astro
<!-- in index.astro -->
<QuienesSomos id="quienes-somos" ... />
<Productos id="productos" ... />
<Servicios id="servicios" ... />
<Contacto id="contacto" ... />
```

```typescript
// in navigation.ts
export const headerData = {
  links: [
    { text: 'Quienes Somos', href: '#quienes-somos' },
    { text: 'Productos', href: '#productos' },
    { text: 'Servicios', href: '#servicios' },
    { text: 'Contacto', href: '#contacto' },
  ],
  actions: [{ text: 'Contactar', href: '#contacto', variant: 'primary' }],
};
```

### Pattern 3: Brand Theming via CSS Custom Properties

All brand colors flow through `CustomStyles.astro` -> CSS variables -> Tailwind config. Never hardcode colors in components.

```astro
<!-- CustomStyles.astro - MUST be updated for campivacorp. brand -->
<style is:inline>
  :root {
    --aw-font-sans: 'Montserrat';
    --aw-font-heading: 'Nunito Sans';

    --aw-color-primary: rgb(149 180 68);       /* #95b444 */
    --aw-color-secondary: rgb(93 111 49);       /* #5d6f31 */
    --aw-color-accent: rgb(203 220 83);         /* #cbdc53 */

    --aw-color-text-heading: rgb(37 39 47);     /* #25272f */
    --aw-color-text-default: rgb(37 39 47);     /* #25272f */
    --aw-color-text-muted: rgb(37 39 47 / 66%);
    --aw-color-bg-page: rgb(255 255 255);
    --aw-color-bg-page-dark: rgb(37 39 47);     /* #25272f */
  }
</style>
```

Then components use Tailwind classes like `text-primary`, `bg-primary`, `font-heading` -- no color hex values in component files.

### Pattern 4: Alternating Section Backgrounds

Corporate landing pages alternate between white and dark (#25272f) section backgrounds for visual rhythm. Use the `isDark` prop and `bg` slot.

```astro
<!-- in index.astro -->
<Stats id="stats" isDark={true}>
  <Fragment slot="bg">
    <div class="absolute inset-0 bg-[#25272f]"></div>
  </Fragment>
</Stats>

<QuienesSomos id="quienes-somos" isDark={false} />

<Productos id="productos" isDark={true}>
  <Fragment slot="bg">
    <div class="absolute inset-0 bg-[#25272f]"></div>
  </Fragment>
</Productos>
```

### Pattern 5: Swiper Initialization Per-Component

Swiper CSS is imported globally in Layout.astro. JS must be initialized in each component that uses it, not globally.

```astro
<!-- in HeroSwiper.astro -->
<div class="swiper hero-swiper">
  <div class="swiper-wrapper">
    <div class="swiper-slide">...</div>
  </div>
  <div class="swiper-pagination"></div>
</div>

<script>
  import Swiper from 'swiper';
  import { Autoplay, Pagination, EffectFade } from 'swiper/modules';

  function initHeroSwiper() {
    new Swiper('.hero-swiper', {
      modules: [Autoplay, Pagination, EffectFade],
      loop: true,
      autoplay: { delay: 5000, disableOnInteraction: false },
      pagination: { el: '.swiper-pagination', clickable: true },
      effect: 'fade',
    });
  }

  initHeroSwiper();
  document.addEventListener('astro:after-swap', initHeroSwiper);
</script>
```

## Anti-Patterns to Avoid

### Anti-Pattern 1: Hardcoded Colors in Components
**What:** Writing `bg-[#95b444]` directly in component markup.
**Why bad:** Breaks theming consistency. If brand changes, must find/replace across all files.
**Instead:** Use CSS variables in CustomStyles.astro, reference via Tailwind classes (`bg-primary`, `text-secondary`).

### Anti-Pattern 2: Skipping WidgetWrapper
**What:** Writing raw `<section>` tags instead of using WidgetWrapper.
**Why bad:** Loses scroll offset for fixed header, loses consistent padding, loses intersection animations, loses dark mode background handling.
**Instead:** Always wrap sections in WidgetWrapper. Use the `containerClass` prop for custom max-widths.

### Anti-Pattern 3: Global Swiper Initialization
**What:** Initializing all Swiper instances in Layout.astro.
**Why bad:** Swiper instances need specific selectors and config. Global init runs before component DOM may be ready, especially with View Transitions.
**Instead:** Initialize Swiper inside each component's `<script>` tag with `astro:after-swap` listener.

### Anti-Pattern 4: Importing Fonts Multiple Times
**What:** Adding Google Fonts via `<link>` in Layout.astro AND via @fontsource.
**Why bad:** Double loading, flash of unstyled text, performance hit.
**Instead:** Use Google Fonts `<link>` in CustomStyles.astro (Nunito Sans 800 + Montserrat 500,700) and remove the `@fontsource-variable/inter` import. One font loading strategy only.

### Anti-Pattern 5: Creating Separate Pages for a Single-Page Site
**What:** Creating `src/pages/productos.astro`, `src/pages/servicios.astro`, etc.
**Why bad:** This is a single-page landing. All sections compose in index.astro.
**Instead:** All sections are components called from index.astro. Navigation uses anchor links (#productos, #servicios).

## File Organization

### Recommended Directory Structure

```
src/
  assets/
    images/
      hero/              # Hero slider images (3 slides)
      productos/         # Product category images/icons
      certificaciones/   # Certification badge images
      ornaments/         # Brand ornament SVGs (leaf shapes)
    styles/
      tailwind.css       # EXISTING - Tailwind directives + custom classes
  components/
    CustomStyles.astro   # MODIFY - campivacorp. brand colors + fonts
    Logo.astro           # MODIFY - SVG isotipo + wordmark
    Favicons.astro       # MODIFY - campivacorp. favicons
    common/              # EXISTING - keep as-is
    ui/                  # EXISTING - keep as-is (WidgetWrapper, Headline, Button, etc.)
    widgets/
      Header.astro       # EXISTING - works with navigation.ts changes
      Footer.astro       # EXISTING - works with navigation.ts changes
      Stats.astro        # MODIFY - add counter animation
      Brands.astro       # REUSE - for certifications
      Contact.astro      # REUSE - for contact form
      HeroSwiper.astro   # NEW - Swiper-based hero with 3 slides
      QuienesSomos.astro # NEW - corporate about section
      Productos.astro    # NEW - 7-category product grid
      Servicios.astro    # NEW - services with icons
      Valores.astro      # NEW - 4 value cards
      Proposito.astro    # NEW - corporate purpose/vision
      ContactoInfo.astro # NEW - contact info panel (email, phone, social)
  layouts/
    Layout.astro         # EXISTING - keep (already has AOS + Swiper CSS)
    PageLayout.astro     # EXISTING - keep
  pages/
    index.astro          # REWRITE - compose all 11 campivacorp. sections
  navigation.ts          # REWRITE - campivacorp. anchor links + footer data
  config.yaml            # MODIFY - campivacorp. site name, SEO, i18n: es
```

## Suggested Build Order (Dependencies)

Build order is constrained by what each component depends on. Components earlier in the list are prerequisites for later ones.

### Phase 1: Foundation (no visible sections yet, but everything depends on this)

1. **CustomStyles.astro** -- brand colors and fonts. Every component reads these CSS variables.
2. **config.yaml** -- site name "campivacorp.", language `es`, disable blog, SEO metadata.
3. **navigation.ts** -- anchor links for all 11 sections, footer data, social links.
4. **Logo.astro** -- SVG isotipo + wordmark. Used by Header and Footer.
5. **Google Fonts setup** -- Remove Inter, add Nunito Sans 800 + Montserrat 500/700.

**Dependency rationale:** CustomStyles must come first because every component inherits colors/fonts from CSS variables. Config and navigation are consumed by the layout shell. Without these, no component renders correctly.

### Phase 2: Shell (Header + Footer + Page Structure)

6. **Header.astro** -- minor tweaks: `isSticky=true`, remove theme toggle, remove RSS. Depends on Logo + navigation.ts.
7. **Footer.astro** -- update via footerData in navigation.ts. Depends on Logo + navigation.ts.
8. **index.astro** -- skeleton with all section slots, even if sections are placeholder divs initially.

**Dependency rationale:** The shell establishes the page frame. All section work happens inside `<main>` between Header and Footer.

### Phase 3: High-Impact Sections (above the fold)

9. **HeroSwiper.astro** -- the first thing visitors see. Swiper slider with 3 slides, overlay, dual CTAs. Independent of other sections.
10. **Stats.astro** -- animated counters (25+ years, 7 categories, etc.). Sits directly below hero. Needs counter JS addition.

**Dependency rationale:** Hero and Stats are above-the-fold -- they define first impression. Hero is the most complex custom component (Swiper integration).

### Phase 4: Content Sections (scrollable body)

11. **QuienesSomos.astro** -- text + photo + ornaments. Independent.
12. **Productos.astro** -- 7 category cards with SVG icons. Independent.
13. **Servicios.astro** -- icon + description grid. Can reuse Features pattern. Independent.
14. **Valores.astro** -- 4 value cards. Independent.
15. **Certificaciones** -- Brands widget with certification images. Independent.
16. **Proposito.astro** -- vision/purpose text section. Independent.

**Dependency rationale:** These 6 sections are independent of each other. They can be built in any order or in parallel. Listed in page-scroll order for logical progress.

### Phase 5: Conversion Section

17. **ContactoInfo.astro** -- info panel (email, phone, social links, optional map placeholder).
18. **Contact section in index.astro** -- combines Contact.astro (form) + ContactoInfo.astro (info) side by side.

**Dependency rationale:** Contact is the conversion endpoint. All CTA buttons point to #contacto. Build last so the destination exists when testing navigation.

### Phase 6: Polish

19. **AOS data attributes** -- add `data-aos="fade-up"` etc. to all custom components.
20. **Responsive testing** -- mobile, tablet, desktop.
21. **Brand ornament backgrounds** -- leaf shapes at 5-10% opacity in strategic sections.
22. **Performance audit** -- image optimization, unused CSS/JS cleanup, Lighthouse check.

## Scalability Considerations

| Concern | Current (single-page) | If Multi-Language Added | If Product Pages Added |
|---------|----------------------|------------------------|----------------------|
| Content management | Props in index.astro | Extract to JSON/YAML data files per locale, import conditionally | Move to Astro content collections |
| Navigation | Anchor links in navigation.ts | Add locale prefix routing | Add product detail page routes |
| SEO | Single page metadata | hreflang tags, separate pages per locale | Dynamic OG tags per product |
| Build time | Sub-second | Still fast (static) | Still fast unless hundreds of pages |

For the current single-page scope, keeping all content as inline props in index.astro is the correct approach. Extracting to data files adds indirection without benefit when there is one page in one language.

## Sources

- Direct inspection of the AstroWind template codebase in the project directory (HIGH confidence)
- AstroWind template conventions observed across all existing widgets (HIGH confidence)
- Astro 5.x component model: layouts > pages > components with props and slots (HIGH confidence)
