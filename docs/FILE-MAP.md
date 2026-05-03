# FILE-MAP — Dr.Web Lawyer Child Theme
> Fotografia: 3 maggio 2026 — v1.1
> Source of truth: documenti Google Drive (00_MASTER, INDEX_DRIVE, snapshot 15/Apr/2026).
> Aggiornare questo file ogni volta che si aggiunge/rimuove/rinomina un file nel child theme.

---

## REGOLE CRITICHE (leggere prima di qualsiasi modifica)

- **`drwstudio`** è l'unico access point per i dati studio → sempre via helpers, mai query dirette
- **Costanti CPT**: usare sempre `DRW_CPT_*` (mai slug hardcoded)
- **ACF Free only**: niente `acf_add_options_page()`, niente ACF Pro
- **No inline JS**: tutto JS nel file dedicato in `js/`, enqueued via `inc/enqueue.php`
- **No `query_posts()`**: usare `WP_Query` o `get_posts()`
- **Slug in inglese**: costanti e slug sempre EN; le label utente possono essere localizzate
- **Vocabolario tassonomico chiuso**: non estendere `03_Tassonomie` senza approvazione Luigi
- **`functions.php` è solo bootstrap**: nessuna logica specifica, solo include di `inc/*`

---

## STRUTTURA ALBERO COMPLETA

```
drw-lawyer-child/
├── css/
│   ├── global.css              # Foundation CSS: variabili, reset, utility base
│   ├── global-layout.css       # Frame visivo: bordi decorativi, background, padding
│   ├── header.css              ⭐ Pattern A (stabile) + B/C (in sviluppo)
│   ├── statue.css              ⭐ Hero statua + hotspot interattivi
│   ├── counters.css            ⭐ Sezione contatori "fossa" navy/oro
│   ├── timeline.css            # Timeline storia studio
│   └── justice.css             # Componente bilancia animata
├── img/
│   └── pen.png                 # Asset decorativo (penna stilografica, 304.5 KB)
├── inc/
│   ├── custom-post-types.php   ⭐ Registra tutti i CPT con costanti DRW_CPT_*
│   ├── taxonomies.php          # Tassonomie: practice-area, case-type, ecc.
│   ├── enqueue.php             ⭐ Nodo centrale CSS/JS — caricamento condizionale
│   ├── helpers.php             ⭐ drwstudio — unico access point dati studio
│   ├── options.php             ⭐⭐ Impostazioni Studio (ACF Free) — file più grande
│   ├── schema.php              ⭐⭐ JSON-LD: LegalService, LocalBusiness, Person...
│   ├── customizer.php          ⭐ WP Customizer: palette, tipografia, identità, switcher
│   ├── testimonianze-template.php  # Rendering CPT Testimonianze (loop, slider)
│   ├── shortcodes-testimonianze.php # Shortcode per blocchi testimonianze in Divi
│   └── demo-switcher.php       # Switcher demo palette — solo uso interno preview
├── js/
│   ├── global.js               ⭐ Namespace DrwLawyer: back-to-top, sticky, nav
│   ├── statue.js               ⭐ Animazioni statua + hotspot interattivi
│   ├── counters.js             # Animazione contatori on-scroll (IntersectionObserver)
│   ├── timeline.js             # Animazione timeline storica studio
│   ├── justice.js              # Micro-script bilancia/giustizia
│   ├── drw-demo-switcher.js    # Switcher front-end demo palette — solo preview
│   └── custom.js               # Placeholder JS cliente (quasi vuoto, 28 B)
├── languages/                  # [VUOTA] Predisposta per i18n (IT, EN, ES, DE)
├── page-templates/             # [VUOTA] Predisposta per page template WP/Divi
├── template-parts/
│   ├── testimonianze/
│   │   └── card-testimonianza.php  # Card singola testimonianza (avatar, stelle, testo)
│   └── no-results.php          # Output "Nessun risultato" per ricerca e archivi
├── 404.php                     # Pagina errore 404
├── archive.php                 # Archivio generico (fallback)
├── archive-area.php            # Archivio CPT Aree di Attività
├── archive-avvocati.php        # Archivio CPT Professionisti/Avvocati
├── archive-casi.php            # Archivio CPT Casi
├── archive-sedi.php            # Archivio CPT Sedi
├── front-page.php              # Homepage (smista demo A/B)
├── functions.php               ⭐ BOOTSTRAP — solo include modulari
├── home.php                    # Indice blog/news (se homepage ≠ blog)
├── page.php                    # Template pagina WP generica
├── page-contatti.php           # Template pagina Contatti
├── page-landing.php            # Template landing (ADV, campagne)
├── page-legal.php              # Template pagine legali (Privacy, Cookie)
├── page-macroarea.php          # Template macro-landing aggregata
├── page-special.php            # Template pagine speciali/di servizio
├── search.php                  # Risultati ricerca
├── single-area.php             # Scheda singola Area di Attività
├── single-avvocati.php         # Scheda singola Avvocato/Professionista
├── single-casi.php             # Scheda singola Caso
├── single-sedi.php             # Scheda singola Sede
├── single-testimonianza.php    # Scheda singola Testimonianza (no archivio pubblico)
├── style.css                   # Header dichiarazione child theme (solo metadati)
├── taxonomy-area-pratica.php   # Archivio tassonomia practice-area
└── taxonomy-tipologia-caso.php # Archivio tassonomia case-type
```

