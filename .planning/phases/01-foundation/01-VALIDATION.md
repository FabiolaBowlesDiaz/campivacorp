---
phase: 1
slug: foundation
created: 2026-03-15
---

# Phase 1: Foundation - Validation Strategy

## Test Framework

| Property | Value |
|----------|-------|
| Framework | Astro check + ESLint + Prettier |
| Quick run | `npm run dev` (visual) + `npm run build` (smoke) |
| Full suite | `npm run check` (astro check + eslint + prettier) |

## Requirements to Test Map

| Req ID | Behavior | Test Type | Validation Method |
|--------|----------|-----------|-------------------|
| FOUN-01 | Brand colors in CSS variables | manual | Visual: no blue/purple, green palette visible |
| FOUN-02 | Fonts loaded (Nunito Sans 800, Montserrat 500/700) | manual | DevTools font tab + visual inspection |
| FOUN-03 | SVG isotipo renders | manual | Logo visible in header/page |
| FOUN-04 | config.yaml correct | smoke | `npm run build` succeeds, title shows "campivacorp." |
| FOUN-05 | DaisyUI removed | smoke | No daisyui in package.json, no console errors |
| FOUN-06 | Blog routes gone | smoke | `npm run build` produces no /blog output |
| FOUN-07 | Ornament component created | unit | Component file exists, renders without error |

## Sampling Rate

- **Per task commit:** `npm run build` (no build errors)
- **Per wave merge:** `npm run check` (full lint + types)
- **Phase gate:** `npm run build` + visual inspection of dev server

## Known Gaps

- No automated visual regression testing -- all color/font/logo verification is manual via `npm run dev`
- Acceptable for foundation phase where visual inspection is primary validation
