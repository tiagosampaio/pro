# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev        # Start dev server on http://localhost:4321
npm run build      # Static build → dist/
npm run preview    # Preview the built output locally
```

## Architecture

Astro static site with zero JS framework dependencies. Outputs pure HTML/CSS/JS for GitHub Pages (`output: 'static'` in `astro.config.mjs`).

### Pages (`src/pages/`)
- `index.astro` — Cinematic home: hero, brand marquee, intro stats, image strip, expertise teaser, CTA
- `about.astro` — Full about page: photo feature, philosophy pillars, career timeline, skills grid
- `work.astro` — Project showcase: alternating card layout (image + copy), tech stack cloud
- `contact.astro` — Contact: availability status, social links, Formspree form

### Shared pieces
- `src/layouts/Layout.astro` — Root HTML shell: loads fonts, global CSS, injects Nav + Footer, runs the scroll-reveal `IntersectionObserver`
- `src/components/Nav.astro` — Fixed navbar with scroll-based glass effect and mobile drawer (self-contained `<script>`)
- `src/components/Footer.astro` — Simple footer
- `src/styles/global.css` — All design tokens, resets, utility classes, button styles, `[data-reveal]` animation system

### Adding photos
Every image slot has a `<!-- Replace with: <img src="/images/..." /> -->` comment. Drop photos into `public/images/` and swap the comment for the real `<img>` tag. The hero background in `index.astro` accepts `<img class="hero-photo">` inside `.hero-img-placeholder`.

### Scroll reveal system
Any element with `data-reveal` fades in on scroll. Optional modifiers:
- `data-reveal="left"` / `data-reveal="right"` — slides in from the side
- `data-delay="1"` through `data-delay="5"` — adds 100ms–500ms stagger delay

The observer lives in `Layout.astro`'s `<script>` block and runs once per page load.

### Key design tokens
| Token | Value |
|-------|-------|
| `--accent` | `#7c6fff` (purple) |
| `--accent-2` | `#a78bfa` (soft purple) |
| `--bg` | `#06080f` (near-black) |
| `--font-display` | Playfair Display (serif, hero/display titles) |
| `--font` | Inter (body) |
| `--mono` | JetBrains Mono (labels, tags, metadata) |

### Deployment (GitHub Pages)
Push to the repo, enable Pages from the `main` branch root, or use GitHub Actions: `npm run build` → deploy `dist/`. No server or adapter needed.

### Contact form
The form in `contact.astro` POSTs to Formspree. Replace `YOUR_ID` in the `action` attribute with a real Formspree endpoint to activate it.
