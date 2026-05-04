# FILE-MAP — Dr.Web Lawyer Child Theme
> Fotografia: 3 maggio 2026 — v1.3
> Source of truth: documenti Google Drive (00_MASTER, INDEX_DRIVE, snapshot 15/Apr/2026).
> Aggiornare questo file ogni volta che si aggiunge/rimuove/rinomina un file nel child theme.

---

## REGOLE CRITICHE (leggere prima di qualsiasi modifica)

- **`drwstudio`** è l'unico access point per i dati studio → sempre via helpers, mai query dirette
- **Costanti CPT**: usare sempre `DRW_CPT_*` (mai slug hardcoded in nessun file)
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
│   ├── taxonomies.php          # Tassonomie: area-pratica, tipologia-caso, tipologia-cliente
│   ├── enqueue.php             ⭐ Nodo centrale CSS/JS — caricamento condizionale
│   ├── helpers.php             ⭐ drwstudio — unico access point dati studio
│   ├── options.php             ⭐⭐ Impostazioni Studio (ACF Free) — file più grande
│   ├── schema.php              ⭐⭐ JSON-LD: LegalService, Review, AggregateRating
│   ├── customizer.php          ⭐ WP Customizer: palette, tipografia, identità, switcher
│   ├── testimonianze-template.php  # Query + rendering CPT Testimonianze
│   ├── shortcodes-testimonianze.php # Shortcode [drw_testimonianza] / [drw_testimonianze]
│   └── demo-switcher.php       # Render widget footer switcher palette — solo uso interno preview
├── js/
│   ├── global.js               ⭐ Namespace DrwLawyer: back-to-top, sticky, nav
│   ├── statue.js               ⭐ Animazioni statua + hotspot interattivi
│   ├── counters.js             # Animazione contatori on-scroll (IntersectionObserver)
│   ├── timeline.js             # Animazione e interazione timeline
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
├── taxonomy-area-pratica.php   # Archivio tassonomia area-pratica
└── taxonomy-tipologia-caso.php # Archivio tassonomia tipologia-caso
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

> ⚠️ **NOTA `single-testimonianza.php`**: La dimensione di 939 B suggerisce un file stub/redirect. Se un utente raggiunge direttamente `/testimonianze/{slug}/`, verificare che il template restituisca un redirect a 301 verso la pagina "Dicono di noi" oppure un 404 — **non** una pagina vuota. Vedere TODO-CLAUDE.

### Taxonomy

| File | Dim. | Slug tassonomia |
|------|------|-----------------|
| `taxonomy-area-pratica.php` | 3.0 KB | `area-pratica` |
| `taxonomy-tipologia-caso.php` | 2.8 KB | `tipologia-caso` |

---

## CARTELLA `inc/` — Logica PHP modulare

> Directory listing confermata al 15/Apr/2026 — 10 file, nessun file extra non documentato.

| File | Dim. | Ruolo | Note critiche |
|------|------|-------|---------------|
| `custom-post-types.php` | 14.1 KB | Registra CPT con costanti `DRW_CPT_*` | Mai hardcodare slug altrove. Analisi dettagliata sotto. |
| `taxonomies.php` | 6.2 KB | Registra 3 tassonomie: `area-pratica`, `tipologia-caso`, `tipologia-cliente` | ⚠️ BUG: 2 CPT collegati con slug hardcoded invece di costanti — vedere TODO-CLAUDE |
| `enqueue.php` | 9.6 KB ⭐ | Nodo centrale enqueue CSS/JS. Caricamento condizionale per moduli. | Aggiornato 15/Apr |
| `helpers.php` | 10.3 KB ⭐ | `drwstudio` = unico access point ai dati studio | Regola permanente: mai query dirette |
| `options.php` | 39.8 KB ⭐⭐ | Pagina "Impostazioni Studio" con field group ACF Free | ACF Free only. File più grande del tema. |
| `schema.php` | 34.3 KB ⭐⭐ | JSON-LD: LegalService, Review, AggregateRating. Pattern builder modulare. | Legge da `options.php` via `drw_get_settings_page_id()`. Allineato a `05_Schema.org` |
| `customizer.php` | 19.5 KB ⭐ | WP Customizer: palette, tipografia, identità visiva, demo switcher | Analisi dettagliata sotto |
| `testimonianze-template.php` | 9.8 KB | Query flessibile + rendering HTML CPT Testimonianze | Dipende da `template-parts/testimonianze/card-testimonianza.php` |
| `shortcodes-testimonianze.php` | 4.1 KB | Shortcode `[drw_testimonianza]` e `[drw_testimonianze]` | Dipende da `testimonianze-template.php` |
| `demo-switcher.php` | 1.2 KB | Render widget footer switcher palette demo | ⚠️ Contiene commenti HTML di debug — rimuovere prima di v1.0 |

