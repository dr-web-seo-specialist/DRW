# Dr.Web Lawyer — Mappa file root (PHP templates)
**Versione:** 2026-05-03 rev.1
**Cartella:** root del child theme
**Fonte:** analisi diretta dei sorgenti inviati da Luigi

---

## Mappa file root analizzati

| File | Tipo | URL | Stato |
|------|------|-----|-------|
| `404.php` | Template WordPress nativo | Qualsiasi URL inesistente | ✅ Stabile (issue #5 da implementare) |
| `archive-area.php` | Archivio CPT | `/aree/` | ✅ Stabile |
| `archive-avvocati.php` | Archivio CPT | `/avvocati/` | ✅ Stabile (issue #6 da correggere) |
| `archive-casi.php` | Archivio CPT | `/casi/` | ✅ Stabile |
| `archive-sedi.php` | Archivio CPT | `/sedi/` | ✅ Stabile (issue #7 da correggere) |

---

## Pattern architetturale comune a tutti i file root

Tutti i template seguono lo stesso contratto:
1. `defined( 'ABSPATH' ) || exit;` — protezione accesso diretto
2. `get_header()` / `get_footer()` — wrapper standard WordPress
3. `<main id="drw-main" class="drw-main drw-main--{context}" role="main">` — wrapper semantico
4. Divi Theme Builder gestisce il layout visivo via hook; il PHP fornisce il guscio semantico + loop di fallback
5. `get_template_part( 'template-parts/no-results' )` nel ramo `else` del loop — fallback consistente
6. `the_posts_pagination()` con classi BEM `drw-pagination` e stringhe i18n

---

## Dettaglio tecnico per file

### `404.php`
**Responsabilità:** pagina errore 404 — guscio semantico + fallback testuale se nessun layout Divi è assegnato.

**Decisione architetturale (Luigi 2026-05-03):**
Il guard Divi originale (`if ( function_exists( 'et_core_is_fb_enabled' ) )`) è stato identificato come errato: quella funzione testa la modalità frontend builder (editing), non l'assegnazione di un layout Theme Builder. In produzione restituisce sempre `false`, causando la visualizzazione del fallback anche quando Divi ha un layout assegnato.

**Soluzione approvata — Opzione A:** rimuovere il guard `if/else` e lasciare il fallback testuale **incondizionato**. Divi inietta i suoi layout via hook WordPress indipendentemente dal markup PHP nel `<main>` — nessun conflitto. Se Divi ha un layout assegnato lo mostra; se non ne ha, il fallback PHP è già presente nel DOM. Zero dipendenze da API interne Divi (scelta più robusta per prodotto riusabile).

**Codice attuale da correggere:**
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
- Lo slug `/contatti/` è hardcodato con `home_url( '/contatti/' )` — vedi issue #1 nel documento `template-parts-map.md`.
- Il placeholder `// get_search_form();` è commentato intenzionalmente — da valutare in Fase 2.
- Testo completamente i18n (`esc_html_e`, text domain `drw-lawyer`).
- Classi BEM: `.drw-error-404`, `.drw-error-404__title`, `.drw-error-404__message`, `.drw-error-404__actions`.

---

### `archive-area.php`
**Responsabilità:** archivio CPT Aree di Attività — indice servizi/aree legali. URL: `/aree/`

**Note per Claude:**
- `post_type_archive_title()` corretto — restituisce il `label` del CPT, già i18n.
- `<a>` sul thumbnail con `tabindex="-1" aria-hidden="true"` — pattern corretto per evitare doppia tabulazione (link già presente nel titolo `<h2>`).
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
L’`<article>` ha `<meta itemprop="name">` + l’`<h2>` ha `itemprop="name"`. Schema.org `Person` prevede `name` una sola volta. Soluzione: rimuovere `<meta itemprop="name">` dall’`<article>` — il valore è già esposto dall’`<h2>`. Il JSON-LD strutturale è gestito da `inc/schema.php`.

**TODO Fase 2 (già tracciati nel file):**
- Filtri per ruolo e area di pratica (Isotope.js o FacetWP)
- Tassonomia `ruolo` (Partner, Associate, Of Counsel, Staff) da definire
- Campo custom `_drw_ruolo` o tassonomia per il badge ruolo nella card
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

**Funzione strategica:**
- Prova sociale concreta (social proof)
- Cluster SEO per query "come risolvere X"
- Differenziazione competitiva

**Note per Claude:**
- `tipologia-caso` usata diversamente da `tipologia-cliente`: mostra solo `$tipologie[0]->name` (primo badge), non un loop. Comportamento corretto per card indice.
- Guard `$tipologie && ! is_wp_error( $tipologie )` gestisce correttamente il caso senza termini assegnati.
- TODO filtri per `tipologia-caso` e `area-pratica` già tracciati come Fase 2.
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

**Funzione strategica:**
- Local SEO multi-sede (segnale NAP per ogni città)
- UX: aiuta l’utente a trovare la sede più vicina
- Schema markup `LocalBusiness` multiplo (gestito da `inc/schema.php`)

**Issue #7 — `itemprop="name"` duplicato (da correggere):**
Stesso pattern di `archive-avvocati.php`: `<meta itemprop="name">` sull’`<article>` + `itemprop="name"` sull’`<h2>`. Soluzione: rimuovere il `<meta>`.

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

## Issues — stato

| # | File | Tipo | Descrizione | Priorità | Stato |
|---|------|------|-------------|----------|-------|
| 5 | `404.php` | ✅ Bug logico risolto | **Guard Divi errato rimosso — Opzione A approvata da Luigi 2026-05-03.** `et_core_is_fb_enabled()` testava editing mode, non layout assegnato. Soluzione: fallback `<section>` incondizionato nel DOM. Divi sovrascrive via hook quando ha un layout; altrimenti il fallback è visibile. Zero dipendenze da API Divi interne. | Alta | ✅ **Chiusa — da implementare** |
| 6 | `archive-avvocati.php` | ✅ Schema.org | **`itemprop="name"` duplicato.** Rimuovere `<meta itemprop="name">` dall’`<article>` — il valore è già esposto dall’`<h2 itemprop="name">`. | Bassa | ✅ **Chiusa — da implementare** |
| 7 | `archive-sedi.php` | ✅ Schema.org | **`itemprop="name"` duplicato.** Stesso pattern — rimuovere `<meta itemprop="name">` dall’`<article>`. | Bassa | ✅ **Chiusa — da implementare** |

---

## Dipendenze incrociate (file root)

| Template | Dipende da | Contratto |
|----------|------------|-----------|
| Tutti | `global-layout.css` → `.drw-container`, `.drw-py-section` | Classi utility layout devono esistere |
| Tutti | `global.css` → `.drw-label`, `.drw-label--accent` | Classi label devono esistere |
| Tutti | `global.css` → `.drw-btn`, `.drw-btn-outline` | Classi button devono esistere |
| Tutti | `global.css` → `.drw-pagination` | Classe paginazione deve esistere |
| Tutti | `template-parts/no-results.php` | Partial no-results sempre presente |
| `404.php` | Divi Theme Builder | Layout opzionale — fallback PHP sempre nel DOM |
| `archive-avvocati.php` | `inc/schema.php` | JSON-LD Person gestito separatamente |
| `archive-sedi.php` | `inc/schema.php` | JSON-LD LocalBusiness gestito separatamente |
| `archive-sedi.php` | `inc/helpers.php` | `drw_get_phone_link()`, `drw_get_email_link()` — Fase 2 |
| `archive-casi.php` | Tassonomia `tipologia-caso` | Già registrata in `inc/` — attiva |
