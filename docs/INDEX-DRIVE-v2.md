# INDEX_DRIVE – Template Avvocati DRW

> **Versione: 2.0 — 4 maggio 2026**
> Aggiornato da: Perplexity/Comet — sulla base di navigazione diretta Drive + analisi file GitHub
> Versione precedente: v1.x (Google Docs — fotografia 15/Apr/2026)
> **Source of truth**: questo documento + `00_MASTER` su Drive + `FILE-MAP.md` su GitHub
>
> ⚠️ **REGOLA**: Aggiornare questo INDEX ogni volta che si aggiunge, rimuove o rinomina un file/cartella.

---

## COME USARE QUESTO INDEX

1. **Prima di qualsiasi modifica architettuale** → Consulta `00_MASTER`
2. **Prima di usare categorie/tag** → Consulta `03_Tassonomie` (vocabolario chiuso)
3. **Per trovare uno specifico file Drive** → Sezione cartella corrispondente in questo INDEX
4. **Per trovare la mappa del codice PHP** → `FILE-MAP.md` su GitHub (repository DRW)
5. **Per trovare decisioni passate / storici** → Cartella `5. ANALISI STATO PROGETTO`
6. **Per task operativi per Claude** → `07_Task per Claude/` in documenti tecnici/
7. **Quando apri una nuova chat con AI** → Carica: `00_MASTER` + `06B_PROMPT_PERSISTENTE_MASTER` + `06c_CLAUDE_MASTER_CONTEXT`

---

## REPOSITORY GITHUB (NOVITÀ — aggiunto aprile/maggio 2026)

> Tutto il codice PHP del child theme è versionato su GitHub.
> I file `docs/` contengono le mappe analitiche generate da Perplexity.

- **URL**: https://github.com/dr-web-seo-specialist/DRW
- **Branch attivo**: `main`
- **Cartella `docs/`** — mappe analitiche del progetto:

| File GitHub | Contenuto | Data |
|---|---|---|
| `FILE-MAP.md` | Mappa completa child theme + regole + TODO-CLAUDE | v1.4 — 4 mag 2026 |
| `css-map.md` | Analisi dettagliata /css (6 file) | mag 2026 |
| `js-map.md` | Analisi dettagliata /js (7 file) | mag 2026 |
| `root-templates-map.md` | Analisi template root PHP (25 file) | mag 2026 |
| `template-parts-map.md` | Analisi template-parts/ | mag 2026 |
| `drive-map.md` | Mappa struttura Google Drive | v1.0 — 4 mag 2026 |
| `INDEX-DRIVE-v2.md` | Questo documento | v2.0 — 4 mag 2026 |

> 📌 **WIKILINK OBSIDIAN**: Per attivare il grafo, importa questi file nel vault e usa `[[FILE-MAP]]`, `[[drive-map]]`, ecc.

---

## VAULT OBSIDIAN

- **Percorso nuovo vault**: `C:\Users\Ludus\WEBDESIGN DR WEB\lawyer template\`
- **Vault vecchio** (non usare): `LAVORO dr web/.obsidian/` su Drive
- **Struttura cartelle Obsidian suggerita** (da creare):

```
WEBDESIGN DR WEB/lawyer template/
├── 00-META/           ← INDEX, changelog, archivio chat
├── 01-MASTER/         ← 00_MASTER, 00_DRW_BRIEFING, 06e_DOC_LANDING_CORE
├── 02-ARCHITETTURA/   ← 01_Templates, 02_Divi, 02b_DESIGN_REFERENCES
├── 03-VOCABOLARIO/    ← 03_Tassonomie
├── 04-SCHEMA-SEO/     ← 05_Schema.org, link PDF Schema/ su Drive
├── 05-CSS-JS/         ← css-map.md, js-map.md da GitHub
├── 06-PROCESSI/       ← 04_Processi, guide clienti
├── 07-PROMPT-AI/      ← 06_Prompt, 06B, 06c, 06d, 06e, task-claude/
├── 08-VERTICALI/      ← APPENDICE_STRATEGIA, backlog
├── 09-ASSETS/         ← presentazioni, testi pagine
└── 10-GITHUB-MAPS/    ← copia locale dei file docs/ da GitHub
```

---

## STRUTTURA GOOGLE DRIVE

### ROOT: LAVORO dr web/

```
LAVORO dr web/
├── .obsidian/               ← VAULT VECCHIO — non usare
├── template avvocati/       ← CARTELLA PRINCIPALE PROGETTO
├── # RIEPILOGO SNAPSHOT — Template Avvocati — 15 Aprile
├── Discovery avvocati
├── INDEX                    ← questo documento (versione Google Docs)
└── riepilogo_claude_snapshot_2026-04-10.md
```

> ⚠️ I file `# RIEPILOGO SNAPSHOT`, `Discovery avvocati` e `riepilogo_claude_snapshot` sono nella ROOT di `LAVORO dr web/`, non dentro `template avvocati/`. Da spostare in `5. ANALISI STATO PROGETTO` nella prossima pulizia.

