# Dr.Web Lawyer — Mappa template-parts + Note per Claude
**Versione:** 2026-05-03 rev.2
**Cartella:** `/template-parts` (child theme)
**Fonte:** analisi diretta dei sorgenti inviati da Luigi

---

## Mappa file `/template-parts`

| File | Richiamato da | Variabili contesto | Stato |
|------|---------------|--------------------|-------|
| `no-results.php` | `search.php`, `home.php`, `archive.php`, `archive-{cpt}.php`, `taxonomy-{tax}.php` | — (autonomo) | ✅ Stabile |
| `card-testimonianza.php` | `drw_render_testimonianza_card()` in `inc/testimonianze-template.php`; shortcode `[drw_testimonianza]`; archivi, slider | `$post_id` (int), `$fields` (array ACF) | ✅ Stabile |

---

## Dettaglio tecnico per file

### `no-results.php`
**Responsabilità:** partial generico "nessun risultato" — fallback riusabile in tutti i contesti senza contenuti.

**Caratteristiche:**
- Non include `get_header()` / `get_footer()` / `<main>` — va inserito dentro il layout del template chiamante
- Chiamata corretta: `get_template_part( 'template-parts/no-results' )`
- Struttura: label accent → H2 → messaggio → CTA primaria (homepage) → CTA secondaria (contatti, solo se `is_search()`) → form di ricerca (solo se `is_search()`)
- Testo messaggio completamente i18n (`esc_html_e`, text domain `drw-lawyer`)
- URL `/contatti/` hardcodato con `home_url( '/contatti/' )` — relativo, non assoluto

**Classi BEM principali:**
```
.drw-no-results
.drw-no-results__inner
.drw-no-results__title
.drw-no-results__message
.drw-no-results__actions
.drw-no-results__search
```

**Note per Claude:**
- Lo slug `/contatti/` è hardcodato. Se il cliente rinomina la pagina contatti (slug diverso), aggiornare qui. Valutare in futuro se leggerlo da un'opzione Customizer (es. `get_theme_mod('drw_contatti_url', '/contatti/')`).
- `get_search_form()` usa il form nativo WordPress. Se il tema o un plugin override il form (es. SearchWP), il risultato sarà quello personalizzato — comportamento corretto, non un bug.
- Il partial è **context-agnostic**: non riceve variabili, non fa query, non dipende dal loop. Può essere chiamato da qualsiasi template senza effetti collaterali.
- Le classi utility `drw-py-section` e `drw-container` devono esistere in `global-layout.css`. Prima di modificare il partial, verificare che queste classi siano definite.
- Nessuna dipendenza JS. Nessun dato-attribute. Nessun ACF.

---

### `card-testimonianza.php`
**Responsabilità:** markup della singola card testimonianza — presentazione di nome, foto, stelle, testo, ruolo, badge tipologia.

**Contratto di chiamata — CRITICO:**
Il file NON va chiamato direttamente con `get_template_part()` a meno che `$post_id` e `$fields` siano già impostati nel contesto. La via sicura è sempre:
```php
drw_render_testimonianza_card( $post_id ); // definita in inc/testimonianze-template.php
```

**Variabili di contesto richieste:**
| Variabile | Tipo | Fonte |
|-----------|------|-------|
| `$post_id` | int | ID del post Testimonianza |
| `$fields` | array | `get_fields( $post_id )` — campi ACF già recuperati |

**Campi ACF letti:**
| Chiave ACF | Fallback | Uso |
|------------|----------|-----|
| `cliente_nome_visualizzato` | `''` | Nome mostrato |
| `cliente_ruolo_tipo` | `''` | Ruolo / tipo cliente |
| `cliente_citta` | `''` | Città |
| `cliente_foto` | `array()` | Oggetto immagine ACF (`url`, `alt`) |
| `testo_testimonianza` | `''` | Testo blockquote |
| `valutazione_stelle` | `0` | Intero 1–5 (0 = non mostrare stelle) |

**Tassonomia `tipologia-cliente`:**
- Letta con `get_the_terms( $post_id, 'tipologia-cliente' )`
- Ogni termine → badge `.drw-badge`
- **Stato: RINVIATA A FASE 2** (decisione Luigi 2026-05-03)

> **Rationale approvato:** La tassonomia è concettualmente valida e coerente con l'architettura multi-vertical (avvocati, commercialisti, notai, medici). Permette agli studi di classificare le testimonianze per tipo di cliente (Privato, Azienda, Startup, Famiglia, ecc.) e potenzialmente integrarsi con archivi clienti esterni (Excel, CRM). Non è urgente per v1.0 — il codice nel partial è già pronto e innocuo finché la tassonomia non viene registrata (`get_the_terms()` restituisce `false`, il blocco badge non viene renderizzato). Si registra in Fase 2 insieme alle altre funzionalità avanzate.

