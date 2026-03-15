# Project Research Summary

**Project:** campivacorp. Landing Page
**Domain:** B2B Agroindustrial / Commodity Trading Corporate Landing Page (Latin America)
**Researched:** 2026-03-15
**Confidence:** HIGH

## Executive Summary

campivacorp. is building a single-page corporate landing page for a Bolivian agroindustrial trading company with 25+ years of experience. The site's sole purpose is credibility-building and inquiry generation — it is not a marketplace, portal, or e-commerce platform. Research confirms this is the correct scope: regional and global agro trading competitors (Camposol, ECOM, Raatz) all use corporate sites that drive visitors to direct contact (phone, email, form), and campivacorp.'s planned feature set is ahead of every regional competitor. The recommended approach is to build all 11 sections in a strict dependency order: Foundation first (brand theming, config, fonts), then Shell (Header/Footer), then above-the-fold sections (Hero, Stats), then content sections in any order, then Contact, and finally Polish. Total scope is 11 sections, 6 new custom components, and 5 adapted existing widgets.

The technology stack is already installed and validated. Astro 5.x + AstroWind beta.52 + Tailwind CSS 3.4.17 is the correct foundation — do not upgrade any of these three during this project. The one confirmed incompatibility is DaisyUI 5.x (designed for Tailwind 4): it must be removed or downgraded to v4 in the very first phase before any component work begins. Fonts must be replaced (Inter → Nunito Sans 800 + Montserrat 500/700) and all AstroWind default colors/config must be overwritten before touching sections — 7 of the 13 identified pitfalls strike at Foundation phase and cascade into everything else if ignored.

The key risk to manage is the "template defaults" problem: AstroWind ships with blue/purple colors, English language, Inter font, blog enabled, and wrong SEO metadata. All of these silently break campivacorp.'s brand if left in place. The WhatsApp floating button is the highest-impact, lowest-effort differentiator in the entire feature set — it should ship in the initial launch. Contact form backend (Formspree or Netlify Forms) must be decided before Phase 5 to avoid a form that silently fails. All other risks are well-understood and preventable with the documented patterns.

---

## Key Findings

### Recommended Stack

The project already has the correct stack installed. No major additions are needed beyond font packages and one SEO schema library. The critical decision is to freeze all dependency versions: Astro 5.12.9, Tailwind 3.4.17, AstroWind beta.52. Upgrading any of these mid-project will break template compatibility in unpredictable ways.

DaisyUI 5.5.x is the only active incompatibility. It targets Tailwind CSS v4 but the project runs v3. The cleanest resolution is removal; fallback is downgrade to `daisyui@4`. For interactive components, use Preline UI (already installed, works via data attributes with Tailwind 3). For static styling, use Tailwind utilities directly.

**Core technologies:**
- **Astro 5.12.9**: Static site generator — zero-JS by default, ideal for corporate landing page performance
- **AstroWind beta.52**: Component template base — provides Layout, Header, Footer, and 10+ reusable widgets; do not rebuild what it provides
- **Tailwind CSS 3.4.17**: Utility-first CSS — all brand theming flows through CustomStyles.astro CSS variables into Tailwind classes
- **Swiper 12.x**: Hero slider — import modules individually (Navigation, Pagination, Autoplay, EffectFade) to avoid 150KB full bundle
- **Preline UI 4.1.2**: Interactive JS behaviors — dropdowns, mobile menu, modal triggers via data attributes
- **AOS 2.x**: Scroll animations — sufficient for corporate fade/slide; initialize globally in Layout.astro, never inside Swiper containers
- **@fontsource/nunito-sans** (weight 800) + **@fontsource/montserrat** (weights 500, 700): Self-hosted fonts, replace Inter default
- **astro-seo-schema**: Type-safe JSON-LD for Organization, WebSite, Service schemas

**What NOT to add:** React/Vue, GSAP/Framer, Lottie, CMS (Strapi/Contentful), Google Maps API, Tailwind v4, Astro v6.

### Expected Features

Research confirms 10 table-stakes features that B2B agro buyers universally expect, plus 4 differentiators worth including at launch.

