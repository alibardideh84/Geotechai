# Stage 2 — Interpretation (→ ground model)

Detailed spec for the interpretation stage. This is the first fully-worked stage and the **template** for the rest: a universal step skeleton, with every code-specific reference held in the Code Pack (see `../02-architecture-and-principles.md` P7, decision D21).

**Reference legend:** ✓ = verified in the source text in `../../Sources/` · ◇ = proposed / standard practice, **to confirm**.

## Backbone (per pack)
- **Eurocode 7:** EN 1997-2 §1.6 + Figure 1.1 — *test results → derived values →* (then via EN 1997-1 §2.4.5.2) *→ characteristic values → design values*; plus §6.3 (evaluation) and §6.4 (establishment of derived values). ✓
- **Australian:** AS 1726-2017 §5 — investigation is an **iterative process** (§5.1.1 ✓), producing a **geotechnical model** (§5.2 ✓) and **reports** (§5.6: factual *geotechnical data report* §5.6.2 ✓; §5.6.2(d) requires regional & local geology ✓).
- **Universal (core, every pack):** the 2.1→2.7 step skeleton, the raw data model, and the rule that the **engineer picks the correlation and owns the design value**. Only the references/systems below re-point.

## Multi-location (per-location vs site-wide)
A real investigation has **many locations** (boreholes, CPTs, test pits, lab samples) at different positions and depths. The stage is split along that axis:
- **Per location (loop):** steps **2.1–2.3** run independently at *every* sounding — correct, classify, derive at each BH/CPT/test.
- **Site-wide (spatial):** steps **2.4–2.7** combine all locations:
  - **2.4 consistency** — explicitly *cross-location*: compare BH vs CPT in the same area, detect outlier/anomalous locations and lateral variability, **and** compare against prior/nearby/regional/geology data.
  - **2.5 ground model** — group soundings into **geotechnical units**, build cross-sections / fence diagrams (2D/3D), establish the groundwater surface across the site.
  - **2.6 characteristic value — per unit, with a spatial subtlety:** EC7 §2.4.5.2's cautious estimate must consider whether the limit state is governed by a **local** value (pile tip, weak lens) or a **spatially-averaged volume** (large raft averaging variability) — which depends on the structure. Local-governing vs volume-averaging change the statistic.
  - **2.7 design recommendations** — produced per unit/zone.

## Steps and references (side-by-side)

| Step | Eurocode 7 pack | Australian pack |
|---|---|---|
| **2.1 Correct measured values** — raw → corrected/normalized per test | EN ISO 22476 methods; SPT energy/overburden Skempton 1986 (Annex F); CPT qₜ Lunne, Robertson & Powell 1997 (Annex D); DMT indices Marchetti 1980 (Annex J) ✓ | AS 1726 §5.5 + App A fieldwork ✓; soil test methods AS 1289 series ◇ |
| **2.2 Classify & behaviour-type** | EN ISO 14688-1/-2 (soil), EN ISO 14689 (rock) ✓; CPT soil-behaviour-type via Lunne et al 1997 | **AS 1726 §6.1 (soil) / §6.2 (rock)** — behavioural, USCS-derived with its own coarse/fine boundary; particle sizes Table 1 ✓ |
| **2.3 Derive parameters** (measured → derived) | sanctioned correlations in **Annexes D–S** — see drill-down below ✓ | AS 1726 §6 = description/classification; derivation correlations **engineer-selected** (same science), design use governed by AS 2159 / AS 5100.3 ◇ |
| **2.4 Consistency & regional reconciliation** | EN 1997-2 §6.3 (evaluation of geotechnical information) ✓ | AS 1726 **§5.1.1 iterative** ✓; **§5.6.2(d) regional & local geology required** ✓ |
| **2.5 Stratigraphy / ground model** | EN 1997-2 §6; EN 1997-1 §3 ✓ | **AS 1726 §5.2 geotechnical model** ✓; factual data report §5.6 ✓ |
| **2.6 Value basis** *(human judgment gate)* | **characteristic value = cautious estimate — EN 1997-1 §2.4.5.2** ✓; Schneider 1997 ◇; national annex | no single clause; cautiously-assessed strength carried to design via **φg** (ultimate → design geotechnical strength): **AS 2159 §1.3.7** risk-based ✓; **AS 5100.3 §7.3.5** ✓ |
| **2.7 Preliminary design recommendations** (fₛ/q_b/bearing profiles) | direct field-test design: EN 1997-2 **§4.3.4 / §4.4.4 / §4.6.4 / §4.10.4 / §4.11** ✓; piles NEN 6743, Fascicule 62; settlement Schmertmann 1970, Burland & Burbidge 1985 | piles fₛ/q_b & φg: **AS 2159** ✓; footing bearing & φg: **AS 5100.3** ✓; investigation → params AS 1726 App A/B ◇ |

