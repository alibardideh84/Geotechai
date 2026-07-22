# Stage 6 — Axial pile design (compressive + tensile capacity)

Second MVP design module, following the shallow-foundation template
([`05-shallow-foundation.md`](05-shallow-foundation.md)): a universal step skeleton with every
code-specific reference held in the Code Pack (`../02-architecture-and-principles.md` P7, D21).
Consumes the **ground model** (Stage 3) and **design basis & actions** (Stage 4); shares plumbing
with interpretation's CPT ingestion (D-rationale in `../05-mvp-scope.md`).

**Reference legend:** ✓ = verified in the source text in `../../Sources/` · ◇ = proposed / standard
practice, **to confirm**.

## Why this module is the hardest test of the Code-Pack abstraction
Flagged from the outset (`../06-decision-log.md`, MVP scope): the axial-pile **safety format is a
two-sided judgment fork** — unlike shallow foundations, where only the AU pack made a gated
in-stage decision, **both packs inject a mid-stage reliability judgment here**, expressed completely
differently:
- **EC7:** characteristic resistance from tests via **correlation factors ξ** (number of test
  profiles, mean-vs-minimum, model factor) — the third must-stay-human judgment gate (`../03` gate #3).
- **AS 2159:** a **quantitative weighted risk assessment** produces the strength-reduction factor
  φg — far richer than AS 5100.3's shallow-footing table lookup — plus a **testing-benefit uplift**.

So piles are where the abstraction must support *"inject a gated reliability judgment on the
resistance,"* not merely *"swap factor tables."* (Detail in the safety-format section below.)

## Backbone (per pack)
- **Eurocode 7:** EN 1997-1 **§7 Pile foundations** — limit states (§7.2 ✓), actions incl.
  **downdrag/heave/transverse** (§7.3.2 ✓), design methods (§7.4 ✓), pile load tests (§7.5 ✓),
  **axially loaded piles §7.6** — compressive resistance from static load tests (§7.6.2.2, ξ₁/ξ₂ ✓),
  **from ground test results (§7.6.2.3, ξ₃/ξ₄ + model factor ✓)**, from dynamic (§7.6.2.4, ξ₅/ξ₆ ✓),
  tensile (§7.6.3 ✓). Safety format = **DA1/2/3 with pile-specific factor sets** (DA1 Comb2 uses
  **R4**, not R1 — §2.4.7.3.4.2(2)P ✓) + partial resistance factors Tables A.6–A.8 (driven/bored/CFA ✓)
  + **correlation factors ξ Tables A.9–A.11 ✓**.
- **Australian:** **AS 2159-2009 is primary** — AS 5100.3 **§11 delegates piles to AS 2159** in full
  ("loads/combinations, design procedures, φg, testing… in accordance with AS 2159" ✓). *(Note the
  inversion vs Stage 5, where AS 5100.3 §10 was primary.)* Core: `R_d,g = φg·R_d,ug` (§4.3.1 ✓) with
  the **risk-based** `φg = φgb + (φtf − φgb)·K ≥ φgb` (§4.3.1/4.3.2 ✓); `R_d,ug` from site
  investigation / dynamic / load test (§4.3.3 ✓); shaft+base capacity §4.4 ✓.
- **Universal (core, every pack):** the 6.0→6.8 skeleton, the per-pile(-group) loop, the
  shaft+base capacity model, the two-mode actions input (D38) extended with **geotech-computed
  downdrag/heave**, and the rule that the **engineer owns the resistance-reliability judgment**
  (ξ choice / risk assessment) and the final geometry.

## Division of labour (Stage 2/3/4 vs Stage 6)
- **From Stage 2.7** (interpretation): the *indicative* fₛ–depth and q_b–depth profiles per unit
  (Look Ch 21 methods, already in `../method-library.md` §2.7c) — the preliminary bridge to design.
- **From Stage 3**: the approved ground model — units, groundwater, characteristic parameters.
- **From Stage 4 / direct entry**: pile type + trial geometry, and the **design actions** (two-mode
  model, D38 — see below).
- **Stage 6 itself** computes shaft+base resistance with the actual pile geometry, forms the
  **characteristic/ultimate resistance** with the pack's reliability judgment (EC ξ / AU risk φg),
  and verifies resistance ≥ action.

## Actions & load combinations (input side)
Reuses the **two-mode model** from Stage 5 (D38): **Mode A (default)** — the structural engineer's
already-combined **design actions** (ULS + SLS axial, plus any tension/lateral) entered directly,
consumed as-is (handles any loading code / special structure); **Mode B (fallback)** — characteristic
actions the tool combines (EC DA1 needs A1 & A2 sets). **Pile-specific extension (flagged in D37):**

- **Downdrag / negative skin friction (§7.3.2.2)** and **heave (§7.3.2.3)** are **ground-driven
  actions the structural engineer cannot supply** — they depend on soil settlement/swelling relative
  to the pile. The **geotech tool computes them** from the ground model and adds them as actions
  (EC7 factors negative skin friction with an **M-set** while the resistance uses a different set —
  a genuine within-pack action/resistance factor split). Task 6.4 handles these.
- **Transverse/lateral actions** arrive as design actions (Mode A); the lateral *capacity* check
  (§7.7) is flagged as an adjacent analysis (see task 6.5 scope note).

## Steps and references (side-by-side)

| Step | Eurocode 7 pack | Australian pack (AS 2159, primary) |
|---|---|---|
| **6.1 Trial pile & parameters** | pile type/geometry; characteristic ground params §2.4.5.2 (Stage 3) | pile type/geometry; params per Stage 3; risk-factor inputs gathered for §4.3.2 |
| **6.2 ULS axial compressive** (shaft+base) | **§7.6** — R_c;k from ground tests (§7.6.2.3, **ξ₃/ξ₄**), static (§7.6.2.2, ξ₁/ξ₂) or dynamic (§7.6.2.4); R_c;d = R_b;k/γ_b + R_s;k/γ_s (Tables A.6–A.8); DA1 Comb1 R1 / Comb2 **R4** ✓ | **§4.3–4.4** — R_d,g = φg·R_d,ug; **φg risk-based** (§4.3.2, Tables 4.3.2(A)–(C)); testing-benefit φtf·K (§4.3.1) ✓ |
| **6.3 ULS tensile / uplift** | §7.6.3 (ξ on R_s;k; γ_s;t tension factors Tables A.6–A.8) ✓ | AS 2159 §4.4 tension (φg applies; reduced for uplift) ◇ |
| **6.4 Downdrag / heave** (ground-driven actions) | §7.3.2.2 negative skin friction (M-set), §7.3.2.3 heave ✓ | AS 2159 §4.4 / §5 negative friction & ground movement ◇ |
| **6.5 SLS settlement** (single pile) | §7.6.4.1 (settlement of single pile), load-settlement from tests / t-z | AS 2159 §4.5 serviceability; AS 5100.3 §7.5 permissible displacements ✓ |
| **6.6 Pile group / block** | §7.6.2.1(3)P block failure; §7.6.4.2 group settlement ✓ | AS 2159 §4.4 group effects; efficiency ◇ |
| **6.7 Geometry iteration** | agnostic — re-run 6.2–6.6 until checks pass | agnostic |
| **6.8 Package / hand-off** | audit trail + ξ/DA basis | audit trail + φg risk-assessment record |

## v1 method set (drill-down)
Per `../05-mvp-scope.md`: **α (Tomlinson), β (effective stress) + 2–3 CPT methods + EC7 ξ correlation
& resistance factors.** Both packs share the **capacity mechanics**; the fork is the reliability
judgment (ξ vs risk-φg) applied to it.

### Shaft & base capacity

| Data available | v1 method | Reference |
|---|---|---|
| c_u (clay, undrained) | **α method** `f_s = α·c_u`; base `q_b = N_c·c_u` (N_c=9) | EN 1997-1 §7.6 via ground params ✓; **API RP 2A-WSD §6.4.2 — α = f(ψ=c/p′₀) ✓** (offshore standard, in library §6.2a); Look Tbl 21.14/21.16 (Poulos α; non-code) ◇; Tomlinson α |
| c′/φ′ or σ′_v (drained) | **β method** `f_s = β·σ′_v = K_s·tanδ·σ′_v`; base `q_b = N_q·σ′_v` (cap 10 MPa) | EN 1997-1 §7.6 ✓; **API RP 2A-WSD §6.4.3 — K·p₀·tanδ + N_q, Table 6.4.3-1 ✓** (in library §6.2b); Look Tbl 21.14/21.16 (k_s·tanδ, N_q; non-code) ◇ |
| **CPT (q_c, f_s)** | **Eslami–Fellenius** (CPTu, q_E=q_t−u₂), **Nottingham–Schmertmann** (K_s/α′/C_f), (LCPC, ICP later) | EN 1997-2 §4.3.4 direct CPT design ✓; **FHWA GEC-12 Ch 7 ✓** (in library §6.2e); LCPC/ICP refs ◇ (to add) |
| **SPT N (no CPT/lab)** | Meyerhof N-method — shaft 2N/N/0.67N, base 40N·L/D | Look Tbl 21.17/21.18 (Meyerhof 1976; non-code fallback) ◇ |
| rock socket | shaft `τ = ψ·√(q_u·P_a)`; base bearing-from-RQD | Look Tbl 22.14/22.17 (non-code) ◇ — already in `../method-library.md` §2.7d |
| **installation φ shift** | shaft φ₁ (bored) / ¾φ₁+10 (driven); base φ₁−3 (bored) / (φ₁+40)/2 (driven) | Look Tbl 21.15 (Poulos 1980; non-code) ◇ |

*(JRC 2013 Ch 8 / Annex A.8 supplies EC-worked pile examples — from-soil-parameters and from-CPT —
to seed the library §6 at step C, as the shallow-foundation module used its Ch 3 / Annex A.3.)*

## Per-pack safety format — the deep treatment (two-sided judgment fork)

The defining feature. **Both packs form the *ultimate/characteristic* resistance from the shared
capacity mechanics, then apply a reliability judgment the engineer owns — but the two judgments are
structurally different.**

| | Eurocode 7 (ξ + partial factors) | AS 2159 (risk-based φg) |
|---|---|---|
| **Ultimate resistance** | R_cal = R_b;cal + R_s;cal from the capacity method(s) | R_d,ug from the same capacity methods |
| **The reliability judgment** | **correlation factor ξ** on the *set of calculated resistances*: `R_c;k = min( (R_cal)_mean/ξ₃ , (R_cal)_min/ξ₄ )`, ξ₃/ξ₄ = f(**n test profiles**), Table A.10; optional **model factor**; stiff cap can divide ξ by 1,1 | **weighted risk assessment**: rate ~9 factors (Site / Design / Installation) IRR 1–5 × weight w_i → **ARR = Σ(w_i·IRR_i)/Σw_i** → **φgb** from Table 4.3.2(C) by ARR band × **redundancy** (low/high) |
| **Test-data reward** | more profiles n → lower ξ → higher R_c;k; static-load-test ξ₁/ξ₂ tighter than dynamic ξ₅/ξ₆ | **testing-benefit uplift**: `φg = φgb + (φtf − φgb)·K`, φtf = 0,9 static / 0,8 dynamic-preformed / 0,75 rapid / 0,85 bi-directional; **K = f(% piles tested p)** |
| **Design resistance** | `R_c;d = R_b;k/γ_b + R_s;k/γ_s`, γ by pile type (driven A.6 / bored A.7 / CFA A.8) and DA set; **DA1 Comb2 uses R4** (γ up to 1,3–1,6) — piles are the EC7 exception | `R_d,g = φg·R_d,ug` (one factor, applied once) |
| **φg / factor range** | γ_b/γ_s 1,0–1,6 by pile type × R-set | **φgb 0,40–0,67** (low redundancy) / **0,47–0,76** (high redundancy); φg then uplifted by testing |
| **Governing inequality** | `F_c;d ≤ R_c;d` (§7.6.2.1 ✓) | `E_d ≤ R_d,g` (§4.3.1 ✓) |
| **The engineer's gated judgment** | choose ξ basis (**n**, mean vs min, model factor) — a must-stay-human gate (`../03` #3) | perform the **risk assessment** (rate 9 factors + redundancy + testing strategy) — a must-stay-human gate |

**Why this is the strongest validation (extends Stage 5's finding):** Stage 5 had a *one-sided* fork
(only AU gated φg in-stage; EC inherited fixed DA sets). Piles are **two-sided** — *both* packs
require an in-stage, engineer-owned judgment on **how reliable the computed resistance is**, but EC
expresses it as a **statistical correlation factor on test data** (objective, n-driven) while AS
expresses it as a **qualitative-weighted risk score** (subjective, judgement-driven). A Code-Pack
architecture that modelled the safety format as "a set of numeric factors" cannot represent this —
it must model **"a gated procedure that converts computed resistance + evidence quality into a
design resistance,"** whose *form* is pack-specific (a ξ lookup vs a risk-assessment workflow). This
is the abstraction requirement the pile module exists to surface.

**A third paradigm — API RP 2A-WSD (Working Stress Design).** The offshore/marine pile standard
(added to the library §6) sizes the pile by **allowable capacity = R_ult/FoS ≥ working load**, with a
**global factor of safety** (1,5–2,0 by load condition, §6.3.4) applied to unfactored actions — not
partial factors (EC LRFD) and not φg (AS LSD). This sharpens the abstraction requirement: the safety
format is not even guaranteed to be *factor-on-resistance* — it can be *global-FoS-on-unfactored-
actions*. Crucially, the API **capacity methods** (α, β/N_q) are **pack-agnostic** (they compute
`R_cal` and can feed EC ξ or AS φg), but the API **FoS format** is only used when **API is the
governing standard**; you never mix a global FoS with partial factors. *(This is the design-side of
the special-structures / marine discussion in D38 — API 2A is exactly that offshore case.)* Detail in
Method Library §6.x(API).

> **Method Library — Stage 6 methods → [`../method-library.md` §6](../method-library.md#stage-6--axial-pile-design).**
> First seeding = **API RP 2A-WSD** *(offshore, WSD)*: α method (clay, §6.2a), K·p₀·tanδ + N_q (sand,
> §6.2b + Table 6.4.3-1), rock sockets (§6.2c), pullout (§6.2d), t-z/Q-z curves (§6.5), and the WSD
> FoS format (§6.x). Full step-C seeding (JRC 2013 Ch 8/Annex A.8, AS 2159 risk-φg, Look Ch 21, CPT
> methods) joins the same steps.

## Task breakdown (operational steps)
🔒 = **step gate**. **Exception gates** also fire mid-task on any trigger (see
`../03-workflow-and-human-ai-model.md` → Gate model). **←CP** = supplied by the selected Code Pack.
The operational "how" (In/Does/Out/Gates) will be in `06-axial-pile-procedures.md` (step B).

**6.0 — Stage entry & input contract** *(once)*
- 6.0.1 Detect entry point (fresh from Stage 3/4 vs jump-in — input contract: characteristic params +
  pile type/geometry + design actions).
- 6.0.2 Resolve the safety-format basis ←CP — **EC:** confirm the Design Approach + pile factor sets
  (note DA1's R4 in Comb2); **AU:** load the AS 2159 **risk-assessment rubric** (Tables 4.3.2(A)–(C))
  and testing strategy — φg is **not** fixed yet, it is built in 6.2.
- 6.0.3 Confirm pile type (driven / bored / CFA — sets the γ table and installation φ shift), group
  redundancy (drives AU φgb), and consequence class.
- 6.0.4 Take action effects (two-mode, D38): direct design actions (default) or characteristic
  (fallback); ensure **both ULS and SLS**; note which actions are **geotech-computed** (downdrag/heave
  → 6.4), not supplied.
- 🔒 Gate 6.0 — confirm inputs, Code Pack, safety-format basis, pile type, and actions.

### Per-pile(-group) loop
**6.1 — Trial pile geometry & representative parameters**
- 6.1.1 Identify the founding stratum + the units along the shaft from the ground model.
- 6.1.2 Pull characteristic params (c_u, φ′/σ′_v, or CPT q_c/f_s profiles) + the Stage 2.7 preliminary
  fₛ/q_b profile.
- 6.1.3 Propose trial pile type, diameter, and length. ←CP
- 🔒 Gate 6.1.

**6.2 — ULS axial compressive resistance (shaft + base)** ←CP method + safety format *(deep)*
- 6.2.1 Filter the Method Library to applicable capacity methods (α / β / CPT direct / SPT-N fallback);
  set the Code-Pack default; apply the installation φ shift. ←CP
- 6.2.2 Compute shaft R_s;cal and base R_b;cal vs depth (with q_b cap), per method; co-present methods
  with scatter (CPT methods disagreeing is normal).
- 6.2.3 Form the characteristic/ultimate resistance with the pack's **reliability judgment** —
  **EC:** R_c;k via ξ₃/ξ₄ (n profiles), mean-vs-min, optional model factor; **AU:** the **risk
  assessment** → φgb → φg (+ testing benefit). *(The must-stay-human gate.)* ←CP
- 6.2.4 Apply the design factors — **EC:** R_c;d = R_b;k/γ_b + R_s;k/γ_s (DA1 run both combos, R1 & R4);
  **AU:** R_d,g = φg·R_d,ug. Verify F_c;d ≤ R_c;d / E_d ≤ R_d,g; report utilization + governing case.
- 🔒 Gate 6.2 — engineer owns the ξ basis (EC) / the risk assessment (AU).

**6.3 — ULS tensile / uplift resistance** *(conditional)*
- 6.3.1 Shaft resistance in tension (no base); EC γ_s;t tension factors / AU φg with uplift reduction;
  check against net uplift action. ←CP
- 🔒 Gate 6.3.

**6.4 — Downdrag (negative skin friction) & heave** *(conditional — ground-driven actions)*
- 6.4.1 Identify settling/swelling strata (fill over soft clay, dewatering, surcharge → downdrag;
  swelling/heave near the surface).
- 6.4.2 Compute the negative-skin-friction **action** on the pile (neutral plane) and add it as a
  factored action (EC M-set); compute heave uplift where relevant. ←CP
- 🔒 Gate 6.4 — a design-critical action the tool computes, not the structural engineer.

**6.5 — SLS settlement (single pile)** *(+ lateral scope note)*
- 6.5.1 Single-pile load–settlement (from load tests / t-z / elastic-continuum); check against the
  permissible displacement. ←CP
- 6.5.2 *Scope note:* transverse/lateral capacity (EN 1997-1 §7.7) is an **adjacent analysis** —
  flagged, not in the axial-MVP core (like overall stability was for footings).
- 🔒 Gate 6.5.

**6.6 — Pile group / block effects**
- 6.6.1 Block-failure check (group acting as a large pier) — take the lower of sum-of-singles and
  block resistance (§7.6.2.1(3)P). ←CP
- 6.6.2 Group settlement (group is stiffer/deeper-stressing than a single pile). ←CP
- 🔒 Gate 6.6.

**6.7 — Geometry iteration & optimization**
- 6.7.1 On fail / over-conservatism, revise pile length/diameter/number and re-run 6.2–6.6.
- 6.7.2 Engineer approves the final pile design + governing case.
- 🔒 Gate 6.7 — engineer owns the final geometry.

### Site-wide
**6.8 — Package / hand-off**
- 6.8.1 Compile per-pile(-group) design summary; zone across the site.
- 6.8.2 Design-stage constraints/risks (installation, spoil, driveability, group sequencing).
- 6.8.3 Stage notices: method choices, **ξ basis (EC) / the full risk-assessment record (AU)**,
  downdrag assumptions, overrides.
- 6.8.4 Hand off to structural pile design (AS 5100.5/.6 or EN 1992/1993 — out of geotechnical scope)
  and the report (Stage 7).

## Open items (◇ — to confirm with founder)
- ~~**(a)** CPT pile-method references (Eslami–Fellenius, Nottingham–Schmertmann)~~ — **added via
  FHWA GEC-12** (library §6.2e). Still open: **LCPC/Bustamante 1982** and **ICP-05** (+ UWA/Fugro/NGI
  offshore CPT-2005) — add from the French annex / API-ISO commentary / Jardine 2005 when sourced.
- **(b)** AS 2159 tension, downdrag, and group-effect clauses (§4.4 sub-clauses) — confirm exact
  references for 6.3/6.4/6.6.
- **(c)** Scope confirmation: transverse/lateral (§7.7) kept **out** of the axial-MVP core (flagged
  adjacent) — confirm.
- **(d)** SLS single-pile load–settlement method (t-z vs elastic-continuum vs test-derived) default —
  confirm.

## Provenance
EN 1997-1 §7 verified in the `.txt` (TOC pp. 70; §7.3.2 actions, §7.6.2.2/.3/.4 resistance pp. 76–82;
Annex A Tables A.6–A.11 pp. 131–134). AS 2159-2009 verified in the `.txt` (§4.3.1 φg formula; §4.3.2
risk assessment + Tables 4.3.2(A)–(C); §4.3.3 R_d,ug — pp. 21–24 printed). AS 5100.3 §11 delegation to
AS 2159 verified in the `.txt` (pp. 22 printed). Risk-factor Table 4.3.2(A) (garbled in `.txt`) and
φgb Table 4.3.2(C) rendered from the PDF to `../../Sources/Handbooks/figures/AS2159_*.png`.

## Next
- **B** — task-level procedures for 6.0–6.8 (In/Does/Out/Gates), with **6.2 (axial resistance +
  two-sided safety-format fork)** and **6.4 (downdrag)** the deeper treatments.
- ✓ **C (essentially done)** — Method Library **§6** now carries **all packs + all four safety-format
  paradigms**: EC7 ξ+partial-factors *(EN 1997-1 §7 / JRC 2013 Ch 8)*, AS 2159 risk-φg, API WSD FoS,
  FHWA/AASHTO φstat; capacity via α/β/CPT (API + GEC-12) + EC/AU routes + Look non-code. See
  [`../method-library.md` §6](../method-library.md#stage-6--axial-pile-design). Optional remaining:
  **LCPC/Bustamante** + offshore **CPT-2005** (ICP/UWA/Fugro/NGI) once sourced.
- After axial pile: the MVP design modules are complete → **Stage 7 report / GDR** packaging.
