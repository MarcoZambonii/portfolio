# Copia di riferimento dell'export Claude Design

I due file in questa cartella sono l'export **intatto** del progetto
[Portfolio v2](https://claude.ai/design/p/b76232c0-540d-4d61-bbed-01d1ce6d7108),
scaricato il 17 agosto 2026 (`Portfolio v2.dc.html` = commit design con l'hero
scontornato e i pill in percentuale).

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
3. Riporta quelle modifiche a mano in `index.html` / `boostnote.html`.
4. Sostituisci i file in questa cartella con il nuovo export, così diventano il
   riferimento per il giro successivo.

Non copiare l'export sopra `index.html`: cancelleresti le patch locali qui sotto.
Per lo stesso motivo non serve diffare l'export contro `index.html` — vengono
fuori centinaia di righe che sono solo le patch.

## Patch locali da preservare a ogni sync

| Area | Nel design | Nel sito |
| --- | --- | --- |
| Nomi file | `Portfolio v2.dc.html`, `BoostNote Info.dc.html` | `index.html`, `boostnote.html` (link riscritti) |
| `<head>` | solo charset e viewport | `<title>`, description, og/twitter, `lang`, favicon, theme-color |
| Paper PDF | link a jsdelivr, `target="_blank"` | `assets/article-risk-neutral-density.pdf`, stessa tab |
| Copertina libro | `<image-slot id="book-cover">` | stesso slot con `src="assets/book-cover.png"` |
| Hero | `<img src="assets/portrait-hero.png">`, misure in px fissi | `assets/portrait-hero.webp` (170 KB invece di 1,26 MB), px convertiti in percentuali |
| Sidecar | `.image-slots.state.json`, tre slot | `image-slots.state.json` (senza punto, `STATE_FILE` corretto in `image-slot.js`), resta solo `kyra-photo` |
| CV | nessun bottone | `assets/cv-marco-zamboni.pdf` + riga "Fair warning: this site is probably more up to date than the CV." |
| Tesi Irbema | `assets/research-irbema.pdf` presente | file **assente** e in `.gitignore` — la password è client-side, vedi commento nel file |

I px in pixel fissi dell'hero e dei pill arrivano dal posizionamento a mano nel
canvas: su schermi stretti non scalano, quindi vanno riconvertiti in percentuali
ogni volta che quel blocco cambia nel design.