**Quando si implementa in Fase 2:**
1. Aggiungere `register_taxonomy( 'tipologia-cliente', 'testimonianza', [...] )` in `inc/` (file da definire)
2. Aggiungere `&& $tipologie !== false` al controllo esistente in `card-testimonianza.php`
3. Definire i termini di vocabolario chiuso in `03Tassonomie-core` e promuoverli in Drive
4. Aggiungere le classi `.drw-badge` al CSS se non già presenti

**Logica immagine (priorità):**
1. `cliente_foto['url']` (ACF) — usa anche `cliente_foto['alt']` se presente
2. `get_the_post_thumbnail_url( $post_id, 'thumbnail' )` — fallback su featured image
3. Nessuna immagine → blocco `.drw-testimonianza-card__foto` non renderizzato

**Stelle:** clamp PHP `max(0, min(5, $stelle))` — valori fuori range corretti silenziosamente.

**Classi BEM principali:**
```
.drw-testimonianza-card
.drw-testimonianza-card__body
.drw-testimonianza-card__stelle
.drw-stella / .drw-stella--attiva
.drw-testimonianza-card__testo        (blockquote)
.drw-testimonianza-card__footer
.drw-testimonianza-card__foto
.drw-testimonianza-card__meta
.drw-testimonianza-card__nome
.drw-testimonianza-card__ruolo
.drw-testimonianza-card__tipologie
.drw-badge
```

**Note per Claude:**
- Il guard iniziale `if ( empty( $post_id ) || empty( $fields ) || ! is_array( $fields ) ) { return; }` è la protezione principale contro chiamate errate. Non rimuovere.
- `wp_kses_post( $testo )` su `testo_testimonianza`: il campo ACF può contenere HTML (es. grassetto, corsivo). Se il campo viene cambiato a plain text, sostituire con `esc_html()`.
- La tassonomia `tipologia-cliente` (slug con trattino) è **rinviata a Fase 2** — vedi sezione sopra. Il codice nel partial è già pronto, NON modificare finché la tassonomia non viene registrata.
- `img` ha dimensioni gestite tramite costante PHP — vedi issue #3 (chiusa) sotto.
- Ruolo e città sono concatenati con ` — ` via `implode`. Se entrambi vuoti, `array_filter` restituisce array vuoto e il blocco non viene renderizzato — comportamento corretto.
- `data-post-id` sull'`<article>` è disponibile per JS futuro (es. lazy load contenuto esteso, carousel, modal). Non ha logica JS attuale — placeholder intenzionale.

---

## Issues — stato

| # | File | Tipo | Descrizione | Priorità | Stato |
|---|------|------|-------------|----------|-------|
| 1 | `no-results.php` | 🟡 Hardcoded slug | `/contatti/` hardcodato — valutare Customizer option `drw_contatti_url` per flessibilità multi-cliente | Media | ⏳ Aperta |
| 2 | `card-testimonianza.php` | ✅ Decisione architetturale | **`tipologia-cliente` rinviata a Fase 2** — decisione Luigi 2026-05-03. Tassonomia valida, non urgente per v1.0. Il blocco badge è innocuo senza registrazione. Implementare in Fase 2 (vedi sezione tassonomia sopra). | Alta | ✅ **Chiusa** |
| 3 | `card-testimonianza.php` | ✅ Decisione design | **Thumbnail provvisoria — gestire con costante PHP configurabile.** Decisione Luigi 2026-05-03. Implementare `define('DRW_TESTIMONIANZA_THUMB_SIZE', 80)` in `functions.php` + `add_image_size('drw-testimonianza-thumb', 80, 80, true)`. Il partial legge la costante con fallback a 64. Uno studio di design che acquista il template cambia una sola riga. | Alta | ✅ **Chiusa — da implementare** |
| 4 | `card-testimonianza.php` | 🔵 Futuro | `data-post-id` pronto per JS — decidere se/quando implementare interazioni (modal, lazy load) | Bassa | ⏳ Aperta |

---

## Dipendenze incrociate

| Partial | Dipende da | Contratto |
|---------|------------|-----------|
| `no-results.php` | `global-layout.css` → `.drw-py-section`, `.drw-container` | Classi utility layout devono esistere |
| `no-results.php` | `global-layout.css` o `global.css` → `.drw-btn`, `.drw-btn-primary`, `.drw-btn-outline` | Classi button devono esistere |
| `no-results.php` | `global.css` → `.drw-label`, `.drw-label--accent` | Classi label devono esistere |
| `card-testimonianza.php` | `inc/testimonianze-template.php` → `drw_render_testimonianza_card()` | Funzione wrapper obbligatoria per uso sicuro |
| `card-testimonianza.php` | ACF Free → campi del gruppo Testimonianza | Se i field key cambiano, aggiornare le chiavi in questo partial |
| `card-testimonianza.php` | Tassonomia `tipologia-cliente` | **Fase 2** — rinviata, il partial è già pronto |
| `card-testimonianza.php` | `functions.php` → costante `DRW_TESTIMONIANZA_THUMB_SIZE` | Da implementare — vedi issue #3 chiusa |
| `card-testimonianza.php` | CSS del child theme → tutte le classi `.drw-testimonianza-card__*` e `.drw-badge` | Classi BEM devono essere definite nel CSS |