---

### `inc/custom-post-types.php` — Analisi dettagliata

**Ultima modifica:** 11/Mar/2026 — **Dimensione:** 14.1 KB

#### Architettura

Il file usa un pattern **"costanti → hook → registrazione"**:
1. Definisce le 5 costanti `DRW_CPT_*` in cima (con guard `if ( ! defined(...) )`)
2. Hooktail unico `add_action( 'init', 'drw_register_cpts' )`
3. `drw_register_cpts()` chiama le 5 funzioni di registrazione in sequenza

Le label sono in italiano e usano `_x()` / `__()` con text-domain `'drw-lawyer'`, pronti per la traduzione.

#### Costanti slug CPT (definite qui, usate in tutto il tema)

| Costante PHP | Valore attuale | Descrizione |
|-------------|----------------|-------------|
| `DRW_CPT_PROFESSIONALS` | `'avvocati'` | Professionisti |
| `DRW_CPT_SERVICES` | `'aree'` | Servizi / Aree di attività |
| `DRW_CPT_CASES` | `'casi'` | Casi / Progetti |
| `DRW_CPT_LOCATIONS` | `'sedi'` | Sedi / Uffici |
| `DRW_CPT_TESTIMONIALS` | `'testimonianze'` | Testimonianze / Social proof |

> ⚠️ **TASK PRE-v1.0 — VERIFICA SLUG**: Gli slug attuali sono in **italiano**. Per mercato anglosassone (US, UK, AU) e per la neutralità verticali vanno valutati. Decisione da prendere **prima della traduzione EN** — modificare gli slug dopo l'indicizzazione Google comporta redirect 301 obbligatori. Vedere TODO-CLAUDE.

#### CPT registrati

| CPT | Slug rewrite | Archive | has_archive | exclude_from_search | show_in_nav_menus | Supports |
|-----|-------------|---------|-------------|--------------------|--------------------|---------| 
| Professionisti | `avvocati` | ✅ `/avvocati/` | `true` | — | — | title, editor, thumbnail, excerpt, revisions |
| Aree/Servizi | `aree` | ✅ `/aree/` | `true` | — | — | title, editor, thumbnail, excerpt, revisions |
| Casi | `casi` | ✅ `/casi/` | `true` | — | — | title, editor, thumbnail, excerpt, revisions |
| Sedi | `sedi` | ✅ `/sedi/` | `true` | — | — | title, editor, thumbnail, excerpt, revisions |
| Testimonianze | `testimonianze` | ❌ No archivio | `false` | `true` | `false` | title, editor, thumbnail |

#### Note aggiuntive per i singoli CPT

**CPT Sedi** (`DRW_CPT_LOCATIONS`):
- Predisposto per NAP (Name/Address/Phone) e mappa integrata
- Le relazioni (sede ↔ avvocati, sede ↔ aree) **non sono ancora implementate** — da fare via tassonomie o ACF
- `menu_position: 8` — separato chiaramente dai CPT core (5–8)

