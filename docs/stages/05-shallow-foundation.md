# Stage 5 — Shallow foundation design (bearing + settlement)

First design-module spec, following the interpretation template (`02-interpretation.md`): a
universal step skeleton with every code-specific reference held in the Code Pack (`../02-architecture-and-principles.md`
P7, D21). Consumes the **ground model** (Stage 3) and the **design basis & actions** (Stage 4) —
per the spine in `../03-workflow-and-human-ai-model.md`.

**Reference legend:** ✓ = verified in the source text in `../../Sources/` · ◇ = proposed /
standard practice, **to confirm**.

## Backbone (per pack)
- **Eurocode 7:** EN 1997-1 **§6 Spread foundations** — limit states (§6.2 ✓), ULS design
  (§6.5: overall stability §6.5.1, bearing resistance §6.5.2 — analytical/Annex D, semi-empirical/Annex E,
  prescriptive/Annex G ✓; sliding §6.5.3 ✓), SLS/settlement (§6.6, Annex F ✓), foundations on rock
  (§6.7, Annex G ✓). Safety format = **Design Approaches DA1/DA2/DA3** (§2.4.7.3.4 ✓, partial-factor
  Tables A.3–A.5 ✓). **Companion:** the JRC/CEN worked-examples report (JRC 2013 EUR 26227, Ch 3 +
  Annex A.3 ✓) transcribes Annex D/F cleanly and works the DA1/2/3 calibration — seeded into the
  Method Library (see the §5.x pointer below).
- **Australian:** AS 5100.3 **§10 Shallow footings** — design requirements (§10.3.1 ✓), footing
  depth/size (§10.3.2 ✓), design for geotechnical strength (§10.3.3: overall stability §10.3.3.2,
  ultimate bearing failure §10.3.3.3, sliding §10.3.3.4 ✓), SLS (§10.3.5 ✓). Safety format =
  **risk-based geotechnical strength reduction factor φg** (§7.3.5 general principle ✓; §10.3.3
  values, Tables 10.3.3(A)/(B) ✓).
- **Universal (core, every pack):** the 5.0→5.7 step skeleton, the per-footing loop, the
  bearing/sliding/settlement check set, and the rule that the **engineer owns geometry sign-off**
  and any safety-format override. Only the references/factors below re-point.

## Division of labour (Stage 2/4 vs Stage 5)
Stage 5 does **not** re-derive ground parameters or re-litigate actions — it consumes them and
runs the geometry-specific check:
- **From Stage 2.7** (interpretation): geometry-independent, *indicative* bearing/settlement
  profiles per unit (already flagged preliminary — see `02-interpretation-procedures.md` §2.7).
