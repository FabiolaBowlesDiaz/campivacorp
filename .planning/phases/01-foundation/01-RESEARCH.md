# Phase 1: Foundation - Research

**Researched:** 2026-03-15
**Domain:** AstroWind template customization (brand colors, typography, SVG logo, config, DaisyUI removal)
**Confidence:** HIGH

## Summary

Phase 1 transforms the stock AstroWind template into the campivacorp. visual identity. The codebase is an AstroWind 5.x template running Astro 5.12, Tailwind CSS 3.4.17, and a broken DaisyUI v5.5.19 installation (DaisyUI v5 requires Tailwind v4, but this project uses Tailwind v3). The foundation work is entirely CSS variables, font imports, config edits, and SVG creation -- no complex logic or API integrations.

The critical risk is DaisyUI v5 + Tailwind v3 incompatibility. DaisyUI v5 was rewritten for Tailwind v4 and will generate console errors or broken styles when paired with Tailwind v3. The decision is to remove DaisyUI entirely (not downgrade), which is the cleanest path. Preline UI (v4.1.2) stays and is compatible with Tailwind v3.

**Primary recommendation:** Remove DaisyUI first (it may be injecting conflicting styles), then rewrite CustomStyles.astro with campivacorp. brand tokens, swap fonts, create Logo.astro SVG, update config.yaml, and delete blog routes.

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions
- DaisyUI: Remove completely (`npm uninstall daisyui`), remove from tailwind.config.js plugins and daisyui config block. Preline UI stays.
- Typography: @fontsource self-hosted. `@fontsource/nunito-sans` weight 800 for headings. `@fontsource/montserrat` weights 500 and 700 for body. Remove `@fontsource-variable/inter` completely. Update CSS variables accordingly.
- SVG Isotipo: 3 organic leaves (central darker #5d6f31 + two lateral #95b444 gradient), rounded edges, dark border #25272f, small stem. Wordmark: "campiva" bold + "corp." regular, both #25272f.
- Brand Colors: --aw-color-primary: #95b444, --aw-color-secondary: #5d6f31, --aw-color-accent: #cbdc53, --aw-color-text-heading: #25272f, --aw-color-text-default: #25272f, --aw-color-text-muted: #25272f at 66%, --aw-color-bg-page: #ffffff. Selection: #cbdc53. Remove dark mode block entirely.
- Brand Ornaments: 1-2 large leaf shapes (300-500px), 5-8% opacity, white sections only. Reusable SVG component.
- Dark Section Strategy: 4 sections use #25272f bg (Stats, Certificaciones, Proposito, Footer). Text on dark: pure white. theme: 'light:only'. Remove .dark CSS block.
- Blog: Disable in config.yaml AND delete src/pages/[...blog]/ directory.
- Config.yaml: site.name "campivacorp.", metadata title "campivacorp.", i18n.language "es", ui.theme "light:only", apps.blog.isEnabled false, remove Google Analytics and Twitter metadata.

### Claude's Discretion
- Exact SVG path coordinates for isotipo (replicating reference image)
- Button styling approach (custom Tailwind classes vs component)
- How to structure the ornament SVG component
- Whether to remove AstroWind demo pages (about, pricing, services, etc.) now or in later phases

### Deferred Ideas (OUT OF SCOPE)
- Eliminar paginas demo de AstroWind (about, pricing, services, homes, landing) -- Phase 2
- SEO schema markup (Organization, WebSite JSON-LD) -- Phase 4
</user_constraints>

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|-----------------|
| FOUN-01 | Brand colors applied via CSS variables (#25272f, #95b444, #cbdc53, #5d6f31, #ffffff) | CustomStyles.astro rewrite -- current file has AstroWind blue/purple. Map hex values to rgb() format matching existing pattern. |
| FOUN-02 | Typography loaded -- Nunito Sans 800 for headings, Montserrat 500/700 for body | @fontsource static packages with per-weight imports. Replace Inter Variable import in CustomStyles.astro frontmatter. |
| FOUN-03 | SVG isotipo created (3 organic overlapping leaves in green gradient) | New Logo.astro component with inline SVG. Must export same interface as current Logo.astro (no props, renders in Header.astro anchor). |
| FOUN-04 | config.yaml updated (site name, language es, blog disabled, dark mode disabled) | Direct edits to src/config.yaml -- all fields identified and mapped. |
| FOUN-05 | DaisyUI v5 removed (Tailwind 3 incompatibility) | npm uninstall + tailwind.config.js plugin/config removal. Confirmed: DaisyUI v5.5.19 requires Tailwind v4, project has Tailwind v3.4.17. |
| FOUN-06 | AstroWind blog routes removed | Delete src/pages/[...blog]/ directory (4 items: index.astro, [...page].astro, [category]/, [tag]/). Blog content collection in src/content/config.ts can stay (harmless). |
| FOUN-07 | Brand ornament SVG patterns created (leaf shapes at 5-10% opacity) | New BrandOrnament.astro component in src/components/ui/. Uses simplified leaf path from isotipo at low opacity. |
</phase_requirements>

## Standard Stack

### Core (Already Installed)
| Library | Version | Purpose | Status |
|---------|---------|---------|--------|
| astro | ^5.12.9 | Static site framework | Keep as-is |
| tailwindcss | ^3.4.17 | Utility CSS | Keep as-is |
| @tailwindcss/typography | ^0.5.16 | Prose styling | Keep as-is |
| preline | ^4.1.2 | Interactive UI components | Keep as-is |
| aos | ^2.3.4 | Scroll animations | Keep as-is |

### To Install
| Library | Version | Purpose | Import Pattern |
|---------|---------|---------|----------------|
| @fontsource/nunito-sans | ^5.2.7 | Heading font (weight 800) | `import '@fontsource/nunito-sans/800.css'` |
| @fontsource/montserrat | ^5.2.5 | Body font (weights 500, 700) | `import '@fontsource/montserrat/500.css'` + `import '@fontsource/montserrat/700.css'` |

### To Remove
| Library | Version | Reason |
|---------|---------|--------|
| daisyui | ^5.5.19 | Requires Tailwind v4, project uses Tailwind v3. Causes broken styles and console errors. |
| @fontsource-variable/inter | ^5.2.6 | Replaced by Nunito Sans + Montserrat |

**Installation commands:**
```bash
npm uninstall daisyui @fontsource-variable/inter
npm install @fontsource/nunito-sans @fontsource/montserrat
```

## Architecture Patterns

### Files to Modify (exact paths from codebase inspection)

```
src/
  components/
    CustomStyles.astro      # REWRITE: font imports + CSS variables
    Logo.astro               # REWRITE: SVG isotipo + wordmark (currently just text + emoji)
    ui/
      BrandOrnament.astro    # CREATE: reusable leaf shape background decoration
  config.yaml                # MODIFY: site name, lang, blog, theme, metadata
  layouts/
    Layout.astro             # VERIFY: no Inter references (imports come from CustomStyles)
  pages/
    [...blog]/               # DELETE: entire directory (index.astro, [...page].astro, [category]/, [tag]/)
tailwind.config.js           # MODIFY: remove daisyui import, plugin entry, config block
package.json                 # AUTO: updated by npm install/uninstall commands
```

### Pattern 1: CSS Variable Flow (AstroWind Convention)
**What:** CustomStyles.astro defines CSS variables in `:root`, tailwind.config.js maps them to Tailwind color/font classes, all components consume via Tailwind utilities.
**When to use:** Always -- this is the established AstroWind pattern.
**Current flow:**
```
CustomStyles.astro (:root vars) --> tailwind.config.js (color/fontFamily extend) --> components (class="text-primary font-heading")
```
**Key insight:** The tailwind.config.js already maps `primary`, `secondary`, `accent`, `default`, `muted` to CSS variables. Changing only CustomStyles.astro values will cascade automatically to all components. No need to touch tailwind.config.js color mappings.

### Pattern 2: Font Loading via @fontsource (AstroWind Convention)
**What:** Font CSS files are imported in component frontmatter, making them available globally.
**Current:** `import '@fontsource-variable/inter'` in CustomStyles.astro frontmatter (line 2).
**New pattern:**
```astro
---
import '@fontsource/nunito-sans/800.css';
import '@fontsource/montserrat/500.css';
import '@fontsource/montserrat/700.css';
---
```
**Important:** Use static (non-variable) packages with explicit weight imports. This produces smaller bundles than importing the full variable font.

### Pattern 3: Logo Component Interface
**What:** Logo.astro is imported in Header.astro (line 3) and rendered inside an anchor tag (line 75). No props are passed.
**Current:** Renders `<span>` with emoji + site name text.
**New:** Must render inline SVG isotipo + wordmark text. No interface change needed -- just replace the content.

### Pattern 4: Color Format
**What:** AstroWind uses `rgb()` format without commas for CSS variable values.
**Current format:** `--aw-color-primary: rgb(1 97 239);`
**New format:** Convert hex to rgb space-separated. Mapping:
```css
--aw-color-primary: rgb(149 180 68);       /* #95b444 */
--aw-color-secondary: rgb(93 111 49);      /* #5d6f31 */
--aw-color-accent: rgb(203 220 83);        /* #cbdc53 */
--aw-color-text-heading: rgb(37 39 47);    /* #25272f */
--aw-color-text-default: rgb(37 39 47);    /* #25272f */
--aw-color-text-muted: rgb(37 39 47 / 66%); /* #25272f @ 66% */
--aw-color-bg-page: rgb(255 255 255);      /* #ffffff */
```

### Anti-Patterns to Avoid
- **Do NOT add hex values directly to CSS variables.** AstroWind's Tailwind config uses `var()` references that expect valid CSS color values. Use `rgb()` format to maintain compatibility with Tailwind's opacity modifiers.
- **Do NOT import full @fontsource packages without weight specifiers.** `import '@fontsource/nunito-sans'` loads ALL weights. Use `import '@fontsource/nunito-sans/800.css'` for only what's needed.
- **Do NOT remove blog content collection from src/content/config.ts.** The postCollection definition is harmless and removing it may cause build errors if any utility still references it. Only remove the page routes.

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Font loading/FOUT | Custom @font-face declarations | @fontsource packages | Handles subsetting, formats (woff2), font-display, cross-browser |
| Color token system | Custom CSS property scheme | AstroWind's existing CSS variable pattern | Already wired to tailwind.config.js, changing values cascades everywhere |
| SVG optimization | Manual path simplification | Keep SVG readable, let astro-compress handle optimization at build time | astro-compress (v2.3.8) is already in devDependencies |

## Common Pitfalls

### Pitfall 1: DaisyUI Removal Leaves Ghost Styles
**What goes wrong:** DaisyUI v5 with `prefix: 'daisy-'` may have injected base styles or CSS layers that affect Tailwind's reset. After uninstalling, some elements may look different.
**Why it happens:** DaisyUI adds a CSS layer with base styles (button resets, form inputs, etc.) that override Tailwind's preflight.
**How to avoid:** After removing DaisyUI, run `npm run dev` and visually inspect. Check browser DevTools for any remaining `daisy-` class references. Search codebase for `daisy-` usage.
**Warning signs:** Buttons looking unstyled, form inputs changing appearance, unexpected colors.

### Pitfall 2: Font Variable Names Must Be Quoted Strings
**What goes wrong:** CSS `font-family` values with spaces must be quoted. `'Nunito Sans'` not `Nunito Sans`.
**Why it happens:** CSS parsing treats unquoted multi-word font names as separate tokens.
**How to avoid:** Always use single quotes inside CSS: `--aw-font-heading: 'Nunito Sans';` and `--aw-font-sans: 'Montserrat';`

### Pitfall 3: Blog Route Deletion May Cause Build Errors
**What goes wrong:** Other pages or components may import from blog utilities (src/utils/blog.ts). Deleting blog pages without checking imports can break the build.
**Why it happens:** AstroWind's blog system is integrated -- widgets like BlogHighlightedPosts.astro and BlogLatestPosts.astro reference blog utilities.
**How to avoid:** Set `apps.blog.isEnabled: false` in config.yaml FIRST. This should disable blog widgets. Then delete the route files. Do NOT delete src/utils/blog.ts or src/components/blog/ directory yet -- they're needed by widget components that may still be referenced.
**Warning signs:** Build errors mentioning `getCollection` or blog utility imports.

### Pitfall 4: Selection Color CSS Syntax
**What goes wrong:** The `::selection` pseudo-element is currently nested inside `:root` (invalid CSS nesting without a preprocessor).
**Why it happens:** AstroWind uses `<style is:inline>` which is plain CSS -- no nesting support.
**How to avoid:** Move `::selection` outside `:root` as a separate rule block.

### Pitfall 5: Dark Mode Remnants in Components
**What goes wrong:** After removing the `.dark` CSS block, components still have `dark:` Tailwind classes (e.g., Header.astro has `dark:text-white`, `dark:hover:bg-gray-700`).
**Why it happens:** AstroWind components use dark variant classes throughout.
**How to avoid:** For Phase 1, just set `ui.theme: 'light:only'` and remove the `.dark` CSS block. The `dark:` classes become inert (they only activate when `class="dark"` is on `<html>`, which won't happen with `light:only`). Full cleanup of dark: classes can wait for later phases.

## Code Examples

### CustomStyles.astro Rewrite
```astro
---
import '@fontsource/nunito-sans/800.css';
import '@fontsource/montserrat/500.css';
import '@fontsource/montserrat/700.css';
---

<style is:inline>
  :root {
    --aw-font-sans: 'Montserrat';
    --aw-font-serif: 'Montserrat';
    --aw-font-heading: 'Nunito Sans';

    --aw-color-primary: rgb(149 180 68);
    --aw-color-secondary: rgb(93 111 49);
    --aw-color-accent: rgb(203 220 83);

    --aw-color-text-heading: rgb(37 39 47);
    --aw-color-text-default: rgb(37 39 47);
    --aw-color-text-muted: rgb(37 39 47 / 66%);
    --aw-color-bg-page: rgb(255 255 255);

    --aw-color-bg-page-dark: rgb(37 39 47);
  }

  ::selection {
    background-color: rgb(203 220 83);
  }
</style>
```

### tailwind.config.js After DaisyUI Removal
```javascript
import defaultTheme from 'tailwindcss/defaultTheme';
import plugin from 'tailwindcss/plugin';
import typographyPlugin from '@tailwindcss/typography';
// daisyui import REMOVED

export default {
  content: [
    './src/**/*.{astro,html,js,jsx,json,md,mdx,svelte,ts,tsx,vue}',
    'node_modules/preline/dist/*.js',
  ],
  theme: {
    extend: {
      colors: {
        primary: 'var(--aw-color-primary)',
        secondary: 'var(--aw-color-secondary)',
        accent: 'var(--aw-color-accent)',
        default: 'var(--aw-color-text-default)',
        muted: 'var(--aw-color-text-muted)',
      },
      fontFamily: {
        sans: ['var(--aw-font-sans, ui-sans-serif)', ...defaultTheme.fontFamily.sans],
        serif: ['var(--aw-font-serif, ui-serif)', ...defaultTheme.fontFamily.serif],
        heading: ['var(--aw-font-heading, ui-sans-serif)', ...defaultTheme.fontFamily.sans],
      },
      animation: {
        fade: 'fadeInUp 1s both',
      },
      keyframes: {
        fadeInUp: {
          '0%': { opacity: 0, transform: 'translateY(2rem)' },
          '100%': { opacity: 1, transform: 'translateY(0)' },
        },
      },
    },
  },
  plugins: [
    typographyPlugin,
    plugin(({ addVariant }) => {
      addVariant('intersect', '&:not([no-intersect])');
    }),
    // daisyui plugin REMOVED
  ],
  // daisyui config block REMOVED
  darkMode: 'class',
};
```

### Logo.astro Structure (SVG isotipo + wordmark)
```astro
---
// No imports needed -- pure SVG + text
---

<span class="flex items-center">
  <svg
    xmlns="http://www.w3.org/2000/svg"
    viewBox="0 0 48 48"
    class="h-10 w-10"
    aria-hidden="true"
  >
    <!-- Central leaf (darker green) -->
    <path d="..." fill="#5d6f31" stroke="#25272f" stroke-width="0.5" />
    <!-- Left leaf -->
    <path d="..." fill="#95b444" stroke="#25272f" stroke-width="0.5" />
    <!-- Right leaf -->
    <path d="..." fill="#95b444" stroke="#25272f" stroke-width="0.5" />
    <!-- Stem -->
    <path d="..." fill="#5d6f31" />
  </svg>
  <span class="ml-2 text-xl">
    <span class="font-heading font-extrabold text-[#25272f]">campiva</span><span class="font-heading font-normal text-[#25272f]">corp.</span>
  </span>
</span>
```
**Note:** Exact SVG path `d` attributes must be crafted to replicate the reference image (3 overlapping organic leaves with gradient green).

### config.yaml Key Changes
```yaml
site:
  name: 'campivacorp.'
  site: 'https://campivacorp.com'
  base: '/'
  trailingSlash: false
  # googleSiteVerificationId REMOVED

metadata:
  title:
    default: 'campivacorp.'
    template: '%s -- campivacorp.'
  description: 'campivacorp. - Soluciones agroindustriales para el mundo. Trading, brokeraje y logistica de productos agroindustriales con certificaciones internacionales.'
  robots:
    index: true
    follow: true
  openGraph:
    site_name: 'campivacorp.'
    images:
      - url: '~/assets/images/default.png'
        width: 1200
        height: 628
    type: website
  # twitter block REMOVED

i18n:
  language: es
  textDirection: ltr

apps:
  blog:
    isEnabled: false

# analytics block REMOVED

ui:
  theme: 'light:only'
```

### BrandOrnament.astro Structure
```astro
---
export interface Props {
  position?: 'top-left' | 'top-right' | 'bottom-left' | 'bottom-right';
  size?: number; // px, default 400
  class?: string;
}

const {
  position = 'top-right',
  size = 400,
  class: className = '',
} = Astro.props;

const positionClasses = {
  'top-left': 'top-0 left-0 -translate-x-1/4 -translate-y-1/4',
  'top-right': 'top-0 right-0 translate-x-1/4 -translate-y-1/4',
  'bottom-left': 'bottom-0 left-0 -translate-x-1/4 translate-y-1/4',
  'bottom-right': 'bottom-0 right-0 translate-x-1/4 translate-y-1/4',
};
---

<div
  class:list={[
    'absolute pointer-events-none opacity-[0.06] overflow-hidden',
    positionClasses[position],
    className,
  ]}
  style={`width: ${size}px; height: ${size}px;`}
  aria-hidden="true"
>
  <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg" class="w-full h-full">
    <!-- Single large leaf shape derived from isotipo -->
    <path d="..." fill="#95b444" />
  </svg>
</div>
```

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| @fontsource-variable/inter (full variable font) | @fontsource/nunito-sans + @fontsource/montserrat (static, per-weight) | This phase | Smaller bundle, only loads needed weights |
| DaisyUI v5 + Tailwind v3 (broken combo) | Pure Tailwind v3 + Preline UI | This phase | Eliminates CSS conflicts, console errors |
| Dark mode support (system/class toggle) | Light only (light:only) | This phase | Simpler CSS, no theme toggle needed |

**Deprecated/outdated in this project:**
- DaisyUI v5 with Tailwind v3: fundamentally incompatible. DaisyUI v5 was rewritten for Tailwind v4's new CSS-first config.
- `@fontsource-variable/inter`: being replaced, not deprecated upstream.

## Open Questions

1. **Exact SVG paths for the isotipo**
   - What we know: 3 organic overlapping leaves, central darker, two lateral lighter, dark border, small stem
   - What's unclear: Precise SVG `d` attribute coordinates to replicate the reference image
   - Recommendation: Craft SVG manually based on the reference screenshot. Start with simple elliptical/organic leaf shapes, iterate visually. The SVG can be refined in later phases.

2. **Whether `daisy-` prefixed classes exist in any component**
   - What we know: DaisyUI was configured with `prefix: 'daisy-'`
   - What's unclear: Whether any AstroWind or custom components actually use `daisy-` classes
   - Recommendation: After uninstalling, grep for `daisy-` across all `.astro` files. Remove any found references.

3. **Blog content collection cleanup**
   - What we know: `src/content/config.ts` defines `postCollection` and `src/data/post/` likely has markdown files
   - What's unclear: Whether removing the collection definition causes cascading errors
   - Recommendation: Leave `src/content/config.ts` and `src/data/post/` untouched. Only delete page routes and disable in config. Safer approach.

## Validation Architecture

### Test Framework
| Property | Value |
|----------|-------|
| Framework | Astro check (astro check) + ESLint + Prettier |
| Config file | astro.config.* (check), eslint.config.* (lint), .prettierrc* (format) |
| Quick run command | `npm run dev` (visual) + `npm run check:astro` (types) |
| Full suite command | `npm run check` (astro check + eslint + prettier) |

### Phase Requirements to Test Map
| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| FOUN-01 | Brand colors in CSS variables | manual | Visual inspection: `npm run dev`, check no blue/purple | N/A manual |
| FOUN-02 | Fonts loaded (Nunito Sans 800, Montserrat 500/700) | manual | Visual inspection + DevTools font tab | N/A manual |
| FOUN-03 | SVG isotipo renders | manual | Visual inspection: logo visible in header | N/A manual |
| FOUN-04 | config.yaml correct | smoke | `npm run build` succeeds with new config | N/A |
| FOUN-05 | DaisyUI removed | smoke | `npm run build` + no daisyui in node_modules | N/A |
| FOUN-06 | Blog routes gone | smoke | `npm run build` + verify no /blog output | N/A |
| FOUN-07 | Ornament component created | unit | Component file exists + renders without error | N/A |

### Sampling Rate
- **Per task commit:** `npm run build` (confirms no build errors)
- **Per wave merge:** `npm run check` (full lint + type check)
- **Phase gate:** `npm run build` + visual inspection of dev server

### Wave 0 Gaps
- No automated visual regression testing -- all color/font/logo verification is manual via `npm run dev`
- This is acceptable for a branding/foundation phase where visual inspection is the primary validation method

## Sources

### Primary (HIGH confidence)
- **Codebase inspection:** package.json (DaisyUI v5.5.19 + Tailwind v3.4.17 confirmed), tailwind.config.js (daisyui plugin + config block identified), CustomStyles.astro (current Inter + blue/purple variables), config.yaml (current AstroWind defaults), Logo.astro (current emoji + text), Header.astro (Logo import pattern), Layout.astro (font import chain)
- **@fontsource/nunito-sans** (npmjs.com) - v5.2.7, supports weights 200-900, import pattern `@fontsource/nunito-sans/800.css`
- **@fontsource/montserrat** (npmjs.com) - v5.2.5, supports weights 100-900, import pattern `@fontsource/montserrat/500.css`

### Secondary (MEDIUM confidence)
- **DaisyUI v5 release notes** (daisyui.com/docs/v5) - Confirmed DaisyUI v5 requires Tailwind v4, complete rewrite
- **Fontsource install guides** (fontsource.org) - Per-weight import syntax verified

### Tertiary (LOW confidence)
- None -- all findings verified against codebase or official sources

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH - directly inspected package.json and all relevant source files
- Architecture: HIGH - AstroWind's CSS variable flow pattern traced through CustomStyles -> tailwind.config -> components
- Pitfalls: HIGH - DaisyUI incompatibility confirmed from version numbers; font/blog pitfalls from direct code inspection

**Research date:** 2026-03-15
**Valid until:** 2026-04-15 (stable -- no fast-moving dependencies)
