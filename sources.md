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

- `metr-pdf` is the inspected version of the METR report. Its text was extracted and read except for the methodology appendix (about pp.73-85, apart from the signing-key table) and figure-only content. `metr-html` was not used as evidence.
- Counts quoted from the report (about 1,200 agents, 533, about 700, 11, 19) were confirmed against the PDF text and are recorded in `ledger/context_counts.csv`.
- `openai-report`: text extracted, not yet read. No evidence cites it.
- `recovered-tables`: README, `data_audit.json` and `source_manifest.json` read; `agents.csv` and `workstream_counts.csv` profiled only for row counts and workstream names. Unexplained gaps: 684 positioned onsets versus 688 declared participants, and 138 Sol rows (about 11%) versus METR's "about 5%" Sol.

## Background reporting (secondary, not evidence)

Used only for the context in `the_incident.md`. See that file for the list.
