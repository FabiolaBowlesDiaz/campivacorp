---
phase: 2
slug: page-shell-above-the-fold
created: 2026-03-16
---

# Phase 2: Page Shell + Above the Fold - Validation Strategy

## Test Framework

| Property | Value |
|----------|-------|
| Framework | Astro check + ESLint + manual visual |
| Quick run | `npm run dev` (visual inspection) |
| Full suite | `npm run build` (no build errors) |

## Requirements to Test Map

| Req ID | Behavior | Test Type | Validation Method |
|--------|----------|-----------|-------------------|
| NAV-01 | Fixed navbar with logo | manual | Visual: navbar visible with PNG logo |
| NAV-02 | Anchor links scroll to sections | manual | Click each nav link, verify smooth scroll |
| NAV-03 | CTA "Contáctanos" styled | manual | Green button in navbar |
| NAV-04 | Mobile hamburger works | manual | Resize to mobile, toggle menu, click link |
| NAV-05 | Navbar bg changes on scroll | manual | Scroll past 60px, white bg + shadow |
| HERO-01 | Swiper 3 slides + overlay | manual | Slides rotate, overlay visible |
| HERO-02 | Slide 1 text correct | manual | "Soluciones agroindustriales para el mundo" |
| HERO-03 | Slide 2 text correct | manual | "Calidad certificada en cada transacción" |
| HERO-04 | Slide 3 text correct | manual | "25 años conectando mercados" |
| HERO-05 | Dual CTAs present | manual | Two buttons on each slide |
| HERO-06 | Autoplay + pagination + arrows | manual | Autoplay ~5s, dots clickable, arrows on desktop |
| STAT-01 | Counter section scroll trigger | manual | Scroll to stats, count-up starts |
| STAT-02 | 4 correct stats | manual | 25+, 7, 5, 100% with correct labels |
| STAT-03 | Count-up animation | manual | Numbers animate from 0 on viewport entry |

## Sampling Rate

- **Per task commit:** `npm run build` (no build errors)
- **Per wave merge:** `npm run build` + visual inspection
- **Phase gate:** Full visual walkthrough desktop + mobile + production build test

## Known Gaps

- No automated visual regression — all verification is manual visual inspection
- `npm run build` is the automated gate (catches import/type errors)
- Acceptable: Phase 2 is predominantly visual UI work
- Production build test (`npm run build && npm run preview`) critical for Swiper
