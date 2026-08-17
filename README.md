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

```bash
git remote add origin git@github.com:MarcoZambonii/<repo>.git
git push -u origin main
```

Then in the repository: **Settings → Pages → Build and deployment → Deploy from a
branch**, branch `main`, folder `/ (root)`. `.nojekyll` is already committed, which
is what keeps Pages from dropping files it would otherwise treat as Jekyll sources.

## Assets still to add

Three files could not be exported from the design project (the export API truncates
any file over 192 KB). Copy them into `assets/` from the original project download:

- `assets/boostnote-icon.png` — currently a broken image on both pages. The original
  is `BoostNote/Assets.xcassets/AppIcon.appiconset/AppIcon-Dark.png` in
  [MarcoZambonii/BoostNote](https://github.com/MarcoZambonii/BoostNote).
- `assets/dispense-probabilita.pdf` — linked from the book section ("Read the PDF").
- `assets/cv-marco-zamboni.pdf` — linked from the contact section ("Download CV").
  This one was never in the design project at all; the link has always been dead.

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
