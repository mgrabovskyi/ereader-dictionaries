# Ereader Dictionaries

Custom dictionaries for ereaders.

The first dictionary in this project is a Spanish to Ukrainian dictionary for Kobo readers. The project can later grow into English to Ukrainian dictionaries, other language pairs, and conversion tools for other ereader formats.

## Download

The public download page is designed to live at:

https://mgrabovskyi.github.io/ereader-dictionaries/

Current local release:

- Spanish to Ukrainian for Kobo: `docs/downloads/dicthtml-es-uk.zip`

## Install on Kobo

1. Download `dicthtml-es-uk.zip`.
2. Connect the Kobo to your computer by USB.
3. Copy the zip file into `.kobo/custom-dict/` on the device.
4. Safely eject the Kobo and restart it.
5. Open a Spanish book and use word lookup.

If the `custom-dict` folder does not exist, create it inside `.kobo`.

## Project Structure

- `docs/` - GitHub Pages site and downloadable files.
- `dictionaries/` - dictionary metadata, notes, and future release assets.
- `research/` - build plans, source research, and implementation notes.

## Future Plans

- Improve coverage and quality of the Spanish to Ukrainian Kobo dictionary.
- Add English to Ukrainian.
- Convert dictionaries for other ereaders and dictionary formats.
- Offer custom dictionary builds for specific ereaders or language pairs.

## Custom Requests

Need a dictionary for a specific ereader, language pair, or format? Open a GitHub issue with the details. I can build custom versions for a small donation when the source data and format make it practical.

## License

Dictionary data may come from multiple upstream sources with their own licenses. Each release should document its source data and license notes before publication.

Repository code and website files are intended to be released under the MIT License unless stated otherwise.
