# FIRST DNS Abuse SIG

This repo holds the source content and publishing tooling for the [FIRST](https://www.first.org/) **DNS Abuse Special Interest Group (SIG)** website and its primary publication, the **DNS Abuse Techniques Matrix**.

> **Looking for the SIG itself?** The official homepage is <https://www.first.org/global/sigs/dns> - the authoritative source for the SIG's mission, goals, meeting schedule, chairs, and how to join. It is generated from the contents of this repository, so the two should always agree; if they ever drift, the homepage wins. This README covers what lives here and how to build it.

## Repository layout

| Path | Contents |
| --- | --- |
| [`index.md`](index.md) | Site landing page. |
| [`policies.md`](policies.md) | Code of Conduct and information-sharing policies. |
| [`dns-abuse-examples.md`](dns-abuse-examples.md) | Real-world examples of DNS abuse techniques. |
| [`stakeholder-advice/`](stakeholder-advice/) | Per-technique advice documents (one Markdown file per abuse technique) plus an index. |
| [`matrix/`](matrix/) | Source of truth and build pipeline for the DNS Abuse Techniques Matrix PDF. See [`matrix/README.md`](matrix/README.md). |
| [`DNS-Abuse-Techniques-Matrix_v1.3.pdf`](DNS-Abuse-Techniques-Matrix_v1.3.pdf) | Current published Matrix. |
| [`DNS-Abuse-Techniques-Matrix_v1.1.pdf`](DNS-Abuse-Techniques-Matrix_v1.1.pdf) / [`*-ja.pdf`](DNS-Abuse-Techniques-Matrix_v1.1-ja.pdf) | Prior version and Japanese translation of v1.1. |
| [`Makefile`](Makefile) | Build targets for the Matrix pipeline. |
| [`AGENTS.md`](AGENTS.md) | Instructions for automated/agentic edits to the Matrix pipeline. |
| [`CHANGES.md`](CHANGES.md) | Reverse-chronological log of notable SIG and document changes. |
| [`CONTRIBUTORS.md`](CONTRIBUTORS.md) | List of contributors. |

The Markdown pages carry an HTML-comment front matter block (`title:`) and are rendered into the FIRST-hosted site.

## The DNS Abuse Techniques Matrix

The Matrix maps, for each DNS abuse technique, whether a given stakeholder is positioned to **detect**, **mitigate**, or **prevent** it. It is intended for incident responders: during an incident, look up the relevant technique under the appropriate action to see which stakeholders you might contact for help.

It is also used beyond this repo. An HTML version is maintained by JPCERT/CC at <https://firstdotorg.github.io/dns-abuse-sig/>, and it has been incorporated into the [MISP Galaxy](https://github.com/MISP/misp-galaxy#first-dns-abuse-techniques-matrix) and the [OASIS STIX Event Type Vocabulary](https://github.com/oasis-open/cti-stix-common-objects/blob/main/extension-definition-specifications/incident-ef7/Incident%20Extension%20Suite.adoc#44-event-type-vocabulary).

### Data model

The Matrix is stored as normalized CSV under `matrix/data/`, so diffs surface individual capability changes rather than reflowed wide tables:

- `actions.csv` - the three incident-response actions (detect, mitigate, prevent).
- `techniques.csv` - abuse techniques and their descriptions.
- `stakeholders.csv` - stakeholder columns and definitions.
- `matrix.csv` - one row per (action, technique, stakeholder) with a `yes`/`no` `capability` and an optional `note`.
- `version_history.csv` - publication history.

Rows are validated against `matrix/schemas/matrix.schema.json`.

## Building the Matrix PDF

Prerequisites for a full build are `python3`, `pandoc`, and `typst`.

Python dependencies for the DOCX import step include `python-docx`.

Targets (run from the repo root):

```bash
make import DOCX_SOURCE="../DNS Abuse Techniques Matrix.docx"  # re-extract Markdown/CSV/assets from the Google Docs DOCX export
make validate                                                  # check the normalized data model
make tables                                                    # regenerate Markdown tables from the CSVs (implies validate)
make assemble                                                  # write build/matrix/report.md (implies tables)
make pdf                                                       # render build/matrix/DNS-Abuse-Techniques-Matrix.pdf (needs pandoc + typst)
make clean                                                     # remove build/ and matrix/generated/
```

### Editing rules

- Treat `matrix/content/`, `matrix/data/`, `matrix/assets/`, and `matrix/templates/` as the source of truth.
- Do **not** hand-edit files under `matrix/generated/` or `build/`, or the generated PDFs - change the source and rebuild.
- Keep `matrix/data/matrix.csv` normalized (one row per action/technique/stakeholder) with capability values restricted to `yes` or `no`.
- Run `make validate` after changing Matrix data, and `make assemble` after changing prose, data, assets, or templates.
- Prefer small, reviewable edits to source files over regenerated wholesale output.

See `AGENTS.md` and `matrix/README.md` for the full pipeline details.

## Contributing

Contributions are welcome. Record notable changes in `CHANGES.md` (reverse chronological) and add contributors to `CONTRIBUTORS.md`. For how to join the SIG and the applicable policies, see the homepage above and `policies.md`.
