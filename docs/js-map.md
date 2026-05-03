# Dr.Web Lawyer — Mappa JS completa + Note per Claude
**Versione:** 2026-05-03  
**Cartella:** `/js` (child theme)  
**File totali:** 7  
**Fonte:** analisi diretta dei sorgenti inviati da Luigi

---

## Mappa file `/js`

| File | Dim. reale | Modificato | Scope | Dipendenze | Stato |
|------|-----------|------------|-------|------------|-------|
| `global.js` | 8.0 KB | 09/Apr/2026 | Namespace `DrwLawyer`, back-to-top, header scroll, skip link | Nessuna (vanilla) | ✅ Stabile |
| `counters.js` | 3.3 KB | 21/Mar/2026 | Animazione circle counter + 4 linear counters | Nessuna (vanilla) | ✅ Stabile |
| `justice.js` | 846 B | 13/Apr/2026 | Scroll reveal `.drw-justice` via IntersectionObserver | Nessuna (vanilla) | ✅ Stabile |
| `statue.js` | 8.1 KB | 07/Apr/2026 | Hotspot, flip card, typewriter, video lightbox, panel slide-in | Nessuna (vanilla) | ✅ Stabile |
| `timeline.js` | 4.5 KB | 07/Apr/2026 | Progress bar timeline + attivazione righe | Nessuna (vanilla) | ✅ Stabile |
| `drw-demo-switcher.js` | 4.9 KB | 09/Apr/2026 | Switcher palette demo (solo sviluppo) | Nessuna (vanilla) | ⚠️ Solo demo |
| `custom.js` | 28 B | 02/Mar/2026 | Placeholder per customizzazioni client | — | 🔵 Quasi vuoto |

---

## Architettura generale

Tutti i file sono **IIFE vanilla JS**, senza dipendenze esterne. Il namespace globale è `window.DrwLawyer` (definito in `global.js`). Gli altri file sono autonomi e non accedono al namespace — comunicano solo tramite DOM (classi CSS, data attributes).

**Pattern di avvio usati:**

| File | Trigger avvio |
|------|--------------|
| `global.js` | `DOMContentLoaded` (con fallback se DOM già pronto) |
| `counters.js` | `DOMContentLoaded` |
| `justice.js` | `window load` (con fallback se già `complete`) |
| `statue.js` | `DOMContentLoaded` |
| `timeline.js` | `window load` + `setTimeout(150ms)` |
| `drw-demo-switcher.js` | `DOMContentLoaded` (con fallback) |

---

## Dettaglio tecnico per file

### `global.js`
**Responsabilità:** script globale del sito, namespace e helper trasversali.

**Metodi del namespace `window.DrwLawyer`:**
- `init()` → chiama i 3 metodi sotto + forza background navy su body (setTimeout 0 per cedere la precedenza a Divi)
- `initBackToTop()` → mostra/nasconde `[data-drw-back-to-top]` oltre 400px scroll; throttle via rAF; scroll smooth al click
- `initColorSchemeSwitcher()` → struttura pronta, **logica TODO Fase 2** (localStorage + classi `drw-scheme-1/2/3`); per ora applica sempre `drw-scheme-1`
- `initNavHelpers()` → aggiunge `is-scrolled` all'header oltre 120px; gestisce skip link `.drw-skip-link`; TODO: navigation.js futuro

**Selettori header (fallback chain):**
```
[data-drw-header] → .et-l--header → header.et-l
```
Se trovato senza `data-drw-header`, lo aggiunge automaticamente (necessario per `header.css`).