## Step 2.3 — parameter derivation: scenario-indexed Method Library

Derivation is **not one formula per parameter** — it is a function of **{ parameter × soil type × drainage × data available }**. At each depth/location the AI reads the actual inputs, **filters the library to the applicable methods**, proposes a default + alternatives (with scatter), and **flags reliability**. Weak scenarios (e.g. *clay, no lab*) still return methods, with a low-confidence flag; the library also encodes when a method is **not recommended** (e.g. clay φ′ from SPT). Australian pack = same science, engineer-selected; design use per AS 2159 / AS 5100.3.

### Eurocode v1 set (drill-down)

| Parameter | From | v1 method | Eurocode reference |
|---|---|---|---|
| Undrained strength sᵤ (clay/silt) | Lab | UU/CU triaxial, UCS, fall-cone | CEN ISO/TS 17892-8/-9 ✓; Annex P ✓ |
| | CPTu | sᵤ = (qₜ − σᵥ₀)/N_kt | Lunne et al 1997 ✓ (Annex D) |
| | Field vane | sᵤ,fv × μ correction | Aas, Lacasse, Lunne & Høeg 1986 ✓ (Annex I); Bjerrum μ ◇ |
| | **SPT, no lab** | sᵤ ≈ f₁·N₆₀ (PI-dep.); sᵤ = 29·N^0.72 | Stroud 1974 ◇; Terzaghi-Peck ◇; Hara et al 1974 ◇ — *low reliability* |
| | any (normalized) | sᵤ/σ′ᵥ = S·OCR^m (SHANSEP) | Ladd & Foott 1974 ◇ |
| Effective φ′, c′ — sand | SPT | N → φ′ | EN 1997-2 Annex F ✓; Peck-Hanson-Thornburn / Hatanaka-Uchida 1996 ◇ |
| | CPT | qc → φ′ | Lunne et al 1997 ✓ (Annex D); Kulhawy & Mayne 1990 ◇ |
| | Lab | CD/CU triaxial, direct shear | CEN ISO/TS 17892-9/-10 ✓; Annex P ✓ |
| Effective φ′, c′ — clay | Lab | CU(+u)/CD triaxial | CEN ISO/TS 17892-9 ✓ |
| | **no lab** | φ′ from plasticity index (φ′↓ with PI) | Mitchell / Mayne ◇ — *low reliability; SPT not a reliable route for clay φ′* |
| Relative density Dᵣ | SPT / CPT | (N₁)₆₀ → Dᵣ; qc → Dᵣ | Skempton 1986 ✓ (Annex F); Lunne et al 1997 ✓ (Annex D) |
| Stiffness E / Eₒₑ𝒹 / M | CPT / DMT / PMT | qc → M; Eᴅ → M; Ménard E_M | Schmertmann 1970 ✓; Marchetti 1980/2001 ✓ (Annex J); Clarke 1995 ✓ (Annex E) |
| | Lab oedometer | Eₒₑ𝒹, mᵥ | CEN ISO/TS 17892-5 ✓; Annex Q ✓ |
| Consolidation Cc, Cr, cᵥ, σ′ₚ, OCR | oedometer | Casagrande / Janbu | CEN ISO/TS 17892-5 ✓; Annex Q ✓; OCR from CPT/DMT ◇ |
| Unit weight γ, index (w, e, PSD, Atterberg) | lab | classification suite | CEN ISO/TS 17892-1/-12 ✓; Annex M ✓ |
| Permeability k | lab / CPTu / grain size | falling-head; dissipation; Hazen | Annex S ✓; CEN ISO/TS 17892-11 ✓; Hazen ◇ |

