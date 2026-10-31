# Abstract Screener

A single-file, browser-based tool for title and abstract screening in a systematic review. Load your references, read each abstract, and mark it Include, Exclude or Maybe. As you work, the tool learns from your decisions and suggests which unscreened records look like includes or excludes. You always make the final decision.

Everything runs locally in your browser. No installation, no server, no uploads, and no internet connection needed once you have the file.

## Quick start

1. Deduplicate your references first (for example with the Reference Deduplicator in the other repository).
2. Download `screener.html` and open it in any modern browser (Chrome, Edge, Firefox, Safari).
3. Drag your reference files onto the drop zone, or click it to choose files. You can add several files at once.
4. Screen each record using the buttons or keyboard shortcuts.
5. Save your session regularly and export your decisions when you are done.

## Supported formats

| Format | Extensions | Typical sources |
|---|---|---|
| RIS | `.ris`, `.txt` | PubMed, Embase, Scopus, Web of Science, EndNote, Zotero |
| BibTeX | `.bib` | Zotero, Google Scholar, Overleaf |
| CSV / TSV | `.csv`, `.tsv` | Rayyan, Covidence, spreadsheets |
| Saved session | `.json` | Files saved by this tool |

CSV files need a header row with at least a `title` column. The tool also recognises `authors`, `year`, `doi`, `journal` and `abstract` columns, matched case-insensitively.

Abstracts are read from your files. If a record has no abstract, the tool says so and you can decide from the title or check the original record. Dragging items directly out of reference managers such as Zotero is not supported; export your collection to a file and drop the file in.

## Screening

Each record is shown one at a time with its title, authors, year, journal, DOI and abstract.

| Action | Button | Shortcut |
|---|---|---|
| Include | Include | `I` |
| Exclude | Exclude | `E` |
| Maybe | Maybe | `M` |
| Undo last decision | Undo | `U` |

After each decision the next unscreened record appears automatically. To review or change an earlier decision, click it in the Records list on the right, then choose a new decision. Use the filter above the list to show only unscreened, included, excluded or maybe records.

## Keyword highlighting

Enter comma-separated terms in the **Include terms** and **Exclude terms** boxes. Matches are highlighted in green (include) and red (exclude) in the title and abstract, which makes scanning faster. Matching is by partial text, so "random" also highlights "randomized". Keywords also nudge the suggestion for each record slightly.

## How the suggestions work

The tool learns only from your own decisions, using a simple word-frequency model (a naive Bayes style approach):

1. After you have marked at least **3 includes and 3 excludes**, it starts learning. Before that, only keywords influence the suggestions.
2. For each word in the titles and abstracts, it compares how often the word appears in records you included versus excluded.
3. Each unscreened record is scored from the words it contains and labelled **Likely include**, **Likely exclude** or **Uncertain**. The words that drove the suggestion are shown beside the abstract.
4. The model retrains after every decision. Nothing is sent anywhere.

Records marked Maybe are not used for learning.

### Ordering the queue

- **File order**: records appear in the order they were imported.
- **Most likely includes first**: relevant records surface early, which is useful for prioritising.
- **Most uncertain first**: records the model is least sure about come first. This usually teaches the model fastest.

## Saving and exporting

- **Save session (JSON)**: stores all records, decisions and keywords. Drop the file back into the page to continue later. Do this regularly.
- **All decisions (CSV)**: every record with its decision (Include, Exclude, Maybe or Unscreened).
- **Included records (RIS)**: for importing into your reference manager or the next stage of your review.
- **Clear everything**: deletes all records and decisions from the page and from the browser's autosave.

The tool also autosaves to your browser's local storage, but browsers can clear this (for example when clearing site data or in private windows). Do not rely on it as your only copy.

## Limitations

- The suggestions are a rough guide based on word patterns. They are weakest at the start, with subtle inclusion criteria, and when relevant records use different vocabulary. They can miss relevant studies.
- Do not stop screening because the remaining records look like excludes. Screen every record unless your protocol explicitly justifies otherwise.
- It supports a single reviewer per copy. For independent double screening, each reviewer should use their own copy of the tool and compare the exported CSVs separately.
- There is no field for exclusion reasons, which are normally recorded at full-text screening.
- Undo history is lost when you reload the page. The Records list shows the first 300 matching records; use the filter to narrow it down.
- Only title, authors, year, journal, DOI and abstract are carried through. Other fields (volume, pages, keywords, notes) are not preserved in the exported files, so keep your original files.
- It does not deduplicate. Deduplicate before screening.
- Please validate the tool on a sample of your own data before relying on it. If you use the suggestions in a review, describe how in your methods section.

## Credits

Created with [Claude](https://claude.ai) (Claude Sonnet 5.5), an AI assistant made by [Anthropic](https://www.anthropic.com), in October 2026. The design, requirements and testing on real review data were directed by the repository author.