**CPT Testimonianze** (`DRW_CPT_TESTIMONIALS`):
- `has_archive = false` → nessuna pagina archivio pubblica
- `exclude_from_search = true` → non appare nella ricerca interna WP
- `show_in_nav_menus = false` → non selezionabile nei menu dall'editor
- Accessibili **solo** via shortcode `[drw_testimonianze]` / `[drw_testimonianza id="X"]` o funzione `drw_get_testimonianze()`
- `menu_position: 26` — nel range libero 25+, separato visivamente dai CPT core

> ⚠️ **NOTA ARCHITETTURALE — Testimonianze `public: true`**: Il CPT ha `public: true` ma `has_archive: false` e `exclude_from_search: true`. Le URL `/testimonianze/{slug}/` sono tecnicamente accessibili se un utente conosce lo slug diretto. Verificare che `single-testimonianza.php` gestisca questo caso con redirect 301 verso la pagina "Dicono di noi" (o 404) — vedere TODO-CLAUDE.

---

### `inc/taxonomies.php` — Analisi dettagliata

Tre tassonomie registrate tramite hook `init` → `drw_register_taxonomies()`.

| Tassonomia | Slug | Tipo | CPT collegato | Archivio pubblico |
|---|---|---|---|---|
| Area di pratica | `area-pratica` | Gerarchica (come categorie) | Avvocati | Sì (`/area-pratica/slug/`) |
| Tipologia Caso | `tipologia-caso` | Piatta (come tag) | Casi | Sì (`/tipologia-caso/slug/`) |
| Tipologia Cliente | `tipologia-cliente` | Piatta (come tag) | Testimonianze | De facto no (CPT ha `has_archive=false`) |

> ⚠️ **BUG — slug hardcoded**: `drw_register_tax_area_pratica()` passa `'avvocati'` come stringa letterale invece di `DRW_CPT_PROFESSIONALS`. Idem `drw_register_tax_tipologia_caso()` passa `'casi'` invece di `DRW_CPT_CASES`. Viola la regola "costanti CPT generiche". Vedere sezione TODO-CLAUDE per il fix.

---

### `inc/schema.php` — Analisi dettagliata

Pattern builder modulare. Hook: `wp_head` priority 20. Ogni entità Schema.org ha la propria `drw_schema_build_*()`.

**Funzioni pubbliche:**

| Funzione | Output Schema.org | Contesto di pagina |
|---|---|---|
| `drw_schema_build_legal_service()` | `LegalService` globale | Tutte le pagine |
| `drw_schema_build_address()` | `PostalAddress` | Usata da LegalService |
| `drw_schema_build_contact_points()` | `ContactPoint[]` | Usata da LegalService |
| `drw_schema_build_opening_hours()` | `OpeningHoursSpecification[]` | Usata da LegalService |
| `drw_schema_build_special_opening_hours()` | `SpecialOpeningHoursSpecification[]` | Usata da LegalService |
| `drw_schema_build_offer_catalog()` | `OfferCatalog` | Usata da LegalService |
| `drw_schema_build_employees()` | `Person[]` | Usata da LegalService |
| `drw_schema_build_reviews_for_legal_service()` | `Review[]` (max 10) | Homepage |
| `drw_schema_build_aggregate_rating()` | `AggregateRating` | Home + "Dicono di noi" |
| `drw_schema_output()` | Output `<script ld+json>` | Hook `wp_head` |

**Logica di pagina in `drw_schema_output()`:**

| Contesto | Schema emesso |
|---|---|
| Homepage (`is_front_page()`) | LegalService + review[] + aggregateRating |
| Pagina "Dicono di noi" | LegalService + aggregateRating |
| Singola Testimonianza | LegalService base + Review singola |
| Tutte le altre | LegalService base |

**TODO aperti nel codice:**

| Blocco | Stato | Nota |
|---|---|---|
| `jobTitle` in `drw_schema_build_employees()` | Commentato | Campo `_drw_ruolo` attende definizione ACF |
| NAP specifico sede in `schema_build_legal_service()` | `// TODO` | Attende campi ACF per CPT Sedi |
| `is_page('dicono-di-noi')` | `// TODO` | Slug da verificare quando la pagina sarà creata |
| `BreadcrumbList` su `single-testimonianza` | `// TODO` | Attende struttura navigazione definitiva |

