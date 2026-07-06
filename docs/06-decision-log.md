# 06 — Decision log

Chronological record of decisions. Status: **Accepted** (agreed) · **Proposed** (assistant recommendation, awaiting confirmation) · **Open**.

## 2026-06-15 — initial design session

| # | Decision | Status |
|---|---|---|
| D1 | Product = agentic-AI assistant for the **individual** geotechnical engineer (not firm/enterprise software); like a calculator that accelerates design. | Accepted |
| D2 | **Not** an advanced numerical analyzer (no PLAXIS/FEM). It automates what an engineer does by hand / with standard methods. | Accepted |
| D3 | Engineer is the **expert of record, accountable and responsible**; AI does ~95%; every step reviewed, overridable, and signed off. | Accepted |
| D4 | **AI orchestrates deterministic, validated calculation engines** (clause-traceable); LLM never does the arithmetic. | Accepted |
| D5 | **Engine boundary = closed-form / hand methods**, not FEM/FDM. | Accepted |
| D6 | **Code-agnostic**: engineer selects standard/code first. Eurocode 7 (Parts 1 & 2, in `../Sources/`) is the reference, not the limit. | Accepted |
| D7 | **Method Library** is core IP: all valid approaches per analysis from codes + peer-reviewed research, stored as a metadata-rich Method Registry. | Accepted |
| D8 | **Method-selection model**: default to code-mandated method; engineer can override to any applicable method; can run several and compare. | Accepted |
| D9 | **Method breadth is phased**: ship a curated v1 method set per analysis, expand over time. | Accepted |
| D10 | Priorities: no marker = P1, `*` = P2, `**` = P3 (full list in `04`). | Accepted |
| D11 | **MVP = interpretation + two design modules** (shallow foundation bearing capacity + axial pile capacity). | Accepted |
| D12 | v1 method set for the two MVP modules defined (see `05`). | Accepted |
| D13 | **Data ingestion**: any format; system suggests needed data, extracts, validates, and asks/calculates/suggests on gaps. | Accepted |
| D14 | Market: **Europe and Australia** first. | Accepted |
| D15 | **Flexible entry**: engineer can start at any stage; stages are modular with **input contracts**; skipped upstream deliverables supplied as direct uploads. | Accepted |
| D16 | **Constitutive soil/rock models** beyond Mohr-Coulomb (Hardening Soil, Cam-Clay, Soft Soil, …) are part of the comprehensive method library; characterization is model-agnostic. Capturing advanced models ≠ running FEM. | Accepted |
| D17 | **Consistency engine**: compare new data vs prior/nearby/regional/geology; AI flags conflicts and recommends a reconciled dataset/approach; engineer decides. | Accepted |
| D18 | **Interpretation outputs preliminary design recommendations** (f_s–depth, q_b–depth, typical bearing capacity) bridging interpretation → design. | Accepted |
| D19 | Advanced constitutive-model **derivations** in MVP: make data model agnostic now, defer HS/Cam-Clay derivations to the settlement/deformation modules. | Accepted |
| D20 | Interpretation procedure designed as a referenced step-by-step workflow (2.1→2.7), grounded in EN 1997-2/EN 1997-1 and AS 1726 / AS 2159 / AS 5100.3, written to `stages/02-interpretation.md` with **both** Eurocode and Australian pack columns. ◇ items flagged for confirmation. | Accepted (draft) |

