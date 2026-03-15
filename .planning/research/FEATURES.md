# Feature Landscape

**Domain:** B2B Agroindustrial / Commodity Trading Corporate Landing Page (Latin America)
**Researched:** 2026-03-15
**Overall confidence:** MEDIUM-HIGH

## Context

campivacorp. is a Bolivian agroindustrial trading company (25+ years, oils/flours/grains/sweeteners/fats/forestry/hydrocarbons). The site is a single-page corporate landing page built on Astro + Tailwind, targeting B2B clients in agro/food industries. This is NOT a marketplace or e-commerce platform -- it is a credibility vehicle that drives inquiries via direct contact.

Competitor reference sites analyzed: Camposol (Peru), Oleaginosa Raatz (Paraguay), ECOM Trading (global), Nutrioil Bolivia, Cargill/Bunge/ADM (global ABCD traders), LA Trading Corp, and regional Bolivian agroindustrial companies.

---

## Table Stakes

Features users expect. Missing = site feels incomplete or untrustworthy for B2B agro.

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| **Fixed navbar with logo + nav + CTA** | Every corporate site has this. Visitors need persistent navigation. Competitors (Camposol, Raatz, ECOM) all use sticky headers. | Low | Already scoped. SVG isotipo + wordmark is correct approach. |
| **Hero section with strong value proposition** | B2B visitors must understand what you do in 5 seconds. Agro trading sites universally lead with hero imagery (fields, ports, products) + clear positioning statement. | Low-Med | Swiper slider with 3 slides is appropriate. Ensure each slide has a distinct message (products, experience, global reach). |
| **Quienes Somos / About** | Trust-critical for B2B. Every competitor site has a company background section. 25+ years of experience is a powerful differentiator that must be front and center. | Low | Already scoped. Key: emphasize longevity and track record, not just text. |
| **Product portfolio display** | Core of what the company sells. B2B buyers need to quickly see if you handle what they need. Competitors display product categories with clear visual hierarchy. | Med | 7 category cards with icons is correct. Each card should list sub-products (e.g., under "Aceites": Crudo de Soya, Crudo de Girasol, etc.). |
| **Services overview** | Differentiates a trader from a simple product listing. Trading, brokerage, logistics, analytics -- these are value-adds B2B clients evaluate. | Low | Icon + description grid. Keep descriptions concise and outcome-focused. |
| **Certifications section** | HACCP, GMP/BPM, ISO 9001/22000/14001 are deal-breakers for B2B food/agro. Buyers will look for these explicitly. Missing = immediate disqualification for many clients. | Low | Banner format with recognizable badge/logo icons. Must be visually prominent, not buried. |
| **Contact section with form** | B2B sites live and die by inquiry generation. Every competitor has a contact form + direct info (email, phone). WhatsApp is critical in Latin American B2B -- many deals start via WhatsApp. | Med | Form + email + phone + WhatsApp link. WhatsApp must be prominent (not just a social icon). |
| **Footer with essential info** | Brand, navigation links, social links, copyright. Standard across all corporate sites. | Low | Already scoped. |
| **Responsive design** | 60%+ of Latin American B2B browsing is mobile. Agro buyers often check sites from phones in the field. | Med | Already scoped as constraint. |
| **Spanish language** | Primary market is Bolivia/Latin America. English is secondary. | Low | Already scoped. All content in Spanish. |
| **Stats / numbers section** | Agro trading companies universally display key metrics (years of experience, tons processed, markets served, certifications). Quantifiable claims build trust faster than narrative text. | Low | Animated counters already scoped: 25+ years, 7 categories, regional markets, 5 certifications. |

---

## Differentiators