**Must have (table stakes):**
- Fixed navbar with logo, anchor links, and CTA — every serious corporate site has this
- Hero with strong value proposition (Swiper slider, 3 slides, dual CTAs) — B2B visitors decide in 5 seconds
- Stats counters (25+ years, 7 categories, markets, 5 certifications) — quantified trust signals are standard in agro
- Quienes Somos / About — 25+ years of experience is a credibility asset that must be front and center
- Product portfolio display — 7 category cards; buyers need to confirm you handle what they need
- Services overview — differentiates trader from product listing; logistics, analytics, brokerage
- Certifications banner — HACCP, GMP/BPM, ISO badges are deal-breakers for food/agro B2B buyers
- Contact section with form + email + phone + WhatsApp — all roads lead to a conversation
- Footer with nav links, social icons, copyright
- Fully responsive design — 60%+ of LATAM B2B browsing is mobile

**Should have (competitive differentiators):**
- WhatsApp floating CTA button — THE primary B2B communication channel in LATAM; outperforms contact forms for first contact
- Values section (Calidad, Respeto, Excelencia, Pasion) — reinforces brand identity
- Corporate purpose/vision section — positions beyond product listing
- Product sub-details (expandable accordion per category card) — buyers confirm specific variants without calling

**Defer to v2+:**
- Social proof / client logos — high impact but requires client to supply logos and permissions
- PDF downloadable product catalog — requires separate design effort
- Value chain visual / flow diagram — narrative text in Quienes Somos covers this adequately at launch
- Blog/news section — only if company commits to content cadence; empty blog hurts credibility
- English language — only if international expansion warrants it

**Anti-features (do not build):** E-commerce/ordering, CMS/backend admin, separate product pages, dark mode, live chat/chatbot, interactive map, animations beyond AOS.

### Architecture Approach

The site is a single-page composition: one `index.astro` file calls 11 section widget components inside `PageLayout.astro` (which wraps with Header + Footer). All content lives as inline props in index.astro — no CMS, no API, no state management. Navigation uses anchor links (`#section-id`), not multi-page routing. Every section widget must follow the AstroWind pattern: wrap in `WidgetWrapper.astro` (provides scroll offset, padding, dark mode, intersection animation), call `Headline.astro`, then render section content. Breaking this pattern loses scroll offset for the fixed header.

**Major components:**
1. **Foundation layer** (CustomStyles.astro, config.yaml, navigation.ts, Logo.astro, fonts) — every other component depends on this; brand colors, fonts, and nav anchors must be correct before any section work
2. **Shell** (Header.astro + Footer.astro + index.astro skeleton) — establishes page frame; minor adaptation of existing AstroWind widgets
3. **HeroSwiper.astro** (NEW custom) — most complex component; Swiper with 3 slides, overlay gradient, dual CTAs; Swiper init must live in component `<script>` tag with `astro:after-swap` listener
4. **Content sections** (QuienesSomos, Productos, Servicios, Valores, Certificaciones, Proposito) — 3 custom + 3 adapted existing widgets; independent of each other, can be built in any order
5. **Contact section** (Contact.astro form + ContactoInfo.astro info panel) — conversion endpoint; all CTAs point here; requires form backend decision
6. **Polish layer** (AOS data attributes, responsive testing, brand ornaments, Lighthouse audit) — applied last across all components

