# Spanish → Ukrainian Kobo dictionary — scope & build plan

Status: **scoping** (no data pulled yet). Target: a `dicthtml-es.zip` sideloadable
dictionary with **multi-sense Ukrainian glosses** (richness like the existing "dict 2"),
that resolves inflected Spanish word-forms on tap.

## Goal & direction

- Reader: Ukrainian speaker reading **Spanish** books on a Kobo.
- Lookup direction: **headword = Spanish**, definition = **Ukrainian**.
- Quality target: per Spanish word, show its part of speech and multiple senses,
  each with a Ukrainian equivalent/gloss — not just a comma list.

## Two hard constraints discovered during research

1. **Kobo matching is prefix-only.** Firmware looks up the tapped word in a MARISA
   trie (`words` file); if no exact hit it falls back to the *first prefix match*
   (`tests` → `test`). It has **no morphology**. Spanish is heavily inflected and
   full of irregulars (`fui`→`ir`, `puedo`→`poder`, `dije`→`decir`), which prefix
   matching cannot resolve. → We MUST add inflected forms as `<k>` variant keys
   pointing at the lemma entry.
   - Source for inflections: Kaikki/wiktextract `forms[]` array (has conjugations,
     plurals, gender forms with `tags`).
   - Format spec: https://pgaskin.net/dictutil/dicthtml/format.html

2. **No Ukrainian Wiktionary extract on Kaikki.** Kaikki ships an English-edition
   extract (Spanish words with English glosses) and per-edition extracts, but no
   Ukrainian edition. So Ukrainian text must be **pivoted in**, not read from a
   uk dump. This is the central design decision (below).

## Confirmed sources (downloadable, June 2026)

| Source | What it gives | Licence | Notes |
|---|---|---|---|
| **Kaikki / wiktextract** English edition | Spanish lemmas, POS, `senses[].glosses` (English), `forms[]` inflections, `translations[]` w/ lang codes | CC-BY-SA | Master backbone. ~2.6GB gz (full) or filter to Spanish. https://kaikki.org/dictionary/Spanish/ |
| **PanLex** | Direct ES→UK word equivalents (bridged across 2500+ dicts) | CC0 / permissive | Best *direct* pair coverage; word-level, no senses. Monthly CSV/JSON/XML snapshot. https://panlex.org |
| **Apertium** `apertium-spa`, bridge via eng | Lemmatized bilingual `.dix`; good morphology data | GPL/free | Useful cross-check for inflections; no direct spa-ukr pair. |
| **dictgen / dictutil** (pgaskin) | Builds the final `dicthtml-es.zip` from a simple text format | MIT | The packaging tool. https://pgaskin.net/dictutil/dictgen/ |

Avoid: Glosbe / Reverso / dict.cc (ToS / licensing).

## The crux: where do the Ukrainian glosses come from?

ES→UK is low-resource; there is no single rich ES→UK sense dictionary. Best result
comes from a **hybrid**, layered by confidence:

- **Layer A — direct pair (PanLex):** attach PanLex ES→UK equivalents at the lemma
  level. High precision where present; no sense disambiguation.
- **Layer B — sense-level pivot via English:** for each Kaikki sense, take its
  English gloss and translate to Ukrainian. Two sub-options:
  - B1: use enwiktionary's English→Ukrainian translation tables (free, but only
    covers headword-ish glosses).
  - B2: machine-translate the English gloss to Ukrainian (covers everything;
    quality varies; needs a translator — local model or cloud API).
- **Merge rule (proposed):** keep Kaikki sense structure as the skeleton; per sense
  show Ukrainian from PanLex if the sense's keyword matches a PanLex equivalent,
  else fall back to the pivoted (B) Ukrainian. De-dup, cap senses per entry.

This yields multi-sense entries with Ukrainian per sense + inflection-aware lookup.

## Proposed pipeline (build phases — not started)

1. **Acquire**: download Kaikki Spanish-filtered extract + PanLex ES/UK snapshot.
2. **Parse backbone**: from Kaikki, build records: lemma, POS, senses[] (English
   gloss), forms[] (inflected variants).
3. **Attach Ukrainian**: join PanLex (Layer A) + pivot English glosses (Layer B).
4. **Emit dictgen source**: one `@ lemma` block per word, Ukrainian senses as body,
   every inflected form as a `& variant` so Kobo resolves conjugations.
5. **Pack**: `dictgen` → `dicthtml-es.zip`; install per dictutil docs.
6. **Validate**: spot-check on-device — irregular verbs, plurals, reflexives,
   accented forms (á/é/í/ó/ú/ñ), multi-sense display.

## Decisions (locked 2026-06-10)

- **Size budget:** ≤100 MB final zip. Non-binding in practice (expect single-digit
  to ~20 MB even at full coverage); so → **go full-coverage, rich**.
- **Ukrainian sourcing = layered hybrid, curated-first:**
  1. PanLex direct ES→UK equivalents (curated),
  2. Wiktionary EN→UK translation tables (curated),
  3. machine translation of the gloss **only as fallback** for uncovered senses.
- **First deliverable = proof-of-concept on letter "a"** end-to-end → installable
  `dicthtml-es.zip`, inspect quality on-device, then scale to full lexicon.

## Build findings (2026-06-10 PoC session)

- **Data**: `es-extract.jsonl.gz` (96 MB) downloaded & valid. 852,749 `lang_code==es`
  entry-lines; **837,601 unique Spanish words**. Letter 'a' alone = 134k single-word
  lemmas (many Basque surnames/proper nouns → must filter `pos!="name"` + lowercase).
- **Schema confirmed**: lemmas carry real `senses[].glosses` (Spanish) + full
  `forms[]` (verbs = 137 conjugations). Inflected forms ALSO exist as standalone
  entries tagged `senses[].tags=["form-of"]` with a `form_of` pointer → DROP these
  as headwords, instead attach lemma's `forms[]` as `<k>` variants.
- **dictgen + dictutil**: installed via `go install`, working (`build-es-uk/bin/`).
- **Tooling**: python3.11, go, curl, jq present. Python urllib needs
  `SSL_CERT_FILE=$(python3 -m certifi)` or downloads fail SSL verify.
- **Translation engine test results**:
  - Argos es→en→uk **pivot = unusable**. Even word-level: aburrido→"панчохи",
    abismo→"Аббревіатури", abrazar→"свінгери". Double-hop destroys meaning. REJECTED.
  - **PanLex API blocked in this sandbox** — `api.panlex.org`/`db.panlex.org` don't
    resolve (only website hosts do). Only the multi-GB bulk dump would be usable here.
  - Decision pending: NLLB-200 direct (offline, ~2.4GB) vs LLM API (best, sense-aware,
    costs at 800k scale) vs PanLex bulk (curated, heavy). See decision below.

## Key references
- Kobo dict format: https://pgaskin.net/dictutil/dicthtml/format.html
- dictgen: https://pgaskin.net/dictutil/dictgen/
- Kaikki Spanish: https://kaikki.org/dictionary/Spanish/
- wiktextract schema: https://github.com/tatuylonen/wiktextract
- PanLex: https://panlex.org