---

### `inc/testimonianze-template.php` — Analisi dettagliata

**Funzioni esposte:**

| Funzione | Ruolo | Dipendenza |
|---|---|---|
| `drw_acf_missing_notice()` | Admin notice graceful degradation se ACF non attivo | Hook `admin_notices` |
| `drw_render_testimonianza_card( $post_id, $echo )` | Rendering HTML card singola via partial | `template-parts/testimonianze/card-testimonianza.php` |
| `drw_get_testimonianze( $args )` | Query flessibile con filtri: numero, evidenza, tipologie, aree, casi | `DRW_CPT_TESTIMONIALS` |
| `drw_render_testimonianze_lista( $posts, $echo )` | Wrapper griglia di card | `drw_render_testimonianza_card()` |

**Argomenti `drw_get_testimonianze()`:**

| Parametro | Tipo | Default | Descrizione |
|---|---|---|---|
| `numero` | int | 6 | Numero massimo risultati |
| `solo_in_evidenza` | bool | false | Solo con meta `evidenza_home = 1` |
| `tipologie` | array | [] | Slug `tipologia-cliente` |
| `aree_ids` | array | [] | ID post CPT Aree (meta LIKE su `relazione_aree`) |
| `casi_ids` | array | [] | ID post CPT Casi (meta LIKE su `relazione_casi`) |
| `orderby` | string | `'date'` | Ordinamento WP_Query |
| `order` | string | `'DESC'` | Direzione |

> 📌 **NOTA TECNICA — meta LIKE su Relationship**: Il filtro per `aree_ids` e `casi_ids` usa `compare => 'LIKE'` con `'"ID"'` per matchare valori serializzati ACF. Funziona correttamente con ACF Free, ma è potenzialmente fragile su ID numerici il cui valore è contenuto in altri ID (es. cercare `"12"` potrebbe matchare `"123"`). Limite noto: nessuna alternativa più precisa con ACF Free senza plugin aggiuntivi.

---

### `inc/shortcodes-testimonianze.php` — Analisi dettagliata

**Shortcode registrati:**

| Shortcode | Parametri | Ruolo |
|---|---|---|
| `[drw_testimonianza id="X"]` | `id` (int, obbligatorio) | Card singola — delega a `drw_render_testimonianza_card()` |
| `[drw_testimonianze ...]` | `numero`, `in_evidenza`, `tipologie`, `aree`, `casi` | Griglia filtrata — delega a `drw_get_testimonianze()` + `drw_render_testimonianze_lista()` |

**Dipendenze:** `inc/testimonianze-template.php` deve essere caricato prima (garantito da `drw_load_includes()` in `functions.php`).

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

> ⚠️ **NOTA ARCHITETTURALE**: `transport` per i colori è impostato su `'refresh'`. Da valutare se ripristinare `postMessage` con JS partial per preview live nel Customizer senza reload.

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

### `inc/demo-switcher.php` — Analisi dettagliata

Hook: `wp_footer` priority 20. Render condizionato da `drw_is_switcher_enabled()` (definita in `customizer.php`).

**Palette disponibili nel widget footer**:
- `classic` (default attivo — classe `drw-palette-active`)
- `verde`
- `mogano`
- `reset`

> ⚠️ **DEBUG attivo nel codice**: il file contiene commenti HTML di debug (`<!-- DRW SWITCHER HOOK FIRED -->`, `<!-- DRW SWITCHER DISABLED/ENABLED -->`). Da rimuovere prima del rilascio v1.0 / consegna cliente.

> 📌 La logica JS del switcher è in `js/drw-demo-switcher.js`. Le palette `verde` e `mogano` devono avere le variabili CSS corrispondenti definite in quel file JS.

---

## TODO-CLAUDE — Modifiche da apportare ai file (con approvazione Luigi)