- **From Stage 3**: the approved ground model — units, groundwater, characteristic values.
- **From Stage 4**: trial geometry options, assembled actions/load combinations, and — **for the
  EC pack only** — the selected Design Approach (DA1/2/3) and its partial-factor sets (this is a
  **project-wide, upstream** choice, `../03` → "Three must-stay-human judgment gates" #1).
- **Stage 5 itself** runs the check with the actual footing geometry and — **for the AU pack** —
  selects φg **locally, per check**, because φg depends on *how* the ultimate strength was
  assessed (test method, investigation quality) for *this* footing, not on a single project-wide
  factor set. This asymmetry is exactly the divergence flagged below.

## Steps and references (side-by-side)

| Step | Eurocode 7 pack | Australian pack |
|---|---|---|
| **5.1 Trial geometry & representative parameters** | unit parameters per §2.4.5.2 characteristic value (from Stage 3); geometry per §6.4(4)P practical considerations ✓ | unit parameters per Stage 3; geometry per AS 5100.3 §10.3.2 footing depth/size ✓ |
| **5.2 ULS — bearing resistance** | **§6.5.2** analytical method (Annex D ✓), semi-empirical/pressuremeter (Annex E ✓), prescriptive/presumed on rock (Annex G ✓); safety format **DA1/DA2/DA3** (§2.4.7.3.4, Tables A.3–A.5 ✓) | **§10.3.3.3** ultimate bearing failure: S* ≤ φg·Rug ✓; φg from **Tables 10.3.3(A)/(B)** by test method + investigation quality ✓ |
| **5.3 ULS — sliding & overall stability** | §6.5.1 overall stability ✓; §6.5.3 sliding, Rd via §2.4 + Table A.5 (γR;h) ✓ | §10.3.3.2 overall stability ✓; §10.3.3.4 sliding, Hug·φg + Epr·φg ≥ S* ✓ |
| **5.4 Foundations on rock** | §6.7 + Annex G presumed bearing (rock groups, Table G.1) ✓ | same §10.3.3.3 framework (Rug from rock data) + Look Ch 22 rock-bearing/RQD tables ◇ (non-code) |
| **5.5 SLS — settlement** | §6.6.2, methods per **Annex F** (stress-strain F.1, adjusted-elasticity F.2, undrained F.3, consolidation F.4, time-rate F.5) ✓ — "commonly recognized methods", no formula mandated | §10.3.5 general performance requirement (displacement + differential) ✓ — method **not prescribed**, engineer-selected (same as EC) |
| **5.6 Geometry iteration** | agnostic — re-run 5.2–5.5 until all checks pass | agnostic — re-run 5.2–5.5 until all checks pass |
| **5.7 Package / hand-off** | audit trail + DA/partial-factor basis | audit trail + φg basis + rationale |

## v1 method set (drill-down)

Per `../05-mvp-scope.md`: **Terzaghi, Meyerhof, Hansen, Vesić + EN 1997-1 Annex D** (shape/depth/
inclination/groundwater corrections, drained vs undrained), **plus a basic SLS settlement check**.
Both packs share the same underlying mechanics — **Non-code (Look 2007 Ch 21/23)** entries fill
the two gaps neither code closes: an **N-based route when only SPT is available**, and a **named
settlement method set** (since both §6.6.2 and §10.3.5 mandate the *requirement*, not a *method*).

### Bearing capacity

| Data available | v1 method | Reference |
|---|---|---|
| c′, φ′ or c_u (lab/derived) | **Terzaghi/Hansen/Vesić** bearing equation, N_c/N_q/N_γ factors, shape/depth/inclination/eccentricity/groundwater corrections | EN 1997-1 Annex D (undrained D.3, drained D.4) ✓; Look 2007 Tbl 21.4/21.5 (Vesic 1973/Hansen 1970) ◇ (non-code, same factors, convenient cross-check table) |
| pressuremeter (P_LM) | k·P*_LE semi-empirical | EN 1997-1 Annex E ✓ |
| **SPT N only (no lab)** | Meyerhof 1956 allowable-bearing-from-N chart (halve if water within B) | Look 2007 Tbl 21.7 ◇ (non-code fallback — **neither code tabulates an SPT-N bearing chart**, same gap pattern as interpretation's SPT-φ′ fallbacks) |
| rock (RQD) | presumed bearing on rock (weak/broken rock groups) | EN 1997-1 Annex G ✓ (EC); Look 2007 Tbl 22.1 (Peck/Hansen/Thorburn 1974) ◇ (non-code, AU pack has no rock-specific bearing annex) |

### Settlement (SLS)

| Data available | v1 method | Reference |
|---|---|---|
| stiffness E/M (lab or CPT/DMT-derived) | stress-strain / adjusted-elasticity method | EN 1997-1 Annex F.1/F.2 ✓ (mandated by name only, not formula) |
| **SPT N only (granular)** | Meyerhof 1965 ρ = f(q, N, B) | Look 2007 Tbl 21.8 ◇ (non-code — this is the *de facto* v1 default for both packs when only SPT exists, since neither code names a formula) |
| consolidating clay | oedometer consolidation settlement + Skempton-Bjerrum μ correction | EN 1997-1 Annex F.4 ✓ (EC); Look 2007 Tbl 21.6/23.x (Tomlinson 1995) ◇ (non-code, both packs) |
| — | tolerable-movement limits to compare against (already in the Method Library from interpretation) | Look 2007 Tbl 23.6–23.8 (Skempton & Macdonald 1955; Wahls 1981; Boscardin & Cording 1989) — see `../method-library.md#27--preliminary-design-recommendations` |

## Per-pack safety format — the deep treatment

This is the deepest Code-Pack split for design (mirrors divergence #2 in interpretation, §2.6
value basis), and it stress-tests the abstraction two ways: **different factors**, and **different
place in the workflow**.

| | Eurocode 7 (DA1/DA2/DA3) | Australian (risk-based φg) |
|---|---|---|
| **What is factored** | actions (γ_F) **and/or** ground strength parameters (γ_M) **and/or** resistance (γ_R) — three prescribed *combinations* of factor sets | a single **ultimate geotechnical strength** Rug (or Hug/Epr for sliding), reduced by one factor φg |
| **Where the choice is made** | **upstream, project-wide** (Stage 0/4): the engineer picks DA1, DA2 or DA3 once per project/National Annex; every check downstream applies that DA's fixed factor sets (Tables A.3–A.5 ✓) | **downstream, per-check** (Stage 5 itself): φg is read off Table 10.3.3(A) by **which method assessed Rug** (advanced in-situ 0.50–0.65, advanced lab 0.45–0.60, CPT 0.40–0.50, SPT 0.35–0.40 ✓), then adjusted within that range by Table 10.3.3(B) factors — investigation quality, calculation sophistication, construction control, consequence of failure, load type, structure permanence, correlation provenance (published vs site-specific) ✓ |
| **What varies the number** | which DA/National Annex is selected (fixed, known before geometry exists) | **how good the input data and process were**, decided case-by-case as each bearing check runs |
| **Governing inequality** | V_d ≤ R_d (§6.5.2.1 ✓), with R_d computed from already-factored design values of actions/parameters/resistance | S* ≤ φg·Rug (§10.3.3.3 ✓), with Rug computed from **characteristic (unfactored)** strength and φg applied once, explicitly, at the end |
| **Consequence for the AI** | Stage 5 **inherits** a fixed factor set — no judgment call inside the bearing/settlement tasks themselves | Stage 5 must **propose φg with a rationale** (test method + investigation quality + consequence class) at 5.2/5.3/5.4, and route it to a gate — a genuinely different kind of mid-task decision than anything DA1/2/3 requires |

**Why this validates the split (extends the interpretation finding):** it is not only that the
*numbers* differ (γ vs φg) — it is that **the decision point moves**. EC7's safety judgment is
front-loaded and invariant per project; AS's is back-loaded and re-made at every bearing/sliding
check, driven by the quality of the specific evidence behind that check. A Code-Pack architecture
that only swapped factor tables would miss this — it has to also let the **AU pack inject a new
gated decision inside Stage 5** that the **EC pack does not have**.

**Sub-divergence *inside* the EC pack — DA2 vs DA2\* (from JRC 2013 §3.3):** even within one Design
Approach the *place* the factor enters can change the geometry, not just the margin. **DA2** applies
partial factors to the **actions at source** → a *design* eccentricity `e_d` → smaller effective
`B′`; **DA2\*** runs the whole calc on characteristic values and factors **only the effect at the
end** → a *characteristic* eccentricity `e_k` → larger `B′`. Since `e_d ≫ e_k`, DA2\* is the more
economic route for eccentric loads. Consequence for the Code Pack: the EC pack's safety format is
not a single scalar swap either — it needs a **factor-application-point** attribute (at-source vs
at-effect), because that toggles how eccentricity feeds `A′ = B′·L′`. Detail + worked numbers in the
Method Library §5.x.

## Task breakdown (operational steps)
🔒 = **step gate**. **Exception gates** also fire mid-task on any trigger (see
`../03-workflow-and-human-ai-model.md` → Gate model). **←CP** = supplied by the selected Code Pack.

**5.0 — Stage entry & input contract** *(once)*
- 5.0.1 Detect entry point (fresh from Stage 3/4 hand-off vs jump-in — input contract: characteristic
  parameters + footing geometry + actions, per `../03` → Flexible entry table).
- 5.0.2 Resolve the active Code Pack; for EC, confirm the selected **Design Approach** (from Stage 0/4);
  for AU, note that φg is *not yet* fixed — it resolves per-check in 5.2–5.4.
- 5.0.3 Confirm the geotechnical category / consequence class (drives φg range in AU; drives which
  DA combination governs in EC).
- 🔒 Gate 5.0 — confirm inputs, Code Pack, and (EC) Design Approach.

### Per-footing loop (every proposed pad / strip / raft)
**5.1 — Trial geometry & representative parameters**
- 5.1.1 Identify the governing geotechnical unit(s) beneath the footprint from the ground model (Stage 3).
- 5.1.2 Pull the characteristic parameters + the Stage 2.7 preliminary bearing/settlement profile for
  those units.
- 5.1.3 Propose trial geometry (B, L, embedment D) from the preliminary profile + structural/practical
  constraints (§6.4(4)P / AS §10.3.2). ←CP
- 🔒 Gate 5.1.

**5.2 — ULS bearing resistance** ←CP method + safety format
- 5.2.1 Filter the Method Library to applicable bearing methods for the data available (lab c′/φ′/c_u,
  CPT/DMT, or SPT-N-only fallback); set the Code-Pack default. ←CP
- 5.2.2 Compute the ultimate bearing resistance (drained/undrained as relevant) with shape, depth,
  inclination, eccentricity, and groundwater corrections.
- 5.2.3 Apply the pack's safety format: **EC** — the design values already carry the selected DA's
  factors (from Stage 4), compute R_d; **AU** — propose φg from Tables 10.3.3(A)/(B) with rationale
  (method, investigation quality, consequence), for engineer approval. ←CP
- 5.2.4 Verify the governing inequality (V_d ≤ R_d, or S* ≤ φg·Rug); report utilization and governing
  case (drained vs undrained, which load combination).
- 🔒 Gate 5.2.

**5.3 — ULS sliding & overall stability**
- 5.3.1 Sliding check — base friction ± passive resistance, safety format ←CP as in 5.2.3.
- 5.3.2 Overall-stability screen (slope, nearby excavation/retaining structure, waterway, mine workings —
  §6.5.1 / §10.3.3.2 trigger list); escalate to Section 11 (EC) style global stability check if triggered.
- 🔒 Gate 5.3.

**5.4 — Foundations on rock** *(conditional — only when founding stratum is rock)*
- 5.4.1 Presumed bearing resistance (EC Annex G rock groups) or RQD-based bearing (AU/Look Tbl 22.1),
  cross-checked against the analytical method where lab/in-situ rock data exist. ←CP/library
- 🔒 Gate 5.4.

**5.5 — SLS settlement**
- 5.5.1 Filter the Method Library to applicable settlement methods (stress-strain/elasticity from lab
  stiffness, CPT/DMT-derived stiffness, or SPT-N-only Meyerhof 1965 fallback). ←CP/library
- 5.5.2 Compute total and differential settlement at SLS actions/combinations.
- 5.5.3 Compare against tolerable-movement limits (Look Tbl 23.6–23.8, already in the Method Library
  from Stage 2.7(f)) — flag if code-mandated settlement calculation is triggered (EC §6.6.2(3)/(16):
  soft clay, or bearing/loading ratio <3 or <2).
- 🔒 Gate 5.5.

**5.6 — Geometry iteration & optimization**
- 5.6.1 If any check fails, or utilization is markedly low (over-conservative), propose revised
  geometry (B, L, D) and re-run 5.2–5.5.
- 5.6.2 Engineer approves the final geometry and governing case.
- 🔒 Gate 5.6 — engineer owns the final geometry.

### Site-wide
**5.7 — Package / hand-off**
- 5.7.1 Compile the per-footing design summary (geometry, governing check, safety-format basis,
  utilization) and zone across the site where multiple footings share units.
- 5.7.2 Compile design-stage constraints/risks (construction sequencing, dewatering, adjacent-structure
  effects) distinct from the interpretation-stage register.
- 5.7.3 Stage notices-register entries: method choices, φg rationale (AU) or DA combination governing
  (EC), overrides, assumptions (D27).
- 5.7.4 Hand off to structural footing design (out of geotechnical scope — flagged interface only) and
  to the report (Stage 7).

> **Method Library — Stage 5 methods → [`../method-library.md` §5](../method-library.md#stage-5--shallow-foundation-design).**
> First design-library seeding, from **JRC 2013 *(EC7 guidance)*** (the EN 1997-1 §6 direct method,
> transcribed clean because the raw Annex-D `.txt` is garbled): **5.2** bearing equation + full
> factor set (drained/undrained shape/inclination/base) + eccentricity rules (`B′=B−2e`, the
> `e>B/3` §6.5.4 precaution vs the middle-third rule, 0,10 m tolerance); **5.3** sliding (`δ_d`,
> 0,4·V_d cap, passive-neglect caveat); **5.4** Terzaghi–Peck indirect chart (ULS→SLS governing
> shift with `B`); **5.5** Annex-H limiting-deformation table + elasticity settlement (`μ₀·μ₁`
> charts) + oedometric consolidation; **5.x** the DA1/2/3(+DA2\*) sets with a worked size-`B`
> calibration (DA2 → 1,21 m, DA1 → 1,50 m, DA3 → 1,74 m) as a bearing-engine **test case**. Four
> chart figures rendered to `../../Sources/Handbooks/figures/JRC2013_*.png`. *(These are the EC-pack
> **code default**, not non-code alternatives — see the source's selection-rule note.)* The library
> §5 now also carries the **AU pack** (AS 5100.3 §10 — pack-shared mechanics + the forked φg safety
> format, Tables 10.3.3(A)/(B)) and the **Look 2007 Ch 21/23 *(non-code)*** fallbacks (presumed
> bearing Tbl 21.3, Meyerhof N-charts for bearing/settlement, FoS, tolerable-movement limits).

## Open items (◇ — to confirm with founder)
- **(a)** Look 2007 Ch 21/23 bearing/settlement tables are proposed as the **SPT-N-only fallback**
  and the **named settlement method set** for both packs (neither code prescribes a formula) —
  confirm this non-code default, same pattern as interpretation's Bjerrum μ/Hazen resolution.
- **(b)** Confirm whether Stage 4 (design basis & actions) or Stage 5 itself is the right home for
  the EC **Design Approach** confirmation gate — currently modeled as inherited from Stage 0/4
  (5.0.2), not re-decided per footing.
- **(c)** AS 5100.3 rock bearing has no dedicated annex (unlike EC Annex G); confirm using Look
  Tbl 22.1 (RQD-based) as the AU-pack non-code default for 5.4, cross-checked against the general
  §10.3.3.3 Rug/φg framework.

## Provenance
EN 1997-1 references verified in the `.txt` conversion (TOC pp. 61–70; §6.5/§6.6 body pp. 62–67;
Annex A partial-factor tables pp. 129–132; Annexes D/E/F/G pp. 157–162). AS 5100.3 references
verified in the `.txt` conversion (§7.3.5 general φg principle; §10.3.2–10.3.5 body; Tables
10.3.3(A)/(B) — pp. 18–20 printed numbering). JRC 2013 (EUR 26227) content verified in the extracted
`.txt` (Ch 3 pp. 15–32; worked strip foundation Annex A.3 pp. 104–117 printed numbering); four chart
figures rendered from the PDF to `../../Sources/Handbooks/figures/JRC2013_*.png`.

## Next
- ✓ **C (done)** — Method Library **§5** now carries **all three sources**: JRC 2013 *(EC7 guidance,
  code default)* direct method; **AU pack** *(AS 5100.3 §10)* — pack-shared mechanics + forked φg
  safety format (Tables 10.3.3(A)/(B)); **Look 2007 Ch 21/23** *(non-code)* fallbacks (presumed
  bearing, Meyerhof N-charts, FoS, tolerable movements). See
  [`../method-library.md` §5](../method-library.md#stage-5--shallow-foundation-design).
- **B (next)** — task-level procedures for 5.0–5.7 (In/Does/Out/Gates), mirroring
  `02-interpretation-procedures.md`, with 5.2 (bearing + safety format) and 5.5 (settlement) getting
  the deeper treatment.
- Optional follow-up flagged in the library §5.x(AU): author a parallel **AU φg worked calibration**
  as a bearing-engine test case (AS 5100.3 ships none; the JRC DA1/2/3 calibration is the EC one).
- After shallow foundation is complete: apply the same three-part treatment to **axial pile**
  (Stage 6) — EN 1997-1 §7 + AS 2159 + AS 5100.3, where the AU risk-based φg (AS 2159 §1.3.7) will
  be revisited against the pile ξ/model-factor judgment gate already flagged in `../03`. *(JRC 2013
  Ch 8 + Annex A.8 also has worked pile examples — reuse this same source there.)*
