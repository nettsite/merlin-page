# Merlin — A/B preview deployment

Drop these files into your existing `magical-mercury/` Astro project:

```
magical-mercury/
└── src/
    ├── layouts/
    │   └── Layout.astro          ← REPLACES existing
    ├── components/
    │   ├── PreviewBar.astro      ← NEW
    │   ├── LogoQuill.astro       ← NEW (Direction A logo)
    │   ├── LogoSparkle.astro     ← NEW (Direction B logo)
    │   ├── InvoiceMock.astro     ← NEW (hero product mock)
    │   └── Modal.astro           ← NEW (request-access form)
    └── pages/
        ├── index.astro           ← REPLACES existing (Direction B)
        └── a.astro               ← NEW (Direction A)
```

## What this gives you

- **`/`** → Direction B (Workshop). Crisp white + warm amber. Default.
- **`/a`** → Direction A (Ledger). Cream paper + ink blue + editorial serif.
- A slim dark bar at the top of each page tells visitors which version they're on
  and links to the other.
- Existing `Welcome.astro` is no longer used — safe to delete, or leave it.
- Existing modal/contact form behaviour preserved (same API endpoint).

## To deploy on Cloudflare Pages

Just `git push` — Cloudflare will rebuild from the new files and `/`, `/a`
will both be live. Share both URLs with friends, point them at `/` first;
they can flip via the top bar.

## After friends weigh in

Once you've picked a winner:

1. If **B wins**, just delete `src/pages/a.astro` and remove the `<PreviewBar />`
   import + element from `index.astro`.
2. If **A wins**, copy the contents of `a.astro` into `index.astro` (changing
   `variant="A"`), then delete `a.astro` and remove the `<PreviewBar />`.

Either way, also drop the `PreviewBar.astro` component file.