> Questa sezione elenca tutte le anomalie tecniche rilevate da Perplexity durante l'analisi dei file. Ogni voce indica il file esatto, la riga/funzione interessata, il problema, il fix proposto e la priorità. Claude deve applicare questi fix solo dopo esplicita approvazione di Luigi.

---

### 🔴 BUG — `inc/taxonomies.php`: slug CPT hardcoded invece di costanti

**File:** `inc/taxonomies.php`  
**Funzioni:** `drw_register_tax_area_pratica()` e `drw_register_tax_tipologia_caso()`  
**Problema:** I CPT collegati alle tassonomie sono passati come stringhe letterali, violando la regola "costanti CPT generiche":

```php
// ATTUALE — sbagliato:
register_taxonomy( 'area-pratica', 'avvocati', $args );  // ← stringa hardcoded
register_taxonomy( 'tipologia-caso', 'casi', $args );    // ← stringa hardcoded

// Solo tipologia-cliente è corretta:
register_taxonomy( 'tipologia-cliente', DRW_CPT_TESTIMONIALS, $args );
```

**Fix da applicare:**
```php
// CORRETTO:
register_taxonomy( 'area-pratica', DRW_CPT_PROFESSIONALS, $args );
register_taxonomy( 'tipologia-caso', DRW_CPT_CASES, $args );
```

**Impatto:** Se le costanti `DRW_CPT_PROFESSIONALS` o `DRW_CPT_CASES` vengono modificate (es. per verticale non-avvocati), le tassonomie si scollegano dai CPT in silenzio senza errore PHP visibile.  
**Priorità:** 🔴 Alta — da correggere prima di qualsiasi deploy su verticale non-avvocati.

---

### 🔴 VERIFICA URGENTE — `single-testimonianza.php`: gestione accesso diretto URL

**File:** `single-testimonianza.php`  
**Problema:** Il CPT Testimonianze ha `public: true` (necessario per le URL singole) ma `has_archive: false` e `exclude_from_search: true`. Un utente che conosca o indovini uno slug `/testimonianze/{slug}/` può accedere direttamente alla pagina singola. Con soli 939 B il file è probabilmente un stub.  
**Azione richiesta:** Verificare il contenuto del file e garantire che gestisca questo caso con:
- **Opzione A** (consigliata): redirect 301 verso la pagina "Dicono di noi" (`wp_redirect( get_page_link(...), 301 )`)
- **Opzione B**: output standard del template ma con tag `<meta name="robots" content="noindex">` nell'`<head>`
- **Opzione C** (minima): nessun cambiamento, ma documentare la scelta

**Priorità:** 🔴 Alta — impatta SEO e UX se la pagina è accessibile ma vuota/non strutturata.

---

### 🟡 TODO aperto — `inc/schema.php`: `jobTitle` avvocati non emesso

**File:** `inc/schema.php`  
**Funzione:** `drw_schema_build_employees()`  
**Problema:** Il campo `jobTitle` nella proprietà `employee[]` del LegalService è commentato:
```php
// TODO: jobTitle da campo ACF _drw_ruolo quando definito.
// $ruolo = get_post_meta( $post_id, '_drw_ruolo', true );
// if ( $ruolo ) { $person['jobTitle'] = $ruolo; }
```
**Azione richiesta:** Definire il campo ACF `_drw_ruolo` nel field group "Dettagli Avvocato" in `inc/options.php`, poi decommentare le 3 righe in `schema.php`.  
**Priorità:** 🟡 Media — impatta la qualità del markup strutturato Person.

---

### 🟡 TODO aperto — `inc/schema.php`: NAP sedi non emesso

**File:** `inc/schema.php`  
**Funzione:** `drw_schema_build_legal_service()` — blocco sedi multiple  
**Problema:** Le sedi vengono incluse nel LegalService come `LocalBusiness` con solo `name` e `url`, senza NAP specifico per sede:
```php
// TODO: aggiungere NAP specifico sede quando i campi ACF Sedi saranno definiti.
```
**Azione richiesta:** Definire i campi ACF per CPT Sedi in `inc/options.php` (indirizzo, telefono, coordinate), poi espandere il blocco `location[]` in `schema.php`.  
**Priorità:** 🟡 Media — rilevante per Local SEO multi-sede.

