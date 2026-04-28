# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev       # Dev server at localhost:4321 (hot reload)
npm run build     # Build to ./dist/
npm run preview   # Preview built output locally
npx astro check  # TypeScript type-check all .astro files
```

## Purpose

Static marketing site for **Merlin** — an AI-powered invoice processing product that lives inside the customer's own books. Core pitch: supplier PDFs → extracted line items → GL account suggestions → auto-posted transactions. Learns per-supplier coding patterns over time. Self-hosted (data never leaves the customer's system). Confidence-scored with a human review queue before anything touches the ledger.

Key differentiators to convey: no third-party processing bureau, learns your specific business (not generic AI), full audit trail, proper double-entry ledger. Target audience: small businesses currently paying a bookkeeper for invoice data entry.

See `/home/will/Projects/merlin/README.md` for full product detail and feature status.

## Current state — A/B preview live

The site currently runs **two parallel design directions** so the owner can collect friend feedback before committing to one. Both are deployed on Cloudflare Pages from the same repo.

- **`/` (default)** → **Direction B "Workshop"** — crisp white background, warm amber accent (`#C8772E`), Inter sans throughout, geometric sparkle logo (`LogoSparkle.astro`). Modern restrained SaaS feel.
- **`/a`** → **Direction A "Ledger"** — warm cream paper (`#F5F0E6`), deep ink-blue accent (`#0F1B3D`), Fraunces serif headlines + Inter body, custom serif "M" logo (`LogoQuill.astro`). Editorial/finance-publication feel.

A `<PreviewBar />` component sits at the top of each page with a slim dark strip telling visitors which version they're viewing and linking to the other. Theme tokens live in `src/layouts/Layout.astro` as CSS custom properties scoped by `html[data-variant='A'|'B']`.

### When the owner picks a winner

1. Strip `<PreviewBar />` import + element from the surviving page.
2. Delete `src/components/PreviewBar.astro`.
3. **If A wins:** copy contents of `src/pages/a.astro` into `src/pages/index.astro` (set `variant="B"` → `variant="A"`), delete `a.astro`, delete `LogoSparkle.astro`.
4. **If B wins:** delete `src/pages/a.astro` and `src/components/LogoQuill.astro`.
5. Either way: keep `InvoiceMock.astro`, `Modal.astro`, the new `Layout.astro`, and the surviving Logo component.

### Design intent (don't drift from this)

- **Tone**: warm + human, small-business friendly. NOT enterprise/corporate, NOT playful/whimsical despite the wizard name.
- **No emoji** in the design (the original page used 📄 💸 🤖 ✓ — these are now SVG icons or numbered markers).
- **No gradient backgrounds** beyond the very subtle hero fade.
- **The "testimonial"** is a marketing line, not a real customer quote — it's framed as a pull-quote, not attributed to anyone. Don't add fake names.
- **Hero shows the product**: an invoice review queue mock with a confidence score and suggested account codes (`InvoiceMock.astro`). Don't replace with stock illustration or hero text alone.
- **Copy** stays close to original — light edits only, preserve meaning and structure.

### File map (post-redesign)

```
src/
├── layouts/
│   └── Layout.astro            # Shared shell + theme tokens for both variants
├── components/
│   ├── PreviewBar.astro        # A/B switcher bar (remove after pick)
│   ├── LogoQuill.astro         # Direction A logo
│   ├── LogoSparkle.astro       # Direction B logo
│   ├── InvoiceMock.astro       # Hero product mock — themed via CSS vars
│   ├── Modal.astro             # Request-access form (was inline in old index)
│   └── Welcome.astro           # UNUSED — Astro starter leftover, safe to delete
└── pages/
    ├── index.astro             # Direction B (default)
    └── a.astro                 # Direction A
```

The contact form still POSTs to `https://api.nettsite.co.za/api/contact` — preserved from the original.

## Architecture

Astro 6 project using the "basics" starter. TypeScript strict mode enabled.

**Routing**: File-based — every `.astro` file under `src/pages/` becomes a route. No router config required.

**Component model**: Astro components have two sections separated by `---`:
- Frontmatter (server-side JS/TS, runs at build time)
- HTML template with optional `<style>` (scoped by default) and `<script>` (client-side)

**Layouts**: `src/layouts/Layout.astro` wraps pages via `<slot />`. Pages import and wrap themselves in a layout — the layout is not applied globally.

**Assets**: Static files in `public/` are served as-is. Processed assets (SVG, images) go in `src/assets/` and are imported directly in components. Logos are now inline SVG inside `LogoQuill.astro` / `LogoSparkle.astro`; the old `public/logo.png` remains only as the OG image fallback.

**No framework yet**: Currently pure Astro components only. To add React/Vue/Svelte, run `npx astro add <integration>` — this updates `astro.config.mjs` and installs the adapter.
