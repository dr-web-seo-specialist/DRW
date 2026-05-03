# Dr.Web Lawyer — Mappa CSS completa + Note per Claude
**Versione:** 2026-05-03  
**Cartella:** `/css` (child theme)  
**File totali:** 7  
**Fonte:** analisi diretta dei sorgenti inviati da Luigi

---

## Mappa file `/css`

| File | Dim. | Modificato | Scope | Stato |
|------|------|------------|-------|-------|
| `counters.css` | 16.5 KB | 08/Apr/2026 | Sezione statistiche/contatori | ✅ Stabile |
| `global-layout.css` | 921 B | 09/Apr/2026 | Bordi body + compensazione header | ✅ Stabile |
| `global.css` | 1.8 KB | 08/Apr/2026 | Scroll dot + demo switcher palette | ⚠️ Contiene switcher demo |
| `header.css` | 17.4 KB | 03/Mag/2026 | Header completo Pattern A | ✅ Aggiornato oggi |
| `justice.css` | 1.6 KB | 04/Apr/2026 | Hero titolo con bg-clip image | ⚠️ URL hardcodata |
| `statue.css` | 16.6 KB | 09/Apr/2026 | Sezione statua + hotspot + panel + lightbox | ✅ Stabile |
| `timeline.css` | 7.7 KB | 08/Apr/2026 | Timeline verticale con progress bar | ✅ Stabile |

---

## Dettaglio tecnico per file

### `global-layout.css`
**Responsabilità:** layout base del sito.

- `border-left/right` sul `body`: `24px` desktop → `10px` tablet (≤980px) → `6px` mobile (≤767px)
- Variabile chiave: `--drw-navy` con fallback `#1a1f3c`
- `.drw-first-section`: `padding-top: 159px !important; margin-top: -159px !important;`  
  → compensa l'altezza header sticky per evitare flash bianco durante scroll

**Note per Claude:**
- Il valore `159px` è hardcodato. Se l'altezza dell'header cambia (es. aggiunta riga utility o modifica padding), questo valore va aggiornato manualmente.
- Il file è minimo (921 B) e non va espanso: ogni regola di layout globale aggiuntiva deve essere valutata con Luigi prima di inserirla qui.

---

### `global.css`
**Responsabilità:** componenti globali trasversali alle pagine.

- `.drw-scroll-dot`: indicatore scroll animato (bounce 2s), nascosto con `.drw-hidden`
- `.drw-demo-switcher`: pannello palette switcher fixed bottom-right, z-index 99999

**Note per Claude:**
- ⚠️ **Il `.drw-demo-switcher` è solo per sviluppo/demo.** Va rimosso o commentato prima del deploy in produzione. Luigi deve confermare quando rimuoverlo.
- Se si aggiungono altri componenti globali (es. cookie banner, floating WhatsApp button), valutare se inserirli qui o in un file dedicato. La scelta dipende dalla dimensione: sotto ~1 KB → qui; sopra → file separato.

---

### `header.css`
**Responsabilità:** header completo Pattern A (Landing Header).

**Architettura classi:**
- `.drw-header-main` + `.et_pb_sticky_placeholder` → background navy
- `[data-drw-header]` → transizioni globali header
- `.is-scrolled` → collassa riga utility, aggiunge box-shadow semi-trasparente
- `.is-transparent` → header trasparente per pagine hero
- `.drw-header-utility-row` → riga top con telefono/icone, sparisce allo scroll e su ≤980px
- `.drw-btn-menu` / `.drw-btn-accent` → voce CTA menu e bottone colonna destra
- `.mobile_menu_bar` → hamburger con area click espansa a tutta colonna su ≤980px

**Variabili usate:**
`--drw-navy`, `--drw-gold`, `--drw-gold-light`, `--drw-bg-light`, `--drw-text`, `--drw-text-on-dark`, `--drw-transition-base`

**Note per Claude:**
- Il selettore `.et_pb_section.et_pb_section_0_tb_header.et_pb_sticky` è specifico per il template header TB attuale. Se il template header viene riassegnato o rinominato, questo selettore rompe il comportamento sticky.
- La riga utility usa `max-height: 0 + opacity: 0` per il collapse (non `display: none`) per mantenere la transizione CSS. Non modificare questo pattern senza testare la transizione.
- Il valore `120px` di scroll per trigger `.is-scrolled` è gestito in `global.js` (non in questo file). Se si vuole cambiare la soglia, modificare il JS, non il CSS.
- `.drw-btn-accent` appare sia come classe menu voice che come classe modulo Button Divi: le due varianti hanno selettori distinti — fare attenzione a non unificarli per non creare conflitti.
- La parentesi graffa spuria segnalata in precedenza è stata confermata corretta da Luigi.

---

### `counters.css`
**Responsabilità:** sezione statistiche/contatori.