---

### 🟡 TODO aperto — `inc/schema.php`: slug pagina "Dicono di noi" da verificare

**File:** `inc/schema.php`  
**Funzione:** `drw_schema_output()`  
**Problema:**
```php
} elseif ( is_page( 'dicono-di-noi' ) ) {
    // TODO: verificare slug definitivo pagina "Dicono di noi"
```
**Azione richiesta:** Quando la pagina "Dicono di noi" viene creata in WordPress, verificare che lo slug corrisponda a `dicono-di-noi` o aggiornare l'argomento di `is_page()`.  
**Priorità:** 🟡 Media — non blocca nulla finché la pagina non esiste.

---

### 🟡 TODO aperto — `inc/schema.php`: BreadcrumbList su single-testimonianza

**File:** `inc/schema.php`  
**Funzione:** `drw_schema_output()` — blocco `is_singular( DRW_CPT_TESTIMONIALS )`  
**Problema:** La BreadcrumbList per la pagina singola Testimonianza non è implementata:
```php
// TODO: aggiungere BreadcrumbList quando la struttura di navigazione
//       delle Testimonianze sarà definita (es. Home > Dicono di noi > [nome]).
```
**Azione richiesta:** Aggiungere `drw_schema_build_breadcrumb_testimonianza()` e richiamarla in `drw_schema_output()` dopo che la struttura di navigazione è confermata.  
**Priorità:** 🟢 Bassa — utile per SEO ma non bloccante.

---

### 🟢 NOTA TECNICA — `inc/testimonianze-template.php`: limite meta LIKE su Relationship

**File:** `inc/testimonianze-template.php`  
**Funzione:** `drw_get_testimonianze()` — filtri `aree_ids` e `casi_ids`  
**Problema:** Il pattern `compare => 'LIKE'` con `'"ID"'` può produrre falsi positivi se un ID (es. `12`) è contenuto come sottostringa in un altro ID serializzato (es. `"123"`). Non è un bug attuale ma un limite architetturale di ACF Free.  
**Azione richiesta:** Nessuna modifica immediata. Documentare come limite noto. Se il problema si manifesta in produzione, valutare un `meta_query` con boundaries più restrittivi o migrazione a tabella relazionale.  
**Priorità:** 🟢 Bassa — solo documentazione.

---

### 🟡 DEBUG da rimuovere — `inc/demo-switcher.php`

**File:** `inc/demo-switcher.php`  
**Problema:** Contiene commenti HTML di debug attivi:
- `<!-- DRW SWITCHER HOOK FIRED -->`
- `<!-- DRW SWITCHER DISABLED -->`
- `<!-- DRW SWITCHER ENABLED -->`  
**Azione richiesta:** Rimuovere tutti i commenti HTML di debug prima di v1.0 o qualsiasi consegna cliente.  
**Priorità:** 🟡 Media — non impatta funzionalità ma espone dettagli implementativi nel sorgente HTML.

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
| `testimonianze/card-testimonianza.php` | 4.2 KB | Card testimonianza: avatar, nome, ruolo, stelle, testo. Riceve `$post_id` e `$fields` via `include` scope da `drw_render_testimonianza_card()`. |

---

## CPT CORE — Riepilogo costanti e slug

| CPT | Costante PHP | Slug WP (IT) | Archive | Single | Taxonomy |
|-----|-------------|--------------|---------|--------|----------|
| Professionisti | `DRW_CPT_PROFESSIONALS` | `avvocati` | `archive-avvocati.php` | `single-avvocati.php` | — |
| Aree/Servizi | `DRW_CPT_SERVICES` | `aree` | `archive-area.php` | `single-area.php` | `area-pratica` |
| Casi | `DRW_CPT_CASES` | `casi` | `archive-casi.php` | `single-casi.php` | `tipologia-caso` |
| Sedi | `DRW_CPT_LOCATIONS` | `sedi` | `archive-sedi.php` | `single-sedi.php` | — |
| Testimonianze | `DRW_CPT_TESTIMONIALS` | `testimonianze` | **No archivio** | `single-testimonianza.php` | `tipologia-cliente` |

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

