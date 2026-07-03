# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev       # start dev server (localhost:4321)
npm run build     # production build → dist/
npm run preview   # preview the dist/ build locally
```

No test runner or linter is configured.

## Architecture

**Astro 6 static site** with Tailwind CSS v4 (loaded via `@tailwindcss/vite` — no `tailwind.config.js`). Node ≥ 22 required.

**Deployment**: GitHub Pages at `https://wpateracki.github.io/bit-voyager`. The `base: 'bit-voyager'` in [astro.config.mjs](astro.config.mjs) means all internal `href` links must be prefixed with `/bit-voyager/` (e.g. `href="/bit-voyager/#contact"`).

**Page structure**: The site is a single-page marketing layout plus project case study pages.

- [src/pages/index.astro](src/pages/index.astro) — composes all section components in order
- [src/layouts/BaseLayout.astro](src/layouts/BaseLayout.astro) — HTML shell; loads Google Fonts (Space Grotesk, Inter, JetBrains Mono); runs the IntersectionObserver that drives `.fade-in` scroll animations site-wide
- [src/layouts/ProjectLayout.astro](src/layouts/ProjectLayout.astro) — case study template; accepts `accent` (hex) and `accentRgb` props that set `--accent` / `--accent-rgb` CSS custom properties for per-project theming
- [src/pages/work/](src/pages/work/) — individual case study pages that pass data as props to `ProjectLayout`

**Styles**: Global design tokens and shared utilities live in [src/styles/global.css](src/styles/global.css). Key shared classes: `.glass-card`, `.bitvoyage-bg`, `.gradient-text`, `.gradient-text-gold`, `.btn-primary`, `.btn-ghost`, `.fade-in`, `.marquee-track`. Component-specific styles are co-located in each component's `<style>` block.

**CSS custom properties** (defined in `global.css`):
- Colors: `--bg-base`, `--bg-surface`, `--bg-card`, `--accent-purple`, `--accent-cyan`, `--accent-gold`, `--text-primary`, `--text-muted`, `--border-subtle`
- Project pages add `--accent` and `--accent-rgb` dynamically

**Animations**: The star-field canvas and typewriter word-cycling in the Hero are implemented in `<script is:inline>` blocks inside [src/components/sections/Hero.astro](src/components/sections/Hero.astro). Scroll fade-ins use the `.fade-in` class + the observer in `BaseLayout`.

**Font roles**: Space Grotesk → headings and company name; Inter → body copy; JetBrains Mono → labels, badges, code-style eyebrows.
