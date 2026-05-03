# Dr.Web Lawyer — Mappa file root (PHP templates)
**Versione:** 2026-05-03 rev.2
**Cartella:** root del child theme
**Fonte:** analisi diretta dei sorgenti inviati da Luigi

---

## Mappa file root analizzati

| File | Tipo | URL / Uso | Stato |
|------|------|-----------|-------|
| `404.php` | Template WordPress nativo | Qualsiasi URL inesistente | ✅ Stabile (issue #5 da implementare) |
| `archive.php` | Archivio generico fallback | Categorie, tag, autore, data | ✅ Stabile |
| `archive-area.php` | Archivio CPT | `/aree/` | ✅ Stabile |
| `archive-avvocati.php` | Archivio CPT | `/avvocati/` | ✅ Stabile (issue #6 da correggere) |
| `archive-casi.php` | Archivio CPT | `/casi/` | ✅ Stabile |
| `archive-sedi.php` | Archivio CPT | `/sedi/` | ✅ Stabile (issue #7 da correggere) |
| `front-page.php` | Homepage statica | `/` (Impostazioni → Lettura) | ✅ Stabile |
| `functions.php` | Bootstrap child theme | — | ✅ Stabile (issue #8 da implementare) |
| `home.php` | Archivio blog fallback | Pagina degli articoli (Divi fallback) | ✅ Stabile |
| `page.php` | Pagina generica | Qualsiasi pagina senza template specifico | ✅ Stabile |
| `page-contatti.php` | Template pagina | Contatti, Richiesta Appuntamento | ✅ Stabile |
| `page-landing.php` | Template landing | Landing ADV, Consulenza Gratuita | ✅ Stabile (issue #9 chiusa via RankMath) |
| `page-legal.php` | Template pagina | Privacy Policy, Cookie, GDPR, T&C | ✅ Stabile |
| `page-macroarea.php` | Template pagina | Landing tematiche diritto (Civile, Penale…) | ✅ Stabile |
| `page-special.php` | Template pagina | Chi siamo variante, pagine fuori schema | ✅ Stabile |

---

## Pattern architetturale comune a tutti i file root

Tutti i template seguono lo stesso contratto:
1. `defined( 'ABSPATH' ) || exit;` — protezione accesso diretto
2. `get_header()` / `get_footer()` — wrapper standard WordPress (**eccezione: `page-landing.php`** usa HTML manuale)
3. `<main id="drw-main" class="drw-main drw-main--{context}" role="main">` — wrapper semantico
4. Divi Theme Builder gestisce il layout visivo via hook; il PHP fornisce il guscio semantico + loop di fallback
5. `get_template_part( 'template-parts/no-results' )` nel ramo `else` del loop — fallback consistente
6. `the_posts_pagination()` con classi BEM `drw-pagination` e stringhe i18n

**Matrice strutturale:**

| Template | `get_header/footer` | Loop WP | `the_content()` | Note |
|----------|--------------------|---------|-----------------|----|
| `page.php` | ✅ std | ✅ | ✅ | Base generica |
| `page-special.php` | ✅ std | ✅ | ✅ | Classe CSS extra |
| `page-contatti.php` | ✅ std | ✅ | ✅ | TODO NAP |
| `page-macroarea.php` | ✅ std | ✅ | ✅ | TODO loop CPT Fase 2 |
| `page-legal.php` | ✅ std | ✅ | ✅ | Data modifica + prose wrapper |
| `page-landing.php` | ❌ HTML manuale | ✅ | ✅ | Unica eccezione — no header/footer std |

---

## Dettaglio tecnico per file

### `404.php`
**Responsabilità:** pagina errore 404 — guscio semantico + fallback testuale se nessun layout Divi è assegnato.

**Decisione architetturale (Luigi 2026-05-03):**
Il guard Divi originale (`if ( function_exists( 'et_core_is_fb_enabled' ) )`) è stato identificato come errato: quella funzione testa la modalità frontend builder (editing), non l'assegnazione di un layout Theme Builder. In produzione restituisce sempre `false`, causando la visualizzazione del fallback anche quando Divi ha un layout assegnato.

**Soluzione approvata — Opzione A:** rimuovere il guard `if/else` e lasciare il fallback testuale **incondizionato**. Divi inietta i suoi layout via hook WordPress indipendentemente dal markup PHP nel `<main>` — nessun conflitto. Se Divi ha un layout assegnato lo mostra; se non ne ha, il fallback PHP è già presente nel DOM. Zero dipendenze da API interne Divi (scelta più robusta per prodotto riusabile).

**Codice da correggere:**
```php
// DA RIMUOVERE:
if ( function_exists( 'et_core_is_fb_enabled' ) ) :
    // Divi gestisce il rendering
else :
    // fallback
endif;

// DIVENTA (fallback incondizionato):
// La sezione .drw-error-404 viene sempre stampata nel DOM.
// Divi la sovrascrive via hook se ha un layout assegnato.
```

**Note per Claude:**
- Lo slug `/contatti/` è hardcodato con `home_url( '/contatti/' )` — vedi issue #1 in `template-parts-map.md`.
- Il placeholder `// get_search_form();` è commentato intenzionalmente — da valutare in Fase 2.
- Testo completamente i18n (`esc_html_e`, text domain `drw-lawyer`).
- Classi BEM: `.drw-error-404`, `.drw-error-404__title`, `.drw-error-404__message`, `.drw-error-404__actions`.

---

### `archive.php`
**Responsabilità:** archivio generico fallback — usato per categorie blog, tag, autore, data. Non usato per CPT (hanno tutti template dedicati).

**Note per Claude:**
- `the_archive_title()` e `the_archive_description()` con wrapper HTML come argomenti — pattern WordPress corretto, gestisce tutti i tipi di archivio in modo localizzato.
- Template di sicurezza: nel progetto è puramente teorico per i CPT (tutti coperti), ma attivo per archivi blog nativi.
- Struttura lista (`drw-archive__list` + `drw-archive-item`) — diversa dalla griglia card usata negli archivi CPT.
- `<time datetime="c">` con `get_the_date( 'c' )` — formato ISO 8601 corretto per accessibilità.
- Nessuna issue aperta.

**Classi BEM principali:**
```
.drw-archive
.drw-archive__header
.drw-archive__title
.drw-archive__description
.drw-archive__list
.drw-archive-item
.drw-archive-item__header
.drw-archive-item__title
.drw-archive-item__excerpt
```

---

### `archive-area.php`
**Responsabilità:** archivio CPT Aree di Attività — indice servizi/aree legali. URL: `/aree/`

**Note per Claude:**
- `post_type_archive_title()` corretto — restituisce il `label` del CPT, già i18n.
- `<a>` sul thumbnail con `tabindex="-1" aria-hidden="true"` — pattern corretto per evitare doppia tabulazione.
- `the_posts_pagination()` usa classe custom `drw-pagination` — verificare che esista nel CSS.
- Nessuna issue aperta.

**Classi BEM principali:**
```
.drw-archive-aree
.drw-archive-aree__header
.drw-archive-aree__title
.drw-archive-aree__grid
.drw-area-card
.drw-area-card__thumbnail
.drw-area-card__body
.drw-area-card__title
.drw-area-card__excerpt
```

---

### `archive-avvocati.php`
**Responsabilità:** archivio CPT Avvocati/Team — griglia professionisti. URL: `/avvocati/`

**Issue #6 — `itemprop="name"` duplicato (da correggere):**
L'`<article>` ha `<meta itemprop="name">` + l'`<h2>` ha `itemprop="name"`. Schema.org `Person` prevede `name` una sola volta. Soluzione: rimuovere `<meta itemprop="name">` dall'`<article>`.

**TODO Fase 2 (già tracciati nel file):**
- Filtri per ruolo e area di pratica (Isotope.js o FacetWP)
- Tassonomia `ruolo` (Partner, Associate, Of Counsel, Staff)
- Campo custom `_drw_ruolo` per badge ruolo nella card
- CTA di chiusura pagina da implementare in Divi

**Classi BEM principali:**
```
.drw-archive-avvocati
.drw-archive-avvocati__header
.drw-archive-avvocati__grid
.drw-avvocato-card
.drw-avvocato-card__photo
.drw-avvocato-card__body
.drw-avvocato-card__name
.drw-avvocato-card__bio
```

---

### `archive-casi.php`
**Responsabilità:** archivio CPT Casi di Studio — portfolio / social proof. URL: `/casi/`

**Funzione strategica:** prova sociale concreta, cluster SEO "come risolvere X", differenziazione competitiva.

**Note per Claude:**
- `tipologia-caso`: mostra solo `$tipologie[0]->name` (primo termine), non un loop. Corretto per card indice.
- Guard `$tipologie && ! is_wp_error( $tipologie )` gestisce correttamente il caso senza termini assegnati.
- TODO filtri Fase 2 già tracciati nel file.
- Nessuna issue aperta.

**Classi BEM principali:**
```
.drw-archive-casi
.drw-archive-casi__header
.drw-archive-casi__grid
.drw-caso-card
.drw-caso-card__thumbnail
.drw-caso-card__body
.drw-caso-card__title
.drw-caso-card__excerpt
.drw-badge.drw-badge--accent
```

---

### `archive-sedi.php`
**Responsabilità:** archivio CPT Sedi/Location — uffici e NAP multi-sede. URL: `/sedi/`

**Funzione strategica:** Local SEO multi-sede, NAP per ogni città, Schema.org `LocalBusiness` multiplo.

**Issue #7 — `itemprop="name"` duplicato (da correggere):**
Stesso pattern di `archive-avvocati.php`. Soluzione: rimuovere `<meta itemprop="name">` dall'`<article>`.

**TODO Fase 2 (già tracciati nel file):**
- Mappa interattiva multi-sede (Google Maps API o Leaflet.js no-cookie)
- Dati NAP da campi ACF: `_drw_indirizzo`, `_drw_citta`, `_drw_cap`, `_drw_telefono`, `_drw_email`
- Helper `drw_get_phone_link()` / `drw_get_email_link()` da `inc/helpers.php`

**Classi BEM principali:**
```
.drw-archive-sedi
.drw-archive-sedi__header
.drw-archive-sedi__grid
.drw-sede-card
.drw-sede-card__thumbnail
.drw-sede-card__body
.drw-sede-card__name
.drw-sede-card__desc
```

---

### `front-page.php`
**Responsabilità:** homepage principale — usata quando è impostata una pagina statica in Impostazioni → Lettura. Ha precedenza su `home.php` e `index.php`.

**Note per Claude:**
- Template minimalista — delega tutto a Divi Builder tramite `the_content()`.
- Layout A (Avvocato Singolo) e Layout B (Studio Legale) sono differenziati tramite layout Divi assegnati alla pagina, non tramite template PHP separati.
- Il `while ( have_posts() )` su pagina statica è idiomaticamente corretto ("page loop").
- La classe `drw-front-page` sull'`<article>` è ridondante se Divi wrappa tutto internamente, ma è coerente col pattern del progetto.
- Nessuna issue aperta.

---

### `functions.php`
**Responsabilità:** bootstrap del child theme — theme support, caricamento file modulari da `/inc/`, flush permalink, update checker.

**Regola critica:** tutti gli enqueue CSS/JS stanno in `inc/enqueue.php` — non aggiungere enqueue diretti in `functions.php`.

**Theme support dichiarati:**
- `title-tag` — WordPress gestisce `<title>` via `wp_head()` (nota: Divi lo dichiara già nel parent; doppia dichiarazione è idempotente)
- `post-thumbnails` — immagini in evidenza per tutti i CPT
- `html5` — markup semantico per form, search, gallery, caption, script, style
- `custom-logo` — logo personalizzato (h:80, w:200, flex entrambi)
- `automatic-feed-links` — feed automatici archivi post
- `editor-styles` — compatibilità futura Gutenberg/FSE

**File caricati da `/inc/` (ordine di caricamento):**
1. `enqueue.php`
2. `custom-post-types.php`
3. `taxonomies.php`
4. `options.php`
5. `schema.php`
6. `helpers.php`
7. `customizer.php`
8. `demo-switcher.php`
9. `testimonianze-template.php`
10. `shortcodes-testimonianze.php`

**Flush permalink automatico (pattern in 3 step):**
1. `after_switch_theme` → scrive flag `drw_flush_rewrite_rules` in options
2. `init` priority 99 → legge flag, esegue `flush_rewrite_rules()`, cancella flag (dopo registrazione CPT/tassonomie)
3. `switch_theme` → flush alla disattivazione (pulizia rules orfane)

**Issue #8 — Update checker con URL placeholder:**
Il filtro `site_transient_update_themes` è agganciato in produzione e tenta una HTTP request ad ogni WordPress update check. Con URL placeholder (`tuodominio.it`) causa timeout inutili.

**Soluzione approvata (Claude 2026-05-03):**
```php
function drw_lawyer_child_check_for_update( $transient ) {

    // Early return se l'endpoint non è ancora configurato.
    if ( strpos( DRW_LAWYER_CHILD_UPDATE_URL, 'tuodominio.it' ) !== false ) {
        return $transient;
    }
    // ... resto della funzione invariato
```
Da inserire come prima riga della funzione, prima di qualsiasi altra logica. Si auto-disattiva sostituendo l'URL reale — zero elementi aggiuntivi da ricordarsi.

**Costanti definite:**
- `DRW_LAWYER_CHILD_SLUG` → `'dr-web-lawyer-template-child'`
- `DRW_LAWYER_CHILD_UPDATE_URL` → `'https://tuodominio.it/updates/drweb-lawyer/info.json'` (placeholder)

---

### `home.php`
**Responsabilità:** archivio blog fallback — usato quando è impostata una "Pagina degli articoli" separata dalla homepage. Divi Theme Builder gestisce il layout principale; questo è il fallback se nessun layout è assegnato.

**Note per Claude:**
- `get_the_category()[0]->name` come badge — stesso pattern di `archive-casi.php` per `tipologia-caso`. Coerenza di approccio.
- Guard `! empty( $category )` corretto.
- `<time datetime="c">` con ISO 8601 — corretto per accessibilità.
- Struttura card con `<footer>` interno (data + CTA) — più ricca di `archive.php` (solo lista).
- Nessuna issue aperta.

**Classi BEM principali:**
```
.drw-blog-archive
.drw-blog-archive__header
.drw-blog-archive__grid
.drw-blog-card
.drw-blog-card__thumbnail
.drw-blog-card__body
.drw-blog-card__header
.drw-blog-card__title
.drw-blog-card__excerpt
.drw-blog-card__footer
.drw-badge
```

---

### `page.php`
**Responsabilità:** template pagina generica — fallback per tutte le pagine istituzionali senza logica speciale (Chi Siamo, Valori, Metodo, Testimonianze, Lavora con Noi, ecc.).

**Note per Claude:**
- Template base minimale — guscio semantico puro, tutto delegato a Divi Builder.
- Nessuna logica PHP specifica.
- Nessuna issue aperta.

---

### `page-contatti.php`
**Template Name:** `Pagina Contatti`
**Responsabilità:** contatti e richieste — form principale + mappa + NAP. Usato anche per "Richiesta Appuntamento" con layout Divi diverso.

**Note per Claude:**
- Nessuna logica form PHP — form gestiti da WPForms o Divi Contact Form Module.
- TODO NAP: dati di contatto strutturati (indirizzo, telefono, email) da aggiungere quando definiti i campi ACF. I dati NAP devono essere **identici** in: questa pagina + footer globale + singole pagine Sedi CPT (coerenza segnale Local SEO).
- Schema markup `LocalBusiness` gestito da `inc/schema.php`.
- Il commento "form mai a freddo" (CTA argomentata prima del form) è reminder UX/CRO da preservare.
- Nessuna issue aperta.

---

### `page-landing.php`
**Template Name:** `Landing / Prima Consulenza`
**Responsabilità:** landing page e funnel di conversione — Landing ADV Avvocato Singolo, Landing ADV Studio Legale, Consulenza Gratuita, future campagne ADV.

**Caratteristiche speciali (unica eccezione tra i template):**
- **No `get_header()` / `get_footer()` standard** — HTML completo manuale con `wp_head()` e `wp_footer()` espliciti.
- Motivo: le landing hanno header/footer ridotti o assenti per massimizzare la conversione. Divi Theme Builder assegna header/footer "landing" tramite condizioni del template.
- `wp_body_open()` posizionato correttamente immediatamente dopo `<body>` — punto di iniezione per GTM noscript e altri script pre-contenuto. ✅
- `body_class( 'drw-landing-body' )` — classe extra per targeting CSS/JS specifico landing.

**Issue #9 — noindex + GTM — chiusa (Luigi + Claude 2026-05-03):**
- **Soluzione adottata:** Rank Math (già in lista plugin approvati).
  - `noindex` configurabile per singola pagina dal pannello — flessibile se il cliente vuole indicizzare in futuro senza toccare codice.
  - GTM tramite campo "Google Tag Manager ID" nelle impostazioni Rank Math — unica fonte per tutto il tracking.
- **Scelta rifiutata:** hook PHP condizionale in `inc/enqueue.php` — crea dipendenza hardcodata sul template name (fragile se rinominato).
- **Fase 2:** rivalutare hook PHP se si sviluppano landing programmatiche per verticali (commercialisti, notai) dove il noindex deve scattare automaticamente.
- **Nota critica:** GTM è gestito da Rank Math — **non aggiungere script GTM manualmente** nel template o in `inc/enqueue.php`.

---

### `page-legal.php`
**Template Name:** `Pagina Legale`
**Responsabilità:** pagine legali e normative — Privacy Policy, Cookie Policy, GDPR, Termini e Condizioni.

**Note per Claude:**
- Layout ottimizzato per testi lunghi: classe `drw-legal__content--prose` per max-width ridotto sul corpo testo.
- `get_the_modified_date()` con `printf` + commento `translators` — pattern i18n corretto per la data di aggiornamento. Rilevante: le pagine legali richiedono aggiornamento periodico (GDPR, Cookie).
- `noindex` gestito via Rank Math per singola pagina (allineato alla decisione issue #9).
- Consiglio nel codice: usare editor WordPress nativo (Gutenberg o classico) per le pagine legali — più semplice per il cliente rispetto a Divi Builder.
- Nessuna issue aperta.

**Classi BEM principali:**
```
.drw-legal-wrapper
.drw-legal
.drw-legal__header
.drw-legal__title
.drw-legal__updated
.drw-legal__content
.drw-legal__content--prose
```

---

### `page-macroarea.php`
**Template Name:** `Macro Area / Pagina Aggregata`
**Responsabilità:** pagine di overview delle grandi aree tematiche — Diritto Amministrativo, Civile, Commerciale, Lavoro, Famiglia, Immobiliare, Penale, Societario, Tributario, Responsabilità Civile.

**Distinzione architetturale importante:**
- Queste sono **pagine WordPress statiche** di "landing tematica"
- Distinte dal **CPT Aree** che contiene le sotto-aree specifiche
- Gerarchia: `page-macroarea` (overview area) → `archive-area` (indice CPT) → `single-area` (singola sotto-area)

**TODO Fase 2 — loop CPT Aree collegate (già tracciato nel file):**
```php
// Struttura attesa:
$aree_collegate = new WP_Query( array(
    'post_type'      => DRW_CPT_SERVICES,
    'posts_per_page' => -1,
    'tax_query'      => array( ... ),  // filtro per macro-area
) );
```
La relazione tra pagine macro-area e CPT Aree dovrà essere definita tramite tassonomia o campo custom.

**Note per Claude:**
- Il TODO usa correttamente la costante `DRW_CPT_SERVICES` — non slug hardcodato.
- Nessuna issue aperta.

---

### `page-special.php`
**Template Name:** `Pagina Speciale`
**Responsabilità:** area riservata e pagine fuori schema — chi-siamo variante, pagine promozionali, contenuti non catalogabili.

**Note per Claude:**
- Template minimalista — differenza strutturale da `page.php`: classi CSS `drw-main--special` e `drw-page--special` per targeting CSS/JS selettivo senza logica PHP aggiuntiva.
- Nessuna issue aperta.

---

## Issues — stato completo

| # | File | Tipo | Descrizione sintetica | Priorità | Stato |
|---|------|------|-----------------------|----------|-------|
| 5 | `404.php` | 🔴 Bug logico | Guard Divi errato (`et_core_is_fb_enabled`). **Soluzione:** rimuovere `if/else`, fallback incondizionato. | Alta | ✅ Approvata Luigi — da implementare |
| 6 | `archive-avvocati.php` | 🟡 Schema.org | `itemprop="name"` duplicato su `<meta>` + `<h2>`. **Soluzione:** rimuovere `<meta>`. | Bassa | ✅ Approvata — da implementare |
| 7 | `archive-sedi.php` | 🟡 Schema.org | Stesso pattern issue #6. **Soluzione:** rimuovere `<meta itemprop="name">`. | Bassa | ✅ Approvata — da implementare |
| 8 | `functions.php` | 🟡 Performance | Update checker attivo con URL placeholder — HTTP request inutile ad ogni WP update check. **Soluzione:** `strpos` early return su `'tuodominio.it'`. | Media | ✅ Approvata Luigi — da implementare |
| 9 | `page-landing.php` | 🟡 SEO/Tracking | `noindex` + GTM mancanti. **Soluzione:** Rank Math (configurazione per-pagina, no PHP). GTM via campo GTM ID in Rank Math. **Non aggiungere GTM manualmente.** Fase 2: rivalutare PHP per verticali programmatici. | Media | ✅ Chiusa — gestita via plugin |

---

## Dipendenze incrociate (file root)

| Template | Dipende da | Contratto |
|----------|------------|-----------|
| Tutti | `global-layout.css` → `.drw-container`, `.drw-py-section` | Classi utility layout devono esistere |
| Tutti | `global.css` → `.drw-label`, `.drw-label--accent` | Classi label devono esistere |
| Tutti | `global.css` → `.drw-btn`, `.drw-btn-outline` | Classi button devono esistere |
| Tutti (con paginazione) | `global.css` → `.drw-pagination` | Classe paginazione deve esistere |
| Tutti (con loop) | `template-parts/no-results.php` | Partial no-results sempre presente |
| `404.php` | Divi Theme Builder | Layout opzionale — fallback PHP sempre nel DOM |
| `archive-avvocati.php` | `inc/schema.php` | JSON-LD Person gestito separatamente |
| `archive-sedi.php` | `inc/schema.php` | JSON-LD LocalBusiness gestito separatamente |
| `archive-sedi.php` | `inc/helpers.php` | `drw_get_phone_link()`, `drw_get_email_link()` — Fase 2 |
| `archive-casi.php` | Tassonomia `tipologia-caso` | Già registrata in `inc/taxonomies.php` |
| `home.php` | Tassonomia `category` WordPress | Categorie blog native |
| `functions.php` | `inc/` (10 file) | Tutti i file `inc/` devono esistere o il `file_exists()` li salta silenziosamente |
| `page-landing.php` | Rank Math | GTM e noindex gestiti dal plugin — non duplicare in PHP |
| `page-contatti.php` | `inc/schema.php` | Schema LocalBusiness per NAP — Fase 2 |
| `page-macroarea.php` | Costante `DRW_CPT_SERVICES` | Definita in `inc/custom-post-types.php` |
