# Stage 5 — Shallow foundation: task procedures

The **operational "how"** for every task in the 5.0–5.7 breakdown of
[`05-shallow-foundation.md`](05-shallow-foundation.md). That document is the architecture and
reference map (the *what* and *which clause*); this one specifies, per task, the procedure the
system runs. It mirrors [`02-interpretation-procedures.md`](02-interpretation-procedures.md).

## Per-task template
Each numbered task is specified as:
- **In** — what it consumes (upstream task output · input-contract upload · Code Pack).
- **Does** — the AI + engine logic, in order. The **engine** does deterministic
  arithmetic (bearing/sliding/settlement); the **AI** scopes, selects methods, explains, drafts, and
  proposes the safety-format choice.
- **Out** — what it produces and hands to the next task.
- **Gates/triggers** — exception triggers (see
  [`../03-workflow-and-human-ai-model.md`](../03-workflow-and-human-ai-model.md) → Gate model) that
  can fire *mid-task*. Each step ends with its 🔒 **step gate**.

**Legend:** ←CP = supplied/parameterized by the selected Code Pack · 🔒 = step gate.
Exception severities: **B** blocking · **D** decision · **A** advisory.

**The one thing that makes this stage different from interpretation:** the **safety format is a
first-class, pack-forking step** (5.2.3 / 5.3 / 5.x). For the **EC pack** it is *inherited* from an
upstream, project-wide choice (the Design Approach, fixed at Stage 0/4) — no judgment inside the
per-footing loop. For the **AU pack** it is a **new gated decision made *inside* Stage 5**, per
bearing/sliding check, because φg depends on *how* the ultimate resistance was assessed for *this*
footing. Watch how the same task branches by pack at 5.2.3 — that asymmetry is the point.

---

## 5.0 — Stage entry & input contract *(runs once)*

### 5.0.1 — Detect entry point
- **In:** the project session (Stage 3 ground model + Stage 4 design basis & actions if present), or
  a direct upload when the engineer jumps in at Stage 5 (input contract per `../03` → Flexible entry:
  characteristic parameters + footing geometry options + actions/load combinations).
- **Does:**
  1. Determine entry mode — *fresh from Stage 3/4* vs *jump-in*. (Per D15, flexible entry.)
  2. For jump-in, read the Stage 5 input contract and check the upload satisfies it (design-basis
     parameter set per unit; actions with load combinations; trial geometry or a sizing brief).
  3. Bind the active Code Pack (from Stage 0) so all downstream ←CP hooks resolve.
- **Out:** an entry-mode flag, the bound Code Pack, and a manifest of what was supplied vs what must
  still be requested.
- **Gates/triggers:** *Missing/insufficient data* (B) if the contract is unmet (e.g. no actions, or
  no characteristic strength for the bearing unit). *Assumption required* (D) if Stage 4 was skipped
  and load combinations must be assembled here from raw actions.

### 5.0.2 — Resolve safety-format basis ←CP
- **In:** the bound Code Pack + (EC) the Stage 0/4 Design Approach selection.
- **Does:**
  1. **EC pack:** confirm the selected **Design Approach (DA1 / DA2 / DA2\* / DA3)** and load its
     partial-factor sets (A/M/R per Method Library §5.x(EC)). This is the *must-stay-human* gate #1
     (`../03`) — it is confirmed here, **not re-decided per footing**.
  2. **AU pack:** record that **φg is *not* fixed yet** — it will be proposed per-check in 5.2.3 /
     5.3 / 5.4 from Table 10.3.3(A)/(B). Load the φg ranges + the where-in-range guide.
  3. Note the factor-application-point for EC (DA2 at-source vs DA2\* at-effect), because it changes
     how eccentricity feeds `A′` in 5.2.
- **Out:** the resolved safety-format configuration (EC: fixed factor sets; AU: φg ranges + selection
  rubric pending).
- **Gates/triggers:** *Assumption required* (D) if Stage 0 was skipped and the DA (EC) had to be
  inferred — surface for confirmation. *Multiple routes* (D) if EC pack and the choice DA2 vs DA2\*
  is left open (flag the economy/eccentricity consequence).

### 5.0.3 — Confirm geotechnical category / consequence class
- **In:** the design brief + Stage 0 category.
- **Does:** confirm the geotechnical category / consequence-of-failure class — it **drives the φg
  band** within Table 10.3.3(B) (AU) and informs which **DA combination is likely to govern** (EC).
