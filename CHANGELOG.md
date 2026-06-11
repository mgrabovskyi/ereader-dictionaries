# Changelog

## en-uk v1.0.0 — 2026-06-12

First release of the **English → Ukrainian dictionary for Kobo**.

- 51,951 headwords, scoped to match common existing EN→UA Kobo dictionaries (skips
  misspellings, concatenated compounds, and ultra-rare/abbreviation noise).
- 83,084 inflected forms mapped to their base entries.
- Multi-sense entries with multiple Ukrainian equivalents per word.
- Packaged as `dicthtml-en-uk.zip` (2.8 MB).

## es-uk v1.0.0 — 2026-06-10

First complete release of the **Spanish → Ukrainian dictionary for Kobo**.

- Complete coverage: 65,122 headwords across all letters A–Z plus Ñ.
- 508,283 inflected forms (verb conjugations, plurals) mapped to their base entries, so
  any form looked up on-device resolves correctly.
- Multi-sense entries: each distinct meaning carries its own Ukrainian equivalent, in the
  same order as the source.
- Packaged as `dicthtml-es-uk.zip` (4.1 MB) for the Kobo `dicthtml` format.

Supersedes the earlier preview build (partial A–O coverage).

### Known limitations

- ~0.28% of entries (183) have a sense count that differs slightly from the source
  (a sense merged or split during translation). All are translated; none are missing.
- Community-grade quality: built from Wiktionary data with generated translations, not a
  hand-edited academic dictionary. Corrections welcome via GitHub issues.
- Not included by design: word-formation suffixes (e.g. `-ito`), proper place names, and
  symbol/abbreviation entries.
