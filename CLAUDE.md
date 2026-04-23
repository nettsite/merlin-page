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

## Architecture

Astro 6 project using the "basics" starter. TypeScript strict mode enabled.

**Routing**: File-based — every `.astro` file under `src/pages/` becomes a route. No router config required.

**Component model**: Astro components have two sections separated by `---`:
- Frontmatter (server-side JS/TS, runs at build time)
- HTML template with optional `<style>` (scoped by default) and `<script>` (client-side)

**Layouts**: `src/layouts/Layout.astro` wraps pages via `<slot />`. Pages import and wrap themselves in a layout — the layout is not applied globally.

**Assets**: Static files in `public/` are served as-is. Processed assets (SVG, images) go in `src/assets/` and are imported directly in components.

**No framework yet**: Currently pure Astro components only. To add React/Vue/Svelte, run `npx astro add <integration>` — this updates `astro.config.mjs` and installs the adapter.
