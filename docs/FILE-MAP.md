# FILE-MAP — Dr.Web Lawyer Child Theme
> Fotografia: 3 maggio 2026
> v1.0 — Mappa operativa ottimizzata per sessioni AI (Perplexity + Claude).
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
│   ├── custom-post-types.php   # Registra tutti i CPT con costanti DRW_CPT_*
│   ├── taxonomies.php          # Tassonomie: practice-area, case-type, ecc.
│   ├── enqueue.php             ⭐ Nodo centrale CSS/JS — caricamento condizionale
│   ├── helpers.php             ⭐ drwstudio — unico access point dati studio
│   ├── options.php             ⭐⭐ Impostazioni Studio (ACF Free) — file più grande
│   ├── schema.php              ⭐⭐ JSON-LD: LegalService, LocalBusiness, Person...
│   ├── customizer.php          # WP Customizer: palette, tipografia, identità
│   ├── testimonianze-template.php  # Rendering CPT Testimonianze (loop, slider)
│   ├── shortcodes-testimonianze.php # Shortcode per blocchi testimonianze in Divi
│   └── demo-switcher.php       # Switcher demo A/B — solo uso interno preview
├── js/
│   ├── global.js               ⭐ Namespace DrwLawyer: back-to-top, sticky, nav
│   ├── statue.js               ⭐ Animazioni statua + hotspot interattivi
│   ├── counters.js             # Animazione contatori on-scroll (IntersectionObserver)
│   ├── timeline.js             # Animazione timeline storica studio
│   ├── justice.js              # Micro-script bilancia/giustizia
│   ├── drw-demo-switcher.js    # Switcher front-end demo A/B — solo preview
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
| `archive-area.php` | 2.3 KB | Aree di Attività | `DRW_CPT_AREAS` |
| `archive-avvocati.php` | 3.4 KB | Professionisti/Avvocati | `DRW_CPT_PROFESSIONALS` |
| `archive-casi.php` | 3.0 KB | Casi | `DRW_CPT_CASES` |
| `archive-sedi.php` | 3.4 KB | Sedi | `DRW_CPT_LOCATIONS` |

### CPT — Single

| File | Dim. | CPT | Costante |
|------|------|-----|----------|
| `single-area.php` | 1.9 KB | Area di Attività | `DRW_CPT_AREAS` |
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
| `customizer.php` | 19.5 KB | WP Customizer: palette, tipografia, identità visiva | Roberta = riferimento per font/palette |
| `testimonianze-template.php` | 9.8 KB | Rendering CPT Testimonianze: query, output HTML slider | Companion di `shortcodes-testimonianze.php` |
| `shortcodes-testimonianze.php` | 4.1 KB | Shortcode per blocchi testimonianze in Divi | Richiama `testimonianze-template.php` |
| `demo-switcher.php` | 1.2 KB | Switching server-side demo A/B | Solo uso interno — non attivare su siti cliente |

---

## CARTELLA `css/` — Stili modulari

| File | Dim. | Ruolo | Accoppiato con |
|------|------|-------|----------------|
| `global.css` | 1.8 KB | Foundation CSS: variabili, reset, utility base | `js/global.js` |
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
| `drw-demo-switcher.js` | 4.9 KB | Switcher front-end demo A/B | **Solo demo/preview** |
| `custom.js` | 28 B | Placeholder JS personalizzato cliente | Quasi vuoto — override cliente qui |

---

## CARTELLA `template-parts/`

| File | Dim. | Ruolo |
|------|------|-------|
| `no-results.php` | 1.8 KB | Output "Nessun risultato" per ricerca e archivi vuoti |
| `testimonianze/card-testimonianza.php` | 4.2 KB | Card testimonianza: avatar, nome, ruolo, stelle, testo |

---

## CPT CORE — Riepilogo costanti e slug

| CPT | Costante PHP | Slug WP | Archive | Single | Taxonomy |
|-----|-------------|---------|---------|--------|----------|
| Professionisti | `DRW_CPT_PROFESSIONALS` | `avvocati` | `archive-avvocati.php` | `single-avvocati.php` | — |
| Aree di Attività | `DRW_CPT_AREAS` | `area` | `archive-area.php` | `single-area.php` | `practice-area` |
| Casi | `DRW_CPT_CASES` | `casi` | `archive-casi.php` | `single-casi.php` | `case-type` |
| Sedi | `DRW_CPT_LOCATIONS` | `sedi` | `archive-sedi.php` | `single-sedi.php` | — |
| Testimonianze | `DRW_CPT_TESTIMONIALS` | `testimonianza` | **No archivio pubblico** | `single-testimonianza.php` | — |

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

## FILE MANCANTI / DA CREARE (prossimi step)

| File | Tipo | Priorità | Note |
|------|------|----------|------|
| `page-templates/*.php` | Page template Divi | Alta | Template da associare alla colonna destra del builder Divi |
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

*Fotografia stato: 3 maggio 2026 — v1.0*
*Autori: Luigi + Perplexity AI*