**Architettura:** Section navy > Row con cornice gold (inset box-shadow) > 3 colonne (laterali navy, centrale bianca con fossa top/bottom) > pannello carta intestata > riga circle counter + riga 4 linear counters.

**Classi principali:**
- `.drw-section-counters` → background navy
- `.drw-row-counters` → cornice navy 10px + filo gold inset 2px
- `.drw-col-navy-left` / `.drw-col-navy-right` → colonne laterali, `display: none` su ≤980px
- `.drw-col-content` → colonna centrale bianca, fossa via `::before`/`::after`
- `.drw-counters-inner-panel` → pannello carta intestata con box-shadow multi-layer `color-mix()`
- `.drw-counter-circle-wrap` + `.drw-circle-svg` → circle counter SVG animato (stroke-dashoffset 1.5s)
- `.drw-counters-row--linear` → 4 counter flex con separatori gold

**Variabili locali** (mappate su globali con fallback):
`--drw-counter-navy` ← `--drw-navy`, `--drw-counter-gold` ← `--drw-gold`, `--drw-counter-gray` ← `--drw-text-light`, `--drw-counter-near-black` ← `--drw-navy-dark`

**Breakpoints:** 5 livelli — >1024, 1024-981, 980-861, 860-768, ≤767

**Note per Claude:**
- `--drw-circle-size: 220px` e `--drw-circle-radius: 66` sono `:root` hardcodati. Il raggio 66 deve essere coerente con il `r` dell'elemento `<circle>` nel SVG nel template PHP/HTML. Se si cambia il raggio SVG, aggiornare anche questa variabile.
- La fossa `::before`/`::after` su `.drw-col-content` ha `height: 26px`. Se si aumenta il padding top/bottom della colonna, verificare che la fossa non risulti troppo piccola visivamente.
- `.drw-pen-slot` e `.drw-letterhead-logo-slot` sono elementi decorativi posizionati in absolute: le coordinate cambiano per ogni breakpoint. Se si sostituisce la penna o il logo, verificare i posizionamenti su tutti i breakpoint.
- Su ≤767px la fossa viene nascosta (`display: none` su `::before`/`::after`).

---

### `justice.css`
**Responsabilità:** hero titolo con immagine in background-clip text.

**Classi principali:**
- `.drw-justice` → scroll reveal (opacity 0 → 1, translateY 24px → 0), triggered da `.drw-is-visible`
- `.drw-justice-heading` → Prata, `clamp(80px, 12vw, 180px)`, uppercase
- `.drw-justice-text` → background-clip text con animazione verticale 30s

**Note per Claude:**
- ⚠️ **URL immagine hardcodata:** `https://doctorwebsolutions.it/template-avvocati-drweb-demo/wp-content/uploads/2026/03/law-firm-19-scaled-1.jpg`  
  Prima di distribuire il template a nuovi clienti, questa URL deve essere:
  1. Sostituita con un percorso relativo all'upload locale del cliente, oppure
  2. Convertita in una variabile CSS impostata via PHP con `get_theme_mod()` o campo ACF.
  Luigi deve decidere l'approccio prima del deploy.
- Font `Prata` usata anche in `statue.css` e `counters.css`. Verificare che venga caricata **una sola volta** in `functions.php` (enqueue Google Fonts). Attualmente il caricamento è in `functions.php` — non aggiungere ulteriori `@import` in CSS.
- La classe trigger `.drw-is-visible` deve essere aggiunta da JS (IntersectionObserver o scroll handler). Verificare che `global.js` o un file JS dedicato la gestisca.

---

### `statue.css`
**Responsabilità:** sezione statua con hotspot interattivi, flip card, panel slide-in, video lightbox.

**Architettura:**
- `.drw-statue-inner-row` → bordo gold 3px + inset navy 6px
- `.drw-flip-card` → 3D con `preserve-3d`, `.drw-flipped` → `rotateY(180deg)` 0.7s
- `.drw-hotspot` + `.drw-hotspot-btn` → bottoni circolari pulsanti (`drw-pulse` 2s, scale 1→2.4)
- `.drw-hotspot-box` → tooltip 240px, direzione `--right`/`--left`, 5° hotspot tooltip verso l'alto
- `.drw-panel` → slide-in fixed da destra, 380px → 100% su ≤780px
- `.drw-video-overlay` → lightbox video fixed z-index 99999

**Hotspot positions per breakpoint:**
Ogni breakpoint riposiziona gli hotspot 2-5 con coordinate `top/left` in `!important`. Attualmente 6 livelli di breakpoint per le coordinate.

