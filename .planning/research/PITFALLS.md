# Pitfalls Research: campivacorp. Landing Page

**Researched:** 2026-03-15
**Confidence:** HIGH

## Critical Pitfalls

### 1. DaisyUI v5 + Tailwind v3 Incompatibility (CRITICAL)
**Risk:** DaisyUI 5.5.19 requires Tailwind CSS v4, but project has Tailwind 3.4.17. DaisyUI will silently produce no styles.
**Warning signs:** Components with `daisy-` prefix render unstyled.
**Prevention:** Downgrade to `daisyui@4` or remove entirely. Since DaisyUI was added speculatively, removal is cleanest.
**Phase:** Foundation (Phase 1)

### 2. AOS Inside Swiper Slides — Invisible Content
**Risk:** AOS uses Intersection Observer which conflicts with Swiper's internal slide positioning. Slides 2-3 will appear invisible.
**Warning signs:** Second and third slides show blank content.
**Prevention:** Keep AOS completely out of the Swiper container. Animate Swiper content with Swiper's own animation effects or CSS transitions.
**Phase:** Hero (Phase 2)

### 3. AstroWind Default Config Leaks
**Risk:** `config.yaml` retains AstroWind defaults — site name "AstroWind", English language, blog enabled, wrong SEO metadata.
**Warning signs:** Page title shows "AstroWind", blog routes accessible, English meta tags.
**Prevention:** Update config.yaml completely in foundation phase before building any sections.
**Phase:** Foundation (Phase 1)

### 4. CustomStyles.astro Default Colors
**Risk:** All `--aw-color-*` CSS variables still point to AstroWind's default blue/purple scheme.
**Warning signs:** Blue accents, purple highlights appearing on components.
**Prevention:** Remap all CSS variables to campivacorp brand colors (#25272f, #95b444, #cbdc53, #5d6f31) before any section work.
**Phase:** Foundation (Phase 1)

### 5. Font Loading — Inter Still Default
**Risk:** AstroWind imports `@fontsource-variable/inter`. If not replaced, headings render in Inter instead of Nunito Sans.
**Warning signs:** Font looks wrong but no errors in console.
**Prevention:** Replace Inter imports with Nunito Sans 800 and Montserrat 500/700 in Layout.astro. Update CSS variables.
**Phase:** Foundation (Phase 1)

## High-Risk Pitfalls

### 6. Swiper Initialization in Astro Static Build
**Risk:** Swiper requires `<script>` tag initialization, not frontmatter imports. Astro's static build strips unscoped JS.
**Warning signs:** Slider works in dev but breaks in production build.
**Prevention:** Use `<script>` tag (not `is:inline`) with proper Swiper module imports. Test with `npm run build && npm run preview`.
**Phase:** Hero (Phase 2)

### 7. Three Overlapping UI Libraries
**Risk:** Tailwind custom + DaisyUI + Preline create class name confusion and potential conflicts.
**Warning signs:** Inconsistent button styles, unexpected padding, components looking different from design.
**Prevention:** Use ONE system: Tailwind utilities for styling + Preline only for interactive JS behaviors (dropdowns, modals). Remove DaisyUI.
**Phase:** Foundation (Phase 1)

### 8. CLS from AOS Animations
**Risk:** AOS hides elements with `opacity: 0` and `transform` until they enter viewport. This can cause Cumulative Layout Shift.
**Warning signs:** Page jumps as user scrolls, Lighthouse CLS score > 0.1.
**Prevention:** Set explicit dimensions on animated containers. Use `data-aos-anchor-placement` to control trigger points. Apply AOS only to decorative elements, not structural ones.
**Phase:** Polish (final phase)

### 9. Navigation Anchor Links on Single-Page
**Risk:** AstroWind's navigation.ts is designed for multi-page routing. Changing to `#section-id` anchors may break active state detection and mobile menu close behavior.
**Warning signs:** Nav links don't scroll to sections, mobile menu stays open after clicking.
**Prevention:** Override navigation.ts with anchor links and add smooth scroll + mobile menu close behavior in BasicScripts or custom script.
**Phase:** Shell (Phase 2)

## Medium-Risk Pitfalls

### 10. Dark Mode Remnants
**Risk:** AstroWind has full dark mode support. Brand book specifies light-only with dark accent sections. Dark mode toggle may confuse the brand.
**Warning signs:** Toggle button visible, dark mode accessible but with wrong brand colors.
**Prevention:** Remove dark mode toggle from header. Set `defaultTheme: 'light:only'` in config.yaml.
**Phase:** Foundation (Phase 1)

### 11. Contact Form Without Backend
**Risk:** Static Astro site has no form submission handler. Contact form will silently fail.
**Warning signs:** Form submits but no email received.
**Prevention:** Decide form handler early — Formspree, Netlify Forms, or mailto: fallback. Wire up in contact section phase.
**Phase:** Contact (Phase 4-5)

### 12. Placeholder Images — Wrong Aspect Ratios
**Risk:** Placeholder images may have different aspect ratios than final client photos, causing layout shifts when swapped.
**Warning signs:** Sections look different after image replacement.
**Prevention:** Define fixed aspect ratios in CSS for all image containers. Use `object-fit: cover` consistently.
**Phase:** All content phases

### 13. AstroWind Blog Routes Still Active
**Risk:** Template includes full blog infrastructure at `/blog/*`. Exposes unbranded content pages.
**Warning signs:** Google indexes blog pages, users find AstroWind demo content.
**Prevention:** Remove or redirect blog routes. Delete blog pages or add `noindex` meta tags.
**Phase:** Foundation (Phase 1)

## Pitfall-to-Phase Mapping

| Phase | Pitfalls | Count |
|-------|----------|-------|
| Foundation | #1, #3, #4, #5, #7, #10, #13 | 7 |
| Hero/Shell | #2, #6, #9 | 3 |
| Content | #12 | 1 |
| Contact | #11 | 1 |
| Polish | #8 | 1 |

**Foundation phase carries the heaviest pitfall load.** Getting this right prevents cascading issues in all subsequent phases.

---
*Researched: 2026-03-15*