---

## ROOT — File PHP principali

### Bootstrap & Core

| File | Dim. | Ruolo | Note |
|------|------|-------|------|
| `style.css` | 492 B | Header dichiarazione child theme. **Non contiene stili reali.** | Richiesto da WP per riconoscere il child theme |
| `functions.php` | 6.4 KB | **Bootstrap centrale**: theme_support, include modulari di `inc/*`, flush permalink. Deve restare MINIMALE. | Modificato 13/Apr |

### Template pagina standard

| File | Dim. | Ruolo | Note |
|------|------|-------|------|
| `page.php` | 994 B | Template pagina WP generica. Layout affidato a Divi. | Fallback |
| `front-page.php` | 1.2 KB | Homepage statica. Smista verso demo A o B. | Modificato 13/Apr |
| `home.php` | 3.1 KB | Indice blog/news (quando homepage ≠ blog). | — |
| `404.php` | 2.0 KB | Pagina errore 404. | — |
| `search.php` | 3.1 KB | Risultati ricerca. | — |
| `archive.php` | 2.3 KB | Archivio generico (fallback). | — |

### Page template speciali

| File | Dim. | Ruolo | Note |
|------|------|-------|------|
| `page-contatti.php` | 1.9 KB | Template pagina Contatti. | Aggiornato 15/Apr |
| `page-landing.php` | 2.3 KB | Template landing ADV/campagne. Layout senza header/footer standard. | Aggiornato 15/Apr |
| `page-legal.php` | 2.4 KB | Template pagine legali (Privacy, Cookie, Termini). | — |
| `page-macroarea.php` | 2.0 KB | Template macro-landing e pagine aggregate. | Aggiornato 15/Apr |
| `page-special.php` | 684 B | Template pagine speciali/di servizio. Stub o redirect. | Aggiornato 15/Apr |

### CPT — Archive

| File | Dim. | CPT | Costante |
|------|------|-----|----------|
| `archive-area.php` | 2.3 KB | Aree di Attività | `DRW_CPT_SERVICES` |
| `archive-avvocati.php` | 3.4 KB | Professionisti/Avvocati | `DRW_CPT_PROFESSIONALS` |
| `archive-casi.php` | 3.0 KB | Casi | `DRW_CPT_CASES` |
| `archive-sedi.php` | 3.4 KB | Sedi | `DRW_CPT_LOCATIONS` |

