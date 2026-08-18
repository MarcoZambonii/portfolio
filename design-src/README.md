# Copia di riferimento dell'export Claude Design

I file in questa cartella sono l'export **intatto** del progetto
[Portfolio v2](https://claude.ai/design/p/b76232c0-540d-4d61-bbed-01d1ce6d7108):
`Portfolio v2.dc.html` e `BoostNote Info.dc.html` scaricati il 17 agosto 2026,
`Portfolio v2 Mobile.dc.html` il 19 agosto 2026.

Servono solo a una cosa: **fare il diff col prossimo export**, per vedere cosa hai
cambiato nel design senza che le modifiche locali facciano rumore.

## Come aggiornare i file locali dal design

1. Scarica l'export aggiornato dal progetto (in una cartella a parte, es. `~/Downloads/export/`).
2. Confronta il nuovo export con questa copia:

   ```bash
   diff -u "design-src/Portfolio v2.dc.html" ~/Downloads/export/"Portfolio v2.dc.html"
   ```

   Quello che esce è **solo** ciò che è cambiato nel design: di solito due o tre
   righe di testo, o un blocco di markup.
3. Riporta quelle modifiche a mano in `index.html` / `mobile.html` / `boostnote.html`.
4. Sostituisci i file in questa cartella con il nuovo export, così diventano il
   riferimento per il giro successivo.

Non copiare l'export sopra `index.html` o `mobile.html`: cancelleresti le patch locali qui sotto.
Per lo stesso motivo non serve diffare l'export contro `index.html` — vengono
fuori centinaia di righe che sono solo le patch.

## Patch locali da preservare a ogni sync

| Area | Nel design | Nel sito |
| --- | --- | --- |
| Nomi file | `Portfolio v2.dc.html`, `Portfolio v2 Mobile.dc.html`, `BoostNote Info.dc.html` | `index.html`, `mobile.html`, `boostnote.html` (link riscritti) |
| `<head>` | solo charset e viewport | `<title>`, description, og/twitter, `lang`, favicon, theme-color |
| Paper PDF | link a jsdelivr, `target="_blank"` | `assets/article-risk-neutral-density.pdf`, stessa tab |
| Copertina libro | `<image-slot id="book-cover">` | stesso slot con `src="assets/book-cover.png"` |
| Hero | `<img src="assets/portrait-hero-2.png">`, `left:50px; top:12px` | `assets/portrait-hero-2.webp` (108 KB invece di 3,9 MB), px convertiti in percentuali |
| Kyra | `<img src="assets/kyra-cutout.png">` | `assets/kyra-cutout.webp` (66 KB invece di 6,3 MB) |
| Sidecar | `.image-slots.state.json` | eliminato: resta solo lo slot `book-cover`, che usa `src` |
| CV | nessun bottone | `assets/cv-marco-zamboni.pdf` + riga "Fair warning: this site is probably more up to date than the CV." |
| Tesi Irbema | `assets/research-irbema.pdf` presente | file **assente** e in `.gitignore` — la password è client-side |
| Redirect viewport | nessuno: due artboard separate | script inline in testa a `index.html` e `mobile.html` (vedi sotto) |
| Canonical | nessuno | `<link rel="canonical" href="index.html">` in `mobile.html` |

I px in pixel fissi dell'hero e dei pill arrivano dal posizionamento a mano nel
canvas: su schermi stretti non scalano, quindi vanno riconvertiti in percentuali
ogni volta che quel blocco cambia nel design. Valgono solo per `index.html`: la
versione mobile è disegnata fluida e non ha né hero scontornato né pill.

## Come vengono servite le due versioni

Nel design sono due artboard separate; GitHub Pages è statico, quindi lo switch
è client-side. In testa a ciascuna pagina c'è uno script che rimanda all'altra:

- `index.html` → `mobile.html` se `innerWidth < 768`
- `mobile.html` → `index.html` se `innerWidth >= 768`

Le due condizioni sono disgiunte, quindi la coppia non può entrare in loop. Usano
`location.replace`, così il tasto indietro non resta intrappolato, e riportano
query e hash, così i deep link (`#papers`) sopravvivono al salto.

Vie di fuga per provare una versione sul dispositivo sbagliato:
`index.html?desktop` e `mobile.html?mobile`.
