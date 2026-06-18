# merlin-page

Marketing site for [Merlin](https://github.com/nettsite/merlin) — self-hosted and hosted accounting software for small businesses.

Built with Astro 6. Deployed on Cloudflare Pages.

## Commands

```bash
npm run dev       # Dev server at localhost:4321
npm run build     # Build to ./dist/
npm run preview   # Preview built output locally
```

## Stack

- Astro 6 (static output)
- Scoped CSS, no framework
- Contact form POSTs to `https://api.nettsite.co.za/api/contact`
- Cloudflare Turnstile on the request-access modal

## Structure

```
src/
├── layouts/
│   └── Layout.astro        # Shell, theme tokens, meta
├── components/
│   ├── LogoSparkle.astro   # Inline SVG logo
│   ├── InvoiceMock.astro   # Hero product mock
│   └── Modal.astro         # Request-access form
└── pages/
    └── index.astro
```

## Deploying

Push to `main` — Cloudflare Pages rebuilds automatically.