### CPT — Single

| File | Dim. | CPT | Costante |
|------|------|-----|----------|
| `single-area.php` | 1.9 KB | Area di Attività | `DRW_CPT_SERVICES` |
| `single-avvocati.php` | 2.0 KB | Avvocato/Professionista | `DRW_CPT_PROFESSIONALS` |
| `single-casi.php` | 2.9 KB | Caso | `DRW_CPT_CASES` |
| `single-sedi.php` | 2.6 KB | Sede | `DRW_CPT_LOCATIONS` |
| `single-testimonianza.php` | 939 B | Testimonianza — **no archivio pubblico** | `DRW_CPT_TESTIMONIALS` |

### Taxonomy

| File | Dim. | Slug |
|------|------|------|
| `taxonomy-area-pratica.php` | 3.0 KB | `practice-area` |
| `taxonomy-tipologia-caso.php` | 2.8 KB | `case-type` |

---

## CARTELLA `inc/` — Logica PHP modulare

| File | Dim. | Ruolo | Note critiche |
|------|------|-------|---------------|
| `custom-post-types.php` | 14.1 KB | Registra CPT con costanti `DRW_CPT_*` | Mai hardcodare slug altrove |
| `taxonomies.php` | 6.2 KB | Registra tassonomie funzionali | Allineato 1:1 con `03_Tassonomie` |
| `enqueue.php` | 9.6 KB ⭐ | Nodo centrale enqueue CSS/JS. Caricamento condizionale per moduli. | Aggiornato 15/Apr |
| `helpers.php` | 10.3 KB ⭐ | `drwstudio` = unico access point ai dati studio | Regola permanente: mai query dirette |
| `options.php` | 39.8 KB ⭐⭐ | Pagina "Impostazioni Studio" con field group ACF Free | ACF Free only. File più grande del tema. |
| `schema.php` | 34.3 KB ⭐⭐ | JSON-LD: LegalService, LocalBusiness, Person, Service, FAQPage, Review | Legge da helpers. Allineato a `05_Schema.org` |
| `customizer.php` | 19.5 KB ⭐ | WP Customizer: palette, tipografia, identità visiva, demo switcher | Vedere analisi dettagliata sotto |
| `testimonianze-template.php` | 9.8 KB | Rendering CPT Testimonianze: query, output HTML slider | Companion di `shortcodes-testimonianze.php` |
| `shortcodes-testimonianze.php` | 4.1 KB | Shortcode per blocchi testimonianze in Divi | Richiama `testimonianze-template.php` |
| `demo-switcher.php` | 1.2 KB | Render footer dello switcher palette demo | Solo uso interno — non attivare su siti cliente |

---

### `inc/customizer.php` — Analisi dettagliata

**Struttura**: 5 sezioni nel pannello `drw_panel` (priorità 30 nel Customizer WP).

#### Sezione 1 — Palette & Colori
Registra **15 custom properties CSS** via `wp_head` (priority 5, id `drw-customizer-vars`):

| Gruppo | Variabili CSS | Default |
|--------|--------------|---------|
| Primario (navy) | `--drw-navy`, `--drw-navy-light`, `--drw-navy-dark` | `#1A1F3C`, `#2A2F56`, `#11162B` |
| Accent (oro) | `--drw-gold`, `--drw-gold-light`, `--drw-gold-dark` | `#C9A96E`, `#E2C99A`, `#A8843E` |
| Testo | `--drw-text`, `--drw-text-light` | `#2C2C2C`, `#6B7280` |
| Sfondi | `--drw-bg-light`, `--drw-bg-section` | `#F8F6F1`, `#F2EFE9` |
| Accent 2 (verticali) | `--drw-accent-2`, `--drw-accent-2-light`, `--drw-accent-2-dark` | `#2D6A4F`, `#52B788`, `#1B4332` |
| Testo su scuro | `--drw-text-on-dark`, `--drw-text-on-dark-muted` | `#FFFFFF`, `#E2E8F0` |