Features that set campivacorp. apart. Not expected, but create competitive advantage.

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| **WhatsApp floating CTA button** | In Latin American B2B, WhatsApp is THE primary business communication channel. A persistent floating WhatsApp button dramatically lowers friction to first contact. Competitors that lack this lose inquiries. | Low | Fixed position bottom-right. Links to wa.me with pre-filled message. Higher conversion than contact forms in LATAM. |
| **Product sub-detail expandable cards** | Most agro competitors show only category names. Showing specific product variants (Crudo de Soya, Refinado de Girasol, etc.) lets buyers immediately confirm availability without needing to call. Saves time for both parties. | Med | Accordion or modal per category card. Avoids need for separate product detail pages (out of scope). |
| **Value chain narrative section** | campivacorp.'s corporate text emphasizes monitoring "all participants of the value chain from field production to industrial transformation, storage, and logistics." Visualizing this as a simple flow or illustrated narrative (farm > processing > storage > logistics > client) differentiates from competitors that just list products. | Med | Could be a horizontal icon-flow diagram or illustrated section. NOT a complex interactive visualization -- a clean, branded static infographic. |
| **Scroll animations (AOS)** | Subtle entrance animations on scroll signal modern, professional web presence. Most regional agro competitors have static, dated sites (Raatz, Nutrioil). This alone creates a premium perception gap. | Low | Already scoped. AOS is initialized. Key: subtle, not distracting. Fade-up on cards, counters animate on scroll. |
| **Brand ornament system** | Organic leaf shapes at 5-10% opacity as section backgrounds. Regional competitors use generic corporate templates. campivacorp.'s custom brand elements (leaves, green gradients) create distinctive visual identity that signals investment in brand = investment in quality. | Med | Already part of brand book. Apply consistently across sections. |
| **Dual-CTA hero strategy** | Primary CTA ("Contactanos") + Secondary CTA ("Ver Productos") gives visitors two conversion paths. Most competitor sites have a single or no CTA on hero. | Low | Already scoped. Primary button (filled green) + secondary (outlined). |
| **Social proof / client logos** | If campivacorp. serves recognizable companies, showing their logos dramatically increases trust. Even a "Confian en nosotros" strip with 4-6 logos is powerful. | Low | Not currently scoped but HIGH impact, LOW effort if logos are available. Consider adding. |
| **PDF downloadable product sheet** | B2B buyers often need to share product info internally for purchasing decisions. A downloadable PDF product catalog (even a single page per category) enables offline decision-making. | Med | Optional enhancement. Could be a single "Descargar Catalogo" CTA linking to a PDF. Requires design effort for the PDF itself. |

---

## Anti-Features

Features to explicitly NOT build. These waste effort or hurt the project.

