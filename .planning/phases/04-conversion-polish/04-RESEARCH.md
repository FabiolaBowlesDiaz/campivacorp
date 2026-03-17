# Phase 4: Conversion + Polish - Research

**Researched:** 2026-03-17
**Domain:** Contact forms, footer, responsive verification, scroll animations (AOS)
**Confidence:** HIGH

## Summary

Phase 4 finalizes the campivacorp. landing page with four workstreams: (1) contact section with Formspree form, (2) footer with navigation and social links, (3) WhatsApp floating CTA, (4) AOS animations across all sections, and (5) responsive verification at 320/768/1024px breakpoints. All pieces use existing AstroWind patterns and the project's established Tailwind CSS token system.

The codebase is well-structured for this phase. The existing `Contact.astro` widget and `Form.astro` UI component provide a base, but need modification for the two-column layout (form + contact info) and Formspree action attribute. The `Footer.astro` widget is fully functional -- it just needs `footerData` in `navigation.ts` populated with real data. AOS is already initialized in `Layout.astro` with `once: true` -- the task is adding `data-aos` attributes to section content elements. The WhatsApp button is a new standalone element in `Layout.astro`.

**Primary recommendation:** Build a custom `ContactoSection.astro` (following the NosotrosSection pattern) rather than trying to force the existing Contact.astro widget into the two-column layout. Keep Formspree as a plain HTML form action -- no JavaScript fetch needed.

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions
- Contact form fields: name, email, company, message (all required except company)
- Backend: Formspree with placeholder endpoint (`https://formspree.io/f/YOUR_ID`) with code comment
- Form uses standard HTML form action (no JS fetch)
- Contact info: rcampbell@campivacorp.com, +59169006424, LinkedIn
- WhatsApp: +59169006424, fixed bottom-right, z-50, green #95b444 circle, pre-filled message
- WhatsApp link: `https://wa.me/59169006424?text=Hola%2C%20me%20interesa%20conocer%20m%C3%A1s%20sobre%20los%20productos%20de%20campivacorp.`
- Social: LinkedIn + WhatsApp ONLY. No Facebook, no Instagram, no placeholder # links
- Footer: dark bg #25272f, logo + nav + LinkedIn/WhatsApp + copyright "2026 campivacorp."
- AOS: fade-up only, once=true, NOT inside Swiper, stagger delays on grids
- Responsive: verify 320px, 768px, 1024px breakpoints

### Claude's Discretion
- Exact form styling (input borders, focus states, button style)
- WhatsApp button exact size and position offset
- AOS delay values for grid items
- Footer column layout and spacing
- Whether to add a "back to top" button
- Contact section layout details

### Deferred Ideas (OUT OF SCOPE)
- SEO JSON-LD schema markup
- Favicon/OG image generation
- Google Analytics integration
- Cookie consent banner
- Performance audit (Lighthouse)
</user_constraints>

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|-----------------|
| CONT-01 | Contact form with name, email, company, message | Custom ContactoSection.astro with Formspree HTML form action |
| CONT-02 | Contact info: email, phone | Two-column layout: form left, contact info right |
| CONT-03 | Social media links (LinkedIn + WhatsApp only per decisions) | Icon links using astro-icon tabler set |
| CONT-04 | WhatsApp floating CTA (fixed bottom-right) | Standalone element in Layout.astro, wa.me deep link |
| CONT-05 | Form submission handler (Formspree) | Plain HTML form action + method POST, placeholder endpoint |
| FOOT-01 | Footer with brand (isotipo + wordmark) | Reuse Logo.astro component, populate footerData |
| FOOT-02 | Navigation links mirroring navbar | footerData.links in navigation.ts with section anchors |
| FOOT-03 | Social media icons (LinkedIn + WhatsApp only) | footerData.socialLinks with tabler icons |
| FOOT-04 | Copyright notice | footerData.footNote with 2026 year |
| RESP-01 | All sections responsive 320px+ / 768px+ / 1024px+ | Systematic breakpoint audit of all sections |
| RESP-02 | Product grid adapts | Verify existing grid: 1-col/2-col/3-col |
| RESP-03 | Hero slider adapts text/CTA for mobile | Verify existing responsive classes |
| ANIM-01 | AOS fade-up on section entries | data-aos="fade-up" on content containers across all widgets |
| ANIM-02 | AOS NOT inside Swiper | Skip HeroSlider.astro, only animate non-Swiper sections |
| ANIM-03 | Subtle, professional animations | fade-up only, 800ms duration (already configured) |
</phase_requirements>