**Alias semantici** aggiunti automaticamente in output:
- `--drw-text-primary` → alias di `--drw-text`
- `--drw-text-secondary` → alias di `--drw-text-light`
- `--drw-bg-alt` → alias di `--drw-bg-section`

> ⚠️ **NOTA ARCHITETTURALE**: `transport` per i colori è impostato su `'refresh'` (era `'postMessage'`). Da valutare se ripristinare `postMessage` con JS partial per preview live nel Customizer senza reload.

> 📌 **NOTA VERTICALI**: `--drw-accent-2` è uno slot riservato per verticali futuri (commercialisti, notai, medici). Nel template avvocati non è usato attivamente nei componenti visivi.

#### Sezione 2 — Tipografia
5 controlli select per font family. Font disponibili: Georgia, Palatino, Times New Roman, Arial, Helvetica Neue, Trebuchet MS, Verdana, Poppins, Montserrat, Raleway, Lato, Open Sans, Playfair Display, Merriweather, Cormorant Garamond, EB Garamond.

| Setting | CSS prodotto | Default |
|---------|-------------|---------|
| `drw_font_h1` | `h1 { font-family: ... !important }` | Georgia, serif |
| `drw_font_h2` | `h2 { font-family: ... !important }` | Georgia, serif |
| `drw_font_h3` | `h3 { font-family: ... !important }` | Palatino Linotype |
| `drw_font_h4` | `h4, h5, h6 { font-family: ... !important }` | Helvetica Neue |
| `drw_font_body` | `body, p, li, td, th { font-family: ... !important }` | Helvetica Neue |

> ⚠️ **TASK APERTO**: Con Divi 5 e il nuovo sistema variabili/gradazioni, verificare se i `!important` sui font rimangono necessari o se possono essere rimossi dopo l'analisi del CSS inline Divi 5.

#### Sezione 3 — Identità Studio
- `drw_studio_nome_display` → nome header/footer (fallback: `get_bloginfo('name')`)
- `drw_studio_tagline_display` → tagline (fallback: `get_bloginfo('description')`)
- Helper pubblici: `drw_get_studio_nome()`, `drw_get_studio_tagline()`

#### Sezione 4 — Demo Switcher
- `drw_switcher_enabled` (checkbox, default `true`) → abilita/disabilita enqueue di `drw-demo-switcher.js`
- Helper pubblico: `drw_is_switcher_enabled()`
- **Enqueue condizionato**: verifica esistenza fisica del file JS prima di registrarlo

---

### `inc/custom-post-types.php` — Analisi dettagliata

#### Costanti slug CPT (definite qui, usate in tutto il tema)

| Costante PHP | Valore attuale | Descrizione |
|-------------|----------------|-------------|
| `DRW_CPT_PROFESSIONALS` | `'avvocati'` | Professionisti |
| `DRW_CPT_SERVICES` | `'aree'` | Servizi / Aree di attività |
| `DRW_CPT_CASES` | `'casi'` | Casi / Progetti |
| `DRW_CPT_LOCATIONS` | `'sedi'` | Sedi / Uffici |
| `DRW_CPT_TESTIMONIALS` | `'testimonianze'` | Testimonianze / Social proof |

> ⚠️ **TASK PRE-v1.0 — VERIFICA SLUG**: Gli slug attuali (`avvocati`, `aree`, `casi`, `sedi`, `testimonianze`) sono in **italiano**. Per il mercato anglosassone (US, UK, AU) e per la neutralità verticali vanno valutati. Decisione da prendere **prima della traduzione EN** — modificare gli slug dopo l'indicizzazione Google comporta redirect 301 obbligatori.

#### CPT registrati

