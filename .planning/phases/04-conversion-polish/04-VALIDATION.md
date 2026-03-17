---
phase: 4
slug: conversion-polish
created: 2026-03-17
---

# Phase 4: Conversion + Polish - Validation Strategy

## Test Framework

| Property | Value |
|----------|-------|
| Framework | npm run build + manual visual + responsive check |
| Quick run | `npm run dev` |
| Full suite | `npm run build && npm run preview` |

## Requirements to Test Map

| Req ID | Behavior | Test Type | Validation Method |
|--------|----------|-----------|-------------------|
| CONT-01 | Contact form with fields | manual | Visual: 4 fields render, submit button |
| CONT-02 | Contact info displayed | manual | Visual: email + phone visible |
| CONT-03 | Social media links | manual | LinkedIn + WhatsApp links work |
| CONT-04 | WhatsApp floating CTA | manual | Button visible bottom-right, opens wa.me |
| CONT-05 | Form submission handler | manual | Submit form, verify Formspree redirect |
| FOOT-01 | Footer with brand | manual | Logo + brand visible in footer |
| FOOT-02 | Footer nav links | manual | 6 section links present, clickable |
| FOOT-03 | Footer social icons | manual | LinkedIn + WhatsApp icons |
| FOOT-04 | Copyright notice | manual | "© 2026 campivacorp." visible |
| RESP-01 | Responsive all sections | manual | Test 320px, 768px, 1024px |
| RESP-02 | Product grid adapts | manual | 1/2/3 col across breakpoints |
| RESP-03 | Hero adapts mobile | manual | Text + CTAs scale on mobile |
| ANIM-01 | AOS fade-up animations | manual | Scroll, elements fade in |
| ANIM-02 | No AOS in Swiper | manual | Hero slides work without AOS conflict |
| ANIM-03 | Subtle animations | manual | Professional, not distracting |

## Sampling Rate

- **Per task commit:** `npm run build`
- **Per wave merge:** Visual inspection desktop + mobile
- **Phase gate:** Full walkthrough + production build + Lighthouse check