| Anti-Feature | Why Avoid | What to Do Instead |
|--------------|-----------|-------------------|
| **E-commerce / online ordering** | campivacorp. operates B2B via negotiated contracts, custom pricing, and direct relationships. Adding a cart/pricing system creates false expectations and adds massive complexity. No agro commodity trader at this scale sells through a website cart. | Contact form + WhatsApp + phone. All roads lead to a conversation, not a transaction. |
| **Blog / news section** | Content marketing requires ongoing effort to maintain. An empty or stale blog signals abandonment and hurts credibility MORE than having no blog. Initial launch should not include this. | Out of scope. Can be added later IF the company commits to regular content (market reports, etc.). |
| **Multi-language (English)** | Correctly deferred. Adding English doubles content management burden. Primary market is Spanish-speaking LATAM. International buyers in this industry expect to communicate in Spanish for Bolivia-based suppliers. | Spanish only. English can be Phase 2 if international expansion warrants it. |
| **CMS / backend admin** | Static site with Astro is correct. A CMS adds hosting complexity, security surface, and maintenance burden for a site whose content changes rarely (product list, company info). | Content managed in code. Updates handled by developer. |
| **Product detail sub-pages** | Single-page landing is the right call. Separate pages for each of 7 categories creates navigation overhead and SEO complexity that is not justified for a corporate presence site. | Expandable cards or modals on the single page. |
| **Dark mode** | Brand is light-focused with strategic dark (#25272f) accent sections. Dark mode adds design complexity with zero B2B conversion benefit. Agro buyers do not choose trading partners based on dark mode support. | Light theme only with branded dark sections as designed. |
| **Live chat / chatbot** | Adds JavaScript bloat, requires someone to monitor it, and creates expectation of instant response. B2B agro inquiries are not urgent enough to warrant this. WhatsApp serves this role better. | WhatsApp floating button covers real-time communication needs. |
| **Interactive map** | Often suggested for companies with "regional markets" but adds Google Maps API dependency, performance cost, and limited actual value. B2B buyers do not choose suppliers based on a map pin. | Mention markets served in text. If needed later, a simple static SVG map of coverage areas. |
| **Animations beyond AOS** | Lottie, GSAP, parallax, WebGL -- all add complexity and performance cost. The brand is "corporate premium, serious, trustworthy" not "creative agency showcase." | Stick with AOS fade/slide animations. Animated stat counters. Nothing more. |
| **User accounts / login** | No B2B portal needed for a corporate landing page. No client area, no document portal, no order tracking. | Direct contact for all business interactions. |

---

## Feature Dependencies

```
Navbar ─────────────────────────────────────────────> All sections (anchor links)
Hero (Swiper) ──────────────────────────────────────> Brand assets (logos, images)
Stats counters ─────────────────────────────────────> AOS library (triggers on scroll)
Product cards ──────> Product sub-details (optional expandable/accordion)
Contact form ───────> Form handling (Astro form action, Netlify Forms, or similar)
WhatsApp CTA ───────> WhatsApp business number configured
Certifications ─────> Certification badge SVGs/images
Footer social links > Social media accounts active
```

Key dependency chain:
1. **Brand assets first** -- Navbar, Hero, and Certifications all need finalized SVG logos/icons before they can be completed
2. **AOS initialization** (already done in Layout.astro) must precede all animated sections
3. **Contact form backend** needs a destination -- Astro static forms need a handler (Netlify Forms, Formspree, or email API)
4. **Product content** must be finalized before product cards can have expandable sub-details

---

## MVP Recommendation

### Must ship (Table Stakes):

1. **Navbar** -- fixed, branded, with anchor links to sections
2. **Hero** -- Swiper slider, 3 slides, overlay text, dual CTAs
3. **Stats counters** -- animated numbers on scroll (25+ years, 7 categories, markets, 5 certs)
4. **Quienes Somos** -- corporate text with brand ornaments
5. **Products grid** -- 7 category cards with SVG icons and sub-product lists visible
6. **Services section** -- icon + description for each service
7. **Certifications banner** -- HACCP, GMP/BPM, ISO badges prominently displayed
8. **Contact section** -- form + email + phone + WhatsApp
9. **Footer** -- brand, nav links, social icons, copyright
10. **Responsive design** -- mobile, tablet, desktop

### Should ship (High-impact differentiators):

11. **WhatsApp floating button** -- persistent, bottom-right, opens WhatsApp with pre-filled message
12. **Values section** -- 4 value cards (Calidad, Respeto, Excelencia, Pasion)
13. **Corporate purpose/vision section** -- reinforces positioning
14. **Product sub-details** -- expandable/accordion within each category card

### Defer:

- **Social proof / client logos** -- requires client to provide logos and permissions. Add when available.
- **PDF product catalog** -- requires separate design effort. Not critical for launch.
- **Value chain visual** -- nice to have, but narrative text in Quienes Somos covers this adequately for launch.
- **Blog/news** -- only if company commits to content cadence.
- **English language** -- Phase 2 if international expansion happens.

---

## Competitive Landscape Summary

| Feature | campivacorp. (planned) | Camposol | Raatz | ECOM Trading | Nutrioil Bolivia |
|---------|----------------------|----------|-------|-------------|-----------------|
| Modern design | YES (Astro + Tailwind) | YES | Dated | YES | Dated |
| Responsive | YES | YES | Partial | YES | Partial |
| Product portfolio | YES (7 categories) | YES (crops) | YES (oils/flours) | YES (coffee/cocoa/cotton) | YES (oil/flour) |
| Certifications | YES (5 certs) | YES | Unknown | YES | YES |
| Sustainability | No | YES (reports) | No | YES (major focus) | No |
| Investor Relations | No (not needed) | YES | No | No | No |
| Contact form | YES | YES | YES | YES | YES |
| WhatsApp CTA | YES (floating) | No | No | No | No |
| Scroll animations | YES (AOS) | Minimal | No | Minimal | No |
| Multi-language | No (deferred) | YES (EN/ES) | No | YES (multi) | No |

campivacorp.'s planned site will be significantly more modern and polished than regional Bolivian/Paraguayan competitors (Raatz, Nutrioil), and competitive with global players (Camposol, ECOM) in visual quality -- while maintaining appropriate scope for a trading company (not a processor or grower).

---

## Sources

- [Camposol Corporate Site](https://www.camposol.com/) -- Peruvian agroindustrial, multi-section corporate site with sustainability focus
- [Oleaginosa Raatz](https://raatz.com.py/) -- Paraguayan oil seed processor, dated but relevant product structure
- [ECOM Trading](https://www.ecomtrading.com/) -- Global commodity trader, modern corporate design
- [Nutrioil Bolivia](https://nutrioil.com.bo/) -- Bolivian agroindustrial, direct local competitor reference
- [LA Trading Corp](https://www.latradingscorp.com/) -- Latin America trading company
- [B2B Website Design Best Practices 2025](https://www.trajectorywebdesign.com/blog/b2b-website-design-best-practices) -- B2B website design patterns
- [Agrix Marketplace](https://agrixmarketplace.com/) -- B2B agro platform communication patterns