- **Out:** a confirmed consequence class carried into 5.2.3 (AU φg rationale) and the notices register.
- **Gates/triggers:** *Design-critical threshold* (D) for high-consequence structures (tightens φg /
  raises scrutiny).

### 5.0.4 — Assemble / verify action combinations (ULS + SLS) ←CP
*(The actions side of `E_d ≤ R_d`; see the stage doc "Actions & load combinations" section. Formally
a Stage 4 job — here it is **verified** when supplied, or **assembled** on a jump-in start.)*
- **In:** the **characteristic (un-factored) actions** — per footing/column, each with: permanent
  `G_k` vs variable `Q_k` (variable tagged by **category** for ψ), **direction/type** (`V`/`H`/`M`),
  and **favourable/unfavourable nature**; plus the pack's loading model (EN 1990/1991 for EC, AS 5100.2
  for AU) and the resolved safety-format basis (5.0.2).
- **Does:**
  1. **Never ask for pre-factored loads** — take characteristic actions and do all factoring here.
  2. Add the actions the system derives itself (not asked): **foundation self-weight + backfill**
     (geometry × density), **water pressure / uplift** (Stage 2.5 groundwater model), **earth
     pressures** on the footing (EN 1997-1 §6.3 / §2.4.2(4) / AU loading code).
  3. **Build both families** — *ULS combinations* (factored: γ_G·G_k + γ_Q·Q_k + ψ·accompanying, per
     the DA sets / AS 5100.2) for bearing & sliding, and *SLS combinations* (partial factors = 1,
     quasi-permanent ψ₂) for settlement. **Both are required** — either limit state can govern
     (Terzaghi–Peck size effect).
  4. Enumerate the **permutations** eccentricity forces: permanent load favourable **and**
     unfavourable, and the **leading variable action switched** (V-led vs H-led) — each yields a
     different `V_d`, `H_d`, `e` → effective area `A′` in 5.2.
- **Out:** the ULS and SLS **load-combination set** per footing (design action effects `V_d`, `H_d`,
  `M_d` for ULS; serviceability `V_ser` etc. for SLS), each combination traceable to its factors.
- **Gates/triggers:** *Missing/insufficient data* (B) if characteristic actions are absent or lack the
  direction/nature metadata. *Assumption required* (D) if Stage 4 was skipped and combinations are
  assembled here (surface the ψ/factor choices), or if an action's favourable/unfavourable role must
  be assumed. *Multiple routes* (D) where it is not obvious which combination governs (carry all).

**🔒 Gate 5.0 — confirm inputs, Code Pack, safety-format basis, and action combinations.** The engineer
reviews the input manifest, the bound pack, (EC) the Design Approach / (AU) the φg selection rubric, and
the assembled **ULS + SLS load combinations**, then approves before the per-footing loop runs.

---

## Per-footing loop — runs at every proposed pad / strip / raft

> Tasks 5.1–5.6 execute once per footing (or per footing group sharing a unit + load case). Outputs
> are per-footing until the site-wide packaging in 5.7.

## 5.1 — Trial geometry & representative parameters

### 5.1.1 — Identify the governing unit(s) beneath the footprint
- **In:** the Stage 3 ground model (units, groundwater, characteristic values) + the footing location.
- **Does:** locate the footing on the ground model; identify the **founding unit** and any unit within
  the stress-influence depth (≈ 1–2·B below base, EC7 §6.6.2(7)); flag layered cases (strong-over-weak
  → punching; weak-over-strong → use the weak layer, EN 1997-1 §6.5.2.2(5)).
- **Out:** the governing unit set + a layering flag for the bearing model.
- **Gates/triggers:** *Design-critical threshold* (D) if a **weak layer / lens** sits within the
  influence depth (governs bearing and settlement). *Out-of-applicability* (B) if the founding stratum
  is rock → route to 5.4 instead of 5.2's soil method.

### 5.1.2 — Pull design-basis parameters + preliminary profile
- **In:** the governing units' design-basis parameter set (Stage 2.6/3) + the Stage 2.7 preliminary
  bearing/settlement profile.
- **Does:** assemble the parameters the bearing/settlement engines need (c′/φ′ or c_u; γ; stiffness
  E/M; groundwater level); carry each value's provenance + reliability flag from interpretation.