## Standard Stack

### Core (Already Installed)
| Library | Version | Purpose | Status |
|---------|---------|---------|--------|
| AOS | ^2.3.4 | Scroll-triggered animations | Installed, initialized in Layout.astro |
| Astro | ^5.12.9 | Static site framework | Project framework |
| Tailwind CSS | ^3.4.17 | Utility CSS | Project styling |
| astro-icon | ^1.1.5 | Icon components (tabler set) | Used throughout |

### External Service
| Service | Purpose | Integration |
|---------|---------|-------------|
| Formspree | Form backend | HTML form action POST, no JS needed |

### No New Dependencies Required
This phase requires zero new npm packages. AOS is installed and configured. Formspree is a pure HTML integration. WhatsApp is a link. All icons use the existing tabler icon set via astro-icon.

## Architecture Patterns

### Pattern 1: Custom Section Widget (ContactoSection)
**What:** A standalone `.astro` widget following the NosotrosSection pattern -- uses WidgetWrapper with id, custom layout inside.
**When to use:** When the existing Contact.astro widget layout does not match requirements (it centers a single form; we need two-column form+info).

```astro
<!-- Pattern from NosotrosSection.astro -->
<WidgetWrapper id="contacto" containerClass="max-w-7xl mx-auto">
  <Headline title="Contactanos" />
  <div class="md:flex md:gap-16">
    <!-- Left: form -->
    <div class="md:basis-1/2">...</div>
    <!-- Right: contact info -->
    <div class="md:basis-1/2">...</div>
  </div>
</WidgetWrapper>
```

### Pattern 2: Formspree HTML Form
**What:** Standard HTML form with `action` and `method="POST"`. No JavaScript fetch, no AJAX. Formspree handles redirect.
**When to use:** Static sites with simple contact forms.

```html
<form action="https://formspree.io/f/YOUR_ID" method="POST">
  <!-- Formspree uses 'name' attributes to identify fields -->
  <input type="text" name="name" required />
  <input type="email" name="email" required />
  <input type="text" name="company" />
  <textarea name="message" required></textarea>
  <!-- Optional: hidden _next field for custom redirect -->
  <input type="hidden" name="_next" value="https://campivacorp.com/#contacto" />
  <button type="submit">Enviar</button>
</form>
```

Key Formspree details:
- Field names map directly to what appears in email/dashboard
- `_next` hidden field controls post-submit redirect URL
- `_subject` hidden field sets email subject line
- No CORS issues since it is a standard form POST
- Free tier: 50 submissions/month

### Pattern 3: footerData Population in navigation.ts
**What:** The Footer.astro widget already consumes `footerData` -- it just needs to be populated.
**When to use:** Footer content is data-driven via the existing pattern.

```typescript
export const footerData = {
  links: [
    {
      title: 'Navegacion',
      links: [
        { text: 'Nosotros', href: '#nosotros' },
        { text: 'Productos', href: '#productos' },
        // ...
      ],
    },
    {
      title: 'Contacto',
      links: [
        { text: 'rcampbell@campivacorp.com', href: 'mailto:rcampbell@campivacorp.com' },
        { text: '+591 69006424', href: 'tel:+59169006424' },
      ],
    },
  ],
  secondaryLinks: [],
  socialLinks: [
    { ariaLabel: 'LinkedIn', icon: 'tabler:brand-linkedin', href: 'https://www.linkedin.com/company/campivacorp/' },
    { ariaLabel: 'WhatsApp', icon: 'tabler:brand-whatsapp', href: 'https://wa.me/59169006424' },
  ],
  footNote: '&copy; 2026 campivacorp. Todos los derechos reservados.',
};
```

### Pattern 4: WhatsApp Floating Button in Layout.astro
**What:** A fixed-position anchor element placed just before `</body>` in Layout.astro (after `<slot />`).
**When to use:** Persistent cross-page elements that are not part of any section.

