# Stack Research: campivacorp. Landing Page

**Researched:** 2026-03-15
**Confidence:** HIGH

## Current Stack (Already Installed)

| Technology | Version | Status | Notes |
|-----------|---------|--------|-------|
| Astro | 5.12.9 | ✓ Good | Keep, do NOT upgrade to 6.x |
| Tailwind CSS | 3.4.17 | ✓ Good | Keep on v3, AstroWind not migrated to v4 |
| AstroWind template | beta.52 | ✓ Good | Customization base |
| Swiper | 12.x | ✓ Good | Import modules individually |
| AOS | 2.x | ✓ Good | Unmaintained since 2021 but sufficient |
| Preline UI | 4.1.2 | ✓ Good | JS components via data attributes |
| DaisyUI | 5.x | ⚠ WARNING | Targets Tailwind 4, we're on Tailwind 3 |

## Critical Issue: DaisyUI 5 + Tailwind 3

DaisyUI 5.5.x is designed for Tailwind CSS 4. The project runs Tailwind CSS 3.4.17. Build succeeds but component styles may silently break.

**Recommendation:** Minimize DaisyUI usage — use it only for simple utilities where the `daisy-` prefix works. For complex components, rely on Tailwind utilities + Preline UI instead. If issues arise, downgrade to DaisyUI 4.x (`npm install daisyui@4`).

## Recommended Additions

### Fonts (Replace Inter)
- `@fontsource/nunito-sans` — weight 800 for headings/logo (Gotham Bold alternative)
- `@fontsource/montserrat` — weights 500, 700 for body text
- Remove `@fontsource-variable/inter` (AstroWind default)
- Self-hosted via @fontsource = zero FOUT, no external requests

### SEO
- `astro-seo-schema` — type-safe JSON-LD for Organization, WebSite, Service schemas
- Existing `@astrolib/seo` handles meta tags adequately

### Image Optimization
- Astro's built-in `<Image />` with sharp is sufficient
- No need for additional image libraries
- Use placeholder images with proper `width`/`height` for CLS prevention

## Swiper Best Practices

Import modules individually to avoid 150KB full bundle:
```js
import Swiper from 'swiper';
import { Navigation, Pagination, Autoplay, EffectFade } from 'swiper/modules';
```

## What NOT To Do

| Don't | Why |
|-------|-----|
| Upgrade to Tailwind 4 | AstroWind template not migrated, high-risk scope creep |
| Upgrade to Astro 6 | Template compatibility unknown |
| Add React/Vue | Astro islands unnecessary for this static site |
| Use heavy animation libs (GSAP, Framer) | AOS is sufficient for corporate fade/slide animations |
| Add a CMS (Strapi, Contentful) | Static content, managed in code |
| Rely heavily on DaisyUI components | v5/v3 Tailwind mismatch |

## Font Loading Strategy

```astro
---
// In Layout.astro frontmatter
import '@fontsource/nunito-sans/800.css';
import '@fontsource/montserrat/500.css';
import '@fontsource/montserrat/700.css';
---
```

Update CSS variables in CustomStyles.astro:
```css
:root {
  --aw-font-sans: 'Montserrat', sans-serif;
  --aw-font-heading: 'Nunito Sans', sans-serif;
}
```

---
*Researched: 2026-03-15*