**Key patterns:**
- Brand theming: CSS variables in CustomStyles.astro → Tailwind config → component classes (never hardcode hex values in components)
- Section backgrounds: alternating white / dark (#25272f) via `isDark` prop and `bg` slot
- Swiper: CSS loaded globally in Layout.astro; JS initialized per-component with `astro:after-swap` listener
- Anchor navigation: section IDs in index.astro match href anchors in navigation.ts; `scroll-mt-[72px]` in WidgetWrapper offsets fixed header

### Critical Pitfalls

1. **DaisyUI v5 + Tailwind v3 incompatibility** — Remove DaisyUI or downgrade to v4 in Phase 1. Silently produces no styles; confirmed incompatibility.
2. **AOS inside Swiper slides causes invisible content** — Intersection Observer conflicts with Swiper's slide positioning. Keep AOS entirely out of the Swiper container. Use Swiper's own animation effects for slide content.
3. **AstroWind default config leaks** — config.yaml ships with site name "AstroWind", English language, blog enabled. Update completely in Phase 1 before touching any sections.
4. **CustomStyles.astro default blue/purple colors** — All CSS variables still point to AstroWind's default scheme. Remap to campivacorp. brand colors (#25272f, #95b444, #cbdc53, #5d6f31) in Phase 1 before any section work.
5. **Swiper initialization in static build** — Swiper `<script>` tag (not frontmatter import, not `is:inline`). Test with `npm run build && npm run preview` — dev works, prod may break.
6. **Contact form with no backend** — Static site has no form handler. Decide on Formspree / Netlify Forms before Phase 5; form silently fails without this.
7. **Three overlapping UI libraries** — Tailwind + DaisyUI + Preline create class conflicts. Resolution: Tailwind utilities for styling + Preline for interactive JS behaviors only. Remove DaisyUI.

---

## Implications for Roadmap

Based on research, the architecture file provides a tested 6-phase build order. The dependency chain is rigid at the top (Foundation must precede everything) and flexible in the middle (content sections are independent). The roadmap should mirror this structure.

### Phase 1: Foundation
**Rationale:** Seven of 13 pitfalls strike here. Every component reads CSS variables, fonts, and navigation data — wrong defaults cascade silently into all subsequent phases. This phase has zero visible output but is the highest-risk phase.
**Delivers:** Correct brand colors, fonts, site config, anchor navigation, campivacorp. logo, blog disabled, dark mode removed, DaisyUI resolved.
**Addresses:** config.yaml, CustomStyles.astro, navigation.ts, Logo.astro, font replacement
**Avoids:** Pitfalls #1 (DaisyUI), #3 (config leaks), #4 (default colors), #5 (Inter font), #7 (overlapping UI libs), #10 (dark mode), #13 (blog routes)

### Phase 2: Shell
**Rationale:** Header and Footer establish the page frame. The fixed navbar must exist before section scroll-offset behavior can be tested. index.astro skeleton (even with placeholder divs) lets all subsequent sections be dropped in incrementally.
**Delivers:** Working fixed navbar with campivacorp. logo and anchor links; footer with social icons; page skeleton.
**Addresses:** Header.astro (isSticky, remove theme toggle), Footer.astro (footerData), index.astro (skeleton composition)
**Avoids:** Pitfall #9 (navigation anchor link behavior, mobile menu close)

### Phase 3: Hero + Stats (Above the Fold)
**Rationale:** First impression sections. Hero is the most complex custom component (Swiper integration) and should be built and tested in isolation before content sections layer in below it. Stats sits immediately below Hero.
**Delivers:** Swiper slider with 3 branded slides, overlay gradient, dual CTAs; animated scroll-triggered stat counters.
**Addresses:** HeroSwiper.astro (NEW), Stats.astro (counter JS addition)
**Avoids:** Pitfall #2 (AOS inside Swiper), #6 (Swiper static build production test)

### Phase 4: Content Sections (Body)
**Rationale:** Six sections (QuienesSomos, Productos, Servicios, Valores, Certificaciones, Proposito) are fully independent of each other. They can be built in parallel or in page-scroll order. All follow the same WidgetWrapper pattern, so execution is predictable.
**Delivers:** Complete page body — about, products with sub-details, services, values, certifications, corporate purpose.
**Addresses:** Must-have table stakes + high-impact differentiators from FEATURES.md
**Avoids:** Pitfall #12 (placeholder images with fixed aspect ratios + object-fit: cover)

### Phase 5: Conversion Section
**Rationale:** Contact is the conversion endpoint that all CTAs point to. Form backend must be decided (Formspree / Netlify Forms) before this phase. Build last so the destination is stable when CTAs are tested end-to-end.
**Delivers:** Contact form wired to email backend, contact info panel (email/phone/social), WhatsApp floating button.
**Addresses:** Contact.astro + ContactoInfo.astro (NEW) + WhatsApp CTA (floating, bottom-right, pre-filled message)
**Avoids:** Pitfall #11 (silent form failure — backend must be decided and configured)

### Phase 6: Polish
**Rationale:** Polish is applied after all sections exist so AOS attributes, responsive behavior, and brand ornaments can be reviewed holistically across the full page. Lighthouse audit runs on the complete build.
**Delivers:** AOS data-aos attributes on all custom components, mobile/tablet/desktop responsive verification, brand ornament SVGs (leaf shapes, 5-10% opacity) in strategic sections, Lighthouse performance baseline.
**Avoids:** Pitfall #8 (CLS from AOS — set explicit dimensions, use data-aos-anchor-placement, apply AOS only to decorative elements)

### Phase Ordering Rationale

- Foundation first because CSS variable errors are silent — you won't see wrong colors until you build components, at which point fixing them requires rebuilding
- Shell before sections because the fixed header's 72px offset must exist before scroll-offset testing is meaningful
- Hero before content sections because it is the most complex (Swiper) and must be isolated for production build testing
- Contact last because all CTAs point to it — a broken contact destination degrades the entire site
- Polish last because it requires all sections to exist to apply coherently

### Research Flags

Phases with standard, well-documented patterns (skip research-phase):
- **Phase 1 (Foundation):** Standard Astro config patterns, well-documented @fontsource approach
- **Phase 2 (Shell):** Minor adaptation of existing AstroWind widgets — low uncertainty
- **Phase 4 (Content Sections):** Six independent sections following identical WidgetWrapper pattern — well-understood

Phases that may need closer attention during planning:
- **Phase 3 (Hero):** Swiper + Astro static build interaction is the only genuinely tricky technical point. Needs explicit production build test (`npm run build && npm run preview`) after Hero is built before proceeding to other phases.
- **Phase 5 (Contact):** Form backend provider decision is a business/infrastructure choice that must be made before coding begins. Formspree (free tier: 50 submissions/month) vs Netlify Forms (if deployed on Netlify) vs email API. This is the one external dependency in the entire project.

---

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Stack | HIGH | Based on direct inspection of the actual installed codebase. All versions confirmed. DaisyUI incompatibility is documented and the fix is clear. |
| Features | MEDIUM-HIGH | Based on analysis of 6 competitor sites + B2B design research. Feature prioritization is well-reasoned but content (sub-product lists, certification badge assets) depends on client to supply. |
| Architecture | HIGH | Based on direct inspection of AstroWind template codebase. Component boundaries, data flow, and build order are confirmed against actual file structure. |
| Pitfalls | HIGH | 13 specific pitfalls identified with clear phase mapping and prevention strategies. 7 of 13 are preventable with Foundation phase discipline. |

**Overall confidence:** HIGH

### Gaps to Address

- **Form backend provider:** Must be decided before Phase 5. Options: Formspree, Netlify Forms, email API. Not a technical question — depends on deployment target and submission volume expectations.
- **Product sub-content:** The 7 product category cards can be built, but sub-product accordion content (specific SKUs, variants) requires client to provide the product list per category. Plan for a content-input checkpoint before Phase 4 Productos.
- **Hero slide imagery:** HeroSwiper requires 3 branded images (fields, ports, products). Must be supplied by client or sourced before Phase 3 can be completed.
- **Certification badge assets:** Brands/Certificaciones section needs HACCP, GMP/BPM, ISO badge SVGs or PNGs. Client must supply official certification images.
- **WhatsApp business number:** Floating CTA requires the configured WhatsApp business number for the `wa.me` URL. Confirm with client before Phase 5.

---

## Sources

### Primary (HIGH confidence)
- Direct inspection of AstroWind template codebase in project directory — architecture patterns, component boundaries, data flow
- Direct inspection of installed `package.json` — all versions and compatibility issues confirmed

### Secondary (MEDIUM confidence)
- Camposol (camposol.com) — Peruvian agroindustrial, multi-section corporate reference
- ECOM Trading (ecomtrading.com) — Global commodity trader, modern corporate design reference
- Oleaginosa Raatz (raatz.com.py) — Paraguayan oil seed processor, regional competitor reference
- Nutrioil Bolivia (nutrioil.com.bo) — Bolivian agroindustrial, direct local competitor reference
- LA Trading Corp (latradingscorp.com) — Latin America trading company reference
- B2B Website Design Best Practices 2025 (trajectorywebdesign.com) — B2B design patterns

### Tertiary (informed inference)
- Swiper 12.x module import pattern — standard practice, needs production build test to confirm
- Form backend options (Formspree / Netlify Forms) — standard static site patterns, specific choice depends on deployment target

---
*Research completed: 2026-03-15*
*Ready for roadmap: yes*