| CPT | Slug rewrite | Archive | has_archive | exclude_from_search | show_in_nav_menus | Supports |
|-----|-------------|---------|-------------|--------------------|--------------------|---------|
| Professionisti | `avvocati` | ✅ `/avvocati/` | `true` | — | — | title, editor, thumbnail, excerpt, revisions |
| Aree/Servizi | `aree` | ✅ `/aree/` | `true` | — | — | title, editor, thumbnail, excerpt, revisions |
| Casi | `casi` | ✅ `/casi/` | `true` | — | — | title, editor, thumbnail, excerpt, revisions |
| Sedi | `sedi` | ✅ `/sedi/` | `true` | — | — | title, editor, thumbnail, excerpt, revisions |
| Testimonianze | `testimonianze` | ❌ No archivio | `false` | `true` | `false` | title, editor, thumbnail |

> 📌 **NOTE SEDI**: Il CPT Sedi è predisposto per NAP (Name/Address/Phone), mappa integrata e relazioni con avvocati/aree. Le relazioni (sede ↔ avvocati, sede ↔ aree) sono ancora da implementare via tassonomie o ACF.

> 📌 **NOTE TESTIMONIANZE**: Accessibili solo via shortcode `[drw_testimonianze]` / `[drw_testimonianza id="X"]` o funzione `drw_get_testimonianze()`. Non compaiono in ricerca, nav menu o archivio pubblico.

---

### `inc/demo-switcher.php` — Analisi dettagliata

Hook: `wp_footer` priority 20. Render condizionato da `drw_is_switcher_enabled()` (definita in `customizer.php`).

**Palette disponibili nel widget footer**:
- `classic` (default attivo — classe `drw-palette-active`)
- `verde`
- `mogano`
- `reset`

> ⚠️ **DEBUG attivo nel codice**: il file contiene commenti HTML di debug (`<!-- DRW SWITCHER HOOK FIRED -->`, `<!-- DRW SWITCHER DISABLED/ENABLED -->`). Da rimuovere prima del rilascio v1.0 / consegna cliente.

> 📌 La logica JS del switcher è in `js/drw-demo-switcher.js`. Le palette `verde` e `mogano` devono avere le variabili CSS corrispondenti definite in quel file JS (via `sessionStorage` o sovrascrittura `:root`).

---

## CARTELLA `css/` — Stili modulari

| File | Dim. | Ruolo | Accoppiato con |
|------|------|-------|----------------|
| `global.css` | 1.8 KB | Foundation CSS: variabili `--drw-*`, reset, utility base | `js/global.js` |
| `global-layout.css` | 921 B | Frame visivo: bordi laterali decorativi, background, padding prima sezione | `js/global.js` |
| `header.css` | 17.4 KB ⭐ | Stili header: Pattern A (stabile), B e C commentati. Sticky/transparent/scrolled. | `js/global.js` |
| `statue.css` | 16.6 KB ⭐ | Hero statua con hotspot interattivi. Responsive. | `js/statue.js` |
| `counters.css` | 16.5 KB ⭐ | Contatori "fossa" navy/oro: circle + 4 lineari, animazione on-scroll | `js/counters.js` |
| `timeline.css` | — | Timeline storia studio: layout desktop/mobile | `js/timeline.js` |
| `justice.css` | 1.6 KB | Componente bilancia animata SVG/CSS | `js/justice.js` |

---

## CARTELLA `js/` — Script modulari

| File | Dim. | Ruolo | Note |
|------|------|-------|------|
| `global.js` | 8.0 KB ⭐ | Namespace `DrwLawyer`: back-to-top, color scheme switcher, nav helpers, header sticky | Nessun JS inline altrove |
| `statue.js` | 8.1 KB ⭐ | Animazioni statua + hotspot (tooltip/panel) | Companion di `statue.css` |
| `counters.js` | 3.3 KB | Animazione contatori on-scroll (IntersectionObserver) | Companion di `counters.css` |
| `timeline.js` | 4.5 KB | Animazione e interazione timeline | Companion di `timeline.css` |
| `justice.js` | 846 B | Trigger animazione bilancia | Companion di `justice.css` |
| `drw-demo-switcher.js` | 4.9 KB | Switcher front-end palette demo (classic/verde/mogano/reset) | **Solo demo/preview** |
| `custom.js` | 28 B | Placeholder JS personalizzato cliente | Quasi vuoto — override cliente qui |