**Note per Claude:**
- Le coordinate hotspot (`top/left` in `!important`) sono calibrate sull'immagine specifica della statua attualmente in uso. Se l'immagine della statua viene sostituita o ridimensionata, **tutte le coordinate per tutti i breakpoint devono essere ricalibrate**.
- `.drw-panel` usa `position: fixed` con z-index 9999. Se il sito ha altri elementi fixed (es. cookie banner, chat widget), verificare conflitti di z-index.
- Il video lightbox usa `display: none` → `flex` (non opacity/visibility): non c'è transizione di apertura. Se si vuole aggiungere un fade-in, occorre un refactor con opacity + pointer-events.
- Font `Prata` usata per `.drw-statue-title`, `.drw-statue-feature h4`, `.drw-statue-bigtitle`, `.drw-typewriter`, `.drw-hotspot-box h4`. Vedi nota in `justice.css`.
- `.drw-flip-hint` e `.drw-flip-cite` usano `color-mix()` con `--drw-text-on-dark`: verificare che la variabile sia sempre definita prima di questo file (dipendenza dal Customizer o da un file di variabili).

---

### `timeline.css`
**Responsabilità:** timeline verticale con progress bar animata via JS.

**Architettura:**
- `.drw-col-marker` (bordo gold destra) + `.drw-col-card` alternati sinistra/destra
- `.drw-timeline-progress` → barra 10px, `height: 0` → animata via JS, glow bianco
- `.drw-timeline-row` + `.drw-timeline-row--active` → card `translateY(-8px)`, marker glow, connector bianco
- `.drw-marker` + `.drw-marker--passed` → triplo ring (navy + white + gold glow)
- `.drw-connector` → trattino 40×7px marker → card
- `.drw-timeline-badge` → pillola gold, font-size 20px, border 5px

**Mobile ≤767px:**
- Marker e connector nascosti, progress bar resta
- Colonne full-width, pseudo-elemento `::before` simula linea centrale gold
- Card centrate 80%, `.drw-card--passed` con outline gold + glow intenso

**Note per Claude:**
- La barra `.drw-timeline-progress` viene **spostata e animata via JS** (verosimilmente `global.js` o un file dedicato). Il CSS definisce solo la forma statica. Non modificare il CSS della progress bar senza verificare la logica JS corrispondente.
- `.drw-marker` ha `z-index: 9999 !important` e `.drw-card` ha `z-index: 3 !important`. Questi valori alti sono necessari per la corretta sovrapposizione con la barra progress. Non abbassarli senza testare.
- `.drw-marker-wrap` e `.drw-marker-wrap--right` hanno `height: 280px` (desktop) / `250px` (mobile): questo valore controlla la spaziatura verticale tra le righe della timeline. Se si aggiungono card più alte, potrebbe essere necessario aumentare questa altezza.
- Il badge usa `top: -40px` in absolute rispetto al `.drw-marker-wrap`: se si cambia l'altezza del badge (font-size, padding), aggiustare questo offset.

---

## Issues aperte (da discutere con Luigi)

| # | File | Tipo | Descrizione | Priorità |
|---|------|------|-------------|----------|
| 1 | `justice.css` | 🔴 Bug deploy | URL immagine hardcodata a dominio demo | Alta |
| 2 | `global.css` | 🟡 Cleanup | `.drw-demo-switcher` da rimuovere prima del lancio | Media |
| 3 | `global-layout.css` | 🟡 Manutenzione | Valore `159px` header hardcodato — aggiornare se altezza header cambia | Media |
| 4 | `justice.css` / `statue.css` / `counters.css` | 🟡 Verifica | Font Prata caricata una sola volta in `functions.php`? | Media |
| 5 | `statue.css` | 🟡 Manutenzione | Coordinate hotspot legate all'immagine statua attuale — ricalibrazione se immagine cambia | Media |
| 6 | `header.css` | 🔵 Info | Selettore `.et_pb_section_0_tb_header` specifico del TB header attuale — attenzione rinominazione | Bassa |

---

## Variabili CSS globali usate (da Customizer / tema padre)

Tutte le variabili sotto devono essere definite **prima** del caricamento di questi file CSS, tipicamente in `functions.php` via `wp_add_inline_style()` o nel tema padre Divi.

| Variabile | Usata in | Fallback hardcodato |
|-----------|----------|---------------------|
| `--drw-navy` | tutti | `#1a1f3c` / `#1A1F3C` |
| `--drw-navy-dark` | counters, statue, timeline | `#11162B` |
| `--drw-gold` | tutti | `#C9A96E` |
| `--drw-gold-light` | header, global | `#E2C99A` |
| `--drw-bg-light` | header, global | `#F8F6F1` |
| `--drw-text` | header | `#2C2C2C` |
| `--drw-text-light` | counters | `#6b7280` |
| `--drw-text-on-dark` | statue, timeline, header | `#ffffff` |
| `--drw-transition-base` | header | `0.3s ease` |
| `--et_body_font` | counters | `"Poppins", sans-serif` |
