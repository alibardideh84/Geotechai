# Stage 2 — Interpretation: task procedures

The **operational "how"** for every task in the 2.0–2.8 breakdown of
[`02-interpretation.md`](02-interpretation.md). That document is the architecture and
reference map (the *what* and *which clause*); this one specifies, per task, the
procedure the system runs.

## Per-task template
Each numbered task is specified as:
- **In** — what it consumes (upstream task output · input-contract upload · Code Pack).
- **Does** — the AI + engine logic, in order. The **engine** does deterministic
  arithmetic/normalization; the **AI** scopes, selects, explains, and drafts.
- **Out** — what it produces and hands to the next task.
- **Gates/triggers** — exception triggers (see
  [`../03-workflow-and-human-ai-model.md`](../03-workflow-and-human-ai-model.md) → Gate
  model) that can fire *mid-task*. Each step ends with its 🔒 **step gate**.

**Legend:** ←CP = supplied/parameterized by the selected Code Pack · 🔒 = step gate.
Exception severities: **B** blocking · **D** decision · **A** advisory.

---

## 2.0 — Stage entry & data scoping *(runs once)*

### 2.0.1 — Detect entry point
- **In:** the project session (Stage 0 code selection + Stage 1 dataset if present), or a
  direct upload when the engineer jumps in at Stage 2 (input contract: digitized test data).
- **Does:**
  1. Determine the entry mode — *fresh from Stage 1* vs *jump-in*. (Per D15, flexible entry.)
  2. For jump-in, read the input contract for Stage 2 and check the upload satisfies it
     (digitized per-location test data with depths, datums, test types).
  3. Resolve the active Code Pack (from Stage 0) so all downstream ←CP hooks bind.
- **Out:** an entry-mode flag, the bound Code Pack, and a manifest of what was supplied vs
  what must still be requested.
- **Gates/triggers:** *Missing/insufficient data* (B) if the input contract is unmet (e.g.
  no datum, no test-type tags). *Assumption required* (D) if Stage 0 was skipped and the
  Code Pack must be inferred — surface the inferred pack for confirmation.

### 2.0.2 — List data needed; check presence; ingest missing; extract & map
- **In:** the entry manifest from 2.0.1; any uploaded files (any format — PDF logs, AGS,
  CPT exports, lab CSVs, scans).
- **Does:**
  1. From the Code Pack + selected design modules, derive the **data-needs list** (which
     tests/parameters each downstream step will want).
  2. Diff needs vs present; for each gap, choose: **ask** the engineer, **derive** later, or
     **estimate** with a flag (per D13).
  3. Ingest uploads: extract structured records (test, depth, value, units, method), map
     vendor/lab field names to the **core raw-data model**, and preserve provenance (file,
     page/row).
- **Out:** a structured, unit-normalized raw dataset keyed by location × depth × test, plus
  a gap register (ask / derive / estimate).
- **Gates/triggers:** *Error/validation failure* (B) on corrupt or unparseable inputs.
  *Missing/insufficient data* (B) for any gap a downstream step hard-requires. *Assumption
  required* (D) where a field had to be inferred during mapping (e.g. assumed units).

### 2.0.3 — Register the investigation
- **In:** the structured dataset from 2.0.2.
- **Does:**
  1. Build the **investigation register**: every location with coordinates, ground/collar
     level, test types present, depth range, and dates.
  2. Normalize coordinates/levels to a single project datum (record the source datum and any
     transform applied).
  3. Sanity-check the layout (duplicate IDs, coincident coordinates, impossible levels).
- **Out:** the investigation register (the spatial backbone reused by 2.4–2.5) and a
  per-location index into the raw dataset.
- **Gates/triggers:** *Inconsistency/conflict* (D) on duplicate IDs or coordinate/level
  clashes. *Assumption required* (D) if a collar level or datum had to be assumed.

