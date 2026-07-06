# 02 — Architecture and principles

## P1 — AI orchestrates deterministic, validated calculation engines
The LLM does **not** do the arithmetic. The actual calculations run in auditable, version-controlled, clause-traceable code ("engines"). The AI's role is to: scope inputs, choose/recommend the method, run the validated engine, interpret results, explain them, draft outputs, and cite the governing code clause.

## P2 — Engine boundary: closed-form, not FEM
Engines implement the **classic closed-form / hand / spreadsheet methods** an engineer would use — e.g. code Annex formulas, Brinch Hansen, Meyerhof, Vesić, Broms, Bishop/Janbu, Boussinesq, α/β pile methods, CPT-based pile methods. They are **not** a numerical FEM/FDM solver. The engineer picks the method; the AI recommends and runs it.

## P3 — The Method Library (core IP / moat)
For each analysis the software aims to include **all valid approaches** — from different codes **and** peer-reviewed research — so the engineer can consider and run any applicable method. The calculation layer is therefore a **Method Registry**, not one formula per analysis.

Each method entry carries metadata so the AI can reason about it, not just execute it:
- **source / citation** (code clause or paper)
- **applicability & conditions** (soil type, drained/undrained, foundation/pile type, data available)
- **required inputs**
- **assumptions & limitations**
- **validation status / version**

**Constitutive soil & rock models are part of this comprehensiveness.** Characterization is model-agnostic: Mohr-Coulomb (c, φ) is only one option, alongside Hardening Soil, Modified Cam-Clay, Soft Soil, etc., each with its parameter set and multiple derivation methods.
> Important: capturing/deriving an advanced constitutive model's parameters is **not** the same as running FEM. It enriches the ground model. Advanced models are fully exploited by the deformation/settlement modules (P3 in the roadmap) and any future numerical export.

## P4 — Method selection model (Accepted)
- **Default** to the code-mandated method for the selected code.
- The engineer can **override** to any applicable method.
- The engineer can **run several methods and compare side-by-side** (especially valuable for piles, where CPT methods scatter).
- Every result carries its method + source + assumptions into the audit trail.

## P5 — Human accountability & audit trail
The engineer signs off at each gate (see `03`). The accumulated sign-offs, with the method, inputs, assumptions, and citations used, form the audit trail that backs the engineer's professional responsibility.

## P6 — Data ingestion
The user uploads data in **any format**. The system suggests which data each step needs, extracts and structures it, validates units/ranges/consistency, and — when something is missing — asks the user, calculates it, or suggests a value based on context.

## P7 — Code-agnostic core + swappable Code Pack
The product is **code-agnostic by construction**, not Eurocode-with-options. Two layers:

- **Code-agnostic core (never changes):** the workflow skeleton (stage steps), the raw data model (tests, units, parameters, soil/rock units), the Method Library, and the human approval gates. The *engineering procedure* and the *physics of deriving parameters from tests* are largely standard-independent.
- **Code Pack (selected at stage 0, fully swappable per project):** everything standard-specific —
  - **classification & terminology** (e.g. USCS/ASTM, EN ISO 14688/14689, AS 1726)
  - **value basis** (EC7 *characteristic* "cautious estimate" vs AASHTO *nominal* vs ASD *representative*)
  - **safety format** (EC7 partial factors DA1/DA2/DA3 vs AASHTO LRFD resistance factors vs AS strength-reduction factors / risk-based φg vs global FoS in ASD/WSD)
  - **method set** (which methods/correlations the code sanctions; the default; the rest stay reachable from the library)
  - **required limit states / load combinations** and **report/deliverable format**

The **Method Library** entries are **tagged by sanctioning code**, so selecting a Code Pack filters and sets defaults, but the engineer can always reach the full library and override. Eurocode 7 is simply the **first Code Pack**; the step skeleton (e.g. interpretation 2.1→2.7) is identical for every standard — only the pack contents re-point.
