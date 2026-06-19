# Ereader Dictionaries

Free, custom-built dictionaries for e-readers.

The first release is a **complete Spanish → Ukrainian dictionary for Kobo** — the first
comprehensive one of its kind. The project can grow into other language pairs and other
ereader formats over time.

## Download

Public download page: **https://mgrabovskyi.github.io/ereader-dictionaries/**

Current releases (both free):

- **Spanish → Ukrainian for Kobo** — `docs/downloads/dicthtml-es-uk.zip`
- **English → Ukrainian for Kobo** — `docs/downloads/dicthtml-en-uk.zip`

### What's in them

| | Spanish → Ukrainian | English → Ukrainian |
|---|---|---|
| Headwords | **65,122** (complete A–Z, plus Ñ) | **91,994** (top ~100k common words) |
| Inflected forms | **508,283** | **125,262** |
| Senses | multiple Ukrainian equivalents per word, in source order | same |
| Size | 4.1 MB | 4.2 MB |
| Works offline | yes — no account, no internet | yes |

## Install on Kobo

1. Download the dictionary you want (`dicthtml-es-uk.zip` or `dicthtml-en-uk.zip`).
2. Connect the Kobo to your computer by USB.
3. Copy the ZIP into `.kobo/custom-dict/` on the device (create the folder if it doesn't exist).
4. Safely eject the Kobo and restart it.
5. Open a book in that language and long-press a word to look it up.

Compatible with Kobo readers that support custom dictionaries (Clara, Libra, Sage, Elipsa,
Forma, Aura, Glo, Nia). Kindle uses a different format — ask if you'd like one.

## Custom builds

Need a **different language pair**, or this dictionary converted for a **different ereader
or format** (Kindle, PocketBook, KOReader, StarDict)? Open a GitHub issue with your device,
language pair, and format. Custom builds take real time to research, convert, test, and
package, so they're done for a **small donation**.

→ [Request a custom build](https://github.com/mgrabovskyi/ereader-dictionaries/issues/new)

## Project structure

- `docs/` — GitHub Pages site (`index.html`) and the public download (`downloads/`).
- `dictionaries/` — per-release metadata (`<pair>/manifest.json`).
- `research/` — build plans, source research, and implementation notes.

## How it's made

Source vocabulary comes from [Kaikki](https://kaikki.org/) / Wiktextract (derived from
Wiktionary). Entries are grouped by word, translated to Ukrainian with multiple senses
preserved, and packaged into the Kobo `dicthtml` format (via
[pgaskin/dictutil](https://github.com/pgaskin/dictutil)). It's a comprehensive,
community-grade resource for reading — not a hand-edited academic dictionary. Corrections
are welcome via issues.

## License

- **Dictionary data:** derived from Wiktionary via Kaikki/Wiktextract — **CC BY-SA**. The
  packaged dictionaries are distributed under the same license; attribution to Wiktionary
  is required, and redistribution is permitted.
- **Repository code and website:** **MIT License**.

Each release documents its data source and license in `dictionaries/<pair>/manifest.json`.

Not affiliated with Rakuten Kobo. "Kobo" is a trademark of its respective owner.
