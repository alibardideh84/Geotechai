# Geo — product specification (living document)

Canonical, version-able spec for **Geo**: an agentic-AI assistant for geotechnical engineers. This folder is the single source of truth for product vision, architecture, workflow, roadmap, and decisions made to date.

## How to use this folder
- **Humans:** read `01` → `06` in order for the full picture. Edit freely; keep the decision log (`06`) updated when a decision changes.
- **AI assistants (future sessions):** read every file in this folder before advising on any stage of development. Treat `06-decision-log.md` as authoritative for what is settled vs. still open. The Eurocode reference texts are in `../Sources/` (EN 1997-1:2004 and EN 1997-2:2007).

## Index
| File | Contents |
|---|---|
| `01-vision-and-positioning.md` | What Geo is, who it's for, what it is *not*, market |
| `02-architecture-and-principles.md` | AI-orchestrates-engines, engine boundary, the Method Library, method selection, accountability, data ingestion |
| `03-workflow-and-human-ai-model.md` | The human/AI loop, the MVP workflow spine, flexible entry & input contracts, consistency engine, judgment gates |
| `04-roadmap-priorities.md` | Full module list with P1/P2/P3 priorities; method-set phasing |
| `05-mvp-scope.md` | MVP definition, v1 method set, open questions, next step |
| `06-decision-log.md` | Chronological log of decisions and their status |
| `method-library.md` | The Method Registry (core IP): all methods/correlations/ranges per step, codes + non-code refs, tagged by source + reliability. Seeded for interpretation 2.0–2.7 from Look 2007. |
| `stages/` | Detailed per-stage specs: `stages/02-interpretation.md` (what/reference map) + `stages/02-interpretation-procedures.md` (per-task In/Does/Out/Gates). |

## Status legend
- **Accepted** — agreed by the founder.
- **Proposed** — recommended by assistant, awaiting confirmation.
- **Open** — not yet decided.

_Last updated: 2026-06-15._
