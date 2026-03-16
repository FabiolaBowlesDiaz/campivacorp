# Phase 2: Page Shell + Above the Fold - Research

**Researched:** 2026-03-16
**Domain:** Swiper slider integration in Astro, navigation rewrite, animated stat counters, single-page architecture
**Confidence:** HIGH

## Summary

Phase 2 transforms the AstroWind demo index page into the campivacorp. single-page landing. The work has three distinct areas: (1) rewriting navigation.ts and updating PageLayout to deliver a flat anchor-link navbar with a branded CTA, (2) creating a new HeroSlider.astro component using Swiper 12.1.2 (already installed) with fade effect, autoplay, pagination, and navigation, and (3) building an animated stats counter section with Intersection Observer.

The codebase is well-prepared. Phase 1 established brand tokens (CSS variables, fonts, logo PNG). Swiper CSS (core, navigation, pagination) is already imported in Layout.astro. The sticky header scroll behavior is already implemented in BasicScripts.astro (adds `.scroll` class after 60px) with corresponding styles in tailwind.css. Smooth scrolling is enabled. The existing WidgetWrapper has `scroll-mt-[72px]` for anchor offset.

**Primary recommendation:** Start with navigation.ts rewrite + PageLayout header props, then build the HeroSlider component with Swiper JS imported in a `<script>` tag (not frontmatter), then the StatsCounter with Intersection Observer, and finally rewrite index.astro to compose them all as a single-page layout.

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions
- Navbar: Rewrite navigation.ts completely -- flat anchor links only (#inicio, #nosotros, #productos, #servicios, #valores, #certificaciones, #contacto). No dropdowns.
- Navbar CTA: "Contactanos" linking to #contacto, styled with bg-[#95b444] text-white hover:bg-[#5d6f31]
- Logo: uses existing Logo.astro (PNG from brand book) -- already done in Phase 1
- Header.astro: set isSticky=true, remove showToggleTheme, remove showRssFeed
- Navbar bg: transparent on top, solid white with subtle shadow on scroll (AstroWind's existing scroll behavior)
- Mobile: AstroWind's ToggleMenu component handles hamburger -- keep it, just update the links
- Hero: Swiper component with 3 full-height slides (100vh or min-h-[600px])
- Each slide: background image with #25272f overlay at 60% opacity
- Background images: use high-quality placeholder images or solid gradient fallbacks in src/assets/images/hero/
- Slide text: large heading (Nunito Sans 800, white), subheading (Montserrat, white/muted) -- exact slide texts specified
- Dual CTAs: "Ver Productos" -> #productos (bg-[#95b444]), "Contactar" -> #contacto (border-white, transparent bg)
- Swiper config: autoplay 5s, fade or slide effect, pagination dots (white), navigation arrows on desktop only
- Import Swiper JS modules individually (Navigation, Pagination, Autoplay, EffectFade) in component `<script>` tag (NOT frontmatter)
- Stats: dark background section (#25272f) immediately after hero
- 4 stat counters: "25+" (Anos experiencia), "7" (Categorias productos), "5" (Certificaciones internacionales), "100%" (Compromiso calidad)
- Numbers animate counting up from 0 when section enters viewport
- Use Intersection Observer for trigger (NOT AOS -- custom counter animation)
- Stats typography: numbers in Nunito Sans 800 3-4rem white, labels in Montserrat 500 smaller white/muted
- Index page: rewrite as single-page with section anchors, remove all AstroWind demo content
- Structure: Hero -> Stats -> (placeholder slots for Phase 3) -> (placeholder for Phase 4)
- AOS NOT used inside Swiper (conflict from Phase 1 research)

### Claude's Discretion
- Exact Swiper effect choice (fade vs slide)
- Hero image placeholders (can use solid color gradients if images unavailable)
- Stats animation timing and easing
- Exact responsive breakpoints for stats grid
- Whether to remove AstroWind demo pages (about, pricing, services, etc.) now or defer
- Navigation scroll offset (accounting for fixed header height)

### Deferred Ideas (OUT OF SCOPE)
- Remove AstroWind demo pages (about.astro, pricing.astro, services.astro, homes/, landing/) -- can be done in Phase 2 or deferred to cleanup
- Footer data in navigation.ts -- Phase 4 handles footer
</user_constraints>

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|-----------------|
| NAV-01 | Fixed navbar with isotipo SVG + "campiva" bold + "corp." regular wordmark | Logo.astro already exists with PNG. Header.astro already imports Logo. Set isSticky=true in PageLayout. |
| NAV-02 | Navbar links scroll to page sections (anchor navigation with smooth scroll) | Rewrite navigation.ts headerData.links as flat array of `{text, href: '#section'}`. Smooth scroll already enabled by BasicScripts.astro (`motion-safe:scroll-smooth`). WidgetWrapper provides `scroll-mt-[72px]` for offset. |
| NAV-03 | CTA button "Contactanos" in #95b444 with hover #5d6f31 | Update headerData.actions to `[{text: 'Contactanos', href: '#contacto'}]`. btn-primary class already maps to brand colors via CSS variables. |
| NAV-04 | Mobile responsive hamburger menu with section links | ToggleMenu.astro already works. BasicScripts.astro handles expand/collapse. Only data in navigation.ts needs changing. |
| NAV-05 | Navbar background changes on scroll (transparent to solid) | Already implemented: BasicScripts.astro adds `.scroll` class at 60px. tailwind.css has `#header.scroll > div:first-child` with bg-white/90 + backdrop-blur + shadow. |
| HERO-01 | Swiper slider with 3 slides, overlay #25272f at 60% opacity | New HeroSlider.astro component. Swiper 12.1.2 installed. CSS imported in Layout.astro (core, navigation, pagination). Need to add effect-fade CSS if using fade. |
| HERO-02 | Slide 1 -- "Soluciones agroindustriales para el mundo" (agricultural fields) | Static content in HeroSlider.astro. Placeholder image or gradient bg. |
| HERO-03 | Slide 2 -- "Calidad certificada en cada transaccion" (products) | Static content in HeroSlider.astro. Placeholder image or gradient bg. |
| HERO-04 | Slide 3 -- "25 anos conectando mercados" (global commerce) | Static content in HeroSlider.astro. Placeholder image or gradient bg. |
| HERO-05 | Dual CTAs on each slide: "Ver Productos" + "Contactar" | Anchor links within each slide div. btn-primary for Ver Productos, outline/ghost for Contactar. |
| HERO-06 | Autoplay with pagination dots, navigation arrows on desktop | Swiper modules: Autoplay (delay 5000), Pagination (clickable), Navigation. Arrows hidden on mobile via CSS. |
| STAT-01 | Animated counter section with scroll trigger | Custom StatsCounter.astro component. Intersection Observer triggers count-up animation. NOT AOS. |
| STAT-02 | 4 stats displayed: 25+ anos, 7 categorias, 5 certificaciones, 100% compromiso | Static data array in component. Dark bg (#25272f), 4-col desktop / 2x2 mobile grid. |
| STAT-03 | Counter animation counts up from 0 on viewport entry | `<script>` tag with IntersectionObserver + requestAnimationFrame counter. Duration ~2s, easeOut. |
</phase_requirements>

## Standard Stack

### Core (Already Installed -- No New Dependencies)
| Library | Version | Purpose | Status |
|---------|---------|---------|--------|
| swiper | 12.1.2 | Hero slider (fade, autoplay, pagination, navigation) | Installed, CSS partially imported |
| astro | 5.12.9 | Static site framework | Installed |
| tailwindcss | 3.4.17 | Utility CSS | Installed |
| aos | 2.3.4 | Scroll animations (for other sections, NOT inside Swiper) | Installed |
| preline | 4.1.2 | Mobile menu toggle (via ToggleMenu) | Installed |

### No New Installs Required

All dependencies are already present. Swiper 12.1.2 was added in Phase 1 along with CSS imports in Layout.astro.

**One CSS addition needed:** If using fade effect, add `import 'swiper/css/effect-fade';` to Layout.astro frontmatter (alongside existing swiper CSS imports).

## Architecture Patterns

### Files to Create
```
src/
  components/
    widgets/
      HeroSlider.astro     # NEW: Swiper-based hero with 3 slides
      StatsCounter.astro    # NEW: Animated counter section with IntersectionObserver
  assets/
    images/
      hero/                 # NEW: directory for hero placeholder images (or gradient fallbacks)
```

### Files to Modify
```
src/
  navigation.ts             # REWRITE: flat anchor links, new CTA action
  pages/index.astro         # REWRITE: single-page layout with section anchors
  layouts/
    PageLayout.astro        # MODIFY: remove showRssFeed, showToggleTheme, remove Announcement
    Layout.astro            # MODIFY: add swiper/css/effect-fade import (if using fade)
```

### Pattern 1: Swiper Initialization in Astro `<script>` Tag
**What:** Swiper JS must be imported and initialized inside a `<script>` tag in the component, NOT in the Astro frontmatter. Frontmatter runs at build time (server-side); Swiper requires DOM access.
**When to use:** Always for client-side libraries in Astro.
**Critical detail:** Astro `<script>` tags are bundled and deduplicated. They run once per page load. With View Transitions (ClientRouter active in Layout.astro), re-initialization is needed on `astro:after-swap`.

```astro
<!-- HeroSlider.astro -->
---
// NO Swiper imports here -- this is server-side
---

<div class="swiper hero-swiper">
  <div class="swiper-wrapper">
    <div class="swiper-slide">...</div>
    <div class="swiper-slide">...</div>
    <div class="swiper-slide">...</div>
  </div>
  <div class="swiper-pagination"></div>
  <div class="swiper-button-prev"></div>
  <div class="swiper-button-next"></div>
</div>

<script>
  import Swiper from 'swiper';
  import { Navigation, Pagination, Autoplay, EffectFade } from 'swiper/modules';

  function initHeroSwiper() {
    const el = document.querySelector('.hero-swiper');
    if (!el) return;

    new Swiper('.hero-swiper', {
      modules: [Navigation, Pagination, Autoplay, EffectFade],
      effect: 'fade',
      fadeEffect: { crossFade: true },
      autoplay: { delay: 5000, disableOnInteraction: false },
      pagination: { el: '.swiper-pagination', clickable: true },
      navigation: { nextEl: '.swiper-button-next', prevEl: '.swiper-button-prev' },
      loop: true,
    });
  }

  initHeroSwiper();
  document.addEventListener('astro:after-swap', initHeroSwiper);
</script>
```

### Pattern 2: Intersection Observer Counter Animation
**What:** Custom counter animation using IntersectionObserver + requestAnimationFrame. Does NOT use AOS.
**When to use:** For stat counters that count up from 0.
**Key details:** Use `threshold: 0.3` so animation starts when 30% of the section is visible. Use easeOutQuart for natural deceleration. Duration ~2000ms.

```astro
<!-- StatsCounter.astro -->
<script>
  function animateCounters() {
    const section = document.getElementById('stats');
    if (!section) return;

    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          const counters = section.querySelectorAll('[data-target]');
          counters.forEach(counter => {
            const target = parseFloat(counter.getAttribute('data-target') || '0');
            const suffix = counter.getAttribute('data-suffix') || '';
            const duration = 2000;
            const startTime = performance.now();

            function update(currentTime) {
              const elapsed = currentTime - startTime;
              const progress = Math.min(elapsed / duration, 1);
              // easeOutQuart
              const eased = 1 - Math.pow(1 - progress, 4);
              const current = Math.floor(eased * target);
              counter.textContent = current + suffix;
              if (progress < 1) requestAnimationFrame(update);
            }
            requestAnimationFrame(update);
          });
          observer.unobserve(entry.target);
        }
      });
    }, { threshold: 0.3 });

    observer.observe(section);
  }

  animateCounters();
  document.addEventListener('astro:after-swap', animateCounters);
</script>
```

### Pattern 3: Navigation.ts Data Structure (Flat Anchors)
**What:** Complete rewrite of headerData -- no nested `links` arrays (dropdowns), just flat top-level items.
**Current structure:** Each link has a `links[]` sub-array creating dropdowns.
**New structure:** Each link has only `text` and `href` (anchor).

```typescript
export const headerData = {
  links: [
    { text: 'Inicio', href: '#inicio' },
    { text: 'Nosotros', href: '#nosotros' },
    { text: 'Productos', href: '#productos' },
    { text: 'Servicios', href: '#servicios' },
    { text: 'Valores', href: '#valores' },
    { text: 'Certificaciones', href: '#certificaciones' },
    { text: 'Contacto', href: '#contacto' },
  ],
  actions: [
    {
      text: 'Contactanos',
      href: '#contacto',
    },
  ],
};
```

### Pattern 4: PageLayout Header Props Update
**What:** Remove showToggleTheme, showRssFeed, and Announcement from PageLayout.
**Current (line 23):** `<Header {...headerData} isSticky showRssFeed showToggleTheme />`
**New:** `<Header {...headerData} isSticky />`
**Also:** Remove or empty the Announcement slot (it shows AstroWind promo banner).

### Pattern 5: Index.astro Single-Page Structure
**What:** Replace all AstroWind demo widgets with campivacorp. sections using id anchors.

```astro
---
import Layout from '~/layouts/PageLayout.astro';
import HeroSlider from '~/components/widgets/HeroSlider.astro';
import StatsCounter from '~/components/widgets/StatsCounter.astro';

const metadata = {
  title: 'campivacorp. - Soluciones agroindustriales para el mundo',
  ignoreTitleTemplate: true,
};
---

<Layout metadata={metadata}>
  <section id="inicio">
    <HeroSlider />
  </section>

  <StatsCounter id="stats" />

  <!-- Phase 3 placeholder sections -->
  <section id="nosotros" class="scroll-mt-[72px]">
    <!-- Quienes Somos - Phase 3 -->
  </section>

  <section id="productos" class="scroll-mt-[72px]">
    <!-- Products Grid - Phase 3 -->
  </section>

  <!-- ...more placeholders... -->
</Layout>
```

### Anti-Patterns to Avoid
- **Do NOT import Swiper in Astro frontmatter.** Frontmatter is server-side. Swiper needs the DOM. Use `<script>` tag.
- **Do NOT use AOS inside the Swiper container.** AOS and Swiper both manipulate transforms, causing visual conflicts.
- **Do NOT use `getPermalink()` for anchor links.** That function is for page routes. Use plain `'#section'` strings directly.
- **Do NOT forget `astro:after-swap` re-initialization.** ClientRouter (View Transitions) is active. Without re-init, Swiper and counters break on page transitions.
- **Do NOT nest hero inside WidgetWrapper.** The hero needs full-viewport height (100vh) without WidgetWrapper's default padding/max-width.

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Carousel/slider | Custom JS slider | Swiper 12.1.2 (already installed) | Touch support, accessibility, fade effects, autoplay pause on interaction |
| Smooth scroll | Custom scrollIntoView logic | AstroWind's existing `motion-safe:scroll-smooth` on html | Already works, handles reduced-motion preference |
| Sticky header scroll detection | Custom scroll listener | AstroWind's BasicScripts.astro `.scroll` class toggle | Already implemented at 60px threshold with rAF |
| Mobile hamburger menu | Custom toggle | AstroWind's ToggleMenu + BasicScripts.astro handlers | Full expand/collapse with body overflow, resize cleanup |
| Section scroll offset | Manual padding calculation | WidgetWrapper's `scroll-mt-[72px]` | Accounts for sticky header height |

## Common Pitfalls

### Pitfall 1: Swiper Not Initializing After View Transitions
**What goes wrong:** Swiper works on initial page load but breaks when navigating away and back (View Transitions swap the DOM).
**Why it happens:** Astro's ClientRouter (`<ClientRouter fallback="swap" />` in Layout.astro) replaces DOM nodes. The old Swiper instance and its event listeners are destroyed.
**How to avoid:** Add `document.addEventListener('astro:after-swap', initHeroSwiper)` in the `<script>` tag. Also destroy the old Swiper instance before creating a new one to prevent memory leaks.
**Warning signs:** Slider shows first slide only, no autoplay, no interaction after navigation.

### Pitfall 2: Hero Height With Sticky Header
**What goes wrong:** Setting `height: 100vh` on the hero causes the bottom of the first slide to be hidden behind the viewport fold, because the sticky header overlaps the top.
**Why it happens:** 100vh includes the area behind the fixed header.
**How to avoid:** Use `min-h-screen` (100vh) but ensure the text content is centered vertically with enough top-padding. Or use `h-[calc(100vh-72px)]` where 72px is the header height. The 100vh approach with centered content is simpler and visually acceptable since the overlay covers the full viewport.
**Recommendation:** Use `min-h-screen` for the hero. Content is centered, so the header overlap is fine -- users see a full-bleed hero behind a transparent navbar.

### Pitfall 3: Swiper Fade Effect Requires crossFade
**What goes wrong:** With `effect: 'fade'`, slides stack on top of each other and the previous slide remains visible during transition.
**Why it happens:** Swiper's fade effect doesn't automatically hide the previous slide unless `crossFade: true` is set.
**How to avoid:** Always set `fadeEffect: { crossFade: true }` when using `effect: 'fade'`.
**Warning signs:** All slides visible simultaneously, or weird layering.

### Pitfall 4: Navigation Arrows Visible on Mobile
**What goes wrong:** Swiper navigation arrows clutter the mobile view where swipe gestures are the primary interaction.
**Why it happens:** Swiper shows navigation by default on all screen sizes.
**How to avoid:** Hide arrows on mobile with CSS: `.swiper-button-prev, .swiper-button-next { @apply hidden md:flex; }` or use Tailwind classes on the arrow elements.

### Pitfall 5: Counter Animation Fires Multiple Times
**What goes wrong:** Scrolling up and down past the stats section triggers the count-up animation repeatedly, looking janky.
**Why it happens:** IntersectionObserver fires on every enter/exit unless explicitly unobserved.
**How to avoid:** Call `observer.unobserve(entry.target)` after the first animation triggers. This is the "animate once" pattern.

### Pitfall 6: `getPermalink()` Adds Leading Slash to Anchors
**What goes wrong:** Using `getPermalink('#contacto')` may produce `/contacto` instead of `#contacto` depending on how the util resolves paths.
**Why it happens:** `getPermalink()` is designed for page routes, not anchor links.
**How to avoid:** Use plain string `'#contacto'` directly in navigation.ts, bypassing `getPermalink()`.

### Pitfall 7: Dropdown CSS Still Active
**What goes wrong:** After removing dropdown `links[]` from nav items, hover/focus still triggers empty dropdown menus because the CSS rule `.dropdown:hover .dropdown-menu { display: block; }` remains in tailwind.css.
**Why it happens:** Header.astro conditionally adds `class="dropdown"` when `links?.length` is truthy. With flat links (no sub-links), this won't trigger. But if any stale data remains, ghost dropdowns appear.
**How to avoid:** Ensure ALL links in navigation.ts have no `links` sub-array. The conditional in Header.astro (`links?.length ? 'dropdown' : ''`) will produce empty string, so no dropdown class is applied.

## Code Examples

### navigation.ts Complete Rewrite
```typescript
// src/navigation.ts
// Single-page anchor navigation for campivacorp.

export const headerData = {
  links: [
    { text: 'Inicio', href: '#inicio' },
    { text: 'Nosotros', href: '#nosotros' },
    { text: 'Productos', href: '#productos' },
    { text: 'Servicios', href: '#servicios' },
    { text: 'Valores', href: '#valores' },
    { text: 'Certificaciones', href: '#certificaciones' },
    { text: 'Contacto', href: '#contacto' },
  ],
  actions: [
    {
      text: 'Contactanos',
      href: '#contacto',
    },
  ],
};

// Footer data -- Phase 4 (placeholder)
export const footerData = {
  links: [],
  secondaryLinks: [],
  socialLinks: [],
  footNote: '',
};
```

### Hero Slide Structure (One Slide)
```html
<div class="swiper-slide relative">
  <!-- Background: gradient fallback or image -->
  <div class="absolute inset-0 bg-gradient-to-br from-[#25272f] to-[#5d6f31]"></div>
  <!-- Overlay -->
  <div class="absolute inset-0 bg-[#25272f]/60"></div>
  <!-- Content -->
  <div class="relative z-10 flex flex-col items-center justify-center min-h-screen text-center px-4">
    <h2 class="font-heading font-extrabold text-white text-3xl md:text-5xl lg:text-6xl mb-4">
      Soluciones agroindustriales para el mundo
    </h2>
    <p class="font-sans text-white/80 text-lg md:text-xl mb-8 max-w-2xl">
      Trading, brokeraje y logistica de productos agroindustriales
    </p>
    <div class="flex flex-col sm:flex-row gap-4">
      <a href="#productos" class="btn-primary px-8 py-3">Ver Productos</a>
      <a href="#contacto" class="btn border-white text-white hover:bg-white/10 px-8 py-3">Contactar</a>
    </div>
  </div>
</div>
```

### Swiper Custom CSS (pagination dots white, arrows desktop-only)
```css
/* In a <style> block within HeroSlider.astro or as is:global */
.hero-swiper .swiper-pagination-bullet {
  background: white;
  opacity: 0.5;
}
.hero-swiper .swiper-pagination-bullet-active {
  opacity: 1;
}
.hero-swiper .swiper-button-prev,
.hero-swiper .swiper-button-next {
  color: white;
  display: none;
}
@media (min-width: 768px) {
  .hero-swiper .swiper-button-prev,
  .hero-swiper .swiper-button-next {
    display: flex;
  }
}
```

### Stats Counter Section Template
```astro
<section id="stats" class="bg-[#25272f] py-16 md:py-20">
  <div class="max-w-6xl mx-auto px-4">
    <div class="grid grid-cols-2 md:grid-cols-4 gap-8 text-center">
      <div>
        <div class="font-heading font-extrabold text-white text-4xl md:text-5xl" data-target="25" data-suffix="+">0</div>
        <div class="font-sans font-medium text-white/70 text-sm mt-2 uppercase tracking-wider">Anos de experiencia</div>
      </div>
      <div>
        <div class="font-heading font-extrabold text-white text-4xl md:text-5xl" data-target="7" data-suffix="">0</div>
        <div class="font-sans font-medium text-white/70 text-sm mt-2 uppercase tracking-wider">Categorias de productos</div>
      </div>
      <div>
        <div class="font-heading font-extrabold text-white text-4xl md:text-5xl" data-target="5" data-suffix="">0</div>
        <div class="font-sans font-medium text-white/70 text-sm mt-2 uppercase tracking-wider">Certificaciones internacionales</div>
      </div>
      <div>
        <div class="font-heading font-extrabold text-white text-4xl md:text-5xl" data-target="100" data-suffix="%">0</div>
        <div class="font-sans font-medium text-white/70 text-sm mt-2 uppercase tracking-wider">Compromiso con la calidad</div>
      </div>
    </div>
  </div>
</section>
```

## Discretion Recommendations

### Swiper Effect: Use Fade
**Recommendation:** `effect: 'fade'` with `crossFade: true`.
**Reason:** Fade transitions are more premium/corporate than sliding. Slide effect feels like a carousel; fade feels like a story. Matches the reference sites (camposol.com uses fade-like hero).

### Hero Images: Gradient Fallbacks
**Recommendation:** Use CSS gradient backgrounds as initial implementation. Three distinct gradients:
- Slide 1: `from-[#25272f] via-[#3a4020] to-[#5d6f31]` (agricultural/field feel)
- Slide 2: `from-[#25272f] to-[#2a3518]` (product/processing)
- Slide 3: `from-[#1a1c23] to-[#25272f]` (global/commerce)

Real images can replace these later by adding `<img>` tags with `object-cover` inside each slide.

### Stats Animation Timing
**Recommendation:** 2000ms duration with easeOutQuart easing. Start when 30% of section is visible. This gives a satisfying "count up" effect without feeling slow.

### Responsive Breakpoints for Stats
**Recommendation:** `grid-cols-2 md:grid-cols-4`. Two columns on mobile (2x2 grid), four columns on desktop. Clean and readable at all sizes.

### AstroWind Demo Pages
**Recommendation:** Defer removal to cleanup. These pages don't interfere with the single-page index and removing them risks breaking component imports. Focus Phase 2 on the visible deliverable.

### Navigation Scroll Offset
**Recommendation:** Use WidgetWrapper's existing `scroll-mt-[72px]` for Phase 3+ sections. The hero itself doesn't need scroll offset (it's the top of the page). For the stats section, since it's full-width dark bg and not wrapped in WidgetWrapper, add `scroll-mt-[72px]` directly if it needs an anchor.

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| AstroWind demo content | Single-page with campivacorp. sections | This phase | Complete index.astro rewrite |
| Dropdown nav with getPermalink() | Flat anchor links with plain #hash | This phase | Simpler navigation.ts, no route dependencies |
| AstroWind Stats widget (static numbers) | Custom StatsCounter with count-up animation | This phase | New component, more visual impact |
| No hero slider | Swiper 12.1.2 with fade + autoplay | This phase | Major visual element, first impression |

## Open Questions

1. **Hero placeholder images vs gradients**
   - What we know: CONTEXT.md says "use high-quality placeholder images (Unsplash-style)" but also allows "solid color gradients if images unavailable"
   - What's unclear: Whether actual placeholder images will be provided
   - Recommendation: Build with gradient fallbacks. Structure the HTML so images can be dropped in later (absolute positioned `<img>` with `object-cover` behind the overlay div). This is non-blocking.

2. **Whether `getPermalink()` should be used for anchor hrefs**
   - What we know: Current navigation.ts uses `getPermalink()` for all links. The function prepends base path and handles trailing slashes.
   - What's unclear: Whether passing `'#contacto'` through `getPermalink()` produces the correct output
   - Recommendation: Do NOT use `getPermalink()` for anchors. Use plain strings. The function is for page routes. Plain `'#contacto'` is correct for same-page anchors.

3. **Swiper instance cleanup on View Transitions**
   - What we know: ClientRouter swaps DOM. Old Swiper instances become orphaned.
   - What's unclear: Whether Astro auto-garbage-collects or if we need explicit `.destroy()`
   - Recommendation: Store Swiper instance in a variable. On `astro:after-swap`, call `.destroy()` on the old instance before creating a new one. Defensive pattern.

## Validation Architecture

### Test Framework
| Property | Value |
|----------|-------|
| Framework | Astro check + ESLint + manual visual |
| Config file | astro.config.*, eslint.config.* |
| Quick run command | `npm run dev` (visual inspection) |
| Full suite command | `npm run build` (confirms no build errors) |

### Phase Requirements -> Test Map
| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| NAV-01 | Fixed navbar with logo | manual | Visual: `npm run dev`, navbar visible with PNG logo | N/A |
| NAV-02 | Anchor links scroll to sections | manual | Click each nav link, verify scroll | N/A |
| NAV-03 | CTA "Contactanos" styled correctly | manual | Visual: green button in navbar | N/A |
| NAV-04 | Mobile hamburger works | manual | Resize to mobile, toggle menu, click link | N/A |
| NAV-05 | Navbar bg changes on scroll | manual | Scroll past 60px, observe white bg + shadow | N/A |
| HERO-01 | Swiper with 3 slides + overlay | manual | Visual: slides rotate, overlay visible | N/A |
| HERO-02 | Slide 1 correct text | manual | Visual inspection of first slide | N/A |
| HERO-03 | Slide 2 correct text | manual | Wait for autoplay or navigate | N/A |
| HERO-04 | Slide 3 correct text | manual | Wait for autoplay or navigate | N/A |
| HERO-05 | Dual CTAs present | manual | Visual: two buttons on each slide | N/A |
| HERO-06 | Autoplay + pagination + arrows | manual | Autoplay rotates at ~5s, dots clickable, arrows on desktop | N/A |
| STAT-01 | Counter section with scroll trigger | manual | Scroll to stats, observe count-up animation | N/A |
| STAT-02 | 4 correct stats | manual | Visual: 25+, 7, 5, 100% with correct labels | N/A |
| STAT-03 | Count-up animation | manual | Numbers start at 0, animate up on viewport entry | N/A |

### Sampling Rate
- **Per task commit:** `npm run build` (confirms no build errors)
- **Per wave merge:** `npm run build` + visual inspection of dev server
- **Phase gate:** Full visual walkthrough on desktop + mobile viewport

### Wave 0 Gaps
- No automated visual regression testing -- all hero/stats/nav verification is manual
- `npm run build` is the automated gate (catches import errors, Astro type errors, broken references)
- This is acceptable: Phase 2 is predominantly visual UI work

## Sources

### Primary (HIGH confidence)
- **Codebase inspection:** Layout.astro (Swiper CSS imports confirmed lines 4-6), BasicScripts.astro (scroll behavior at 60px, smooth scroll, mobile menu), Header.astro (props interface, dropdown conditional), navigation.ts (current structure), tailwind.css (#header.scroll styles), WidgetWrapper.astro (scroll-mt-[72px]), Stats.astro (existing widget structure), PageLayout.astro (header props pass-through), Button.astro (variant classes)
- **Swiper 12.1.2** installed in node_modules -- modules directory confirmed: navigation.mjs, pagination.mjs, autoplay.mjs, effect-fade.mjs all present
- **package.json:** swiper ^12.1.2, no new dependencies needed

### Secondary (MEDIUM confidence)
- [Swiper Getting Started](https://swiperjs.com/get-started) - Module import pattern: `import { Navigation, Pagination } from 'swiper/modules'`
- [Swiper API](https://swiperjs.com/swiper-api) - Configuration options for fade, autoplay, pagination
- [Swiper EffectFade Types](https://swiperjs.com/types/interfaces/types_modules_effect_fade.FadeEffectOptions) - crossFade option confirmed

### Tertiary (LOW confidence)
- None -- all findings verified against installed code or official Swiper docs

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH - all libraries already installed, versions confirmed from package.json and node_modules
- Architecture: HIGH - every file to modify/create was read and its interface verified from codebase
- Pitfalls: HIGH - Swiper script tag pattern verified from Phase 1 research + Astro docs pattern; View Transitions re-init pattern from existing AOS/Preline code in Layout.astro

**Research date:** 2026-03-16
**Valid until:** 2026-04-16 (stable -- no fast-moving dependencies, all versions locked)