---

## CARTELLA `template-parts/`

| File | Dim. | Ruolo |
|------|------|-------|
| `no-results.php` | 1.8 KB | Output "Nessun risultato" per ricerca e archivi vuoti |
| `testimonianze/card-testimonianza.php` | 4.2 KB | Card testimonianza: avatar, nome, ruolo, stelle, testo |

---

## CPT CORE — Riepilogo costanti e slug

| CPT | Costante PHP | Slug WP (IT) | Archive | Single | Taxonomy |
|-----|-------------|--------------|---------|--------|----------|
| Professionisti | `DRW_CPT_PROFESSIONALS` | `avvocati` | `archive-avvocati.php` | `single-avvocati.php` | — |
| Aree/Servizi | `DRW_CPT_SERVICES` | `aree` | `archive-area.php` | `single-area.php` | `practice-area` |
| Casi | `DRW_CPT_CASES` | `casi` | `archive-casi.php` | `single-casi.php` | `case-type` |
| Sedi | `DRW_CPT_LOCATIONS` | `sedi` | `archive-sedi.php` | `single-sedi.php` | — |
| Testimonianze | `DRW_CPT_TESTIMONIALS` | `testimonianze` | **No archivio** | `single-testimonianza.php` | — |

> ⚠️ Slug in italiano — task PRE-v1.0: valutare migrazione a slug EN (`lawyers`, `services`, `cases`, `offices`, `testimonials`) per mercato anglosassone e neutralità verticali.

---

## PALETTE COLORI — Valori attuali (da customizer.php)

| Variabile CSS | Valore | Uso |
|--------------|--------|-----|
| `--drw-navy` | `#1A1F3C` | Colore primario principale |
| `--drw-navy-light` | `#2A2F56` | Primario chiaro |
| `--drw-navy-dark` | `#11162B` | Primario scuro |
| `--drw-gold` | `#C9A96E` | Accent oro |
| `--drw-gold-light` | `#E2C99A` | Accent chiaro |
| `--drw-gold-dark` | `#A8843E` | Accent scuro |
| `--drw-text` | `#2C2C2C` | Testo principale |
| `--drw-text-light` | `#6B7280` | Testo secondario |
| `--drw-bg-light` | `#F8F6F1` | Sfondo chiaro |
| `--drw-bg-section` | `#F2EFE9` | Sfondo sezioni alternate |
| `--drw-accent-2` | `#2D6A4F` | Slot verticali (non usato in avvocati) |
| `--drw-accent-2-light` | `#52B788` | Slot verticali chiaro |
| `--drw-accent-2-dark` | `#1B4332` | Slot verticali scuro |
| `--drw-text-on-dark` | `#FFFFFF` | Testo su sfondo scuro |
| `--drw-text-on-dark-muted` | `#E2E8F0` | Testo secondario su sfondo scuro |

---

## DEMO SWITCHER — Stato

| Palette | Trigger | Stato |
|---------|---------|-------|
| `classic` | `data-drw-palette="classic"` | ✅ Default attivo |
| `verde` | `data-drw-palette="verde"` | 🔧 Variabili da verificare in `drw-demo-switcher.js` |
| `mogano` | `data-drw-palette="mogano"` | 🔧 Variabili da verificare in `drw-demo-switcher.js` |
| `reset` | `data-drw-palette="reset"` | Ripristina palette default |

> ⚠️ **DEBUG DA RIMUOVERE** prima di v1.0: commenti HTML di debug in `demo-switcher.php` (`HOOK FIRED`, `DISABLED`, `ENABLED`).

