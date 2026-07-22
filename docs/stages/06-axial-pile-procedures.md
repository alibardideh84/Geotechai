# Stage 6 — Axial pile: task procedures

The **operational "how"** for every task in the 6.0–6.8 breakdown of
[`06-axial-pile.md`](06-axial-pile.md). That document is the architecture and reference map (the
*what* and *which clause*); this one specifies, per task, the procedure the system runs. It mirrors
[`05-shallow-foundation-procedures.md`](05-shallow-foundation-procedures.md).

## Per-task template
Each numbered task is specified as **In · Does · Out · Gates/triggers** (see
[`../03-workflow-and-human-ai-model.md`](../03-workflow-and-human-ai-model.md) → Gate model). The
**engine** does deterministic capacity/settlement arithmetic; the **AI** scopes, selects methods,
explains, drafts, and proposes the reliability judgment. Each step ends with its 🔒 **step gate**.

**Legend:** ←CP = supplied/parameterized by the selected Code Pack · 🔒 = step gate. Exception
severities: **B** blocking · **D** decision · **A** advisory.

**What makes this stage distinct (vs shallow foundation):** the safety format is a **two-sided,
engineer-owned judgment gate** at 6.2 — *both* the EC pack (correlation factor **ξ**) and the AU pack
(**risk-assessment φg**) require an in-stage reliability decision on the computed resistance, not just
an inherited factor. And piles add **ground-driven actions the tool computes itself** — downdrag and
heave (6.4). Watch 6.2.3 (the fork) and 6.4 (the computed action).

---

## 6.0 — Stage entry & input contract *(runs once)*

### 6.0.1 — Detect entry point
- **In:** the project session (Stage 3 ground model + Stage 4 design basis & actions if present), or a
  direct upload on jump-in (input contract: characteristic params + pile type/geometry + design actions).
- **Does:** determine entry mode (fresh vs jump-in, D15); for jump-in, check the contract; bind the
  active Code Pack so ←CP hooks resolve.
- **Out:** entry-mode flag, bound Code Pack, manifest of supplied vs still-needed.
- **Gates/triggers:** *Missing/insufficient data* (B) if the contract is unmet (no actions, or no
  characteristic strength / CPT profile for the founding stratum).

### 6.0.2 — Resolve safety-format basis ←CP
- **In:** the bound Code Pack + (EC) the Stage 0/4 Design Approach.
- **Does:**
  1. **EC pack:** confirm the **Design Approach** and load the pile factor sets — note **DA1 runs two
     combinations** and **Comb2 uses R4** (the pile-specific severity); load the **ξ tables** (A.9
     static / A.10 ground profiles / A.11 dynamic). ξ is **not** fixed yet — it depends on how many
     tests/profiles exist (resolved in 6.2.3).
  2. **AU pack:** load the **AS 2159 risk-assessment rubric** (Tbl 4.3.2(A)–(C)) + the testing-benefit
     terms (φtf, K). φg is **not** fixed yet — it is *built* in 6.2.3.
  3. Record the reliability judgment as a **pending gated decision** (both packs), unlike shallow
     foundations where EC inherited fixed factors.
- **Out:** the resolved safety-format configuration (factor tables + the pending ξ / φg judgment).
- **Gates/triggers:** *Assumption required* (D) if the DA (EC) had to be inferred.

### 6.0.3 — Confirm pile type, redundancy, consequence class
- **In:** the design brief + Stage 0 category.
- **Does:** confirm **pile type** (driven / bored / CFA — sets the EC γ table A.6/A.7/A.8 and the
  installation φ shift) and, for AU, the **group redundancy** (high vs low — drives φgb) and the
  consequence class (feeds the AS risk factors + EC checks).
- **Out:** pile type + redundancy + consequence class carried to 6.2.3.
- **Gates/triggers:** *Design-critical threshold* (D) for high-consequence / low-redundancy systems.

### 6.0.4 — Take action effects (ULS + SLS) — two input modes ←CP
- **In:** one of — **Mode A (default)** the structural engineer's already-combined **design actions**
  (ULS compressive `F_c;d` / `E_d`, any tension, lateral; + SLS), consumed as-is; **Mode B (fallback)**
  characteristic actions the tool combines (EC DA1 needs A1 & A2 sets). (D38.)
