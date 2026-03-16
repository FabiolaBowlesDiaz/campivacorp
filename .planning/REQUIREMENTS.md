# Requirements: campivacorp. Landing Page

**Defined:** 2026-03-15
**Core Value:** Present campivacorp. as a credible, premium agroindustrial trading partner -- the site must convey trust, expertise, and professionalism through precise brand execution.

## v1 Requirements

### Foundation

- [x] **FOUN-01**: Brand colors applied via CSS variables (#25272f, #95b444, #cbdc53, #5d6f31, #ffffff)
- [x] **FOUN-02**: Typography loaded -- Nunito Sans 800 for headings, Montserrat 500/700 for body
- [x] **FOUN-03**: SVG isotipo created (3 organic overlapping leaves in green gradient)
- [x] **FOUN-04**: config.yaml updated (site name, language es, blog disabled, dark mode disabled)
- [x] **FOUN-05**: DaisyUI v5 removed or downgraded to v4 (Tailwind 3 incompatibility)
- [x] **FOUN-06**: AstroWind blog routes removed or noindexed
- [x] **FOUN-07**: Brand ornament SVG patterns created (leaf shapes at 5-10% opacity)

### Navigation

- [ ] **NAV-01**: Fixed navbar with isotipo SVG + "campiva" bold + "corp." regular wordmark
- [ ] **NAV-02**: Navbar links scroll to page sections (anchor navigation with smooth scroll)
- [ ] **NAV-03**: CTA button "Contactanos" in #95b444 with hover #5d6f31
- [ ] **NAV-04**: Mobile responsive hamburger menu with section links
- [ ] **NAV-05**: Navbar background changes on scroll (transparent to solid)

### Hero

- [x] **HERO-01**: Swiper slider with 3 slides, overlay #25272f at 60% opacity
- [x] **HERO-02**: Slide 1 -- "Soluciones agroindustriales para el mundo" (agricultural fields)
- [x] **HERO-03**: Slide 2 -- "Calidad certificada en cada transaccion" (products)
- [x] **HERO-04**: Slide 3 -- "25 anos conectando mercados" (global commerce)
- [x] **HERO-05**: Dual CTAs on each slide: "Ver Productos" (secondary) + "Contactar" (primary)
- [x] **HERO-06**: Autoplay with pagination dots, navigation arrows on desktop

### Stats

- [x] **STAT-01**: Animated counter section with AOS scroll trigger
- [x] **STAT-02**: 4 stats displayed: 25+ anos, 7 categorias de productos, Mercados regionales, 5 certificaciones
- [x] **STAT-03**: Counter animation counts up from 0 on viewport entry

### About

- [ ] **ABOU-01**: "Quienes Somos" section with exact corporate text from brief
- [ ] **ABOU-02**: Placeholder image (agricultural/industrial theme)
- [ ] **ABOU-03**: Brand ornament leaf shapes in background at low opacity

### Products

- [ ] **PROD-01**: Grid of 7 product category cards (Aceites, Harinas y Tortas, Granos, Endulzantes, Grasas, Derivados Forestales, Hidrocarburos)
- [ ] **PROD-02**: Each card has thematic SVG icon, category name, and list of specific products
- [ ] **PROD-03**: Cards styled with #95b444 border or subtle shadow, hover effect
- [ ] **PROD-04**: Expandable/accordion sub-detail showing all products within each category

### Services

- [ ] **SERV-01**: Services section with icon + title + description for each service
- [ ] **SERV-02**: 7 services displayed: Trading, Brokeraje, Logistica, Analitica de mercados, Asesoramiento, Maquilas, Analisis de laboratorio

### Values

- [ ] **VALU-01**: 4 value cards: Calidad, Respeto, Excelencia, Pasion
- [ ] **VALU-02**: Each card has icon, title, and description text from brief

### Certifications

- [ ] **CERT-01**: Certification banner section prominently displaying HACCP, GMP/BPM, ISO 9001, ISO 22000, ISO 14001
- [ ] **CERT-02**: Badge/shield SVG icons for each certification

### Purpose

- [ ] **PURP-01**: Corporate purpose/vision section with exact text from brief
- [ ] **PURP-02**: Dark background (#25272f) section for visual contrast

### Contact

- [ ] **CONT-01**: Contact form with fields: name, email, company, message
- [ ] **CONT-02**: Contact info displayed: rcampbell@campivacorp.com, +59169006424
- [ ] **CONT-03**: Social media links: Facebook, WhatsApp, Instagram, LinkedIn
- [ ] **CONT-04**: WhatsApp floating CTA button (fixed bottom-right, persistent across all sections)
- [ ] **CONT-05**: Form submission handler (Formspree, Netlify Forms, or mailto: fallback)

### Footer

- [ ] **FOOT-01**: Footer with campivacorp. brand (isotipo + wordmark)
- [ ] **FOOT-02**: Navigation links mirroring navbar sections
- [ ] **FOOT-03**: Social media icons (Facebook, WhatsApp, Instagram, LinkedIn)
- [ ] **FOOT-04**: Copyright notice with current year

### Responsive

- [ ] **RESP-01**: All sections responsive across mobile (320px+), tablet (768px+), desktop (1024px+)
- [ ] **RESP-02**: Product grid adapts (1 col mobile, 2 col tablet, 3-4 col desktop)
- [ ] **RESP-03**: Hero slider adapts text size and CTA layout for mobile

### Animation

- [ ] **ANIM-01**: AOS fade-up animations on section entries (cards, stats, text blocks)
- [ ] **ANIM-02**: AOS NOT used inside Swiper container (conflict prevention)
- [ ] **ANIM-03**: Subtle, professional animations -- no distracting effects

## v2 Requirements

### Social Proof
- **SOCL-01**: Client/partner logo strip ("Confian en nosotros")
- **SOCL-02**: Testimonials from key clients

### Content
- **BLOG-01**: News/market reports blog section
- **PDF-01**: Downloadable PDF product catalog per category

### International
- **I18N-01**: English language version
- **I18N-02**: Language switcher in navbar

### Visual
- **VISU-01**: Value chain infographic (farm to processing to storage to logistics to client)
- **VISU-02**: Interactive coverage map (LATAM markets)

## Out of Scope

| Feature | Reason |
|---------|--------|
| E-commerce / online ordering | B2B operates via negotiated contracts, not cart transactions |
| CMS / backend admin | Static site, content managed in code, rarely changes |
| Product detail sub-pages | Single-page landing with expandable cards covers this |
| Dark mode | Brand is light-focused with strategic dark accent sections |
| Live chat / chatbot | WhatsApp floating button covers real-time communication |
| User accounts / login | No B2B portal needed for corporate landing page |
| Heavy animations (GSAP, Lottie, parallax) | AOS sufficient; brand is "corporate premium" not "creative agency" |
| Interactive map (Google Maps) | Adds API dependency and performance cost with minimal B2B value |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| FOUN-01 | Phase 1 | Complete |
| FOUN-02 | Phase 1 | Complete |
| FOUN-03 | Phase 1 | Complete |
| FOUN-04 | Phase 1 | Complete |
| FOUN-05 | Phase 1 | Complete |
| FOUN-06 | Phase 1 | Complete |
| FOUN-07 | Phase 1 | Complete |
| NAV-01 | Phase 2 | Pending |
| NAV-02 | Phase 2 | Pending |
| NAV-03 | Phase 2 | Pending |
| NAV-04 | Phase 2 | Pending |
| NAV-05 | Phase 2 | Pending |
| HERO-01 | Phase 2 | Complete |
| HERO-02 | Phase 2 | Complete |
| HERO-03 | Phase 2 | Complete |
| HERO-04 | Phase 2 | Complete |
| HERO-05 | Phase 2 | Complete |
| HERO-06 | Phase 2 | Complete |
| STAT-01 | Phase 2 | Complete |
| STAT-02 | Phase 2 | Complete |
| STAT-03 | Phase 2 | Complete |
| ABOU-01 | Phase 3 | Pending |
| ABOU-02 | Phase 3 | Pending |
| ABOU-03 | Phase 3 | Pending |
| PROD-01 | Phase 3 | Pending |
| PROD-02 | Phase 3 | Pending |
| PROD-03 | Phase 3 | Pending |
| PROD-04 | Phase 3 | Pending |
| SERV-01 | Phase 3 | Pending |
| SERV-02 | Phase 3 | Pending |
| VALU-01 | Phase 3 | Pending |
| VALU-02 | Phase 3 | Pending |
| CERT-01 | Phase 3 | Pending |
| CERT-02 | Phase 3 | Pending |
| PURP-01 | Phase 3 | Pending |
| PURP-02 | Phase 3 | Pending |
| CONT-01 | Phase 4 | Pending |
| CONT-02 | Phase 4 | Pending |
| CONT-03 | Phase 4 | Pending |
| CONT-04 | Phase 4 | Pending |
| CONT-05 | Phase 4 | Pending |
| FOOT-01 | Phase 4 | Pending |
| FOOT-02 | Phase 4 | Pending |
| FOOT-03 | Phase 4 | Pending |
| FOOT-04 | Phase 4 | Pending |
| RESP-01 | Phase 4 | Pending |
| RESP-02 | Phase 4 | Pending |
| RESP-03 | Phase 4 | Pending |
| ANIM-01 | Phase 4 | Pending |
| ANIM-02 | Phase 4 | Pending |
| ANIM-03 | Phase 4 | Pending |

**Coverage:**
- v1 requirements: 48 total
- Mapped to phases: 48
- Unmapped: 0

---
*Requirements defined: 2026-03-15*
*Last updated: 2026-03-15 after roadmap creation (4-phase mapping)*