---

## PATTERN ACCOPPIATI CSS+JS

Caricare sempre condizionalmente via `inc/enqueue.php`:

| Componente | CSS | JS | Dove usato |
|------------|-----|-----|------------|
| Statua + Hotspot | `statue.css` | `statue.js` | Hero landing, homepage Studio |
| Timeline | `timeline.css` | `timeline.js` | Sezione storia studio |
| Contatori | `counters.css` | `counters.js` | Sezione KPI |
| Justice/Bilancia | `justice.css` | `justice.js` | Sezione decorativa legal |
| Demo Switcher | — | `drw-demo-switcher.js` | **Solo demo/preview** |

---

## HEADER PATTERN — Stato attuale

`css/header.css` contiene tutti e tre i pattern in sezioni commentate:

| Pattern | Stato | Contesto d'uso |
|---------|-------|----------------|
| Pattern A | ✅ Stabile | Landing, pagine speciali, ADV |
| Pattern B | 🔧 In sviluppo | Demo Avvocato Singolo |
| Pattern C | 🔧 In sviluppo | Demo Studio Legale |

---

## TASK APERTI — Pre-v1.0

| Task | Priorità | File coinvolti | Note |
|------|----------|----------------|------|
| Estrai CSS inline Divi 5 | 🔴 CRITICO | — | Prerequisito per architettura variabili |
| Rimuovi debug HTML da demo-switcher.php | 🟡 Media | `inc/demo-switcher.php` | Prima di qualsiasi consegna cliente |
| Verifica palette verde/mogano in switcher JS | 🟡 Media | `js/drw-demo-switcher.js` | Testare che le variabili CSS vengano sovrascritte correttamente |
| Verifica slug CPT per mercato EN/AU/US | 🟡 Media | `inc/custom-post-types.php` | Slug attuali in IT — decisione prima traduzione EN |
| Valuta `postMessage` vs `refresh` per colori Customizer | 🟢 Bassa | `inc/customizer.php` | Preview live senza reload |
| Verifica `!important` font con Divi 5 | 🟢 Bassa | `inc/customizer.php` | Dopo analisi CSS Divi 5 |
| Relazioni Sedi ↔ Avvocati ↔ Aree | 🟡 Media | `inc/custom-post-types.php` + ACF | Da implementare via tassonomie o ACF |
| Implementa `page-templates/*.php` | 🔴 Alta | `page-templates/` | Cartella vuota — necessaria per Divi |
| File i18n `languages/` | 🟢 Bassa | `languages/` | Dopo stabilizzazione stringhe |

---

## FILE MANCANTI / DA CREARE (prossimi step)

| File | Tipo | Priorità | Note |
|------|------|----------|------|
| `page-templates/*.php` | Page template Divi | Alta | Cartella attualmente vuota |
| `languages/drw-lawyer-child-it_IT.po` | Localizzazione | Media | Dopo stabilizzazione stringhe |

---

## DOCUMENTI UFFICIALI CORRELATI

| Documento | Dove si trova | Contenuto |
|-----------|--------------|-----------|
| `INDEX_DRIVE` | Google Drive | Mappa completa file + link diretti |
| `00_MASTER` | Drive + Space Claude | Architettura & Regole permanenti |
| `01_Templates PHP` | Google Drive | Elenco file & mappatura dettagliata |
| `03_Tassonomie` | Google Drive | 49 categorie + 274 tag. **Vocabolario chiuso.** |
| `05_Schema.org` | Google Drive | Architettura JSON-LD |
| `ANALISISTATO` | Google Drive | Snapshot operativo marzo 2026 |

---

*Fotografia stato: 3 maggio 2026 — v1.1*
*File analizzati in questa versione: `inc/customizer.php`, `inc/custom-post-types.php`, `inc/demo-switcher.php`*
*Autori: Luigi + Perplexity AI*