| D21 | **Code-agnostic core + swappable Code Pack** architecture (see `02` P7). Code-specifics (classification, value basis, safety format, method set, checks, report) live in a per-project Code Pack; the workflow skeleton, data model, Method Library, and gates are universal. Method Library entries tagged by sanctioning code. Eurocode 7 = first pack. | Accepted |
| D22 | Build **two Code Packs early** given launch markets: **Eurocode 7** and **Australian** (AS 1726 / AS 2159 / AS 5100.3). The differing AS safety format (risk-based φg piling) de-risks the abstraction by forcing it early. Texts now in `Sources/` (AS2159.txt, AS5100.3.txt converted; AS1726 converting). | Accepted |
| D23 | **Interpretation is multi-location.** Steps 2.1–2.3 run per sounding (each BH/CPT/test); 2.4–2.7 are site-wide/spatial — cross-location consistency & outlier detection, zoning into geotechnical units + cross-sections, characteristic value per unit with local-governing-vs-volume-averaging (EC7 §2.4.5.2). | Accepted |
| D24 | **Method Library is scenario-indexed** by { parameter × soil type × drainage × data available }. AI filters to applicable methods for the actual inputs at each depth/location; includes **no-lab fallback chains** (e.g. clay sᵤ from SPT via Stroud/Hara) with explicit reliability flags, and encodes when a method is NOT recommended (e.g. clay φ′ from SPT). | Accepted |
| D25 | **Two-tier gate model.** Step gates (scheduled, end of each step) PLUS **exception gates** that fire mid-task on any trigger — error, missing/insufficient data, ≥2 routes/methods, methods disagree, low confidence/accuracy, inconsistency/conflict, out-of-applicability, required assumption, design-critical threshold. Engineer decides; logged. Severity blocking vs advisory; thresholds configurable. See `03-workflow-and-human-ai-model.md`. | Accepted |
| D26 | **Out-of-code method trigger.** If the selected Code Pack has no recommended/discussed method for a case, the system notifies the engineer, proposes an applicable method from the wider Method Library (other code / peer-reviewed research) flagged *outside selected standard (from [reference])*, and requests approval. | Accepted |
| D27 | **Notices register.** All gate events/decisions (assumptions, low-confidence items, method choices, conflicts, missing-data estimates, out-of-applicability overrides, out-of-code methods, design-critical items, recommendations) are compiled into a cautions/assumptions/notices register at the **end of the report**. | Accepted |
| D28 | **Task-level procedures for interpretation (2.0–2.8) drafted** in `stages/02-interpretation-procedures.md` — every task as **In · Does · Out · Gates/triggers** (with ←CP markers and B/D/A exception severities); 2.4 consistency and 2.5/2.6 ground-model/value get the deeper treatment (agnostic-first → pack specifics). The 2.4 reconciliation methodology is given a concrete default (classify-before-reconcile + reconciliation ladder + robust median/MAD), **Proposed** pending references (open item (c)). | Accepted (draft) |
| D29 | **Method Library file created** (`method-library.md`) and **seeded for interpretation 2.0–2.7 from Look 2007** (*Handbook of Geotechnical Investigation & Design Tables*, non-code reference), organized by procedure step → parameter, each entry tagged source + reliability (High/Med/Low/NR). **Selection rule (extends D8/D26):** when code AND library both have a method, **offer both, compare, engineer selects**; non-code methods are comparison alternatives, not just fallbacks. Procedure steps link to it via per-step pointers. Garbled PDF tables rendered to PNGs in `Sources/Handbooks/figures/`. Resolves ◇ items: **Bjerrum μ** (Tbl 4.14) and **Hazen** (Tbl 8.3). | Accepted |

## 2026-06-29 — handbook integration & methodology confirmation

| # | Decision | Status |
|---|---|---|
| D30 | **Open item (c) closed.** Founder **confirmed** the 2.4/2.6 statistical methodology — classify-before-reconcile + reconciliation ladder + robust median/MAD + **lognormal** distribution + the **COV backbone** — and its references (Kulhawy 1992; Poon & Kulhawy 1999; Look & Griffiths 2004; Duncan & Wright 2005) as the **accepted default** (was Proposed under D28/D29). | Accepted |

## Open / next
- Remaining ◇ items in `stages/02-interpretation.md`: (a) AS test-method standards for 2.1, (b) AS 2159/AS 5100.3 as the AU derivation→design anchor, and Schneider 1997 / Kulhawy & Mayne 1990 (confirm or swap). ~~(c) reconciliation references~~ — **closed (D30).**
- ~~Detail the procedure for the interpretation breakdown (2.0–2.8)~~ — **done (D28).** ~~Seed interpretation Method Library from Look 2007~~ — **done (D29).**
- **NEXT (in progress):** apply the same referenced, In/Does/Out/Gates treatment to a **design module**, seeding its library from Look's design chapters (Ch 19–23). *(Skipping the AS-pack/second-source seeding for now per founder.)*
