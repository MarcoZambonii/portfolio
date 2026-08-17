# marcozamboni.dev — personal site

Personal portfolio of Marco Zamboni: education, the probability handbook, research
projects, apps and code. Two static pages, no build step, no dependencies to install.

Exported from the Claude Design project *Portfolio v2* and turned into a plain
static site.

## Pages

| File | URL | Was |
| --- | --- | --- |
| `index.html` | `/` | `Portfolio v2.dc.html` |
| `boostnote.html` | `/boostnote.html` | `BoostNote Info.dc.html` |

Both pages are Design Canvas documents: the markup inside `<x-dc>` is a template
that `support.js` hydrates with React at runtime (React, ReactDOM and Babel are
pulled from unpkg with SRI hashes — the pages need a network connection on first
load, nothing else).

## Files

```
index.html                 portfolio (home)
boostnote.html             BoostNote deep-dive page
support.js                 Design Canvas runtime (generated — do not edit)
image-slot.js              <image-slot> web component (starter scaffold)
image-slots.state.json     photos + framing for the three image slots
assets/                    photos, favicon, PDFs
.nojekyll                  keep GitHub Pages from filtering files
.claude/launch.json        `preview_start` config for local serving
```

`image-slots.state.json` holds the portrait, book cover and dog photo as data URLs
together with their pan/zoom framing; it is fetched at page load. The same photos
also exist as files in `assets/` and are wired as `src=` fallbacks on the slots, so
the page still shows them if that fetch fails (for example when the file is opened
straight from disk over `file://`).

## Run locally

The pages fetch a JSON sidecar, so serve them over HTTP — don't just double-click
`index.html`.

```bash
python3 -m http.server 4173
```

Then open http://localhost:4173.

## Deploy (GitHub Pages)

The remote is [MarcoZambonii/portfolio](https://github.com/MarcoZambonii/portfolio)
(`origin`, branch `main`). To serve it: **Settings → Pages → Build and deployment →
Deploy from a branch**, branch `main`, folder `/ (root)` — which publishes to
`https://marcozambonii.github.io/portfolio/`. `.nojekyll` is already committed, and
that is what keeps Pages from dropping files it would otherwise treat as Jekyll
sources.

For a root URL instead, the contents of this repo can go into the existing
`marcozambonii.github.io` repository — every asset path here is relative, so the site
works at either depth.

## Assets

The design-project export truncates any file over 192 KB, so three assets came over
incomplete and were re-fetched from the source repositories instead:

- `assets/boostnote-icon.png` — `AppIcon-Dark.png` from
  [MarcoZambonii/BoostNote](https://github.com/MarcoZambonii/BoostNote), downscaled
  1024 → 384 px (the largest on-page rendering is 128 px, and the original was 1 MB).
- `assets/dispense-probabilita.pdf` — `Dispense_Probabilità.pdf` from
  [MarcoZambonii/Dispense-Probabilita](https://github.com/MarcoZambonii/Dispense-Probabilita),
  1.2 MB, unchanged.

Still missing:

- `assets/cv-marco-zamboni.pdf` — the contact section's "Download CV" button points
  here, but the file was never in the design project either, so that link 404s until
  you add it.

`assets/handbook-src.pdf` came over intact but no page links to it.

## ⚠️ The confidential thesis is not protected

`index.html` gates the Irbema thesis behind a password prompt, but the check runs in
the browser: the password is in plain text in the page source, and the PDF would sit
at a public URL that anyone can request directly. Client-side gating is decoration,
not access control.

`assets/research-irbema.pdf` is therefore **not** in this repo and is listed in
`.gitignore`. If the thesis genuinely contains company data, keep it out of any
public deployment and send it on request instead. If you want it on the site, it
needs a real backend check (a serverless function issuing a signed URL, or hosting
behind an authenticated service) — and the password should not be in the page.