## Human gates inside interpretation
The engineer (a) picks the correlation per parameter in **2.3**, (b) resolves the conflicts the consistency engine raises in **2.4**, and (c) owns the **value basis** in **2.6**. Everything else the AI proposes.

## Three divergences that validate the Code Pack split
1. **Classification is not shared** — AS 1726 deliberately differs from USCS at the coarse/fine boundary; the classification engine is pack-specific.
2. **Value basis is the deepest split** — EC7 picks a characteristic value then applies partial factors (DA1/2/3); AU assesses strength then applies a risk-based φg (AS 2159). Steps 2.6–2.7 are fully pack-driven.
3. **Prescriptiveness differs** — EN 1997-2 tabulates sanctioned correlations in annexes; AS 1726 leaves derivation to the engineer, so the Australian pack's 2.3 default is lighter and the Method Library does more of the work.

## Open items (◇ — to confirm with founder)
- **(a)** AS test-method standards for 2.1 (AS 1289 series for soil; CPT/field per AS 1726 App A) — confirm exact references.
- **(b)** Confirm AS 2159 / AS 5100.3 as the anchor for Australian derivation → design parameters.
- ~~**(c)** Step 2.4 reconciliation methodology~~ — **CONFIRMED (founder 2026-06-29, D30):** the COV + lognormal/median default from Look 2007 Ch 10 (Kulhawy; Poon & Kulhawy; Look & Griffiths; Duncan & Wright) in [`../method-library.md`](../method-library.md#24--consistency--cross-location-reconciliation) is the accepted methodology for 2.4/2.6.
- Eurocode ◇ items in the drill-down — **Bjerrum μ** and **Hazen** now sourced from Look 2007 (Tbl 4.14, Tbl 8.3) in the Method Library; Schneider 1997 / Kulhawy & Mayne 1990 still to confirm or swap for house standards.

## Provenance
AS references verified by reading the scanned AS 1726-2017 PDF pages directly (contents p3; §6.1 classification p16; §5.2/§5.6 framing p7/p14 — printed page numbers). AS 2159 / AS 5100.3 verified in their `.txt` conversions. Eurocode references verified in `en.1997.2` / `en.1997.1` `.txt`.

## Task breakdown (operational steps)
🔒 = **step gate**. **Exception gates** also fire mid-task on any trigger (see `../03-workflow-and-human-ai-model.md` → Gate model). **←CP** = supplied by the selected Code Pack.

**2.0 — Stage entry & data scoping** *(once)*
- 2.0.1 Detect entry point (fresh data vs jump-in with existing parameters/ground model — input contract).
- 2.0.2 List data needed; check presence; request/ingest missing (any format); extract & map fields.
- 2.0.3 Register the investigation: locations, coordinates, ground levels, test types, dates.
- 🔒 Gate 2.0 — confirm dataset & scope.

### Per-location loop (every borehole / CPT / test)
**2.1 — Correct & normalize**
- 2.1.1 Identify test types & depths.
- 2.1.2 Validate raw data (units, continuity, ranges, gaps, spikes); flag suspect readings. ←CP
- 2.1.3 Apply corrections/normalizations (SPT N₆₀/(N₁)₆₀; CPT qₜ, Fr, normalization; DMT indices; vane peak/remoulded). ←CP/library
- 2.1.4 Reconcile depth datum & water level.
- 2.1.5 Present corrected profiles.
- 🔒 Gate 2.1.

**2.2 — Classify & provisional layers**
- 2.2.1 Classify samples/intervals (type, index properties). ←CP
- 2.2.2 Behaviour-type from in-situ tests to fill gaps.
- 2.2.3 Provisional layer boundaries (single-location).
- 2.2.4 Reconcile log vs test-based classification; flag mismatches.
- 2.2.5 Tag special materials (fill, organic, sensitive, expansive, collapsible, cemented). ←CP
- 🔒 Gate 2.2.

**2.3 — Derive parameters (scenario-indexed)**
- 2.3.1 Determine scenario per parameter × layer (soil, drainage, data available).
- 2.3.2 Filter library to applicable methods. ←CP default. If the Code Pack is silent (no sanctioned method), surface an applicable method from the wider library (other code / research), flagged *outside selected standard*, for approval.
- 2.3.3 Compute default + alternatives; show value, scatter, reliability flag, source.
- 2.3.4 Co-present multi-test estimates of the same parameter.
- 2.3.5 Apply no-data fallbacks with low-confidence flags; surface "not recommended".
- 2.3.6 Assemble per-location parameter profiles.
- 🔒 Gate 2.3.

### Site-wide (across all locations)
**2.4 — Consistency & cross-location reconciliation**
- 2.4.1 Spatially register all locations; build layout.
- 2.4.2 Cross-test consistency at near/co-located soundings.
- 2.4.3 Lateral-variability analysis; detect outlier/anomalous locations.
- 2.4.4 Compare vs prior/nearby/regional/geology. ←CP priors
- 2.4.5 Flag conflicts; propose reconciliation with rationale.
- 2.4.6 Consistency summary.
- 🔒 Gate 2.4.

**2.5 — Ground-model assembly**
- 2.5.1 Correlate layers across locations → geotechnical units.
- 2.5.2 Build sections / fence diagrams / surfaces (2D/3D); interpolate; show uncertainty.
- 2.5.3 Groundwater regime across the site.
- 2.5.4 Assign each unit a representative parameter set.
- 2.5.5 Identify design-critical features (weak lenses, rockhead, voids, variable fill).
- 🔒 Gate 2.5.

**2.6 — Characteristic / design value** *(judgment gate)*
- 2.6.1 Assemble the value population per unit × parameter.
- 2.6.2 Choose the statistic per limit state & footprint (local-governing vs volume-averaged). ←CP value basis
- 2.6.3 Propose a cautious value + rationale, range, sensitivity.
- 2.6.4 Per-unit / per-parameter overrides with justification.
- 🔒 Gate 2.6 — engineer owns the value.

**2.7 — Preliminary design recommendations**
- 2.7.1 Design-ready profiles (fₛ–depth, q_b–depth, bearing for typical footings, subgrade reaction, earth-pressure params). ←CP correlations + safety format
- 2.7.2 Zone across the site with ranges.
- 2.7.3 Constraints, risks, further-investigation notes.
- 2.7.4 Compile interpretive output.
- 🔒 Gate 2.7.

**2.8 — Package / hand-off**
- 2.8.1 Assemble ground model + parameters + recommendations + audit trail → design stages & report.
- 2.8.2 Compile the **Notices register** at the end of the report — cautions, assumptions, low-confidence items, method choices, out-of-code methods, conflicts resolved, and recommendations (see `../03-workflow-and-human-ai-model.md` → Notices register).

## Next
- ✓ **Task-level procedures for 2.0–2.8** are now in [`02-interpretation-procedures.md`](02-interpretation-procedures.md) — every task as In/Does/Out/Gates, with the consistency engine (2.4) and ground-model/value (2.5/2.6) given the deeper treatment.
- Then apply this same per-pack treatment to a **design module** (shallow foundation or axial pile).
