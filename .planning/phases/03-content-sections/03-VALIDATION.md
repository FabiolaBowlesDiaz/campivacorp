---
phase: 3
slug: content-sections
created: 2026-03-16
---

# Phase 3: Content Sections - Validation Strategy

## Test Framework

| Property | Value |
|----------|-------|
| Framework | Astro check + npm run build + manual visual |
| Quick run | `npm run dev` (visual inspection) |
| Full suite | `npm run build` (no build errors) |

## Requirements to Test Map

| Req ID | Behavior | Test Type | Validation Method |
|--------|----------|-----------|-------------------|
| ABOU-01 | Quiénes Somos with exact corporate text | manual | Visual: text matches brief |
| ABOU-02 | Placeholder image | manual | Visual: image/placeholder visible |
| ABOU-03 | Brand ornament in background | manual | Visual: subtle leaf shapes visible |
| PROD-01 | 7 product category cards | manual | Visual: 7 cards rendered in grid |
| PROD-02 | Each card has icon + name + products | manual | Visual: icon, title, product list |
| PROD-03 | Cards styled with border/shadow + hover | manual | Visual: hover effect on cards |
| PROD-04 | Expandable accordion sub-detail | manual | Click card, sub-products appear |
| SERV-01 | Services with icon + title + description | manual | Visual: 7 services displayed |
| SERV-02 | All 7 services listed | manual | Count services, verify names |
| VALU-01 | 4 value cards | manual | Visual: 4 cards visible |
| VALU-02 | Each has icon + title + description | manual | Visual: content matches brief |
| CERT-01 | 5 certifications on dark bg | manual | Visual: HACCP, GMP, ISO badges |
| CERT-02 | Badge/shield icons | manual | Visual: certification icons |
| PURP-01 | Corporate purpose exact text | manual | Visual: text matches brief |
| PURP-02 | Dark background section | manual | Visual: #25272f bg, white text |

## Sampling Rate

- **Per task commit:** `npm run build`
- **Per wave merge:** Visual inspection of dev server
- **Phase gate:** Full scroll-through of all 6 sections desktop + mobile

## Known Gaps

- All verification is manual visual — no automated content checks
- Acceptable: Phase 3 is content sections, visual inspection is primary