> ⚠️ **DEBUG DA RIMUOVERE** prima di v1.0: commenti HTML di debug in `demo-switcher.php`.

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
| **Fix slug hardcoded in taxonomies.php** | 🔴 Alta | `inc/taxonomies.php` | Usare `DRW_CPT_PROFESSIONALS` e `DRW_CPT_CASES` — vedere TODO-CLAUDE |
| **Gestione accesso diretto single-testimonianza** | 🔴 Alta | `single-testimonianza.php` | Redirect 301 o noindex — vedere TODO-CLAUDE |
| Estrai CSS inline Divi 5 | 🔴 CRITICO | — | Prerequisito per architettura variabili |
| Definire campo ACF `_drw_ruolo` per avvocati | 🟡 Media | `inc/options.php` + `inc/schema.php` | Sblocca `jobTitle` nel markup Person |
| Definire campi ACF per CPT Sedi | 🟡 Media | `inc/options.php` + `inc/schema.php` | Sblocca NAP sedi nel LegalService |
| Verificare slug pagina "Dicono di noi" | 🟡 Media | `inc/schema.php` | Quando la pagina verrà creata |
| Rimuovi debug HTML da demo-switcher.php | 🟡 Media | `inc/demo-switcher.php` | Prima di qualsiasi consegna cliente |
| Verifica palette verde/mogano in switcher JS | 🟡 Media | `js/drw-demo-switcher.js` | Testare che le variabili CSS vengano sovrascritte correttamente |
| Verifica slug CPT per mercato EN/AU/US | 🟡 Media | `inc/custom-post-types.php` | Slug attuali in IT — decisione prima traduzione EN |
| Aggiungere BreadcrumbList su single-testimonianza | 🟢 Bassa | `inc/schema.php` | Dopo definizione struttura navigazione |
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

*Fotografia stato: 3 maggio 2026 — v1.3*  
*File analizzati in questa versione: directory `inc/` confermata (10 file, nessun extra), `inc/custom-post-types.php` analisi codice sorgente completa, nuovo TODO-CLAUDE `single-testimonianza.php` accesso diretto*  
*Autori: Luigi + Perplexity AI*


---

## DOCUMENTI CORRELATI (docs/)

> Wikilink per Obsidian — aggiungere [[doppia parentesi]] nel vault per attivare il grafo.

| File | Contenuto |
|---|---|
| [[FILE-MAP]] | Questo documento — mappa completa child theme |
| [[css-map]] | Analisi dettagliata cartella /css (6 file) |
| [[js-map]] | Analisi dettagliata cartella /js (7 file) |
| [[root-templates-map]] | Analisi dettagliata template root PHP |
| [[template-parts-map]] | Analisi cartella template-parts/ |
| [[drive-map]] | Mappa Google Drive — struttura cartelle e documenti |

---

## REPOSITORY GITHUB

- **URL**: https://github.com/dr-web-seo-specialist/DRW
- **Branch**: `main`
- **Cartella docs/**: contiene tutte le mappe di questo progetto
- Aggiornato al: 4 maggio 2026

---

## NOTE SU CARTELLE VUOTE

| Cartella | Stato | Roadmap |
|---|---|---|
| `languages/` | VUOTA — predisposta per i18n | Da popolare dopo stabilizzazione stringhe. File atteso: `drw-lawyer-it_IT.po` |
| `page-templates/` | VUOTA — predisposta per Divi | Da implementare quando layout Divi saranno definiti (post v1.0) |

---

*Fotografia stato: 4 maggio 2026 — v1.4*
*Aggiornamento v1.4: aggiunta sezione DOCUMENTI CORRELATI (wikilink Obsidian), riferimento GitHub, note cartelle vuote, allineamento data.*