**Note per Claude:**
- Il `setTimeout(0)` per forzare il background navy è un workaround per un bug Divi. Non rimuovere senza verificare che Divi non sovrascriva più il background del body.
- `initColorSchemeSwitcher()` ha il codice localStorage **commentato**. In Fase 2, quando le palette CSS per scheme-2 e scheme-3 saranno definite, decommentare il blocco e testare. Non attivare localStorage prima che le variabili CSS siano pronte.
- La soglia `120px` di `initNavHelpers()` è hardcodata. È la stessa indicata in `header.css` come dipendenza. Se cambia, aggiornare entrambi i file in sincronia.
- `TODO` esplicito nel codice: i helper hamburger mobile, aria-expanded, ESC dropdown e aria-current vanno in `navigation.js` (file non ancora creato). Segnalare a Luigi prima di creare il file per stabilire naming e struttura.
- Il namespace `window.DrwLawyer` è esposto per debug e accesso esterno. Se in futuro si aggiungono metodi pubblici, documentarli qui.

---

### `counters.js`
**Responsabilità:** animazione contatori al primo ingresso in viewport.

**Elementi target:**
- `.drw-counters-wrap` → sezione osservata
- `.drw-circle-progress` → circle SVG (legge `r` dall'attributo, fallback 66)
- `.drw-circle-number` → numero centrale
- `.drw-counter-number[data-target][data-suffix]` → counter lineari (fino a 4)

**Logica:**
- Circonferenza calcolata da `r` letto dall'elemento SVG (non hardcodata)
- `animateValue()` → animazione lineare con `requestAnimationFrame`, da 0 al `data-target`
- Circle: `stroke-dashoffset` animato via CSS transition `1.8s ease-out`
- IntersectionObserver con `threshold: 0.35` — fallback: avvia subito se IO non disponibile
- Flag `animated` garantisce una sola esecuzione

**Note per Claude:**
- Il raggio SVG viene letto dal DOM con fallback a `66`. La variabile CSS `--drw-circle-radius: 66` in `counters.css` deve rimanere coerente con l'attributo `r` dell'elemento `<circle>` nel template PHP. Se si modifica il raggio, aggiornare **entrambi**.
- `animateValue()` usa interpolazione lineare (non easing). Se si vuole un easing (es. ease-out), sostituire `progress` con una funzione di easing applicata prima del calcolo di `value`.
- I `data-target` e `data-suffix` devono essere presenti nel markup HTML generato da Divi o dal template PHP. Se mancano, il counter non anima silenziosamente (il controllo `isNaN` lo gestisce).
- `observer.unobserve()` chiamato dopo la prima animazione: il counter non si ri-anima se si torna in viewport. Comportamento intenzionale — non modificare.

---

### `justice.js`
**Responsabilità:** scroll reveal della sezione hero `.drw-justice`.

**Logica:**
- IntersectionObserver con `threshold: 0.15` e `rootMargin: '0px 0px -30% 0px'`
- Aggiunge classe `.drw-is-visible` all'elemento (letta da `justice.css` per la transizione)
- Fallback se IO non disponibile: aggiunge la classe immediatamente
- Avvio su `window load` (non DOMContentLoaded) per attendere il rendering Divi

**Note per Claude:**
- Usa `window load` invece di `DOMContentLoaded`: scelta deliberata per attendere che Divi abbia finito di costruire il DOM. Non cambiare in `DOMContentLoaded` senza testare con Divi attivo.
- Il `rootMargin: '-30% 0px'` bottom significa che l'animazione parte quando l'elemento è già dentro il viewport di 30%. Se l'elemento è alto, potrebbe non attivarsi correttamente. Testare su mobile (viewport più piccolo).
- Classe `.drw-is-visible` è il contratto CSS-JS: se rinominata in uno dei due file, aggiornarla nell'altro.

---

### `statue.js`
**Responsabilità:** gestione completa della sezione statua interattiva.

**Architettura multi-istanza:** supporta più `.drw-statue-section-wrap` nella stessa pagina tramite `forEach(initStatueSection)`.

**Elementi gestiti per statua:**
- `.drw-flip-card` + `.drw-flip-close` → flip 3D con typewriter al retro
- `.drw-typewriter` → effetto macchina da scrivere, testo da `data-drw-typewriter`
- `.drw-hotspot[data-action]` → 4 azioni: `flip`, `video`, `panel`, `scroll`
- `#drw-video-overlay` + `#drw-video-player` → lightbox video (globale, uno per pagina)
- `#drw-panel-metodologia` → slide-in panel (globale)

**Logica touch:**
- `isTouch` rilevato a inizio script (una volta per pagina)
- Su touch: primo tap apre tooltip, secondo tap esegue l'azione
- Su desktop: click esegue direttamente l'azione

**Tasto ESC:** chiude video + panel + flip + tutti i tooltip.

**Hotspot data attributes:**
```html
data-action="flip|video|panel|scroll"
data-video="url-video.mp4"
data-panel="id-panel"
data-target=".selector-sezione"
```

**Note per Claude:**
- `#drw-video-overlay` e `#drw-panel-metodologia` sono **globali** (getElementById): devono esistere **una sola volta** nel DOM anche se ci sono più sezioni statua. Se si replica la sezione, NON replicare questi due elementi.
- Il `data-drw-typewriter` è letto dall'attributo del wrapper `.drw-statue-section-wrap`. Se non presente, usa il testo latino di default. Per personalizzare il testo per cliente, aggiungere l'attributo nel markup Divi.
- `console.log('[DRW Statue] Init section: ...')` è presente nel codice. **Rimuovere prima del deploy in produzione** (o condizionarlo a un flag debug).
- `document.body.style.overflow = 'hidden'` quando il panel è aperto: potrebbe interagire con altri plugin che usano lo stesso pattern (es. lightbox, modal CF7). Testare in presenza di terze parti.
- `getHeaderHeight()` ha una fallback chain di 4 selettori per trovare l'header Divi. Se il template header cambia struttura, verificare che uno dei selettori funzioni ancora.
- La funzione `openVideo()` chiama `.play().catch()` per gestire il blocco autoplay del browser. Il `catch` logga solo un warning — comportamento corretto, non modificare.

---

### `timeline.js`
**Responsabilità:** progress bar animata e attivazione righe timeline.

**Logica:**
- `positionBar()` → posiziona `.drw-timeline-progress` in absolute rispetto a `.drw-timeline`, allineata alla colonna marker (desktop) o al centro (mobile ≤767px)
- `updateTimelineProgress()` → calcola `fillHeight` in base allo scroll e aggiorna height della barra; applica `.drw-marker--passed` e `.drw-card--passed` ai marker superati
- IntersectionObserver con `threshold: 0.5` e `rootMargin: '-20%'` → gestisce `.drw-timeline-row--active` per evidenziare la riga attiva
- Avvio su `window load` + `setTimeout(150ms)` per attendere il layout Divi

**Breakpoint JS:** ≤767px (corrisponde al breakpoint mobile in `timeline.css`).

**Viewport reference:**
- Desktop: `window.innerHeight * 0.5` (metà viewport)
- Mobile: `window.innerHeight * 0.85`

**Note per Claude:**
- Il `setTimeout(150ms)` al load è necessario perché Divi calcola le altezze delle colonne dopo il parsing. Non rimuovere senza testare che `positionBar()` funzioni correttamente.
- `progressBar.parentElement !== timeline` → se la barra non è figlia diretta di `.drw-timeline`, viene spostata. Safety check per layout Divi. Non rimuovere.
- `console.warn('Timeline: nessuna riga trovata.')` → da rimuovere o condizionare a flag debug prima del deploy.
- Il breakpoint `767px` in JS deve rimanere sincronizzato con il breakpoint mobile in `timeline.css`. Se uno cambia, aggiornare entrambi.
- `window.addEventListener('resize', positionBar)` e `window.addEventListener('resize', updateTimelineProgress)` sono due listener separati senza throttle. Se le performance diventano un problema, aggiungere debounce.

---

### `drw-demo-switcher.js`
**Responsabilità:** switcher palette colori per il sito demo.

**Palette disponibili:** `classic`, `verde`, `mogano`  
**API pubblica:** `window.drwSwitcher.apply('classic')`, `.reset()`, `.list`  
**Persistenza:** `sessionStorage` (si azzera alla chiusura del browser)

**Note per Claude:**
- ⚠️ **Solo per sviluppo/demo.** Come `global.css` contiene il switcher CSS, questo file contiene la logica JS. Entrambi vanno disabilitati/rimossi prima del deploy. L'enqueue in `functions.php` dovrebbe essere condizionato a un controllo Customizer (già previsto secondo il commento nel file).
- Le palette `verde` e `mogano` sono complete di tutte le variabili `--drw-*`. Se si aggiungono nuove variabili globali al progetto, aggiungerle anche a tutte e 3 le palette per evitare che rimangano al valore di fallback quando si cambia palette.
- `document.body.style.setProperty('background-color', ...)` in `applyPalette()`: workaround per lo stesso problema di Divi gestito in `global.js`. Le due soluzioni devono rimanere coerenti.

---

### `custom.js`
**Responsabilità:** placeholder per customizzazioni specifiche del cliente.

- File attualmente quasi vuoto (28 B).
- Enqueuato in `functions.php` — non rimuovere l'enqueue anche se vuoto.

**Note per Claude:**
- Questo file è il punto di ingresso per codice cliente specifico che non deve finire negli altri file. Prima di aggiungere codice qui, valutare se è più corretto creare un file dedicato (es. `cpt-casi.js`, `navigation.js`) o inserirlo in `custom.js`.
- Se si aggiunge codice qui, usare lo stesso pattern IIFE degli altri file per isolare lo scope.

---

## Issues aperte (da discutere con Luigi)

| # | File | Tipo | Descrizione | Priorità |
|---|------|------|-------------|----------|
| 1 | `drw-demo-switcher.js` | 🔴 Cleanup deploy | Rimuovere/disabilitare prima del lancio (coordinare con `global.css`) | Alta |
| 2 | `statue.js` | 🟡 Cleanup | `console.log` debug da rimuovere prima del deploy | Media |
| 3 | `timeline.js` | 🟡 Cleanup | `console.warn` da rimuovere o condizionare a flag debug | Media |
| 4 | `global.js` | 🟡 TODO Fase 2 | `initColorSchemeSwitcher()` — logica localStorage commentata, da attivare in Fase 2 | Media |
| 5 | `global.js` | 🔵 TODO futuro | `navigation.js` da creare per hamburger mobile, aria-expanded, ESC dropdown | Bassa |
| 6 | `timeline.js` | 🔵 Performance | Doppio listener `resize` senza debounce — valutare se necessario | Bassa |

---

## Dipendenze CSS-JS (contratti tra file)

Queste coppie devono rimanere sincronizzate: se si modifica uno, verificare l'altro.

| CSS | JS | Contratto |
|-----|----|-----------|
| `justice.css` → `.drw-is-visible` | `justice.js` | Classe aggiunta da JS, letta da CSS per transizione |
| `header.css` → `.is-scrolled` | `global.js` → `initNavHelpers()` | Soglia 120px in entrambi |
| `counters.css` → `--drw-circle-radius: 66` | `counters.js` → fallback `r = 66` | Raggio SVG circle |
| `timeline.css` → breakpoint ≤767px | `timeline.js` → `window.innerWidth <= 767` | Breakpoint mobile |
| `statue.css` → `.drw-hotspot--active` | `statue.js` → `closeAllTooltips()` | Classe tooltip aperto |
| `statue.css` → `.drw-video--open` | `statue.js` → `openVideo()`/`closeVideo()` | Classe lightbox aperto |
| `statue.css` → `.drw-panel--open` | `statue.js` → `openPanel()`/`closePanel()` | Classe panel aperto |
| `statue.css` → `.drw-flipped` | `statue.js` → `openFlip()`/`closeFlip()` | Classe flip card |
| `timeline.css` → `.drw-marker--passed` `.drw-card--passed` | `timeline.js` | Classi marker/card superati |
| `timeline.css` → `.drw-timeline-row--active` | `timeline.js` | Classe riga attiva |