- **Out:** a per-footing parameter set ready for the ULS/SLS engines.
- **Gates/triggers:** *Missing/insufficient data* (D) if a required parameter was never derived → loop
  back to Stage 2.3/2.6 (or apply an SPT-N fallback with a low-reliability flag, per 5.2.1).

### 5.1.3 — Propose trial geometry ←CP
- **In:** the preliminary profile + structural constraints (column/wall loads, spacing) + practical
  constraints (EN 1997-1 §6.4(4)P / AS 5100.3 §10.3.2: economic excavation, setting-out tolerance,
  working space, GWT depth, seasonal/frost depth, scour).
- **Does:** propose trial `B`, `L`, embedment `D` — seed `B` from the §5.1 presumed-bearing table
  (Method Library §5.1a) or the preliminary profile, so the first ULS pass is close.
- **Out:** a trial geometry to check.
- **Gates/triggers:** *Assumption required* (D) where embedment is set by non-bearing constraints
  (frost/seasonal/scour) rather than capacity.

> **Method Library → [`../method-library.md` §5.1](../method-library.md#51--preliminary--presumed-bearing-prescriptive-method).**
> Presumed-bearing table (Look Tbl 21.3) for the trial-`B` seed; EN 1997-1 §6.4 / AS 5100.3 §10.3.2
> depth-selection considerations (near-identical between packs).

**🔒 Gate 5.1.** Engineer reviews the governing units, parameter set, and trial geometry; approves
before the checks run.

---

## 5.2 — ULS bearing resistance *(deep treatment)*

The core ULS check and the **first place the packs fork on safety format**. The *bearing mechanics*
(equation + factor set) are **pack-shared** — AS 5100.3 prescribes no equation of its own, so both
packs use the EN 1997-1 Annex D method (Method Library §5.2). Only the **safety application (5.2.3)**
differs, and it differs structurally, not just numerically:

| | EC pack | AU pack |
|---|---|---|
| Ultimate resistance R_ug | same Annex-D computation | same Annex-D computation |
| Safety applied | design values **already factored** upstream (γ_M on params and/or γ_R on resistance per the fixed DA) → compute `R_d` | one **φg** proposed **here**, per this check, from Table 10.3.3(A)/(B) → `φg·R_ug` |
| Judgment inside 5.2 | **none** — factor sets are inherited | **a gated φg proposal** — method + investigation quality + consequence |
| Inequality | `V_d ≤ R_d` | `S* ≤ φg·R_ug` |

### 5.2.1 — Filter Method Library to applicable bearing methods ←CP
- **In:** the per-footing parameter set + drainage condition + the data actually available.
- **Does:**
  1. Query Method Library §5.2 for methods matching the scenario (drained c′/φ′ vs undrained c_u;
     lab-derived vs CPT/DMT-derived vs **SPT-N-only fallback**).
  2. Set the **pack code default** — the EN 1997-1 Annex D analytical method (both packs). Keep the
     Meyerhof N-chart (Look Tbl 21.7) as the reachable **fallback** when no strength parameter exists,
     and the presumed-bearing table (§5.1a) as a sanity band.
  3. If a scenario has no code-sanctioned route, surface a library method flagged *outside selected
     standard* for approval (D26).
- **Out:** a candidate-method set with one default marked + the drainage basis.
- **Gates/triggers:** *Multiple routes* (D) if drained **and** undrained could govern (fine-grained
  soil — check both, §6.5.2.2(2)P). *Out-of-applicability* (B) if the only route is an SPT-N fallback
  outside its granular-soil validity. *Assumption required* (D) when drainage must be assumed.

### 5.2.2 — Compute ultimate bearing resistance
- **In:** the default method + parameters + geometry.
- **Does:** run the **bearing engine** (Method Library §5.2a–e):
  1. Bearing factors `N_q, N_c, N_γ` from φ′ (drained) or the `(2+π)c_u` form (undrained).
  2. Corrections — **shape** `s`, **load-inclination** `i` (with the `m` exponent from `B′/L′` and the
     H direction), **base-inclination** `b`, and **groundwater** (submerged γ′ below the water table).
  3. **Eccentricity → effective area:** `B′ = B − 2e_B`, `L′ = L − 2e_L`; compute on `A′ = B′·L′`.
     Apply the §6.5.4 large-eccentricity precaution (`e > B/3`) and a construction tolerance up to
     0,10 m; **note the EC DA2-vs-DA2\* branch here** — design eccentricity `e_d` (DA2, factored
     actions) gives a smaller `B′` than characteristic `e_k` (DA2\*, factor-at-effect).
  4. Handle the layering flag from 5.1.1 (weak-over-strong → weak-layer strength; strong-over-weak →
     punching check).
- **Out:** the ultimate bearing resistance `R_ug` (characteristic/unfactored) + the utilised effective
  geometry + the full factor breakdown (clause-traceable).
- **Gates/triggers:** *Design-critical threshold* (D) when eccentricity exceeds `B/3`. *Error/
  validation* (B) if `H > A′·c_u` (undrained inclination invalid) or a factor goes non-physical.

### 5.2.3 — Apply the pack safety format ←CP *(the fork)*
- **In:** `R_ug` + the resolved safety-format basis (from 5.0.2).
- **Does — EC pack:**
  1. The design values of actions/parameters/resistance already carry the selected DA's factors;
     assemble `V_d` (permanent + variable, with ψ combination factors) and `R_d`.
  2. If **DA1**, run **both combinations** (C1: A1+M1+R1, C2: A2+M2+R1) — either may govern.
  3. No new judgment — the factor sets are inherited.
- **Does — AU pack:**
  1. **Propose φg** from Table 10.3.3(A) by the method that produced `R_ug` (advanced in-situ
     0,50–0,65 · advanced lab 0,45–0,60 · CPT 0,40–0,50 · SPT 0,35–0,40).
  2. **Place it within the range** using Table 10.3.3(B) — investigation extent, calculation
     sophistication, construction control, consequence class (from 5.0.3), load type (cyclic/static),
     structure permanence, correlation provenance (published vs site-specific).
  3. Present the proposed φg **with its rationale** and route it to the gate (this is the AU-only
     mid-stage decision).
- **Out:** `R_d` (EC) or the proposed `φg` + `φg·R_ug` (AU), each with the factor/again rationale.
- **Gates/triggers:**
  - **AU:** *Multiple routes / assumption required* (D) — the φg proposal **always** gates (the
    engineer owns the safety factor). *Design-critical threshold* (D) for high-consequence class.
  - **EC:** *Multiple routes* (D) only if DA2 vs DA2\* is still open (economy/eccentricity trade-off).

### 5.2.4 — Verify the inequality; report utilization & governing case
- **In:** `V_d`/`R_d` (EC) or `S*`/`φg·R_ug` (AU).
- **Does:** check `V_d ≤ R_d` (EC) or `S* ≤ φg·R_ug` (AU); compute **utilization** (`V_d/R_d` or
  `S*/(φg·R_ug)`); identify the **governing case** — drained vs undrained, which load combination,
  which DA combination (EC DA1), which favourable/unfavourable action permutation.
- **Out:** pass/fail + utilization + governing-case record for this footing.
- **Gates/triggers:** *Design-critical threshold* (D) if utilization > 100% (fail → 5.6 iterate) or
  markedly < 100% (over-conservative → 5.6 optimise). *Methods disagree* (A→D) if the fallback and a
  primary method give materially different `R_ug`.

> **Method Library → [`../method-library.md` §5.2](../method-library.md#52--uls-bearing-resistance-ec7-direct-method-annex-d)
> and [§5.x(EC)](../method-library.md#5xec--safety-format-da1da2da3--da2--ec-pack) /
> [§5.x(AU)](../method-library.md#5xau--safety-format-risk-based-φg--au-pack).** Bearing equation +
> factor/correction set + eccentricity rules (5.2a–e); the DA1/2/3(+DA2\*) sets with the worked
> size-`B` calibration (validation case); the AU φg Tables 10.3.3(A)/(B). Non-code fallbacks
> (Meyerhof N-chart, Vesic/Hansen cross-check, FoS) in 5.2(f).

**🔒 Gate 5.2.** Engineer reviews the bearing computation, the safety-format basis (**AU: approves
the φg + rationale**; EC: confirms the governing DA combination), utilization, and governing case.

---

## 5.3 — ULS sliding & overall stability

### 5.3.1 — Sliding resistance ←CP
- **In:** the horizontal design action `H_d`/`S*`, vertical load, base type, interface parameters.
- **Does:**
  1. Compute base shear resistance — drained `V′_d·tanδ_d` (`δ_d = φ′_cv,d` cast-in-situ, `⅔φ′_cv,d`
     smooth precast; **neglect c′**) or undrained `A_c·c_u` (**cap at 0,4·V_d** where full contact
     isn't assured).
  2. Apply the pack safety format as in 5.2.3 — EC `R_d = V′_d·tanδ_k/γ_Rh` (or factor-parameter form);
     **AU** `H_ug·φg + E_pr·φg ≥ S*` (propose φg per 5.2.3).
  3. **Passive resistance:** default to **neglect** `R_{p,d}` (mobilisation-mismatch + front-soil
     removal caveat); include only with justification and a movement-compatibility note.
- **Out:** sliding pass/fail + utilization.
- **Gates/triggers:** *Assumption required* (D) if passive resistance is included. *Design-critical
  threshold* (D) for horizontal-load-dominated footings.

### 5.3.2 — Overall-stability screen
- **In:** the site setting (slope, excavation, retaining structure, waterway, mine workings) from the
  ground model.
- **Does:** run the trigger checklist (EN 1997-1 §6.5.1 / AS 5100.3 §10.3.3.2); if any triggers, flag
  a **global stability** analysis (EC Section 11 style, DA-appropriate factors — outside the
  spread-footing engine) as a required companion check.
- **Out:** an overall-stability flag + (if triggered) a hand-off to the global-stability check.
- **Gates/triggers:** *Out-of-applicability* (B) — a triggered global-stability condition means the
  local bearing check alone is insufficient; must be resolved before sign-off.

**🔒 Gate 5.3.** Engineer reviews sliding and the overall-stability screen; approves or commissions
the global check.

---

## 5.4 — Foundations on rock *(conditional — founding stratum is rock)*

### 5.4.1 — Rock bearing
- **In:** rock parameters (UCS, RQD, weathering, defects) from interpretation §2.2/§2.3; geometry.
- **Does:**
  1. **EC pack:** presumed bearing from EN 1997-1 **Annex G** (weak/broken rock groups, settlement
     ≤ 0,5 %·B basis).
  2. **AU pack / non-code:** RQD-based bearing (Look Tbl 22.1, Peck/Hansen/Thorburn) — **AS 5100.3 has
     no rock-specific annex**, so this is the AU-pack default, flagged *outside selected standard*,
     cross-checked against the general §10.3.3.3 `R_ug`/φg framework.
  3. Where lab/in-situ rock data support it, cross-check against an analytical bearing method; take
     the **lesser** of presumed, UCS-limited, and concrete-allowable (Look §2.7d rule).
- **Out:** rock bearing resistance + basis + `preliminary`/`presumed` flag.
- **Gates/triggers:** *Code Pack silent* (D, AU rock) → library method for approval. *Design-critical
  threshold* (D) for defect-controlled or karst-prone rock (breaks the presumed-bearing assumptions).

> **Method Library → [`../method-library.md` §2.7(d)](../method-library.md#27d-rock-foundations--bearing--rock-socket-shaft-ch-22)**
> (rock bearing-from-RQD + socket τ, already transcribed) and EN 1997-1 Annex G groups. Rock-socket
> shaft resistance belongs to Stage 6 (piles), not here.

**🔒 Gate 5.4.** Engineer reviews the rock-bearing basis and approves.

---

## 5.5 — SLS settlement & tolerable movements *(deep treatment)*

Both packs mandate the **requirement** but **no method** (EN 1997-1 §6.6 / AS 5100.3 §10.3.5), so the
settlement methods are **pack-shared** and engineer-selected — the fork here is only in the *limit
values* the movement is checked against. SLS uses **serviceability** actions with partial factors = 1
and quasi-permanent ψ₂ combinations.

**The three settlement components** (EN 1997-1 §6.6.2 / JRC §3.4): `s_0` immediate + `s_1`
consolidation + `s_2` creep. Which methods apply depends on the soil + data:

| Scenario | Method | Source |
|---|---|---|
| stiffness E/M available (lab / CPT / DMT) | elasticity / adjusted-elasticity `s = qB·Σ[(1−ν²)/E · I]`, `I = μ₀·μ₁` | Method Library §5.5b (EC7 Annex F.2, JRC μ₀/μ₁ charts) |
| consolidating clay | oedometric `S_ed = ΣΔH_i` with `C_c/C_s`, Boussinesq/Newmark stress increments | §5.5c (EC7 Annex F.4) |
| **granular, SPT-N only** | Meyerhof 1965 `ρ = f(q, N, B)` | §2.7(b) Look Tbl 21.8 *(non-code fallback)* |
| granular, indirect (size from allowable) | Terzaghi–Peck allowable-bearing chart (settlement ≤ 25 mm built-in) | §5.4 / §2.7(a) |

### 5.5.1 — Filter to applicable settlement methods ←CP/library
- **In:** the founding units' stiffness/consolidation parameters + drainage + data available.
- **Does:** select the method(s) per the table above; set the default; keep the SPT-N Meyerhof route
  as the fallback where stiffness wasn't derived. Decide whether **immediate + consolidation** both
  apply (partially/fully saturated fine soil) or immediate alone (granular).
- **Out:** the settlement-method set + component list (`s_0`/`s_1`/`s_2`).
- **Gates/triggers:** *Missing/insufficient data* (D) if no stiffness/consolidation parameter exists →
  SPT-N fallback with a low-reliability flag. *Code-mandated calc* (D): EN 1997-1 §6.6.2 — soft clay
  **always** calc; bearing/loading ratio < 3 (calc) or < 2 (non-linear stiffness) forces the deeper
  method.

### 5.5.2 — Compute total & differential settlement
- **In:** the method set + SLS actions + the ground model (layer stiffnesses, groundwater).
- **Does:**
  1. Run the **settlement engine** — layer-sum elasticity (with the `μ₀·μ₁` influence factors) and/or
     oedometric consolidation; sum components.
  2. **Depth-of-influence choice is decisive** (JRC §A.3.5 showed 3,3 cm vs 24,3 cm for the same case):
     integrate to where Δσ < ~20 % σ′_v0 (EN 1997-1 §6.6.2(6)) — or shallower only with an explicit
     over-consolidation/cementation justification.
  3. Derive **differential** settlement / angular distortion (β) from the total-settlement field
     (empirical δ–w_max correlation, JRC Fig.3.3.7, or the structural layout).
- **Out:** total settlement, differential settlement, angular distortion / tilt per footing (or across
  the footing group).
- **Gates/triggers:** *Assumption required* (D) — the **depth-of-influence cutoff** must be logged
  (design-critical, as it can swing the result by ×5). *Low confidence* (A→D) for SPT-N-fallback
  settlements affecting a governing SLS check.

### 5.5.3 — Compare against tolerable-movement limits
- **In:** computed movements + the limiting-deformation set.
- **Does:** compare total settlement, differential settlement, angular distortion and tilt against the
  limits — **EC/JRC Table 3.4.1** (Annex-H aligned) as the pack default (Method Library §5.5a),
  cross-checked with the non-code Look Tbl 23.6–23.8 (Wahls δ/L, Skempton–Macdonald, Boscardin–Cording)
  in §2.7(f); apply the structure-specific SLS values from the structural/bridge code where tighter.
- **Out:** SLS pass/fail per criterion + the governing SLS limit.
- **Gates/triggers:** *Design-critical threshold* (D) if any SLS limit is exceeded → 5.6 iterate.
  *Multiple routes* (A) where EC and non-code limit tables disagree on the acceptable value.

> **Method Library → [`../method-library.md` §5.5](../method-library.md#55--sls-settlement--tolerable-movements).**
> Annex-H limiting-deformation table (5.5a), elasticity settlement with μ₀/μ₁ charts (5.5b),
> oedometric consolidation (5.5c), and the pack/non-code notes (5.5d) with the Meyerhof SPT-N
> settlement fallback + Skempton–Bjerrum consolidation correction.

**🔒 Gate 5.5.** Engineer reviews the settlement method, the depth-of-influence assumption, computed
movements, and the tolerable-movement comparison; approves the SLS basis.

---

## 5.6 — Geometry iteration & optimization

### 5.6.1 — Re-size on fail or over-conservatism
- **In:** the ULS (5.2/5.3) and SLS (5.5) results + utilizations.
- **Does:** if any check **fails**, or utilization is markedly **low** (over-sized), propose revised
  geometry (`B`, `L`, `D`) and **re-run 5.2–5.5**. Use the Terzaghi–Peck insight — for large `B` the
  governing limit state shifts from bearing (ULS) to settlement (SLS), so grow `B` only until the
  *binding* check is satisfied, not past it.
- **Out:** a converged trial geometry with all checks passing and the governing check identified.
- **Gates/triggers:** *Multiple routes* (D) where several geometries satisfy all checks (cost vs
  robustness trade-off). *Design-critical threshold* (D) when the governing check is settlement on a
  compressible unit.

### 5.6.2 — Finalize geometry
- **In:** the converged geometry + all check records.
- **Does:** present the final geometry, the **governing limit state**, utilizations, and the
  safety-format basis (EC: DA combination; AU: approved φg per check) for sign-off.
- **Out:** the approved per-footing design.
- **Gates/triggers:** none specific (feeds the step gate).

**🔒 Gate 5.6 — engineer owns the final geometry.** The engineer reviews the converged design, the
governing case, and the safety basis; approves the footing design.

---

## Site-wide

## 5.7 — Package / hand-off

Closes the design module. Over already-approved per-footing designs — so **no new step gate**:
everything was signed off at Gates 5.2–5.6; the package feeds the report (Stage 7).

### 5.7.1 — Compile per-footing summaries; zone across the site
- **In:** every approved footing design (5.6) + the site zoning (units from Stage 3).
- **Does:** assemble a design schedule — geometry, governing check, safety-format basis, utilization
  per footing; **zone** where footings share a unit + load band so the output reads as a coherent set,
  not isolated calcs.
- **Out:** the footing design schedule + zoned summary.
- **Gates/triggers:** *Inconsistency/conflict* (D) if footings on the same unit resolve to
  inconsistent bases (e.g. different φg for equivalent evidence) — reconcile before packaging.

### 5.7.2 — Compile design-stage constraints & risks
- **In:** design-critical features (Stage 2.5.5), the overall-stability flags (5.3.2), special
  materials (2.2.5), groundwater (2.5.3).
- **Does:** compile the **design-stage** constraints/risks — construction sequencing, dewatering /
  base heave / running sand, adjacent-structure interaction, scour, seasonal/reactive-clay effects —
  distinct from the interpretation-stage register.
- **Out:** the design constraints/risks list.
- **Gates/triggers:** *Design-critical threshold* (D) per construction-critical item.

### 5.7.3 — Stage notices-register entries
- **In:** every decision across 5.0–5.6.
- **Does:** stage the **Notices register** entries (D27, `../03` → Notices register) — method choices
  where ≥2 routes existed, **AU φg values + rationale** / **EC governing DA combination**, overrides,
  assumptions (drainage, depth-of-influence, passive-resistance inclusion), out-of-code methods (AU
  rock), design-critical items.
- **Out:** staged notices entries for the report.
- **Gates/triggers:** a completeness check surfaces any unresolved blocking item.

### 5.7.4 — Hand off
- **In:** the footing schedule + constraints + notices + full audit trail.
- **Does:** package for the **report (Stage 7)**; flag the **structural footing design** interface
  (reinforcement, punching-shear of the footing itself — AS 5100.5/.6 or EN 1992; out of geotechnical
  scope); version/freeze the design against the ground-model version (upstream change → re-validate).
- **Out:** the Stage 5 hand-off package.
- **Gates/triggers:** *Missing/insufficient data* (B) if a report/interface contract is unmet.

## Status — complete
- **5.0–5.7 task procedures drafted** (In/Does/Out/Gates), with **5.2 (bearing + safety-format fork)**
  and **5.5 (settlement)** given the deeper treatment. The safety-format fork (EC inherited-upstream
  vs AU proposed-and-gated-per-check) is surfaced as the defining feature of the stage.

**Stage 5 shallow foundation now has all three parts:** A (stage doc) ✓ · B (this procedures doc) ✓ ·
C (Method Library §5, both packs + non-code) ✓. Next planned work (per `../06-decision-log.md`): apply
the same three-part treatment to **axial pile** (Stage 6) — EN 1997-1 §7 + AS 2159 + AS 5100.3, reusing
JRC 2013 Ch 8 / Annex A.8 worked pile examples.