- **Does:**
  1. Consume/assemble actions per the mode (as in 5.0.4); record which combination/A-set each carries
     (EC) so 6.2 pairs the right factors.
  2. **Flag the ground-driven actions the tool must compute itself** — **downdrag/negative skin
     friction and heave** (→ 6.4) — these are *not* supplied by the structural engineer.
  3. Ensure both **ULS and SLS** are present (SLS drives 6.5).
- **Out:** the per-pile action-effect set + a note of which actions are geotech-computed.
- **Gates/triggers:** *Missing/insufficient data* (B) if a required set is absent. *Inconsistency*
  (D) if EC design actions don't match the resolved DA (double-factor risk).

**🔒 Gate 6.0 — confirm inputs, Code Pack, safety-format basis, pile type, and actions.**

---

## Per-pile(-group) loop

## 6.1 — Trial pile geometry & representative parameters

### 6.1.1 — Identify the founding stratum + shaft units
- **In:** the Stage 3 ground model + pile location.
- **Does:** identify the **founding stratum** at the trial toe and the **units along the shaft**; flag
  weak layers below the toe (punching, §7.6.2.1(11): weak ground within 4 base-diameters) and any
  strata that will **settle (→ downdrag) or swell (→ heave)** for 6.4.
- **Out:** the shaft/base unit profile + downdrag/heave flags.
- **Gates/triggers:** *Design-critical threshold* (D) for a weak layer below the base or a
  downdrag-prone stratum.

### 6.1.2 — Pull design-basis parameters + preliminary profile
- **In:** the units' characteristic parameters (c_u, φ′/σ′_v, or CPT q_c/f_s profiles) + the Stage 2.7
  preliminary fₛ/q_b profile.
- **Does:** assemble what the capacity methods need; carry each value's reliability + provenance.
- **Out:** a per-pile parameter set (with CPT profiles kept intact for the direct methods).
- **Gates/triggers:** *Missing/insufficient data* (D) → SPT-N fallback (Look) with a low-reliability flag.

### 6.1.3 — Propose trial pile ←CP
- **In:** the preliminary profile + structural demand + constructability.
- **Does:** propose trial **pile type, diameter, length** (seed length from the preliminary fₛ/q_b so
  the first capacity pass is close).
- **Out:** a trial pile geometry.
- **Gates/triggers:** *Assumption required* (D) where installation constraints (driveability, rockhead)
  bound the geometry.

**🔒 Gate 6.1.** Engineer reviews the founding stratum, parameters, and trial pile.

---

## 6.2 — ULS axial compressive resistance (shaft + base) *(deep treatment)*

The core ULS check and the **two-sided safety-format fork**. The **capacity mechanics are
pack-agnostic** — the same α/β/CPT methods compute an ultimate shaft+base resistance — but forming the
**design resistance** requires a pack-specific, engineer-owned reliability judgment:

