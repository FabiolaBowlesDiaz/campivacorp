# Phase 1: Foundation - Context

**Gathered:** 2026-03-15
**Status:** Ready for planning

<domain>
## Phase Boundary

The Astro project renders with campivacorp. brand identity -- correct colors, fonts, logo, and config -- so every subsequent component inherits the right visual system. Zero visible output to the end user, but everything in Phases 2-4 depends on this being right.

</domain>

<decisions>
## Implementation Decisions

### DaisyUI Handling
- Remove DaisyUI completely (`npm uninstall daisyui`)
- Remove from tailwind.config.js plugins and daisyui config block
- Preline UI stays for interactive components (accordions, dropdowns, mobile menu)
- All styling via Tailwind utilities only

### Typography
- Install via @fontsource (self-hosted, zero FOUT, no external CDN)
- `@fontsource/nunito-sans` weight 800 for headings and logo
- `@fontsource/montserrat` weights 500 and 700 for body text
- Remove `@fontsource-variable/inter` completely
- Update CSS variables: `--aw-font-heading: 'Nunito Sans'`, `--aw-font-sans: 'Montserrat'`

### SVG Isotipo
- User has image reference of the exact logo (screenshot provided)
- 3 organic leaves: one central vertical (darker #5d6f31) + two lateral symmetric (#95b444 gradient)
- Rounded edges, dark border (#25272f), small stem/union at the base
- Build as SVG inline, replicating the reference image faithfully
- Wordmark: "campiva" in bold + "corp." in regular weight, both #25272f, punto same color

### Brand Colors (CSS Variables)
- `--aw-color-primary`: #95b444 (green principal)
- `--aw-color-secondary`: #5d6f31 (green oscuro)
- `--aw-color-accent`: #cbdc53 (green lima)
- `--aw-color-text-heading`: #25272f (carbon oscuro)
- `--aw-color-text-default`: #25272f
- `--aw-color-text-muted`: #25272f at 66% opacity
- `--aw-color-bg-page`: #ffffff
- Selection color: #cbdc53 (green lima)
- Remove dark mode theme block entirely from CustomStyles.astro

### Brand Ornaments
- 1-2 large leaf shapes (300-500px) from the isotipo, placed in section corners
- Opacity 5-8%, subtle and elegant
- Applied only to sections with white background
- NOT on dark (#25272f) background sections
- Create as reusable SVG component for consistency

### Dark Section Strategy
- 4 sections use #25272f background: Stats, Certificaciones, Proposito Corporativo, Footer
- All other sections use white (#ffffff) background
- Text on dark sections: pure white #ffffff only (no green accents on dark bg)
- Dark mode toggle disabled: set `ui.theme: 'light:only'` in config.yaml
- Remove `.dark` CSS block from CustomStyles.astro entirely

### Blog Routes
- Desactivar blog en config.yaml (`blog.isEnabled: false`)
- AND borrar archivos de blog en src/pages/[...blog]/ directory
- Maxima limpieza -- no ghost routes

### Config.yaml Updates
- site.name: "campivacorp."
- metadata.title.default: "campivacorp."
- metadata.title.template: "%s -- campivacorp."
- metadata.description: Updated to Spanish corporate description
- i18n.language: "es"
- ui.theme: "light:only"
- apps.blog.isEnabled: false
- Remove Google Analytics and Twitter metadata

### Claude's Discretion
- Exact SVG path coordinates for isotipo (replicating reference image)
- Button styling approach (custom Tailwind classes vs component)
- How to structure the ornament SVG component
- Whether to remove AstroWind demo pages (about, pricing, services, etc.) now or in later phases

</decisions>

<code_context>
## Existing Code Insights

### Files to Modify
- `src/components/CustomStyles.astro` — Font imports + CSS variables (FULL rewrite)
- `src/config.yaml` — Site metadata, language, blog, theme (FULL rewrite)
- `tailwind.config.js` — Remove DaisyUI plugin + config block
- `src/layouts/Layout.astro` — Update font imports from Inter to Nunito Sans + Montserrat
- `package.json` — Remove daisyui, add @fontsource packages

### Files to Create
- `src/components/Logo.astro` — SVG isotipo + wordmark (replaces default AstroWind logo)
- `src/components/ui/BrandOrnament.astro` — Reusable leaf shape background decoration

### Files to Delete
- `src/pages/[...blog]/` directory — All blog route files
- Any blog-related content collections

### Established Patterns
- AstroWind uses CSS variables in CustomStyles.astro that flow to tailwind.config.js mappings
- Font loading via @fontsource imports in component frontmatter
- Logo component is imported in Header.astro widget
- WidgetWrapper.astro provides section padding, scroll offsets, and intersection animations

### Integration Points
- Header.astro imports Logo component -- new Logo.astro must match the interface
- CustomStyles.astro is imported in Layout.astro -- variables cascade to all components
- tailwind.config.js references CSS variables for color and font classes

</code_context>

<specifics>
## Specific Ideas

- Logo reference image provided: 3 organic leaves with gradient green, "campiva" bold + "corp." regular, punto in same color as text
- References de estilo visual: camposol.com, raatz.com.py, mimpexgroup.com
- Corporativo premium, serio, confiable -- no "AI generico"
- NO usar: gradientes morados, azules corporativos genericos, sombras muy oscuras

</specifics>

<deferred>
## Deferred Ideas

- Eliminar paginas demo de AstroWind (about, pricing, services, homes, landing) -- puede ser en Phase 2 cuando se reescriba index.astro
- SEO schema markup (Organization, WebSite JSON-LD) -- Phase 4 polish

</deferred>

---

*Phase: 01-foundation*
*Context gathered: 2026-03-15*