---

## CARTELLA PRINCIPALE: template avvocati/

### 1. ANALISI stato progetto template/

> Storico sessioni di lavoro — cartella di sola lettura/archivio.
> **Non modificare i file qui dentro.** Sono snapshot storici.

**Sottocartella: 23 marzo/** (`https://drive.google.com/drive/folders/1aDgofoNSp1RfSTUAshOg63x3OOojqD-r`)

| File | Tipo | Contenuto |
|---|---|---|
| 001_2026-02_template-strategia-sorgenti | Doc+PDF | Strategia iniziale e sorgenti |
| 002_2026-02_documento-master-template-v2-1 | Doc+PDF | Master v2.1 — versione storica |
| 003_2026-03_update-system-vendita-supporto | Doc+PDF | Sistema vendita e supporto |
| 004_2026-03_dot-menu-onepage-ux-sito-personale | Doc+PDF | UX menu dot e onepage |
| 005_2026-03_plugin-demo-import-immagini-licenze | Doc+PDF | Plugin, demo, licenze immagini |
| 006_2026-03_prompt-claude-template-pagine | Doc | Prompt Claude per template pagine |
| 007_2026-03_architettura-pagine-categorie-tag-header | Doc | Architettura pagine e header |
| 008_2026-03_cpt-aree-template-php-tassonomie-schema-docs | Doc | CPT Aree + tassonomie + schema |
| 009_2026-03_cpt-testimonianze-functions-acf-roadmap | Doc | CPT Testimonianze + ACF + roadmap |
| 010_2026-03_landing-template-avvocati-divi5-header-regia-layout | PDF | Landing Divi5 — layout regia |
| 011_2026-03_landing-oro-menu-layout-timeline-statua-video | Doc | Landing oro + menu + timeline |
| 012_2026-03_prompt-stitch-layout-header-pagine-usage | Doc | Prompt stitch layout header |
| 013_2026-03_claude-setup-js-refactor-timeline-economia | Doc | Setup JS + refactor timeline |
| 014_2026-03_landing-contatori-html-css-js-fossa | Doc | Landing contatori HTML/CSS/JS |
| 015_2026-03_tassonomie-definitive-priorita-documenti-customizer | Doc+PDF | Tassonomie definitive + customizer |
| 016_2026-03_workflow-aggiornamento-docs-hamburger-mobile-css | PDF | Workflow docs + hamburger mobile |
| 017_2026-03_global-js-header-css-layout-statua-panel-video | PDF | Global JS + header CSS |
| 018_2026-03_fotografia-theme-docs-analisi-stato | Doc | Fotografia stato tema — snapshot |
| 15docs / 16-google docs ver / 17-docs ver / 18-docs ver | Doc+PDF | Versioni intermedie sessioni |
| SOMMARIO | Doc | Sommario generale sessioni |

**Cartella attiva**: `https://drive.google.com/drive/folders/1a76pAkPjrH4cSrxTFoHmzl7XT7RpfaI7`

> 📌 Classificazione Obsidian: `00-META/ARCHIVIO/analisi-marzo-2026/`

---

### 2. documenti per clienti/

`https://drive.google.com/drive/folders/10guTnBUmrC3qDvv9aIfzABMxFQyrTsr4`

| File | Link | Stato | Contenuto |
|---|---|---|---|
| A1_Guida cliente – Uso Template Avvocati | https://docs.google.com/document/d/... | Stabile | Manuale d'uso per il cliente finale |
| A2_Guida cliente – Creare nuove aree/avvocati/casi/sedi | https://docs.google.com/document/d/... | Stabile | Guida operativa contenuti CPT |

> 📌 Classificazione Obsidian: `06-PROCESSI/guide-clienti/`

---

### 3. documenti presentazione/

`https://drive.google.com/drive/folders/1I0aSWwASKb-eest04VMoPxXU3_HUt5Xz`

| File | Destinatario | Stato |
|---|---|---|
| P1 – Presentazione template per collaboratori (Roberta) | Collaboratori interni | Stabile |
| P2 – Presentazione commerciale per clienti (leave-behind) | Clienti finali | Stabile |

> 📌 Classificazione Obsidian: `09-ASSETS/presentazioni/`

---

### 4. documenti tecnici/

`https://drive.google.com/drive/folders/1L3A4HzjcWNmSi4FkRvf7lomXviLYyuYz`

> Cartella principale della documentazione tecnica operativa.
> **Questi sono i file da caricare nelle sessioni AI.**

#### Sottocartella: 07_Task per Claude/

> Task operativi per Claude. Priorità alta. Usare in combinazione con `06B_PROMPT_PERSISTENTE_MASTER`.

| File | Priorità | Note |
|---|---|---|
| 07-00_MASTER TASK LIST | 🔴 CRITICO | Elenco completo task — riferimento operativo principale |
| 07-01_TASK – Refactor helpers + schema + drw_studio | Alta | Task specifico |
| 07-02_TASK – Refactor mirato schema.php | Alta | Task specifico |
| 07-03_TASK – Footer base | Media | Task specifico |
| 07-04_TASK – Audit finale modulo Counters | Media | Task specifico |
| 07-05_TASK – WhatsApp floating + CTA sticky | Media | Task specifico |
| 07-06_TASK – Barra progresso + anchor nav long-form | Media | Task specifico |
| 07-07_TASK – Allineamento header, top bar, utility | Alta | Task specifico |
| 07-08_TASK – Sezione come lavoriamo | Media | Task specifico |
| 07-09_TASK – Page templates PHP selezionabili dal builder | Alta | Attende `page-templates/` vuota |
| 07-20_search | Media | Task ricerca |
| 07-47_TASK – Completamento enqueue.php | Alta | Task specifico |
| 07-48_TASK – Pulizia ACF doppio + verifica drw_studio() | 🔴 Alta | Vedi bug in FILE-MAP.md |
| # SKILL_CPT_GENERIC | Permanente | Skill CPT cross-verticale per Claude |
| # SKILL_DIVI_LAYOUT_FIRST | Permanente | Skill Divi > CSS per Claude |
| # SKILL_FILES_PHP | Permanente | Skill file PHP child theme per Claude |
| # SKILL_LOGGING | Permanente | Skill log di fine risposta per Claude |
| # Stato progetto Dr.Web Lawyer Template | Operativo | Snapshot operativo corrente |
| # STATO_PROGETTO SHORT | Operativo | Versione breve snapshot |

> 📌 Classificazione Obsidian: `07-PROMPT-AI/task-claude/`

#### Sottocartella: Schema/

> PDF di riferimento ufficiali Schema.org. Restano su Drive, non migrano in Obsidian.
> In Obsidian: crea note in `04-SCHEMA-SEO/` con link diretti a questi PDF.

| File PDF | Tipo Schema.org |
|---|---|
| Attorney - Schema.org Type.pdf | Attorney |
| LegalService - Schema.org Type.pdf | LegalService |
| LocalBusiness - Schema.org Type.pdf | LocalBusiness |
| Organization - Schema.org Type.pdf | Organization |
| Person - Schema.org Type.pdf | Person |
| ProfessionalService - Schema.org Type.pdf | ProfessionalService |
| Thing - Schema.org Type.pdf | Thing |

> 📌 Classificazione Obsidian: `04-SCHEMA-SEO/riferimenti-pdf/` — mantieni su Drive, linka da Obsidian

#### File diretti in documenti tecnici/

| File | Link Google Docs | Priorità | Stato | Obsidian |
|---|---|---|---|---|
| 00_MASTER – Architettura & Regole | https://docs.google.com/document/d/1O2a57ltoA-2AGIiOS1SEMNkwjHj4N7DEKHxuj_RseR | 🔴 CRITICO | Stabile | `01-MASTER/` |
| 00_DRW_BRIEFING_UNIVERSALE | https://docs.google.com/document/d/1xlUEuhlF39HIHuQWTOdZrI2Qb-_XU9_Su1XDCB2jGAA | 🔴 CRITICO | Stabile | `01-MASTER/` |
| 01_Templates PHP – Elenco file & mappatura | https://docs.google.com/document/d/13faP0e7x2zgBrlnWlOiynCL5vnVk8We8IyPj_S01ZT0 | Alta | Stabile | `02-ARCHITETTURA/` |
| 01_Templates PHP – Bozza mappatura | https://docs.google.com/document/d/18oTALgGnDSv1VlzF0NsbX5E5fp0Cirog6bUTgYcetvU | Bassa | Bozza — da archiviare | `02-ARCHITETTURA/` |
| 02_Divi – Layout, Libreria, Blocchi | https://docs.google.com/document/d/1fT5dkyObBDnnG4jQl7mGsSIdQVUV15Vqo2pCsQmrKk4 | Alta | In aggiornamento | `02-ARCHITETTURA/` |
| 02b_DOC_DESIGN_REFERENCES | https://docs.google.com/document/d/1kFDIMp0blWBd70R3uuX3kdIiqUPktV6J8JwEu2dSgkc | Alta | Stabile | `02-ARCHITETTURA/` |
| 02b_DOC_DESIGN_REFERENCES – SHORT | https://docs.google.com/document/d/1pdlxBtNgyRn9R7eqTZh2zUobqUmbQfWgVCxeZ4HUggo | Media | Stabile | `02-ARCHITETTURA/` |
| 03_Tassonomie | https://docs.google.com/document/d/1EFHZATdnHjbpMYXLufUyPvpi0lQBoKtc9o0adkUNf5o | 🔴 CRITICO | Definitivo — 49 cat. + 274 tag | `03-VOCABOLARIO/` |
| 04_Processi | https://docs.google.com/document/d/1utsH2boSIeG257P1lAfm62V7PxHtMZxP6ft6nySWAL8 | Alta | Stabile | `06-PROCESSI/` |
| 05_Schema.org – Architettura & Implementazione | https://docs.google.com/document/d/19FlHCAdl9bT0Mwi-hbWEhjsP6F1VcAn59iSOkymhFfU | 🔴 CRITICO | Da aggiornare per baseline ACF free | `04-SCHEMA-SEO/` |
| 06_Prompt AI – Dr.Web Lawyer | https://docs.google.com/document/d/1PhO3CyTb32nZ7R6x60GMLzlwjNwAfmQjlKc-k2LLZHg | Alta | Stabile | `07-PROMPT-AI/` |
| 06B_PROMPT_PERSISTENTE_MASTER | https://docs.google.com/document/d/1FrKU2t-7iuTqqsnzgN19_bwQNVn6j2rirqO4BL_BoJA | 🔴 CRITICO | Specifico per Claude | `07-PROMPT-AI/` |
| 06c_CLAUDE_MASTER_CONTEXT | https://docs.google.com/document/d/1TTbwxpkHXlFHwCJnm5LOGiux78WWhfXAlz7XwUp16UE | 🔴 CRITICO | Specifico per Claude | `07-PROMPT-AI/` |
| 06d_DOC_ROBERTA | https://docs.google.com/document/d/1KNtrRUdb_IQDd3-PCWVJ771I93Fq7UUtFOGF05J7FmM | Alta | Stabile | `07-PROMPT-AI/` |
| 06d_DOC_ROBERTA – SHORT | https://docs.google.com/document/d/1ztO5yWf8RihQWD5Si_uwWix1GhDncWWbZKqkuPWCpEo | Media | Stabile | `07-PROMPT-AI/` |
| 06e_DOC_LANDING_CORE | https://docs.google.com/document/d/1q7DpnzyJbYE0STyRM5eebZ3M5_gFcazqe5GvOIIBBxc | Alta | Stabile | `01-MASTER/` |
| ANALISI_STATO_2026-03_theme+docs | https://docs.google.com/document/d/1vYoUhIgmGpOhfRqfd6x6RYYbnTpkVpT0ZC1R34_ov5E | Bassa | Stabile — snapshot marzo 2026 | `00-META/ARCHIVIO/` |
| APPENDICE_STRATEGIA – Verticali & Conversione | https://docs.google.com/document/d/1qM9L5EmMkEHEzZKsZGucj6AiNm4b_uhVt7UN0HPIJko | Alta | Stabile | `08-VERTICALI/` |
| PROMPT_CORE_Template_Avvocati_DrWeb | (link da verificare) | Alta | Stabile | `07-PROMPT-AI/` |
| riepilogo_claude_snapshot_2026-04-10.md | https://drive.google.com/file/d/1NxswGnFZHprRjiAxZcwxfxUpmGUkfy4p | Media | Stabile — snapshot 10 apr 2026 | `07-PROMPT-AI/` |
| tags | (link da verificare) | Bassa | Contenuto da verificare | da classificare |

> ⚠️ **NOTA su `01_Templates PHP – Bozza mappatura`**: Probabilmente obsoleto rispetto a `Elenco file & mappatura` + `FILE-MAP.md` su GitHub. Verificare e archiviare.
> ⚠️ **NOTA su `05_Schema.org`**: Stato "da aggiornare per baseline ACF free" — allinearlo con l'implementazione in `inc/schema.php` dopo i fix del TODO-CLAUDE in `FILE-MAP.md`.
> ⚠️ **NOTA su `tags`**: File con contenuto non verificato. Potenziale duplicato di `03_Tassonomie`. Controllare prima di classificare.

---

### 5. documenti temporanei/

`https://drive.google.com/drive/folders/13eW9IQKUwkiLQOLiu8vESvo68PRHDUbw`

> 🚨 **Da pulire.** Questi file NON sono fonte stabile. Consultare come archivio, non come riferimento.

| File | Azione consigliata | Priorità |
|---|---|---|
| TMP – Idee/Backlog Template Avvocati | Migrare in `08-VERTICALI/backlog.md` | Media |
| TMP – Bozze testi pagine | Migrare in `09-ASSETS/testi/` se ancora utili | Media |
| TMP – Appunti & Thread da riassumere | ARCHIVIA dopo lettura | Bassa |
| TMP - tags e categorie da terminare | Consolida in `03_Tassonomie` poi elimina | Alta |
| TEMPLATE AVVOCATI DR WEB 3.0 | Mantieni come documento storico su Drive | Bassa |
| TAGS e CATEGORIES per template avvocati | Verifica duplicato di `03_Tassonomie` | Alta |
| RIEPILOGO_PER_CLAUDE_2026-03-05 | ARCHIVIA in `5. ANALISI STATO PROGETTO` | Bassa |
| piano di sviluppo (storico non piu' valido) | ARCHIVIA — non usare come riferimento | Bassa |
| Header Globale — Guida Divi 5 Theme Builder | Migrare in `02-ARCHITETTURA/` se aggiornato | Media |
| CPT e struttura file | Migrare in `02-ARCHITETTURA/` se aggiornato, altrimenti archivia | Media |
| BASI DI PARTENZA per landing e grafica template | Migrare in `01-MASTER/` se ancora valido | Media |
| APPENDICE – STRATEGIA VENDITA E SUPPORTO | Verificare se duplicato della versione in tecnici/ | Alta |

---

## FILE DA CARICARE IN UNA NUOVA SESSIONE AI

> Questo blocco va copiato come checklist all’inizio di ogni nuova chat con Claude o Perplexity.

### 🔴 OBBLIGATORI (carica sempre)
1. `00_MASTER` – Architettura & Regole
2. `06B_PROMPT_PERSISTENTE_MASTER` – Spec macchina Claude
3. `06c_CLAUDE_MASTER_CONTEXT` – Context master permanente

### 🟡 CONTESTUALI (carica in base al task)
| Se stai lavorando su... | Carica anche... |
|---|---|
| Tassonomie / categorie / tag | `03_Tassonomie` |
| Schema.org / JSON-LD | `05_Schema.org` + PDF Schema/ |
| Layout / Divi / CSS | `02_Divi` + `02b_DOC_DESIGN_REFERENCES` |
| Codice PHP / template | `FILE-MAP.md` (GitHub) + `01_Templates PHP` |
| Task operativi Claude | `07-00_MASTER TASK LIST` + task specifico |
| Design / UI con Roberta | `06d_DOC_ROBERTA` |
| Processi editoriali | `04_Processi` |
| Verticali / nuovi clienti | `APPENDICE_STRATEGIA` |
| Landing / ADV | `06e_DOC_LANDING_CORE` |

---

## PIANO MIGRAZIONE IN OBSIDIAN

### Fase 1 — Immediata (prima sessione Obsidian)
> Documenti più critici. Creare note stub con link Drive.

1. `00_MASTER` → `01-MASTER/00-MASTER.md`
2. `00_DRW_BRIEFING_UNIVERSALE` → `01-MASTER/00-BRIEFING.md`
3. `03_Tassonomie` → `03-VOCABOLARIO/tassonomie.md`
4. `05_Schema.org` → `04-SCHEMA-SEO/schema-architettura.md`
5. `06B_PROMPT_PERSISTENTE_MASTER` → `07-PROMPT-AI/prompt-persistente.md`
6. `06c_CLAUDE_MASTER_CONTEXT` → `07-PROMPT-AI/claude-master-context.md`
7. `FILE-MAP.md` (da GitHub) → `10-GITHUB-MAPS/FILE-MAP.md`
8. Questo INDEX → `00-META/INDEX-DRIVE-v2.md`

### Fase 2 — Entro prima settimana
9. `02_Divi` + `02b_DOC_DESIGN_REFERENCES` → `02-ARCHITETTURA/`
10. `01_Templates PHP` (Elenco file) → `02-ARCHITETTURA/`
11. `04_Processi` → `06-PROCESSI/`
12. `APPENDICE_STRATEGIA` → `08-VERTICALI/`
13. `07-00_MASTER TASK LIST` + task specifici → `07-PROMPT-AI/task-claude/`
14. `06d_DOC_ROBERTA` → `07-PROMPT-AI/`
15. `06e_DOC_LANDING_CORE` → `01-MASTER/`
16. Chat storicizzate (esportate) → `00-META/ARCHIVIO/chat/`

### Fase 3 — Pulizia documenti temporanei
- Consolidare TMP/* in destinazioni definitive
- Archiviare storici (piano di sviluppo, riepilogo marzo)
- Eliminare duplicati dopo verifica
- Decidere destino BASI DI PARTENZA e CPT e struttura file

---

## NOVITÀ RISPETTO A v1.x (aprile → maggio 2026)

| Novità | Dettaglio |
|---|---|
| Repository GitHub attivo | `dr-web-seo-specialist/DRW` — branch `main` |
| 7 file `docs/` su GitHub | FILE-MAP, css-map, js-map, root-templates-map, template-parts-map, drive-map, INDEX-DRIVE-v2 |
| FILE-MAP.md v1.4 | Analisi completa 15 file PHP, 10 file inc/, TODO-CLAUDE con 6 issue tracciate, wikilink Obsidian |
| Nuovo vault Obsidian | `C:\Users\Ludus\WEBDESIGN DR WEB\lawyer template\` (vecchio vault .obsidian/ su Drive deprecato) |
| Struttura Obsidian definita | 10 cartelle suggerite (00-META → 10-GITHUB-MAPS) |
| Bug `taxonomies.php` tracciato | Slug CPT hardcoded — fix in TODO-CLAUDE ð´ Alta priorità |
| drive-map.md creato | Prima mappa strutturata di tutto il Google Drive |

---

## NOTE CRITICHE

- **`drwstudio`** è l'unico access point per i dati studio → sempre via helpers, mai query dirette
- **Costanti CPT** (`DRW_CPT_*`): mai hardcodare slug — vedi bug in `inc/taxonomies.php`
- **ACF Free only**: niente `acf_add_options_page()`, niente Pro
- **`functions.php` è solo bootstrap**: nessuna logica specifica
- **Vocabolario tassonomico chiuso**: non estendere `03_Tassonomie` senza approvazione Luigi
- **`page-templates/` e `languages/` sono VUOTE**: non sono dimenticate, attendono sviluppo futuro
- **`demo-switcher.php`** contiene commenti HTML di debug — rimuovere prima di v1.0
- **`single-testimonianza.php`** (939 B) è probabilmente uno stub — verificare gestione accesso diretto URL

---

*Fotografia stato: 4 maggio 2026 — v2.0*
*Autori: Luigi + Perplexity/Comet*
*Prossimo aggiornamento: dopo setup Obsidian e prima sessione nuova chat*

> 📌 **WIKILINK OBSIDIAN**: `[[INDEX-DRIVE-v2]]` `[[FILE-MAP]]` `[[drive-map]]` `[[css-map]]` `[[js-map]]` `[[root-templates-map]]` `[[template-parts-map]]`


---

## CARTELLA CLIENTI (nuova — 17 maggio 2026)

> Cartella parallela a `template avvocati/` nella root del Drive Dr.Web.
> Contiene tutti i progetti cliente indipendentemente dal verticale del template utilizzato.

📁 **Clienti**
https://drive.google.com/open?id=1qZpKy3V9dxx77zWB2YidnVXce95iugny

---

### 📁 SuperMario24
https://drive.google.com/open?id=1KBx9s-xFRSgq8SH90TnDS6VELjOFfnaA

Cliente: Mario De Filpo — SuperMario24 (Milano)
Settore: Servizi tecnici multiservice (climatizzatori, caldaie, fotovoltaico)
Stato: In trattativa — primo cliente del template verticale idraulico/multiservice
Avviato: 17 maggio 2026

📄 **2026-05-17 — Comunicazione cliente**
https://drive.google.com/open?id=1H4Qjbo-YIx1fJkcnl-xpQ4sAQKxM3OpCwaoNBlDAfZY
Prima comunicazione formale al cliente con piano di lavoro in 3 fasi e tabella opzioni climatizzatori.

📄 **2026-05-17 — Analisi siti preliminare**
https://drive.google.com/open?id=10LesHxJllAmCSAjqxTnW3CWJnpzaOQoOranMU8VJ6c4
Analisi tecnica dei siti esistenti: assistenza-caldaie-milano.com e supermario24.it/climatizzatori/milano/

📄 **2026-06 — Preventivo (quando pronto)**
https://drive.google.com/open?id=1_5GO0262g82v4h9-CaqJ47ecx6cvaM87-BvSTw0pX68
Preventivo formale — da completare.

---

### 📁 ATV Milano
https://drive.google.com/open?id=1UO90n0myv2MBXOA32eTuZrSjytQ-7Odk

Cliente: ATV Milano
Settore: Service audio/video/luci per eventi e congressi
Stato: Potenziale — contatto da riprendere post-23 maggio 2026
Realizzazione prevista: da ottobre 2026

---

### 📁 Abil Assistance
https://drive.google.com/open?id=1nBiLiRWUa7gQcCTF35l4-IXOCi-DX34t

Cliente: Abil Assistance
Settore: Assistenza
Stato: Lavoro aperto non completato — decisione pendente (opzione A/B/C entro 31 maggio 2026)

---

*Aggiornamento: 17 maggio 2026 — Perplexity/Comet*