> **Handbook additions (Look 2007) → [`../method-library.md` §2.0](../method-library.md#20--stage-entry--data-scoping).**
> Adequacy criteria (extent/spacing/depth Tbl 1.8; quality Tbl 1.14; GC Tbl 1.6; risk Tbl 1.10)
> feed the **dataset-sufficiency check in 2.0.2** and the *missing/insufficient-data* trigger;
> sample-disturbance (Tbl 1.12) and specimen-size (Tbl 1.13) flags are **carried forward as
> reliability caveats** into 2.3 and the notices register. *(Non-code; engineer decides — see the
> selection rule in the library header.)*

**🔒 Gate 2.0 — confirm dataset & scope.** The engineer reviews the investigation register,
the data-needs/gap list, and the bound Code Pack, then approves the dataset and scope before
the per-location loop runs.

---

## Per-location loop — runs independently at every borehole / CPT / test

> Tasks 2.1–2.3 execute once per sounding. Outputs are per-location and stay separate until
> the site-wide steps (2.4+) combine them.

## 2.1 — Correct & normalize

### 2.1.1 — Identify test types & depths
- **In:** one location's raw records from 2.0.
- **Does:** enumerate the test types present (SPT, CPT/CPTu, DMT, PMT, field vane, lab
  samples…), their depth coverage, sampling interval, and any depth gaps.
- **Out:** a per-location test inventory with depth coverage per test.
- **Gates/triggers:** *Missing/insufficient data* (A) if coverage is sparse relative to what
  later derivation needs (advisory here; may escalate at 2.3).

### 2.1.2 — Validate raw data ←CP
- **In:** the per-location raw records + test inventory.
- **Does:**
  1. Run validity checks per test type: units, monotonic/continuous depth, physical ranges,
     gaps, spikes/dropouts (e.g. CPT q_c sign/zero-offset, SPT N out of range, vane torque
     dropouts). Range envelopes are ←CP / Method-Library metadata.
  2. Flag each suspect reading with a reason and a proposed treatment (drop, clip, interpolate,
     keep-with-note).
- **Out:** a cleaned raw profile with a per-reading validity flag and an exceptions list.
- **Gates/triggers:** *Error/validation failure* (B) for out-of-range/corrupt readings the
  engineer must adjudicate. *Low confidence* (A) for noisy intervals carried forward.

### 2.1.3 — Apply corrections/normalizations ←CP / library
- **In:** the cleaned raw profile.
- **Does:** run the **deterministic correction engines** the Code Pack sanctions for each
  test, e.g.:
  - SPT → N₆₀ (energy/rod/borehole) and (N₁)₆₀ (overburden) — EC7 Annex F / Skempton 1986. ←CP
  - CPT → q_t (pore-pressure correction), F_r, and normalized Q_t/F_r — Lunne et al 1997. ←CP
  - DMT → I_D, K_D, E_D — Marchetti 1980. ←CP
  - Vane → peak/remoulded, sensitivity. ←CP
  Each correction records its formula, inputs used, and source clause.
- **Out:** corrected/normalized per-location profiles, each value clause-traceable.
- **Gates/triggers:** *Missing/insufficient data* (D) when a correction input is absent (e.g.
  no hammer-energy ratio → AI offers a Code-Pack default to approve). *Multiple routes* (D)
  if the pack sanctions >1 normalization and the choice is consequential. *Code Pack silent*
  (D) → propose a library normalization flagged *outside selected standard*.

### 2.1.4 — Reconcile depth datum & water level
- **In:** corrected profiles + the investigation register (collar levels, datum).
- **Does:**
  1. Convert all depths to the common project datum (depth-below-ground ↔ reduced level).
  2. Place observed groundwater / piezometric readings (standpipe, CPTu u₂, drilling
     observations) on the same datum; note date/time and whether equilibrium.
  3. Reconcile multiple water observations at one location into a per-location water note.
- **Out:** datum-consistent profiles + a per-location groundwater observation.
- **Gates/triggers:** *Inconsistency/conflict* (D) when water observations disagree (e.g.
  drilling level vs CPTu u₂). *Assumption required* (D) if equilibrium water level is assumed.

### 2.1.5 — Present corrected profiles
- **In:** all of the above for the location.
- **Does:** render the corrected/normalized profiles vs depth (raw vs corrected overlay,
  flags visible, datum and water level shown), with a short narrative of what was corrected
  and why.
- **Out:** the review-ready corrected-profile view for this location.
- **Gates/triggers:** none specific (feeds the step gate).

> **Handbook additions (Look 2007) → [`../method-library.md` §2.1](../method-library.md#21--correct--normalize).**
> Concrete numeric factors the engineer can apply/compare against the ←CP normalizations:
> SPT C_N (Skempton, with fine/coarse values) + the full C_ER breakdown (hammer-by-country, rod,
> sampler, borehole) + the silty-sand dilatancy correction; CPT q_T/a_N, B_q, t₅₀; DMT/PMT index
> formulas; vane **Bjerrum μ vs PI** (*resolves the ◇ open item*); DCP. Plus 2.1.2 validity flags
> (errors Tbl 5.1; SPT RW/HW/HB, N>60, increment check). *(Non-code; engineer selects/compares.)*

**🔒 Gate 2.1.** Engineer reviews corrected profiles, correction choices, flagged/edited
readings, and the water observation; approves before classification.

---

## 2.2 — Classify & provisional layers

### 2.2.1 — Classify samples/intervals ←CP
- **In:** corrected profiles + lab index/description data for the location.
- **Does:** apply the **pack-specific classification system** to each sampled interval —
  EN ISO 14688/14689 (EC pack) or AS 1726 §6.1/§6.2 (AU pack) — using index properties
  (PSD, Atterberg, moisture) and descriptions. (Classification is *not* shared — see the
  Code-Pack split, divergence #1.)
- **Out:** a classified interval log (material + class + basis) per sampled depth.
- **Gates/triggers:** *Multiple routes* (A) at borderline classes (e.g. coarse/fine
  boundary, which differs by pack). *Missing/insufficient data* (A) where index data is
  absent and class rests on description only.

### 2.2.2 — Behaviour-type from in-situ tests to fill gaps
- **In:** normalized in-situ data (CPT Q_t–F_r, DMT I_D) over unsampled depths.
- **Does:** infer **soil behaviour type** between/around samples (e.g. CPT SBT via Lunne et
  al/Robertson) to fill log gaps; tag each as inferred (lower weight than lab class).
- **Out:** a continuous behaviour-type profile complementing the sampled log.
- **Gates/triggers:** *Inconsistency/conflict* (A→D) when behaviour-type contradicts the
  adjacent sample classification (escalates if it would move a layer boundary).

### 2.2.3 — Provisional layer boundaries (single-location)
- **In:** the classified log + behaviour-type profile.
- **Does:** segment the location into provisional layers where class/behaviour/parameters
  change; pick boundary depths; record the evidence for each boundary.
- **Out:** a provisional single-location stratigraphy (layers with depth ranges + basis).
- **Gates/triggers:** *Multiple routes* (D) where boundary placement is ambiguous and
  design-relevant.

### 2.2.4 — Reconcile log vs test-based classification
- **In:** driller's/field log vs the test-/lab-based classification.
- **Does:** compare the two; flag mismatches (e.g. log "clay" vs CPT sand-like behaviour);
  propose which governs and why.
- **Out:** a reconciled classification with mismatch notes.
- **Gates/triggers:** *Inconsistency/conflict* (D) on material mismatches the engineer must
  resolve.

### 2.2.5 — Tag special materials ←CP
- **In:** the reconciled classification + index/description cues.
- **Does:** flag fill, organic/peat, sensitive/quick, expansive, collapsible, dispersive, and
  cemented materials using pack-specific cues; attach the handling implication (e.g.
  "expansive → reactive-site checks downstream").
- **Out:** special-material tags on the relevant layers/intervals.
- **Gates/triggers:** *Design-critical threshold* (D) when a special material is flagged
  (forces engineer attention). *Low confidence* (A) where the tag rests on weak evidence.

> **Handbook additions (Look 2007) → [`../method-library.md` §2.2](../method-library.md#22--classify--provisional-layers).**
> Non-code cross-checks for **2.2.1** (USCS symbols, gradings C_u/C_c, plasticity/Atterberg, origin
> & residual-soil-by-parent-rock); **in-situ behaviour-type charts for 2.2.2** (CPT Tbl 5.12/5.13,
> DMT I_D Tbl 5.17) to fill gaps between samples; **2.2.5** structure/special-material flags
> (fissured clay ≈⅔ strength → carries to 2.3). Rock: weathering/RQD/intact-strength/defect
> classification + **rock-mass RMR & Q** systems. *(Pack classification governs; these complement.)*

**🔒 Gate 2.2.** Engineer reviews the provisional stratigraphy, classification basis,
reconciled mismatches, and special-material tags; approves before parameter derivation.

---

## 2.3 — Derive parameters (scenario-indexed)

> The core of the Method Library at work. Derivation = a function of
> **{ parameter × soil type × drainage × data available }** (D24), evaluated per layer.

### 2.3.1 — Determine scenario per parameter × layer
- **In:** the approved provisional stratigraphy + corrected test profiles + the parameter
  needs list (from 2.0.2, driven by the design modules).
- **Does:** for each (layer × required parameter), assemble the scenario key — soil type,
  drainage condition (drained/undrained), and which test data actually exist at that depth.
- **Out:** a scenario key per (layer × parameter).
- **Gates/triggers:** *Assumption required* (D) when drainage condition must be assumed.

### 2.3.2 — Filter library to applicable methods ←CP
- **In:** the scenario keys.
- **Does:**
  1. Query the Method Library for methods whose applicability metadata matches the scenario.
  2. Set the **Code-Pack default** method; keep the rest as reachable alternatives.
  3. If the **Code Pack is silent** (no sanctioned method for the scenario), surface an
     applicable method from the wider library (other code / peer-reviewed research), flagged
     *outside selected standard (from [reference])*, for approval. (D26.)
- **Out:** a per-scenario candidate-method set with one default marked.
- **Gates/triggers:** *Multiple routes* (D) when ≥2 consequential methods apply. *Code Pack
  silent* (D, out-of-code) per above. *Out-of-applicability* (B) if the only candidate sits
  outside its valid conditions.

### 2.3.3 — Compute default + alternatives; show value, scatter, reliability, source
- **In:** the candidate-method sets + corrected data.
- **Does:** run the **derivation engines** for the default and alternatives at each depth;
  produce value(s) vs depth, inter-method scatter, a reliability flag, and the source clause
  per method.
- **Out:** per-parameter derived profiles with scatter band + reliability + provenance.
- **Gates/triggers:** *Methods disagree materially* (D) when scatter exceeds threshold.
  *Low confidence* (A→D) when a reliability flag affects a governing value.

### 2.3.4 — Co-present multi-test estimates of the same parameter
- **In:** derived estimates of the same parameter from different tests (e.g. s_u from CPTu,
  vane, lab).
- **Does:** overlay the independent estimates on one depth axis; quantify agreement; propose
  a provisional preferred estimate (or a reconciliation note) per depth.
- **Out:** a consolidated per-parameter view across tests, per location.
- **Gates/triggers:** *Inconsistency/conflict* (D) when independent tests disagree beyond
  threshold.

### 2.3.5 — Apply no-data fallbacks; surface "not recommended"
- **In:** scenarios where the preferred data is absent (e.g. clay s_u, no lab).
- **Does:** apply the library's **fallback chain** (e.g. s_u ≈ f₁·N₆₀ via Stroud) with an
  explicit **low-reliability flag**; and where a route is encoded as *not recommended* for the
  scenario (e.g. clay φ′ from SPT), surface that rather than silently using it.
- **Out:** fallback-derived values, each flagged; explicit "not recommended" notices where
  applicable.
- **Gates/triggers:** *Low confidence* (D) — every no-data fallback is logged for the notices
  register. *Out-of-applicability* (B) if a not-recommended route would otherwise be used.

### 2.3.6 — Assemble per-location parameter profiles
- **In:** all derived/consolidated parameters for the location.
- **Does:** compile the per-location parameter profiles (parameter × depth, with chosen
  method, scatter, reliability, provenance) ready for site-wide combination.
- **Out:** the location's complete derived-parameter profile set.
- **Gates/triggers:** none specific (feeds the step gate).

> **Handbook additions (Look 2007) → [`../method-library.md` §2.3](../method-library.md#23--derive-parameters-scenario-indexed).**
> The scenario-indexed numeric methods that 2.3.2–2.3.5 filter and run. **Part A — strength:**
> s_u (clay) from consistency/PP/SPT/CPTu/vane/DCP/effective-overburden with N_k & reliability;
> φ′ (sand) from SPT/(N₀)₆₀-D_r/CPT/DMT/DCP + φ_crit/φ_peak = 30+A+B(+C); clay c′/φ′ typicals
> (clay φ′-from-SPT marked **NR**); D_r; intact rock UCS (I_s(50), Schmidt). **Part B:** unit weight;
> stiffness E/M/m_v (CPT, SPT, Cu+PI, OCR+PI, Poisson, long/short β); consolidation c_v/OCR/P_c;
> permeability (Hazen, classification, fines flags); rock mass strength (RQD) & joint friction.
> *(Non-code; engineer selects/compares.)*

**🔒 Gate 2.3.** Engineer reviews derived parameters, picks/confirms the correlation per
parameter (a must-stay-human choice), accepts fallbacks/flags, and approves the per-location
profiles. *(This closes the per-location loop; 2.4+ go site-wide.)*

---

## Site-wide steps — combine all locations

> Tasks 2.4+ take the per-location profiles from 2.3 and reason across the whole site.

## 2.4 — Consistency & cross-location reconciliation *(deep treatment)*

The consistency engine is core IP (D17) and the **least code-defined** step — codes require
the activity (EN 1997-2 §6.3 "evaluation of geotechnical information"; AS 1726 §5.1.1
iterative + §5.6.2(d) regional & local geology required) but prescribe little *method*. So the
**logic is agnostic core**; only the *priors framework* and the *obligation to consider
regional/local geology* are ←CP.

**The central problem it must get right:** a disagreement between two numbers is not
automatically an error. It is one of four things, and the correct response differs for each —
so the engine's job is to **classify before it reconciles**:

| Conflict nature | What it means | Correct response |
|---|---|---|
| **Measurement / process error** | datum, units, equipment, transcription | correct or exclude; re-run upstream |
| **Method-reliability gap** | a low-reliability route disagrees with a strong one | prefer the higher-reliability derivation; down-weight the fallback |
| **Genuine ground variability** | the site really does change laterally/vertically | **do not average it away** — preserve it as a zoning driver (2.5) and possible local-governing value (2.6) |
| **Prior mismatch** | new data departs from regional/prior expectation | weigh quality & recency; site-specific data usually governs, but flag the departure |

> **Status note (open item (c)) — CONFIRMED (founder 2026-06-29, D30):** the statistical machinery
> below — robust estimators (median/MAD), distance-weighting, lognormal distribution, optional
> variogram/Bayesian updating — is the **accepted default**, with the COV backbone from the Method
> Library §2.4 (Look 2007 / Kulhawy / Poon & Kulhawy / Duncan & Wright). Thresholds remain
> engineer-configurable; default conservative (surface more rather than less, per D25).

### 2.4.1 — Spatially register all locations; build layout
- **In:** the investigation register (2.0.3) + per-location parameter profiles (2.3.6).
- **Does:**
  1. Place every location in plan (and on candidate section lines); compute inter-location
     distances.
  2. Group locations into **comparison neighbourhoods** — co-located/near sets (within a
     configurable radius) and broader clusters — defining who is compared with whom.
  3. Establish the spatial framework reused by 2.4.2–2.4.3 and by zoning in 2.5.
- **Out:** a spatial layout with neighbour/cluster relationships and candidate section lines.
- **Gates/triggers:** *Assumption required* (D) where coordinates/levels were estimated.
  *Inconsistency/conflict* (D) for a location that plots off-site or implausibly far from its ID.

### 2.4.2 — Cross-test consistency at near/co-located soundings
- **In:** the layout + per-location profiles, with each derivation's **reliability rank** (from
  the Method Library, set in 2.3).
- **Does:**
  1. Where ≥2 different test types are co-located/near, align their independent estimates of the
     **same parameter** onto common depth bins / provisional layers (from 2.2).
  2. Compute the discrepancy per bin and classify agreement against a tolerance band that is
     **widened by the lower reliability rank** of the pair (a fallback disagreeing with a lab
     value is expected, not a conflict).
  3. Emit a cross-test agreement view per parameter; mark genuine disagreements for 2.4.5.
- **Out:** a cross-test agreement matrix (parameter × neighbourhood) with flagged disagreements.
- **Gates/triggers:** *Methods disagree materially* (D) beyond the reliability-adjusted band.
  *Low confidence* (A) where only low-reliability routes are available to cross-check.

### 2.4.3 — Lateral-variability analysis; detect outlier/anomalous locations
- **In:** the layout + per-location profiles.
- **Does:**
  1. For each layer × parameter, characterize spatial behaviour — trends/gradients (deepening,
     thickening), and the natural spread — using robust statistics (median/MAD) so a single bad
     point doesn't distort the picture.
  2. Detect locations that deviate from their neighbours beyond expected variability, and
     **classify each candidate**: *likely error*, *genuine local feature* (weak lens, fill
     pocket, rockhead high), or *undetermined*.
  3. Distinguish a smooth lateral *trend* (carry into zoning) from a discrete *anomaly*
     (investigate or isolate).
- **Out:** per-layer lateral-variability characterization + an outlier/anomaly list with a
  provisional cause for each.
- **Gates/triggers:** *Inconsistency/conflict* (D) for outliers needing adjudication.
  *Design-critical threshold* (D) when an anomaly is a *weak* feature that could govern a limit
  state (forces review even if it is "just one point").

### 2.4.4 — Compare vs prior / nearby / regional / geology ←CP priors
- **In:** per-location profiles + the **priors framework** (←CP): regional geology, published
  local correlations, prior SI on the site, and any adjacent-project data the engineer supplies.
- **Does:**
  1. Compare the new stratigraphy and parameter ranges against the expected ranges from
     regional geology / prior data.
  2. Flag departures from regional expectation (e.g. strengths well outside the published band
     for the formation).
  3. ←CP obligation: EC pack frames this as EN 1997-2 §6.3 evaluation; AU pack enforces
     AS 1726 §5.6.2(d) (regional & local geology *required* in the factual report).
- **Out:** prior-comparison notes; departures flagged for 2.4.5.
- **Gates/triggers:** *Inconsistency/conflict* (D) on departures from priors. *Low confidence*
  (A) where priors are weak/absent. *Assumption required* (D) if a regional model had to be
  assumed.

### 2.4.5 — Flag conflicts; propose reconciliation with rationale *(the core)*
- **In:** every flagged disagreement from 2.4.2–2.4.4.
- **Does:** for each conflict —
  1. **Classify its nature** against the four-way table above.
  2. **Apply the reconciliation ladder** (below) to produce a proposed resolution.
  3. Attach **rationale + confidence + impact** (does it touch a governing parameter/zone?).
- **Reconciliation ladder (agnostic core):**
  1. **Error?** → identify source (datum, units, equipment, transcription); correct or exclude;
     re-run the affected upstream task. Log it.
  2. **Method-reliability gap?** → prefer the higher-reliability derivation per the library
     ranking; down-weight/retire the fallback; keep both visible.
  3. **Genuine ground variability?** → **preserve it** — do not average across it. Tag it as a
     zoning driver for 2.5 and, if it governs locally, a candidate local value for 2.6.
  4. **Prior mismatch?** → weigh data quality & recency; site-specific data normally governs the
     regional prior, but record the departure (and revisit the prior if many points depart).
  5. **Undetermined or design-critical?** → recommend further investigation; meanwhile carry the
     conservative interpretation forward with an explicit flag.
- **Out:** a **conflict register** — each conflict with nature, proposed reconciliation,
  rationale, confidence, impact.
- **Gates/triggers:** *Inconsistency/conflict* (D) — the engineer adjudicates each (this is the
  must-stay-human resolution called out in `02-interpretation.md` → Human gates (b)).
  *Design-critical threshold* (D). *Methods disagree* (D).

### 2.4.6 — Consistency summary
- **In:** the agreement matrix, anomaly list, prior-comparison notes, and conflict register.
- **Does:** compile a site-wide consistency summary — what agrees, what conflicted and how it is
  proposed to reconcile, residual uncertainty, and any further-investigation recommendations;
  stage the corresponding entries for the **Notices register** (D27).
- **Out:** the consistency summary section + staged notices entries.
- **Gates/triggers:** none specific (feeds the step gate).

> **Handbook additions (Look 2007) → [`../method-library.md` §2.4](../method-library.md#24--consistency--cross-location-reconciliation).**
> The empirical numbers behind this engine: **in-situ test COV ranking** (DMT/CPT 5–15% … SPT
> 15–45%) sets the reliability-adjusted tolerance band in 2.4.2; **inherent-property COV** (γ 7% …
> C_u 34%, C_c 37%) + site natural variation (5–15%) set the outlier envelope in 2.4.3; **lognormal
> + median/MAD** (normal gives negative strengths) set the statistic. **Closes open item (c)** —
> founder-confirmed default reconciliation methodology (2026-06-29, D30).

**🔒 Gate 2.4.** Engineer reviews the conflict register, **decides each reconciliation**, and
approves a site-wide-consistent dataset before ground-model assembly. Decisions and any
carried-forward flags pass to the notices register.

## 2.5 — Ground-model assembly *(deep treatment)*

The ground model is the **hub of the whole spine** — everything funnels into it and both design
modules read from it (`../03` spine). 2.5 converts the reconciled, per-location data from 2.4
into one spatial site model: **geotechnical units + geometry + groundwater + per-unit parameters
+ flagged critical features.** Agnostic core = the assembly logic and the *unit* concept; ←CP =
the classification system feeding unit definition and the deliverable framing (EC: EN 1997-1 §3 /
EN 1997-2 §6; AU: AS 1726 §5.2 geotechnical model, §5.6 factual report).

> **Key concept — the geotechnical unit.** A unit is an *engineering* grouping (similar behaviour
> + parameters + stratigraphic position), which may **merge or split** the provisional
> single-location layers from 2.2. The genuine variability *preserved* by 2.4 constrains this:
> never merge a weak lens into a strong unit just because it is thin.

### 2.5.1 — Correlate layers across locations → geotechnical units
- **In:** reconciled per-location stratigraphy + parameters (post-2.4), the spatial layout
  (2.4.1), and the conflict register's variability tags (2.4.5).
- **Does:**
  1. Match provisional layers across locations by classification + behaviour-type + parameter
     signature + stratigraphic position.
  2. Group correlated layers into **geotechnical units** — merge where consistent, **split where
     2.4 flagged genuine variability or a weak feature**.
  3. Record each unit's defining criteria and the locations evidencing it.
- **Out:** a site-wide set of geotechnical units (membership + defining criteria).
- **Gates/triggers:** *Multiple routes* (D) where cross-location correlation is ambiguous (is the
  sand at BH1 the same body as at BH3?). *Inconsistency/conflict* (D) where layers won't correlate
  cleanly. *Design-critical threshold* (D) when defining a weak/governing unit.

### 2.5.2 — Build sections / fence diagrams / surfaces (2D/3D); interpolate; show uncertainty
- **In:** geotechnical units + spatial layout + candidate section lines.
- **Does:**
  1. Build cross-sections / fence diagrams along section lines; interpolate unit boundaries
     between locations.
  2. Optionally build boundary **surfaces** (top-of-unit, rockhead) by interpolation across the
     site (2D/3D).
  3. **Show interpolation uncertainty** — wide bands where data is sparse — and **flag
     extrapolation** beyond the data envelope.
- **Out:** cross-sections + boundary surfaces with uncertainty bands and extrapolation flags.
- **Gates/triggers:** *Low confidence* (A→D) in sparsely-constrained zones that carry design.
  *Assumption required* (D) where boundaries are interpolated across large gaps. *Out-of-
  applicability* (A) flag on extrapolation beyond the data.

### 2.5.3 — Groundwater regime across the site
- **In:** per-location water observations (2.1.4), CPTu u₂/dissipation, standpipe/piezometer
  data, and regional hydrogeology (←CP priors).
- **Does:**
  1. Combine the per-location observations into site groundwater **surface(s)**; distinguish
     phreatic vs piezometric / perched / artesian, and equilibrium vs short-term.
  2. Establish the **design groundwater regime**, including seasonal/long-term high where
     available; flag assumptions where data is thin.
  3. Reconcile disagreeing water observations (reusing the 2.4 reconciliation logic).
- **Out:** a site groundwater model (surface(s) + regime + design-level assumptions).
- **Gates/triggers:** *Inconsistency/conflict* (D) on disagreeing observations. *Assumption
  required* (D) for the design water level. *Design-critical threshold* (D) — water level governs
  effective stress / buoyancy, so it always forces review.

### 2.5.4 — Assign each unit a representative parameter set
- **In:** per-location derived parameters (2.3) grouped by unit; the unit definitions.
- **Does:**
  1. Pool each unit's member-location values into a **unit population** per parameter (the bridge
     to 2.6).
  2. Assign a provisional **representative** profile/value per unit — constant or depth-trend.
     This is **descriptive** (central tendency + spread), **not** the cautious characteristic value
     (that judgment is 2.6).
  3. Support **multiple constitutive models** per unit (model-agnostic, D16) — the parameter set
     depends on which model(s) the downstream modules consume.
- **Out:** each unit carrying a representative parameter population + provisional central profile,
  per constitutive model.
- **Gates/triggers:** *Methods disagree* (A) carried from 2.3. *Low confidence* (A) for thin
  populations.

### 2.5.5 — Identify design-critical features
- **In:** units + sections + variability/anomaly tags (2.4) + special-material tags (2.2.5).
- **Does:** explicitly call out features that can **govern** design — weak lenses / soft bands,
  rockhead level & variability, voids / karst / old workings, variable or uncontrolled fill,
  perched water, expansive / collapsible / reactive zones — each with its design implication and
  *where* it bites.
- **Out:** a design-critical features register attached to the ground model.
- **Gates/triggers:** *Design-critical threshold* (D) per feature. *Out-of-applicability* (B) if a
  feature breaks a downstream method's assumptions (e.g. karst under a spread-footing method).

> **Handbook additions (Look 2007) → [`../method-library.md` §2.5](../method-library.md#25--ground-model-assembly).**
> Attributes to attach to units (assembly logic stays procedure/code-driven): **excavatability/
> rippability** (Franklin 1971; Weaver 1975 — seismic velocity/RQD/weathering → blast-required) and
> bulking factors → constructability features in 2.5.5; **terrain category & landslide
> susceptibility** (Tbl 15.6/15.7) → site-scale design-critical features; volume-sampled ratio →
> widen interpolation uncertainty in 2.5.2. *(Non-code; attributes, not the zoning method.)*

**🔒 Gate 2.5.** Engineer reviews units, sections/surfaces + uncertainty, the groundwater model,
representative parameters, and critical features; approves **the ground model** — the master input
feeding both design modules and the report.

---

## 2.6 — Characteristic / design value *(judgment gate, deep treatment)*

The **value basis** is the deepest Code-Pack split (divergence #2) and one of the three
must-stay-human gates: EC7 §2.4.5.2's "cautious estimate" is **judgment, not a formula**. Agnostic
core = (i) assemble the population per unit × parameter, (ii) **the engineer owns the final
value**. ←CP = the value basis itself — *what kind of value design wants*, and *how safety is then
applied*:

| | EC pack | AU pack |
|---|---|---|
| Value design wants | **characteristic value Xₖ** = cautious estimate of the value affecting the limit state (EN 1997-1 **§2.4.5.2**); statistical aid Schneider 1997 ◇ / national annex | **cautiously-assessed (ultimate) geotechnical strength** (no single "characteristic" clause) |
| Safety applied later | partial factors per **DA1/DA2/DA3** (Stages 4–6) | **risk-based φg** strength-reduction factor → design geotechnical strength (**AS 2159 §1.3.7** piles · **AS 5100.3 §7.3.5** footings) |

So the cautious value lands at a *different point in the chain* and is *factored differently* — but
the upstream population-assembly and the human-ownership rule are identical. Steps 2.6.1–2.6.4 are
agnostic; only the meaning of "cautious value" and the downstream safety hook are ←CP.

### 2.6.1 — Assemble the value population per unit × parameter
- **In:** the unit parameter populations from 2.5.4 + reliability/provenance metadata.
- **Does:**
  1. Assemble the full population of derived values per unit × parameter (across locations &
     tests), each tagged with reliability, test type, depth, location.
  2. Compute robust descriptive statistics (n, central tendency, spread, depth-trend), **weighted
     by reliability**; display the population and residual scatter.
  3. Flag **thin populations** (low n) where statistics are unreliable.
- **Out:** a per-unit × parameter value population + descriptive statistics.
- **Gates/triggers:** *Low confidence* (A→D) for low-n populations affecting a governing value.

### 2.6.2 — Choose the statistic per limit state & footprint ←CP value basis
- **In:** the populations + the design modules' limit states + footprint (or an assumption).
- **Does:**
  1. Determine whether the governing limit state is sensitive to a **local minimum** (pile tip in
     a thin bearing layer, weak lens under a strip) or to a **spatial average** over a large
     volume (raft / embankment averaging out variability).
  2. Select the statistic accordingly — **local-governing → cautious estimate of the low value**
     (low fractile); **volume-averaging → cautious estimate of the mean** (lower uncertainty,
     because averaging reduces variance). ←CP defines exactly what "cautious estimate" /
     "cautiously assessed strength" means in the active pack.
  3. Where footprint is unknown at interpretation time, **default to the local (more
     conservative) basis** and flag the assumption.
- **Out:** the chosen statistic + spatial basis per unit × parameter × use.
- **Gates/triggers:** *Assumption required* (D) when footprint/limit state must be assumed.
  *Multiple routes* (D) where both bases are plausible. *Design-critical threshold* (D).

### 2.6.3 — Propose a cautious value + rationale, range, sensitivity
- **In:** the population + chosen statistic + spatial basis.
- **Does:**
  1. Compute the proposed cautious value (EC: characteristic Xₖ via §2.4.5.2, statistical aid
     optional; AU: cautiously-assessed strength).
  2. Give **rationale** (how cautious and why), the plausible **range** (e.g. mean → low fractile),
     and a **sensitivity note** (how far the design result moves if the value shifts within range).
  3. Show provenance: which population, which statistic, which clause.
- **Out:** a proposed cautious value per unit × parameter, with rationale + range + sensitivity.
- **Gates/triggers:** *Design-critical threshold* (D) — this value governs. *Low confidence* (A→D).

### 2.6.4 — Per-unit / per-parameter overrides with justification
- **In:** the engineer's edits.
- **Does:** let the engineer **override** any value, recording the justification (experience,
  regional knowledge, deliberate conservatism, extra data); recompute downstream sensitivity; log
  the override.
- **Out:** the finalized characteristic / design-basis value set, each with provenance or override
  justification.
- **Gates/triggers:** *Assumption required* (D) to capture each override's justification.

> **Handbook additions (Look 2007) → [`../method-library.md` §2.6](../method-library.md#26--characteristic--design-value).**
> Concrete fractiles for the cautious estimate (Tbl 10.17): **ULS 1% (structure)/5% (road), SLS
> 5%/10%**, tighter at interfaces; "lowest value only if few samples" → the local-vs-averaged handle
> in 2.6.2. Critical caveat: a 5% fractile on a *normal* fit ≈ 10–30% on **lognormal** (and normal can
> go negative) — compute on the lognormal population with COV from §2.4. Builds on §2.4's backbone.

**🔒 Gate 2.6 — engineer owns the value.** A must-stay-human judgment gate (§2.4.5.2 cautious
estimate is judgment, not a formula). The engineer reviews each proposed value, basis, range, and
sensitivity, edits/approves, and the approved set becomes the **design-basis parameter set** carried
to Stages 4–6 and the report. Every choice feeds the notices register.

## 2.7 — Preliminary design recommendations

The bridge **interpretation → design** (D18), Geotechnical-Interpretive-Report style. It produces
**geometry-independent, design-ready profiles** from the ground model + design-basis values, using
the pack's sanctioned direct field-test correlations and safety format — explicitly flagged
**preliminary**. ←CP = which correlations are sanctioned + the safety format applied.

> **Guardrail — division of labour.** 2.7 gives capacity/parameter profiles *per unit/depth*,
> **independent of a specific foundation geometry or load**. The geometry-and-action-specific
> check is the **design stage** (Stage 5/6). The output is labelled *indicative*; it must not be
> mistaken for the final design.

### 2.7.1 — Design-ready profiles ←CP correlations + safety format
- **In:** the design-basis parameter set (2.6) + ground model (2.5) + groundwater (2.5.3) + the
  pack's design correlations and safety format.
- **Does:**
  1. Compute geometry-independent design profiles per unit/depth:
     - **unit skin friction fₛ vs depth** (pile shaft) and **base resistance q_b vs depth**;
     - **bearing** for typical shallow-footing types/widths;
     - **subgrade reaction** modulus; **earth-pressure** parameters (K₀/Kₐ/K_p).
       *(MVP modules consume fₛ/q_b/bearing first; the rest round out the GIR output.)*
  2. Use ←CP sanctioned correlations — EC: EN 1997-2 §4.3.4/§4.4.4/§4.6.4/§4.10.4/§4.11 direct
     field-test design, piles NEN 6743 / Fascicule 62, settlement Schmertmann 1970 / Burland &
     Burbidge 1985; AU: fₛ/q_b per AS 2159, bearing per AS 5100.3.
  3. Apply the pack's **safety format** at the preliminary level, **clearly labelling** what is
     characteristic/ultimate vs factored (EC: characteristic + indicative partial-factored; AU:
     φg-reduced design geotechnical strength).
- **Out:** geometry-independent design-ready profiles per unit, each with method + clause +
  safety-format basis + *preliminary* flag.
- **Gates/triggers:** *Multiple routes* (D) where ≥2 correlations apply. *Methods disagree
  materially* (D) — expected for CPT pile methods (scatter is the norm). *Code Pack silent* (D,
  out-of-code). *Design-critical threshold* (D).

### 2.7.2 — Zone across the site with ranges
- **In:** the profiles + the site zoning (units/sections from 2.5).
- **Does:** present the profiles **zoned spatially** (per geotechnical zone/section), each as a
  **range** reflecting variability and uncertainty (not a single deterministic line), consistent
  with the local-governing vs volume-averaged basis chosen in 2.6.
- **Out:** zoned design-ready profiles with ranges.
- **Gates/triggers:** *Low confidence* (A) in poorly-constrained zones. *Design-critical
  threshold* (D) where a zone governs.

### 2.7.3 — Constraints, risks, further-investigation notes
- **In:** design-critical features (2.5.5), consistency residuals (2.4.6), thin-population /
  low-confidence flags, special-material tags (2.2.5).
- **Does:** compile geotechnical **constraints and risks** — settlement-sensitive units, negative
  skin friction potential, aggressive ground/chemistry, construction issues (base heave, running
  sand), liquefaction/seismic susceptibility flags — plus explicit **further-investigation**
  recommendations where data is insufficient.
- **Out:** a constraints / risks / recommendations list.
- **Gates/triggers:** *Design-critical threshold* (D). *Low confidence* → further-investigation
  recommendation (logged for the notices register).

### 2.7.4 — Compile interpretive output
- **In:** all of 2.7.
- **Does:** assemble the GIR-style interpretive output — narrative + zoned design profiles +
  constraints/risks — marked **preliminary/indicative** and pointing to the design stages for the
  geometry-and-action-specific check.
- **Out:** the preliminary-design-recommendations package.
- **Gates/triggers:** none specific (feeds the step gate).

> **Handbook additions (Look 2007) → [`../method-library.md` §2.7](../method-library.md#27--preliminary-design-recommendations).**
> The design-ready numbers for 2.7.1: **bearing** (Terzaghi eqn + Vesic/Hansen N_c/N_q/N_γ, Meyerhof
> N-charts, FoS); **settlement** (Meyerhof ρ=q/N, Burland ρ_u/ρ_T, Skempton-Bjerrum μ); **pile fₛ/q_b**
> (Poulos α & k_s·tanδ, Meyerhof N-methods, q_b cap 10 MPa); **rock** bearing-from-RQD + rock-socket
> τ=ψ√(q_u·P_a); **earth pressures** K₀/Kₐ/K_p (Caquot-Kerisel images); **tolerable movements**
> (Wahls δ/L, Skempton-Macdonald limits). *(Non-code; indicative — geometry/actions are the design stage.)*

**🔒 Gate 2.7.** Engineer reviews the design-ready profiles, zoning, correlation choices,
safety-format basis, and constraints/risks; approves the preliminary design recommendations. *(This
is the Stage-2 sign-off in the spine: "approved derived parameters + recommendations".)*

---

## 2.8 — Package / hand-off

Closes interpretation. Two mechanical jobs over already-approved material — so **no new step
gate**: everything in 2.8 was signed off at Gates 2.5–2.7; the package is then consumed by the
design stages and the final report sign-off (Stage 7).

### 2.8.1 — Assemble package → design stages & report
- **In:** the ground model (2.5), the design-basis value set (2.6), the recommendations (2.7), and
  the full **audit trail** (every gate approval, method choice, override, assumption — P5).
- **Does:**
  1. Package the deliverables to satisfy the **input contracts** of the downstream design stages
     (Stage 5 shallow foundation ← characteristic params + bearing profiles; Stage 6 axial pile ←
     fₛ/q_b profiles) — per D15, the same package works whether design runs in-app or is exported.
  2. Bundle the audit trail: method + inputs + assumptions + citations + sign-off per item.
  3. **Version/freeze** the ground model as the master input; later upstream changes trigger
     downstream re-validation.
- **Out:** the interpretation hand-off package (ground model + design-basis params +
  recommendations + audit trail), contract-shaped for design + report.
- **Gates/triggers:** *Missing/insufficient data* (B) if a downstream input contract is unmet
  (a parameter a design module requires was never derived) → loop back to 2.3/2.6.

### 2.8.2 — Compile the Notices register
- **In:** every notice staged across 2.0–2.7.
- **Does:** compile the structured **Notices register** (D27, `../03` → Notices register) — each
  entry: *item · location/context · trigger · options considered · decision · rationale · approver
  · date* — grouped for the end of the report: assumptions, low-confidence / no-lab fallbacks,
  method choices where ≥2 routes existed, conflicts resolved, missing-data estimates, out-of-
  applicability overrides, out-of-code methods, design-critical items, and further-investigation
  recommendations. Flag any still-open items.
- **Out:** the Notices register section for the report.
- **Gates/triggers:** a final completeness check surfaces any unresolved blocking item (normally
  none — gates resolve inline).

## Status — complete
- Installment 1 — per-location loop (2.0–2.3) ✓
- Installment 2 — 2.4 consistency engine (deep) ✓
- Installment 3 — 2.5 ground model + 2.6 characteristic value (deep) ✓
- Installment 4 — 2.7 preliminary design recommendations + 2.8 package / notices ✓

**All of 2.0–2.8 now have task-level procedures.** Next planned work (per `../06-decision-log.md`):
apply this same per-pack, In/Does/Out/Gates treatment to a **design module** (shallow foundation or
axial pile).
