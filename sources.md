# Sources

Inspected sources for the ledger (see `schema.md`). Retrieved 2026-10-28.

| source_id | Title | Location | Version / hash | Use |
| --- | --- | --- | --- | --- |
| metr-html | METR, OpenAI / Hugging Face incident investigation (HTML) | https://metr.org/blog/2026-08-26-openai-hugging-face-incident-investigation/ | Published 2026-08-26 (per URL); no hash | Primary contribution-evidence corpus: attributed episodes, selected messages, findings, methodology, appendices. |
| metr-pdf | METR, Hugging Face incident investigation report (PDF) | https://metr.org/hugging-face-incident-report-aug-2026.pdf; local: `sources/hugging-face-incident-report-aug-2026.pdf` | sha256 `f7e87133833363da6eee661df0c86926ef852f8b202ad87695ea6596b92d3c73` | Page-addressable reference. Another representation of `metr-html`, not independent corroboration (same `source_family_id`). |
| openai-report | OpenAI, Hugging Face Incident Technical Report (PDF) | https://cdn.openai.com/pdf/67869394-cb91-4c12-888c-5cbd85c7814c/OpenAI-Hugging-Face%20Incident-Technical-Report.pdf; local: `sources/OpenAI-Hugging-Face Incident-Technical-Report.pdf` | sha256 `dd635cf6e5f39f0e1f646f08c36549090d77156ed89cbd3d733ed496648cae9c` | Cross-check outcomes and chronology. Some actions are attributed only to unnamed agents. |
| recovered-tables | Recovered tables from the METR incident figures (Hugging Face dataset, unofficial third-party extraction) | https://huggingface.co/datasets/LauraGomezjurado/openai-hf-incident-recovered-tables | git commit `1d6ac42d70df4c8d827eba67cf3a5bc10ba07454` | Structured activity data (`agents.csv`, `workstream_counts.csv`, `cohort_followup.csv`). Provenance caveats apply; chart-row identifiers are not agent handles. |

## Source families

- `metr-html` and `metr-pdf` are one family.
- `recovered-tables` derives from METR's published chart data, so it is not independent of the METR family.
- `openai-report` is a separate publisher. Whether its observations are independent of METR's is unresolved.

## Status

- Text of `metr-html` was read only in part (roughly the first 100,000 of about 238,000 characters). Counts quoted from it are unconfirmed.
- The PDFs have not yet been text-extracted.
- The README, schemas and audit files of `recovered-tables` have not yet been read.

## Background reporting (secondary, not evidence)

Used only for the context in `the_incident.md`. See that file for the list.
