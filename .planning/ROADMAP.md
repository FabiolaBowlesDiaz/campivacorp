# Roadmap: campivacorp. Landing Page

## Overview

Build a single-page corporate landing page for campivacorp., a Bolivian agroindustrial trading company. The site progresses from invisible brand foundation (colors, fonts, config) through page frame and first-impression sections, into the full content body, and finally conversion endpoints with site-wide polish. Every phase delivers a verifiable capability; the final result is a responsive, brand-accurate static site that drives B2B inquiries.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3, 4): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [x] **Phase 1: Foundation** - Brand theming, fonts, config, SVG assets, DaisyUI fix -- zero visible output, everything depends on it
- [x] **Phase 2: Page Shell + Above the Fold** - Navbar, hero slider, stats counters -- the first thing visitors see
- [ ] **Phase 3: Content Sections** - About, products, services, values, certifications, corporate purpose -- the full page body
- [ ] **Phase 4: Conversion + Polish** - Contact form, footer, WhatsApp CTA, responsive behavior, animations -- the site ships

## Phase Details

### Phase 1: Foundation
**Goal**: The Astro project renders with campivacorp. brand identity -- correct colors, fonts, logo, and config -- so every subsequent component inherits the right visual system
**Depends on**: Nothing (first phase)
**Requirements**: FOUN-01, FOUN-02, FOUN-03, FOUN-04, FOUN-05, FOUN-06, FOUN-07
**Success Criteria** (what must be TRUE):
  1. Running `npm run dev` shows the site with campivacorp. green palette (#25272f, #95b444, #cbdc53, #5d6f31) -- no AstroWind blue/purple remnants visible anywhere
  2. All headings render in Nunito Sans 800 and body text in Montserrat 500/700 -- Inter font is completely gone
  3. The campivacorp. SVG isotipo (3 overlapping leaves) renders correctly in at least one place (e.g., a test component or the default page)
  4. Blog routes return 404 or are removed, site title shows "campivacorp." in the browser tab, and no DaisyUI class conflicts appear in the console
**Plans:** 2/2 plans complete

Plans:
- [x] 01-01-PLAN.md -- Remove DaisyUI, install brand fonts, rewrite CSS variables and config.yaml, delete blog routes
- [x] 01-02-PLAN.md -- Create SVG isotipo Logo.astro and BrandOrnament.astro decorative component

### Phase 2: Page Shell + Above the Fold
**Goal**: A visitor landing on the site sees a professional fixed navbar with the campivacorp. logo, a full-screen hero slider with three branded slides, and animated stat counters -- the complete first impression
**Depends on**: Phase 1
**Requirements**: NAV-01, NAV-02, NAV-03, NAV-04, NAV-05, HERO-01, HERO-02, HERO-03, HERO-04, HERO-05, HERO-06, STAT-01, STAT-02, STAT-03
**Success Criteria** (what must be TRUE):
  1. The navbar is fixed at the top, displays the campivacorp. isotipo + wordmark, section links scroll smoothly to anchors, the "Contactanos" CTA button is visible, and the navbar transitions from transparent to solid on scroll
  2. The hero section shows a Swiper slider cycling through 3 slides with overlay, each with headline text and dual CTAs ("Ver Productos" + "Contactar"), with autoplay, pagination dots, and desktop navigation arrows
  3. The mobile hamburger menu opens, shows all section links, and closes after a link is tapped
  4. Scrolling past the hero reveals 4 animated stat counters (25+ anos, 7 categorias, mercados regionales, 5 certificaciones) that count up from zero when entering the viewport
  5. The page works in production build (`npm run build && npm run preview`) -- Swiper initializes correctly, not just in dev mode
**Plans:** 3/3 plans complete

Plans:
- [x] 02-01-PLAN.md -- Rewrite navigation.ts with flat anchor links, clean PageLayout.astro, add Swiper fade CSS
- [x] 02-02-PLAN.md -- Create HeroSlider.astro (Swiper) and StatsCounter.astro (IntersectionObserver) components
- [x] 02-03-PLAN.md -- Rewrite index.astro as single-page layout, visual verification checkpoint

### Phase 3: Content Sections
**Goal**: The full page body is populated -- a visitor can scroll through About, Products, Services, Values, Certifications, and Corporate Purpose sections, understanding what campivacorp. does, what they sell, and why they are trustworthy
**Depends on**: Phase 2
**Requirements**: ABOU-01, ABOU-02, ABOU-03, PROD-01, PROD-02, PROD-03, PROD-04, SERV-01, SERV-02, VALU-01, VALU-02, CERT-01, CERT-02, PURP-01, PURP-02
**Success Criteria** (what must be TRUE):
  1. The "Quienes Somos" section displays the exact corporate text from the brief with a placeholder image and brand ornament leaf shapes at low opacity in the background
  2. The Products section shows 7 category cards (Aceites, Harinas y Tortas, Granos, Endulzantes, Grasas, Derivados Forestales, Hidrocarburos) each with an SVG icon, and expanding a card reveals the specific product list within that category
  3. The Services section displays all 7 services (Trading, Brokeraje, Logistica, Analitica de mercados, Asesoramiento, Maquilas, Analisis de laboratorio) with icons and descriptions
  4. Values (Calidad, Respeto, Excelencia, Pasion) and Certifications (HACCP, GMP/BPM, ISO 9001, ISO 22000, ISO 14001) sections are visible with appropriate icons/badges
  5. The Corporate Purpose section displays exact text from the brief on a dark (#25272f) background for visual contrast
**Plans**: TBD

Plans:
- [ ] 03-01: TBD
- [ ] 03-02: TBD
- [ ] 03-03: TBD

### Phase 4: Conversion + Polish
**Goal**: The site is complete and shippable -- all CTAs lead to a working contact form, the footer closes the page, WhatsApp is one tap away, and the entire site is responsive with professional scroll animations
**Depends on**: Phase 3
**Requirements**: CONT-01, CONT-02, CONT-03, CONT-04, CONT-05, FOOT-01, FOOT-02, FOOT-03, FOOT-04, RESP-01, RESP-02, RESP-03, ANIM-01, ANIM-02, ANIM-03
**Success Criteria** (what must be TRUE):
  1. The contact section has a working form (name, email, company, message) wired to a form backend (Formspree/Netlify Forms/mailto fallback), plus contact info (email, phone) and social media links displayed alongside
  2. A WhatsApp floating button is persistently visible at bottom-right across all sections and opens a pre-filled WhatsApp message on tap
  3. The footer displays campivacorp. brand (isotipo + wordmark), navigation links mirroring the navbar, social media icons, and copyright with current year
  4. All sections are responsive: product grid adapts columns (1/2/3-4 across mobile/tablet/desktop), hero text and CTAs scale for mobile, and no horizontal overflow or broken layouts at 320px width
  5. AOS fade-up animations trigger on scroll for cards, stats, and text blocks across all sections, with no AOS inside the Swiper container, and all animations are subtle and professional
**Plans**: TBD

Plans:
- [ ] 04-01: TBD
- [ ] 04-02: TBD
- [ ] 04-03: TBD

## Progress

**Execution Order:**
Phases execute in numeric order: 1 -> 2 -> 3 -> 4

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Foundation | 2/2 | Complete    | 2026-03-16 |
| 2. Page Shell + Above the Fold | 3/3 | Complete    | 2026-03-16 |
| 3. Content Sections | 0/3 | Not started | - |
| 4. Conversion + Polish | 0/3 | Not started | - |
