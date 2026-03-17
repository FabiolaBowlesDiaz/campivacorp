# Phase 4: Conversion + Polish - Context

**Gathered:** 2026-03-17
**Status:** Ready for planning

<domain>
## Phase Boundary

The site is complete and shippable — all CTAs lead to a working contact form, the footer closes the page, WhatsApp is one tap away, and the entire site is responsive with professional scroll animations. This is the FINAL phase.

</domain>

<decisions>
## Implementation Decisions

### Contact Form (CONT-01, CONT-02, CONT-03, CONT-05)
- Form fields: name, email, company, message (all required except company)
- Backend: Formspree with placeholder endpoint (`https://formspree.io/f/YOUR_ID`)
  - Comment in code explaining how to replace with real Formspree endpoint
  - Form uses standard HTML form action (no JS fetch needed for Formspree)
- Contact info displayed alongside form:
  - Email: rcampbell@campivacorp.com
  - Phone: +59169006424
  - LinkedIn: https://www.linkedin.com/company/campivacorp/
- Section on white background with two-column layout (form left, info right)

### WhatsApp Floating CTA (CONT-04)
- Fixed bottom-right position, persistent across all sections (z-50)
- Green circle (#95b444) with WhatsApp icon (white)
- Number: +59169006424
- Pre-filled message: "Hola, me interesa conocer más sobre los productos de campivacorp."
- Link format: `https://wa.me/59169006424?text=Hola%2C%20me%20interesa%20conocer%20m%C3%A1s%20sobre%20los%20productos%20de%20campivacorp.`
- Subtle hover animation (slight scale + shadow)

### Social Media Links (CONT-03, FOOT-03)
- Only LinkedIn has a real URL: https://www.linkedin.com/company/campivacorp/
- WhatsApp links to wa.me with the same number
- Facebook and Instagram: omit entirely (no accounts exist) — do NOT use placeholder # links
- Footer social icons: LinkedIn + WhatsApp only

### Footer (FOOT-01, FOOT-02, FOOT-03, FOOT-04)
- Dark background #25272f (decided Phase 1)
- campivacorp. brand: Logo PNG (reuse from navbar) + company description line
- Navigation links mirroring navbar sections (Nosotros, Productos, Servicios, Valores, Certificaciones, Contacto)
- Social icons: LinkedIn + WhatsApp only
- Contact info: email + phone
- Copyright: "© 2026 campivacorp. Todos los derechos reservados."
- Update footerData in navigation.ts with real data

### Responsive (RESP-01, RESP-02, RESP-03)
- Test and fix all sections across 320px (mobile), 768px (tablet), 1024px (desktop)
- Product grid: 1-col mobile, 2-col tablet, 3-col desktop (already set, verify)
- Hero: text scales down, CTAs stack on mobile (already set, verify)
- Footer: stack columns on mobile
- No horizontal overflow at any breakpoint

### AOS Animations (ANIM-01, ANIM-02, ANIM-03)
- Add `data-aos="fade-up"` to section content containers
- Stagger delays on grid items (data-aos-delay="100", "200", etc.)
- AOS NOT inside Swiper container (decided Phase 1 — already clean)
- Subtle: only fade-up, no flip/zoom/bounce
- `data-aos-once="true"` on all elements (fire once only)
- Test with prefers-reduced-motion (AOS respects this by default)

### Claude's Discretion
- Exact form styling (input borders, focus states, button style)
- WhatsApp button exact size and position offset
- AOS delay values for grid items
- Footer column layout and spacing
- Whether to add a "back to top" button
- Contact section layout details

</decisions>

<code_context>
## Existing Code Insights

### Reusable Assets
- `Logo.astro` — PNG logo, reuse in footer
- `Button.astro` — CTA button component
- `WidgetWrapper.astro` — section wrapper
- `navigation.ts` — footerData needs updating with real links/social/footNote

### Established Patterns
- index.astro has #contacto placeholder section
- Footer.astro widget exists, consumes footerData from navigation.ts
- AOS already initialized in Layout.astro with astro:after-swap re-init
- WhatsApp button should go in Layout.astro (persistent across pages)

### Integration Points
- navigation.ts footerData → Footer.astro widget
- Contact section replaces #contacto placeholder in index.astro
- WhatsApp button in Layout.astro (before </body>)
- AOS attributes added to existing section components

</code_context>

<specifics>
## Specific Ideas

- Contact form should feel inviting, not corporate-cold — "Hablemos" or "Contáctanos" as section title
- WhatsApp button is THE primary conversion channel for LATAM B2B (from research)
- Footer should feel complete but not cluttered — minimal, clean, authoritative
- AOS animations should be barely noticeable — the user should feel "this site is smooth" not "this site is animated"

</specifics>

<deferred>
## Deferred Ideas

- SEO JSON-LD schema markup (Organization, WebSite)
- Favicon/OG image generation (web-asset-generator skill)
- Google Analytics integration
- Cookie consent banner
- Performance audit (Lighthouse)

</deferred>

---

*Phase: 04-conversion-polish*
*Context gathered: 2026-03-17*