```html
<a href="https://wa.me/59169006424?text=..." target="_blank" rel="noopener noreferrer"
   class="fixed bottom-6 right-6 z-50 w-14 h-14 bg-[#95b444] rounded-full flex items-center justify-center shadow-lg hover:scale-110 hover:shadow-xl transition-all duration-300"
   aria-label="Contactar por WhatsApp">
  <Icon name="tabler:brand-whatsapp" class="w-7 h-7 text-white" />
</a>
```

### Pattern 5: AOS Attribute Application
**What:** Add `data-aos="fade-up"` and optional `data-aos-delay` to section content containers. AOS is already initialized globally with `once: true` and `duration: 800`.
**When to use:** Every content section EXCEPT HeroSlider (Swiper conflict) and StatsCounter (has its own IntersectionObserver animation).

```html
<!-- On section headings/containers -->
<div data-aos="fade-up">
  <Headline ... />
</div>

<!-- On grid items with stagger -->
<div data-aos="fade-up" data-aos-delay="0">Card 1</div>
<div data-aos="fade-up" data-aos-delay="100">Card 2</div>
<div data-aos="fade-up" data-aos-delay="200">Card 3</div>
```

### Anti-Patterns to Avoid
- **AOS inside Swiper:** Causes animation conflicts -- Swiper manages its own slide transitions
- **AOS on StatsCounter:** Already has IntersectionObserver-based counter animation; double-animating would be jarring
- **JavaScript fetch for Formspree:** Unnecessary complexity; standard form POST works perfectly for static sites
- **Placeholder social links (#):** User explicitly banned this -- omit social networks that have no real URL

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Form backend | Custom API endpoint | Formspree HTML action | Zero infrastructure, works with static sites |
| Scroll animations | Custom IntersectionObserver per section | AOS data attributes | Already installed and configured globally |
| Footer layout | Custom footer component | Existing Footer.astro + footerData | Widget already handles responsive grid, social icons, links |
| WhatsApp deep link | Custom chat widget | wa.me URL with text parameter | Universal, no JS, works on all devices |
| Icon rendering | SVG inline sprites | astro-icon with tabler set | Already the project standard |

## Common Pitfalls

### Pitfall 1: Formspree Redirect Loop
**What goes wrong:** After form submission, Formspree redirects to its own thank-you page by default, breaking the single-page experience.
**Why it happens:** No `_next` hidden field specified.
**How to avoid:** Add `<input type="hidden" name="_next" value="https://yourdomain.com/#contacto" />` to redirect back to the contact section.
**Warning signs:** Testing form submit and landing on formspree.io instead of your site.

### Pitfall 2: AOS + WidgetWrapper Double Animation
**What goes wrong:** WidgetWrapper already has `motion-safe:md:intersect:animate-fade` CSS classes. Adding AOS `data-aos="fade-up"` on the same or nested elements creates double/conflicting fade animations.
**Why it happens:** AstroWind's WidgetWrapper uses CSS `intersect` observer for its own fade animation.
**How to avoid:** Either (a) remove the `intersect-once intersect-quarter motion-safe:md:opacity-0 motion-safe:md:intersect:animate-fade` classes from WidgetWrapper when AOS is active, or (b) add AOS only to inner content elements (not the wrapper itself) and accept the WidgetWrapper fade as the container animation. Option (b) is safer -- less code change, layered effect.
**Warning signs:** Elements flashing, appearing then fading, or animating twice on scroll.

### Pitfall 3: Footer Dark Theme on Light Site
**What goes wrong:** Footer.astro has `theme` prop but the site has dark mode disabled. The `dark:` Tailwind variants do not activate. Text in footer on dark bg becomes invisible or low contrast.
**Why it happens:** `dark:` variants require either `class="dark"` or `media: prefers-color-scheme`.
**How to avoid:** The Footer.astro already supports `{ dark: theme === 'dark' }` class on the footer element, and WidgetWrapper uses `{ dark: isDark }`. Pass `theme="dark"` to Footer and verify `isDark` context enables `dark:text-slate-300` etc. Alternatively, override text colors explicitly with Tailwind classes (e.g., `text-gray-300` instead of `dark:text-gray-300`).
**Warning signs:** Footer text being the default dark color (#25272f) on dark bg (#25272f) = invisible.

### Pitfall 4: WhatsApp Button Overlapping Content on Mobile
**What goes wrong:** Fixed bottom-right button covers important content or the contact form submit button on small screens.
**Why it happens:** `fixed bottom-6 right-6` does not account for small viewports where the button may overlap the last section.
**How to avoid:** Use `bottom-4 right-4` on mobile with `sm:bottom-6 sm:right-6`. Consider adding `mb-20` to the footer or last section to ensure the WhatsApp button does not overlap the copyright.
**Warning signs:** On 320px width, button covers text or interactive elements.

### Pitfall 5: AOS Elements Starting Invisible with No-JS
**What goes wrong:** AOS sets elements to `opacity: 0` via CSS. If JavaScript fails to load, content remains invisible.
**Why it happens:** AOS CSS applies `[data-aos]` opacity transform by default.
**How to avoid:** AOS 2.3.4 handles this gracefully -- if `AOS.init()` is never called, the CSS does not hide elements. Since AOS is initialized in a `<script>` tag in Layout.astro with `astro:after-swap` re-init, this should be fine. Just verify during testing.

### Pitfall 6: Responsive Grid Overflow at 320px
**What goes wrong:** Product cards or service grids overflow horizontally on 320px screens.
**Why it happens:** Fixed widths, padding, or grid gaps that exceed viewport at very small sizes.
**How to avoid:** Test each section at exactly 320px. Common fixes: reduce `gap` on mobile, ensure `max-w-full` and `overflow-hidden` on containers, use `text-sm` for card content on mobile.
**Warning signs:** Horizontal scrollbar appearing on mobile.

## Code Examples

### ContactoSection.astro Structure
```astro
---
import WidgetWrapper from '~/components/ui/WidgetWrapper.astro';
import Headline from '~/components/ui/Headline.astro';
import Button from '~/components/ui/Button.astro';
import { Icon } from 'astro-icon/components';
---

<WidgetWrapper id="contacto" containerClass="max-w-7xl mx-auto">
  <Headline title="Contactanos" subtitle="Estamos para ayudarte" />

  <div class="grid md:grid-cols-2 gap-12">
    <!-- Form column -->
    <form action="https://formspree.io/f/YOUR_ID" method="POST" class="space-y-6">
      <!-- Replace YOUR_ID with your Formspree form ID -->
      <input type="hidden" name="_next" value="/#contacto" />
      <div>
        <label for="name" class="block text-sm font-medium mb-1">Nombre *</label>
        <input type="text" name="name" id="name" required
          class="py-3 px-4 block w-full rounded-lg border border-gray-200 focus:border-primary focus:ring-1 focus:ring-primary" />
      </div>
      <div>
        <label for="email" class="block text-sm font-medium mb-1">Email *</label>
        <input type="email" name="email" id="email" required
          class="py-3 px-4 block w-full rounded-lg border border-gray-200 focus:border-primary focus:ring-1 focus:ring-primary" />
      </div>
      <div>
        <label for="company" class="block text-sm font-medium mb-1">Empresa</label>
        <input type="text" name="company" id="company"
          class="py-3 px-4 block w-full rounded-lg border border-gray-200 focus:border-primary focus:ring-1 focus:ring-primary" />
      </div>
      <div>
        <label for="message" class="block text-sm font-medium mb-1">Mensaje *</label>
        <textarea name="message" id="message" rows="4" required
          class="py-3 px-4 block w-full rounded-lg border border-gray-200 focus:border-primary focus:ring-1 focus:ring-primary"></textarea>
      </div>
      <Button variant="primary" type="submit">Enviar Mensaje</Button>
    </form>

    <!-- Contact info column -->
    <div class="flex flex-col justify-center space-y-8">
      <div class="flex items-start gap-4">
        <Icon name="tabler:mail" class="w-6 h-6 text-primary flex-shrink-0 mt-1" />
        <div>
          <p class="font-medium">Email</p>
          <a href="mailto:rcampbell@campivacorp.com" class="text-muted hover:text-primary">
            rcampbell@campivacorp.com
          </a>
        </div>
      </div>
      <div class="flex items-start gap-4">
        <Icon name="tabler:phone" class="w-6 h-6 text-primary flex-shrink-0 mt-1" />
        <div>
          <p class="font-medium">Telefono</p>
          <a href="tel:+59169006424" class="text-muted hover:text-primary">+591 69006424</a>
        </div>
      </div>
      <div class="flex items-start gap-4">
        <Icon name="tabler:brand-linkedin" class="w-6 h-6 text-primary flex-shrink-0 mt-1" />
        <div>
          <p class="font-medium">LinkedIn</p>
          <a href="https://www.linkedin.com/company/campivacorp/" target="_blank" rel="noopener noreferrer"
            class="text-muted hover:text-primary">campivacorp</a>
        </div>
      </div>
    </div>
  </div>
</WidgetWrapper>
```

### Footer Data in navigation.ts
```typescript
export const footerData = {
  links: [
    {
      title: 'Navegacion',
      links: [
        { text: 'Nosotros', href: '#nosotros' },
        { text: 'Productos', href: '#productos' },
        { text: 'Servicios', href: '#servicios' },
        { text: 'Valores', href: '#valores' },
        { text: 'Certificaciones', href: '#certificaciones' },
        { text: 'Contacto', href: '#contacto' },
      ],
    },
    {
      title: 'Contacto',
      links: [
        { text: 'rcampbell@campivacorp.com', href: 'mailto:rcampbell@campivacorp.com' },
        { text: '+591 69006424', href: 'tel:+59169006424' },
      ],
    },
  ],
  secondaryLinks: [],
  socialLinks: [
    { ariaLabel: 'LinkedIn', icon: 'tabler:brand-linkedin', href: 'https://www.linkedin.com/company/campivacorp/' },
    { ariaLabel: 'WhatsApp', icon: 'tabler:brand-whatsapp', href: 'https://wa.me/59169006424' },
  ],
  footNote: '&copy; 2026 campivacorp. Todos los derechos reservados.',
};
```

### WhatsApp Floating Button (in Layout.astro)
```astro
<!-- WhatsApp Floating CTA - add after <slot /> in Layout.astro body -->
<a href="https://wa.me/59169006424?text=Hola%2C%20me%20interesa%20conocer%20m%C3%A1s%20sobre%20los%20productos%20de%20campivacorp."
   target="_blank" rel="noopener noreferrer"
   class="fixed bottom-4 right-4 sm:bottom-6 sm:right-6 z-50 w-14 h-14 bg-[#95b444] rounded-full flex items-center justify-center shadow-lg hover:scale-110 hover:shadow-xl transition-all duration-300"
   aria-label="Contactar por WhatsApp">
  <!-- Use inline SVG or astro-icon -->
</a>
```

Note: Layout.astro uses `import { Icon } from 'astro-icon/components'` -- since the WhatsApp button is static HTML in a layout, use an inline SVG for the WhatsApp icon rather than the `<Icon>` component (which requires Astro component context). Alternatively, create a small `WhatsAppButton.astro` component.

### AOS Application Map
```
Section               | AOS? | Notes
---------------------|------|------
HeroSlider           | NO   | Swiper conflict (ANIM-02)
StatsCounter         | NO   | Has own IntersectionObserver animation
NosotrosSection      | YES  | fade-up on text + image columns
ProductosSection     | YES  | fade-up on headline, stagger on product cards
ServiciosSection     | YES  | fade-up on headline, stagger on service items
ValoresSection       | YES  | fade-up on headline, stagger on value cards
CertificacionesSection| YES | fade-up on headline, stagger on cert badges
PropositoSection     | YES  | fade-up on text content
ContactoSection      | YES  | fade-up on form + info columns
Footer               | NO   | Already has intersect-animate-fade via AstroWind
```

## State of the Art

| Component | Current Approach | Notes |
|-----------|-----------------|-------|
| AOS | v2.3.4 (latest stable) | No v3 exists; 2.3.4 is the standard |
| Formspree | HTML form action POST | Free tier 50 subs/month, no JS required |
| WhatsApp deep link | wa.me/{number}?text={encoded} | Universal standard, works all platforms |

## Validation Architecture

### Test Framework
| Property | Value |
|----------|-------|
| Framework | Manual visual verification (browser DevTools responsive mode) |
| Config file | none |
| Quick run command | `npm run dev` + browser DevTools |
| Full suite command | `npm run build && npm run preview` |

### Phase Requirements Test Map
| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| CONT-01 | Contact form renders with 4 fields | manual | DevTools inspect form fields | N/A |
| CONT-02 | Contact info visible (email, phone) | manual | Visual check | N/A |
| CONT-03 | LinkedIn + WhatsApp links only (no FB/IG) | manual | Inspect footer social links | N/A |
| CONT-04 | WhatsApp button fixed bottom-right | manual | Scroll page, verify fixed position | N/A |
| CONT-05 | Form action points to Formspree | manual | Inspect form element action attribute | N/A |
| FOOT-01 | Footer has brand logo + wordmark | manual | Visual check | N/A |
| FOOT-02 | Footer nav links match navbar | manual | Compare header/footer links | N/A |
| FOOT-03 | Footer social: LinkedIn + WhatsApp only | manual | Inspect social icons | N/A |
| FOOT-04 | Copyright "2026 campivacorp." visible | manual | Visual check | N/A |
| RESP-01 | No overflow at 320/768/1024px | manual | DevTools responsive mode at each breakpoint | N/A |
| RESP-02 | Product grid 1/2/3 col responsive | manual | DevTools at 320/768/1024 | N/A |
| RESP-03 | Hero text/CTAs adapt on mobile | manual | DevTools at 320px | N/A |
| ANIM-01 | Sections fade-up on scroll | manual | Slow scroll through page | N/A |
| ANIM-02 | No AOS inside Swiper | manual | Verify HeroSlider has no data-aos | N/A |
| ANIM-03 | Animations subtle (fade-up only) | manual | Scroll and verify no zoom/flip/bounce | N/A |

### Sampling Rate
- **Per task commit:** `npm run dev` + visual check at 3 breakpoints
- **Per wave merge:** `npm run build` to verify no build errors
- **Phase gate:** Full responsive audit at 320/768/1024px + all requirements visual check

### Wave 0 Gaps
None -- this phase is UI implementation with manual visual verification. No test framework setup needed.

## Open Questions

1. **Formspree _next redirect URL**
   - What we know: `_next` hidden field redirects after submission. But the production domain is unknown.
   - What's unclear: Final deployment domain (Netlify? Vercel? Custom?)
   - Recommendation: Use relative path `/#contacto` which works regardless of domain. Add comment in code.

2. **Footer logo on dark background**
   - What we know: Logo.astro uses `campivacorp-logo-full.png` which may be dark-on-transparent.
   - What's unclear: Whether the PNG has sufficient contrast on #25272f background.
   - Recommendation: Test visually. May need a white/light version of the logo for the footer, or apply CSS filter (brightness/invert) as fallback.

3. **WidgetWrapper intersect animation vs AOS**
   - What we know: WidgetWrapper has `motion-safe:md:intersect:animate-fade` built in. AOS adds its own scroll animation.
   - What's unclear: Whether both firing simultaneously looks good or janky.
   - Recommendation: Add AOS to inner elements only (not the wrapper level). The WidgetWrapper fade is subtle enough to coexist with inner content fade-up.

## Sources

### Primary (HIGH confidence)
- Project codebase: Layout.astro, Footer.astro, Contact.astro, Form.astro, navigation.ts, WidgetWrapper.astro (direct file inspection)
- AOS 2.3.4: Configuration already in Layout.astro (`once: true`, `duration: 800`, `easing: ease-in-out`)
- Formspree documentation (training data): HTML form action POST pattern, `_next` redirect field

### Secondary (MEDIUM confidence)
- Formspree free tier limits (50 submissions/month) -- from training data, may have changed

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH - all libraries already installed and configured in project
- Architecture: HIGH - patterns derived from existing codebase widgets (NosotrosSection, Footer, WidgetWrapper)
- Pitfalls: HIGH - identified from direct code inspection (WidgetWrapper double-animation, dark theme, Formspree redirect)
- Responsive: MEDIUM - verification is manual; issues will be discovered during implementation

**Research date:** 2026-03-17
**Valid until:** 2026-04-17 (stable stack, no fast-moving dependencies)