| | EC pack | AU pack | API (WSD) | AASHTO (US LRFD) |
|---|---|---|---|---|
| Ultimate resistance | `R_cal = R_b;cal + R_s;cal` | `R_d,ug = f_m,s·A_s + f_b·A_b` | `Q_d = Q_f + Q_p` | `R_n = R_s + R_p` |
| Reliability judgment | **ξ** on the resistance set (n tests/profiles, mean-vs-min) | **risk assessment** → φgb (+ φtf·K) | global **FoS** by load condition | **φstat** calibrated per method |
| Design resistance | `R_c;d = R_b;k/γ_b + R_s;k/γ_s` (DA1 R1 & R4) | `R_d,g = φg·R_d,ug` | `Q_all = Q_d/FoS` | `R_r = φstat·R_n` |
| Gated? | **yes** (ξ = gate #3) | **yes** (risk assessment) | no (FoS fixed by condition) | no (φstat fixed by method) |

### 6.2.1 — Filter Method Library to applicable capacity methods ←CP
- **In:** the parameter set + data available (c_u, φ′/σ′_v, CPT, or SPT-N-only) + pile type.
- **Does:**
  1. Query Method Library §6.2 for methods matching the scenario — **clay** α (API/Look) · **sand**
     β/K·p₀·tanδ + N_q (API/Look) · **CPT** Eslami–Fellenius / Nottingham–Schmertmann (GEC-12) ·
     **SPT-N** Meyerhof fallback (Look). Apply the **installation φ shift** (bored vs driven, Look Tbl
     21.15).
  2. Set the pack default and keep the rest as reachable alternatives; if the pack is silent, surface a
     library method flagged *outside selected standard* (D26).
- **Out:** a candidate-method set with one default marked.
- **Gates/triggers:** *Multiple routes* (D) when ≥2 methods apply (the norm for piles). *Out-of-
  applicability* (B) for carbonate/calcareous or micaceous sands (API/Look table caveats) — needs
  site-specific parameters.

### 6.2.2 — Compute shaft R_s;cal & base R_b;cal
- **In:** the candidate methods + parameters/CPT profiles + geometry.
- **Does:** run the **capacity engine** per method — shaft resistance integrated over the length, base
  resistance at the toe (with the toe-averaging / influence-zone rules for CPT methods, and the
  **q_b cap** ~10 MPa / API limits); handle plugged vs unplugged. **Co-present the methods with
  scatter** — CPT methods disagreeing is expected, not an error.
- **Out:** per-method `R_s;cal`, `R_b;cal` (and `R_cal`) with the depth breakdown + method provenance.
- **Gates/triggers:** *Methods disagree materially* (A→D) — expected; carried to the ξ/φg step, not
  auto-reconciled.

### 6.2.3 — Form the design resistance — the pack reliability judgment ←CP *(the fork)*
- **In:** the calculated resistances + the resolved safety-format basis (6.0.2) + how many
  tests/profiles exist + the AS risk inputs (6.0.3).
- **Does — EC pack:**
  1. Choose the ξ basis: **ξ₁/ξ₂** if from static load tests, **ξ₃/ξ₄** if from ground-test profiles
     ("Model Pile"), **ξ₅/ξ₆** if dynamic — indexed by **n** (number of tested piles / profiles).
  2. `R_c;k = Min( R_cal,mean/ξ , R_cal,min/ξ )`; apply the **stiff-structure ÷1,1** and/or a **model
     factor** (ground-parameter route) where justified.
  3. Present the ξ choice (n, mean-vs-min, model factor) **for engineer approval** — this is
     must-stay-human **gate #3** (`../03`).
- **Does — AU pack:**
  1. **Perform the risk assessment**: rate the ~9 factors (Site / Design / Installation) IRR 1–5 with
     weights → **ARR = Σ(w·IRR)/Σw** → **φgb** from Tbl 4.3.2(C) by ARR band × redundancy.
  2. Apply the **testing-benefit uplift** `φg = φgb + (φtf − φgb)·K` from the testing strategy (φtf by
     test type; K by % piles tested).
  3. Present the **risk assessment + φg** with rationale **for engineer approval** (must-stay-human).
- **Does — API / AASHTO** (when governing): read the fixed **FoS** by load condition / the **φstat** by
  method — no in-stage judgment.
- **Out:** the design resistance (`R_c;d` EC / `R_d,g` AU / `Q_all` API / `R_r` AASHTO) + the recorded
  reliability judgment.
- **Gates/triggers:** *Decision* (D) — **the reliability judgment always gates for EC and AU** (the
  engineer owns ξ / the risk assessment). *Design-critical threshold* (D) for low-redundancy/high-
  consequence.

### 6.2.4 — Verify the inequality; utilization & governing case
- **In:** the design resistance + the design action `F_c;d` / `E_d`.
- **Does:** check `F_c;d ≤ R_c;d` (EC) / `E_d ≤ R_d,g` (AU) / `Q ≤ Q_all` (API); compute **utilization**;
  identify the **governing case** — which method, which DA combination (EC DA1 C1 vs C2), which load
  combination; note **block vs single** deferred to 6.6.
- **Out:** pass/fail + utilization + governing-case record.
- **Gates/triggers:** *Design-critical threshold* (D) if utilization > 100% (→ 6.7 iterate) or markedly
  low (over-conservative). *Methods disagree* (D) if the chosen method vs alternatives would change the
  outcome.

> **Method Library → [`../method-library.md` §6.2](../method-library.md#62--axial-capacity-shaft-f_s--base-q_b)**
> (capacity: API α/β, CPT Eslami–Fellenius / Nottingham–Schmertmann, EC/AU/Look routes) and the
> safety-format subsections [§6.x(EC)](../method-library.md#6xec--safety-format-characteristic-resistance-ξ--partial-factors--ec-pack)
> / [§6.x(AU)](../method-library.md#6xau--safety-format-as-2159-risk-based-φg--au-pack)
> / [§6.x(API)](../method-library.md#6xapi--safety-format-working-stress-design-global-fos--api)
> / [§6.x(US)](../method-library.md#6xus--safety-format-aashto-method-keyed-φstat-us-lrfd--fhwa-gec-12).

**🔒 Gate 6.2 — engineer owns the reliability judgment.** Reviews the capacity methods + scatter, and
**approves the ξ basis (EC) / the risk assessment + φg (AU)**, the design resistance, utilization, and
governing case. *(This is the pile analogue of the Stage 3 characteristic-value gate — a must-stay-human
decision.)*

---

## 6.3 — ULS tensile / uplift resistance *(conditional)*

### 6.3.1 — Tension check
- **In:** the net uplift action + shaft parameters.
- **Does:** compute **shaft resistance in tension only** (no base; API §6.5: ultimate pullout ≤ Q_f);
  include effective pile weight + hydrostatic uplift + soil-plug weight. Apply the pack tension format —
  **EC** γ_s;t (tension factors, larger than compression); **AU** φg with the uplift reduction. Verify
  against the net uplift.
- **Out:** tension pass/fail + utilization.
- **Gates/triggers:** *Design-critical threshold* (D) — brittle load-shedding risk for short tension
  piles in loose sand (API caveat); *Low confidence* (A) where tension calibration is weak.

**🔒 Gate 6.3.**

---

## 6.4 — Downdrag (negative skin friction) & heave *(conditional — ground-driven actions, deep)*

The **actions the structural engineer cannot supply** — they depend on soil movement relative to the
pile, so the **geotech tool computes them** and adds them as factored actions (D40). This is the pile
extension of the D37/D38 actions model.

### 6.4.1 — Identify moving strata & the neutral plane
- **In:** the ground model + loading/dewatering/surcharge context + the 6.1.1 downdrag/heave flags.
- **Does:**
  1. Identify strata that will **settle relative to the pile** (fill over soft clay, fresh fill,
     dewatering, surcharge → **downdrag**) or **swell/heave** (reactive clay, unloading → **heave**).
  2. Locate the **neutral plane** (where relative soil–pile movement, hence shear direction, reverses):
     above it skin friction is **negative (drags down)**; below it, positive (resists).
- **Out:** the moving-strata profile + neutral-plane depth.
- **Gates/triggers:** *Assumption required* (D) for the neutral-plane location / future fill/dewatering
  scenario. *Design-critical threshold* (D) when downdrag is significant.

### 6.4.2 — Compute the downdrag / heave action ←CP
- **In:** the moving strata + neutral plane + shaft parameters.
- **Does:**
  1. Compute the **negative skin friction** as an **action** on the pile (integrated above the neutral
     plane) and add it to the axial demand; **EC factors it with an M-set** (§7.3.2.2 — a within-pack
     action/resistance factor split, distinct from the resistance γ). Compute **heave uplift** where
     relevant (§7.3.2.3).
  2. Re-enter it into the 6.2 verification: downdrag **adds to `F_c;d`** and moves the effective
     resistance datum to below the neutral plane (shaft above it no longer resists).
- **Out:** the downdrag/heave action + the revised axial demand for 6.2.4.
- **Gates/triggers:** *Design-critical threshold* (D) — downdrag can govern pile length; *Inconsistency*
  (D) if the assumed settlement scenario conflicts with the Stage 2.5 groundwater/consolidation model.

**🔒 Gate 6.4 — a design-critical action the tool computes, not the structural engineer.**

---

## 6.5 — SLS settlement (single pile) *(+ lateral scope note)*

### 6.5.1 — Single-pile load–settlement
- **In:** SLS actions + the load-transfer parameters (t-z / Q-z curves, or elastic-continuum / test
  data).
- **Does:** compute single-pile head settlement at SLS (API t-z/Q-z springs, §6.5 in the library; full
  base mobilisation needs ~10% of base diameter); check against the **permissible displacement**
  (AS 5100.3 §7.5 / project SLS limit).
- **Out:** single-pile settlement vs limit.
- **Gates/triggers:** *Design-critical threshold* (D) if settlement exceeds the SLS limit (→ 6.7).
  *Low confidence* (A) for t-z curves not site-calibrated.

### 6.5.2 — Lateral / transverse scope note
- **Does:** transverse capacity + p-y analysis (EN 1997-1 §7.7; API §6.8 Matlock/Reese p-y curves) is
  an **adjacent analysis**, flagged and **out of the axial-MVP core** (like overall stability was for
  footings). Note it where lateral actions exist; do not run it here.
- **Gates/triggers:** *Out-of-applicability* (A) flag when significant lateral loading exists.

**🔒 Gate 6.5.**

---

## 6.6 — Pile group / block effects

### 6.6.1 — Block-failure check
- **In:** the group layout (spacing, cap) + single-pile resistances.
- **Does:** check the group acting as a **block** (large-diameter equivalent pier, §7.6.2.1(3)P); take
  the **lower** of Σ single-pile resistance and block resistance.
- **Out:** the governing group compressive resistance.
- **Gates/triggers:** *Design-critical threshold* (D) for closely-spaced groups where block governs.

### 6.6.2 — Group settlement
- **In:** the group geometry + compressible strata below the group.
- **Does:** compute **group settlement** (the group stresses a deeper, larger volume than a single pile
  — equivalent-raft or interaction methods); check against the SLS limit.
- **Out:** group settlement vs limit.
- **Gates/triggers:** *Design-critical threshold* (D) when group settlement governs over single-pile.

**🔒 Gate 6.6.**

---

## 6.7 — Geometry iteration & optimization

### 6.7.1 — Re-size on fail / over-conservatism
- **In:** the ULS (6.2/6.3), downdrag (6.4), SLS (6.5), group (6.6) results.
- **Does:** if any check fails or utilization is markedly low, revise **length / diameter / pile
  number** and re-run 6.2–6.6 (adding length past the neutral plane is the usual downdrag remedy).
- **Out:** a converged pile design with the governing check identified.
- **Gates/triggers:** *Multiple routes* (D) where several geometries pass (cost vs robustness).

### 6.7.2 — Finalize
- **Does:** present the final geometry, governing case, utilizations, and the safety basis (EC: DA
  combination + ξ; AU: the φg + risk record) for sign-off.
- **Gates/triggers:** none specific (feeds the step gate).

**🔒 Gate 6.7 — engineer owns the final geometry.**

---

## Site-wide

## 6.8 — Package / hand-off

Closes the module — over already-approved per-pile designs, so **no new step gate**.

### 6.8.1 — Compile per-pile(-group) summaries; zone
- **Does:** assemble the pile schedule (type, geometry, governing check, safety basis, utilization);
  zone across the site where piles share units/load.
- **Out:** the pile design schedule.
- **Gates/triggers:** *Inconsistency* (D) if equivalent-evidence piles resolve to inconsistent ξ / φg.

### 6.8.2 — Design-stage constraints & risks
- **Does:** installation issues (driveability, refusal, plugging, spoil), group sequencing, downdrag
  monitoring, and any **load-testing programme** (which *lifts* the AU φg via K and tightens the EC ξ
  via n — so the test plan is a design lever, not just verification).
- **Out:** constraints/risks + a recommended test regime.

### 6.8.3 — Notices register
- **Does:** stage entries — method choices + scatter, the **ξ basis (EC)** / the **full risk-assessment
  record (AU)**, downdrag assumptions, out-of-code methods, overrides (D27).

### 6.8.4 — Hand off
- **Does:** package for the **report (Stage 7)**; flag the **structural pile design** interface
  (AS 5100.5/.6 or EN 1992/1993 — reinforcement, driving stresses; out of geotechnical scope); version/
  freeze against the ground-model version.
- **Gates/triggers:** *Missing/insufficient data* (B) if a report/interface contract is unmet.

## Status — complete
- **6.0–6.8 task procedures drafted** (In/Does/Out/Gates), with **6.2 (axial resistance + two-sided
  safety-format fork)** and **6.4 (downdrag)** given the deeper treatment. The reliability judgment
  (EC ξ / AU risk-φg) is surfaced as an **engineer-owned in-stage gate for both packs** — the defining
  feature of the pile module.

**Stage 6 axial pile now has all three parts:** A (stage doc) ✓ · B (this procedures doc) ✓ ·
C (Method Library §6, all packs + four safety-format paradigms) ✓. **Both MVP design modules are now
complete** (Stage 5 + Stage 6). Next planned work (per `../06-decision-log.md`): **Stage 7 report /
GDR** packaging; optional extras — LCPC/ICP CPT methods, parallel worked calibrations.
