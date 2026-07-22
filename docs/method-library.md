# Method Library (Method Registry) — core IP

The registry of **all valid methods, correlations, parameter ranges, and design-rule guidance**
per analysis, from **codes and non-code references**, tagged by source so the AI can reason about
them, not just execute them (architecture `02` P3, D7, D24). The interpretation procedure
([`stages/02-interpretation-procedures.md`](stages/02-interpretation-procedures.md)) **queries**
this file at each step; the per-pack code references live in
[`stages/02-interpretation.md`](stages/02-interpretation.md).

> **Scope now:** seeded for **Stage 2 interpretation (2.0–2.8)** from **Look 2007** (see Sources
> below). Organized **by procedure step → parameter/topic**. Other sources (codes, papers, more
> packs) and other stages drop into the same structure without changing the procedure.

## Sources
- **Look 2007** — Burt G. Look, *Handbook of Geotechnical Investigation and Design Tables*,
  Taylor & Francis/Balkema, 2007. **Non-code reference** (a data-table/correlation handbook). Look
  cites original authors per table (e.g. Kulhawy, Vaughan, Rowe, Robertson) — recorded in the
  *Source* column. File: `../Sources/Handbooks/Handbook+of+Geotechnical+Investigation&DesignTables 1`.
  - **Figures/garbled tables:** tables that don't extract cleanly from the txt are rendered from
    the PDF (PyMuPDF) and saved to `../Sources/Handbooks/figures/` as named PNGs (authoritative); the
    library links them and transcribes the decision-relevant content.
- **JRC 2013 (EC7 Worked Examples)** — A.J. Bond, B. Schuppener, G. Scarpelli & T.L.L. Orr,
  *Eurocode 7: Geotechnical Design — Worked examples* (workshop, Dublin 13–14 June 2013), JRC
  Report **EUR 26227 EN**, Publications Office of the EU, 2013. **Code-companion / official
  guidance** — this is the JRC/CEN implementation-support document for EN 1997-1, *not* a
  standalone non-code correlation set. **Tag: *(EC7 guidance)*.** Distinction that matters for the
  selection rule: unlike Look *(non-code)*, its methods **are** the Eurocode's own — it transcribes
  Annex D/F cleanly and works the DA1/DA2/DA3 safety format, so entries seeded from it carry the EC
  pack's safety basis directly (they are the code default, not a comparison alternative). File:
  `../Sources/Handbooks/2013_06_WS_GEO.pdf` (+ `.txt`). Chapter 3 = shallow foundations; Annex A.3 =
  worked strip-foundation calibration. Chart figures rendered to `../Sources/Handbooks/figures/` as
  `JRC2013_*.png`.
- **API RP 2A-WSD** — American Petroleum Institute, *Recommended Practice for Planning, Designing and
  Constructing Fixed Offshore Platforms — Working Stress Design*, **Section 6 (Foundation Design)**.
  **Industry recommended practice** — *the* offshore/marine pile standard. **Tag: *(API RP —
  offshore, WSD)*.** Two-part role for the selection rule: its **capacity methods** (α method for
  clay, `K·p₀·tanδ` + N_q for sand, t-z/Q-z curves) are **pack-agnostic** — they compute an ultimate
  shaft/base resistance that can feed **any** safety format (EC ξ→R_c;k, AS R_d,ug, or API's own),
  so they enter the library as **alternative methods** (High reliability — extensive offshore
  load-test calibration). But API's **safety format is Working Stress Design** (global factors of
  safety 1.5–2.0 by load condition) — a **third paradigm** distinct from EC7 partial factors (LRFD)
  and AS 2159 φg (LSD); it applies when **API is the governing standard** (offshore/marine — see the
  D38 special-structures discussion). File: `../Sources/Handbooks/API-RP-2A-WSD-Section6-Foundations.pdf`
  (+ `.txt`, an excerpt: §5.5, §6). Table 6.4.3-1 + the t-z figure rendered to
  `../Sources/Handbooks/figures/API-RP-2A_*.png`.
- **FHWA GEC-12** — U.S. FHWA / NHI, *Design and Construction of Driven Pile Foundations*, Vol. I,
  Publication **FHWA-NHI-16-009 (GEC No. 12)**, 2016. **National design manual** (US DOT) — Chapter 7
  consolidates the standard **static analysis + CPT pile methods** with **AASHTO LRFD resistance
  factors**. **Tag: *(FHWA/AASHTO — US LRFD)*.** Role for the selection rule: supplies the **CPT
  direct methods** (Eslami–Fellenius 1997; Nottingham–Schmertmann 1975) as **pack-agnostic capacity
  methods** (the fₛ/q_b computation), *and* documents a **fourth safety-format variant** — AASHTO's
  **method-keyed static resistance factor φstat** (e.g. Schmertmann 0,50, α-method 0,35, Nordlund
  0,45 — a *per-method calibrated* resistance factor, distinct from AS 2159's risk-derived φg). File:
  `../Sources/Handbooks/nhi16009_v1.pdf` (+ `.txt`, full). Ch-7 design curves/tables rendered to
  `../Sources/Handbooks/figures/FHWA-GEC12_*.png`.

## How the procedure uses this library — the selection rule
Per the founder's rule (extends D8 run-several-and-compare + D26 out-of-code):
1. **Code has a method AND the library has one** → **offer both**, optionally run both, **compare
   side-by-side for the engineer**, and **the engineer selects** (or keeps both with a note). Non-code
   methods are surfaced as **comparison alternatives, not just fallbacks**.
2. **Code is silent** → propose the applicable library method, flagged ***outside selected standard
   — [source]*** (D26), for engineer decision.
3. Every method carries its **reliability flag**; "not recommended" routes are surfaced as such,
   never used silently.

**Legend — reliability:** High · Medium · Low · **NR** = not recommended for this scenario.
**Tag:** non-code entries are marked *(non-code)* so the safety basis still comes from the active
Code Pack downstream.

---

## 2.0 — Stage entry & data scoping
*Procedure tasks 2.0.1–2.0.3.* These are **scoping aids and data-quality flags**, not parameter
correlations — they let the AI (a) judge whether the supplied investigation is **adequate** in
2.0.2 and flag gaps, and (b) attach **data-quality caveats** at intake that propagate to 2.3
reliability flags and the notices register.

### 2.0(a) Investigation adequacy — extent, spacing, depth
Use to check coverage of the supplied dataset and flag insufficiency / recommend further
investigation. *(Source: Look 2007 Table 1.8; guideline only.)*

| Development | Test spacing | Depth of investigation (guide) |
|---|---|---|
| Building | 20–50 m | 2B–4B (pads/strips) shallow; piles: 3 m or 3 pile-Ø below founding (rock: N\*>100, RQD>25%); rafts 1.5B |
| Bridge | each pier | 4B–5B shallow; 10 pile-Ø in competent strata; rock: ≥3 m coring, 3 pile-Ø below target (N\*>150, RQD>50%, ≥mod. weathered, ≥medium strength) |
| Embankment | 25–50 m (critical), else as roads | beyond base of compressible alluvium |
| Cut slope | 25–50 m (H>5 m); 50–100 m (H<5 m) | 5 m below toe or 3 m into bedrock below toe |
| Landslip | ≥3 BH/pits along critical section | below slide zone (~2× slope height if unknown) |
| Pavement/road | 50–500 m by class | 2 m below formation |
| Dam | 25–50 m | 2× dam height / 5 m below toe / 3 m into bedrock; to low-k zone |

Companion checks: **1.11 sample amount** — sample/test every **1.5 m or at strata change**;
undisturbed samples in clay, penetration tests in granular. **Table 1.14 quality** — a *good*
detailed investigation shows ≥40% of the quality factors (phasing, tests/hectare ≈10–20
buildings/bridges, safety/environmental plan, …); use to score investigation quality.

### 2.0(b) Geotechnical Category & relative risk → scoping emphasis
*(Source: Look 2007 Table 1.6 GC; Table 1.10 risk; cross-checks Stage 0 category.)*
- **GC1** small/simple, straightforward ground, non-seismic, SI ≈0.1–0.5% of cost — qualitative may suffice.
- **GC2** conventional, risk to neighbours, below water table, low seismicity — quantitative studies.
- **GC3** large/unusual, extreme risk, very permeable/specialist, high seismicity — two-stage investigation.
- **Relative risk ranking** (A least → F highest) by development type (offshore, tunnels, dams,
  nuclear high; sub-stations, road embankments lower) sets the **emphasis/extent** of data required.
- **Volume sampled** (Table 1.9, Kulhawy 1993): sampled/loaded volume ratio ≈10⁴–10⁶; earthworks
  sample more intensely — context for judging representativeness.

### 2.0(c) Data-quality flags carried forward (sampling effects)
Attach at intake; these become **reliability caveats** on derived strengths in 2.3 and notices entries.

| Flag | Condition | Effect / action | Source |
|---|---|---|---|
| Sample disturbance | soft clay, low PI | undrained strength **very large decrease** → measured sᵤ likely **unconservative-biased low**; flag | Look 2007 Tbl 1.12 (Vaughan 1993) *(non-code)* |
| | soft clay, high PI | large decrease | |
| | stiff clay, low PI | negligible | |
| | stiff clay, high PI | **large increase** → measured sᵤ may be **biased high**; flag | |
| Specimen size | fissured / fabric / sand-layered clay | small specimens invalidate cᵤ, c′, mᵥ, cᵥ — need 75–250 mm per fabric | Look 2007 Tbl 1.13 (Rowe 1972) *(non-code)* |

---

## 2.1 — Correct & normalize
*Procedure tasks 2.1.2 (validate) · 2.1.3 (corrections/normalization) · 2.1.4 (water).* These are
**concrete numeric correction factors and validity flags** that the engineer can apply/compare
against the pack's ←CP normalizations. All entries Look 2007 *(non-code)*; original authors noted.

### 2.1(a) Validation & suspect-reading flags → task 2.1.2
| Flag | Meaning / trigger | Source |
|---|---|---|
| Error taxonomy | inherent variability · sampling error · measurement error · statistical variation — each with a mitigation | Tbl 5.1 |
| `RW` / `HW` | rod-weight-only / hammer+rod-weight-only causes penetration (N<1) → very soft, suspect | Tbl 4.6 |
| `HB` | hammer bouncing (typically N\*>50) → refusal/hard | Tbl 4.6 |
| N>60 | likely cemented sand, gravel, cobbles, boulders or rock (not soil) | Tbl 4.6 |
| Incremental check | the three 150 mm increments over the 450 mm drive should be examined — a drop signals fall-in (false low) or a layer/strength change | Tbl 4.6 |

### 2.1(b) SPT corrections → task 2.1.3
**(N₀)₆₀ = C_N · C_ER · N** (normalized to σ′ᵥ=100 kPa and 60% energy).

**Overburden C_N** (granular only — *not* applied in clays), Skempton 1986 (Tbl 4.8):

| σ′ᵥ (kPa) | 0 | 25 | 50 | 100 | 200 | 300 | 400 |
|---|---|---|---|---|---|---|---|
| C_N fine sand | 2.0 | 1.6 | 1.3 | 1.0 | 0.7 | 0.5 | 0.5 |
| C_N coarse sand | 1.5 | 1.3 | 1.2 | 1.0 | 0.8 | 0.6 | 0.5 |

- Fine/silty sand **below water table**, dilatancy: for N>15 use **N′ = 15 + ½(N − 15)**.
- Borehole water balance required below WT (else blow-out → false low N).

**Energy/equipment/borehole C_ER = C_H·C_R·C_S·C_B**, Skempton 1986 / Tokimatsu & Seed 1987 (Tbl 4.9):

| Factor | Case → value |
|---|---|
| Hammer C_H | donut free-fall (Tombi, Japan) 1.3 · donut rope-pulley Japan 1.1 · safety rope-pulley USA 1.0 · donut trip (Europe/China/Australia) 1.0 · donut rope-pulley China 0.8 · donut rope-pulley USA 0.75 |
| Rod C_R | >10 m 1.0 · 6–10 m 0.95 · 4–6 m 0.85 · 3–4 m 0.75 |
| Sampler C_S | standard 1.0 · US sampler w/o liners 1.2 |
| Borehole C_B | 65–115 mm 1.0 · 150 mm 1.05 · 200 mm 1.15 (negligible in cohesive soils) |

### 2.1(c) CPT / CPTu corrections → task 2.1.3 (Tbl 4.10)
- **q_T = q_c + (1 − a_N)·u₂** (tip correction for pore pressure on shoulder); net-area ratio
  a_N ≈ 0.75–0.82 (10 cm² cones), 0.65–0.80 (15 cm² cones) — from manufacturer.
- **F_R = F_s/q_c** (friction ratio); **B_q = (u_d − u₀)/(q_T − P_o)**.
- Dissipation **t₅₀** → c_v (Look: more reliable than the oedometer for c_v). Electric vs mechanical
  cones interpreted differently.

### 2.1(d) DMT / PMT index computations → task 2.1.3 (Tbl 4.11, 4.12)
- DMT: **I_D = (p₁ − p_o)/(p_o − u₀)** · **E_D = 34.7·(p₁ − p_o)** · **K_D = (p_o − u₀)/σ′ᵥₒ**
  (p_o = corrected A-reading, p₁ = corrected B-reading).
- PMT: **E_PMT = 2(1+ν)·(V/ΔV)·ΔP** · lift-off P₀ = σ_ho · limit pressure P_L.

### 2.1(e) Vane shear → task 2.1.3 (Tbl 4.13, 4.14)
- **s_uv = 6·T_max /(7πD³)** for H/D=2; record **peak** and **remoulded** (10 revolutions) →
  sensitivity.
- **Bjerrum (1972) correction μ vs plasticity index** — *resolves the ◇ "Bjerrum μ" open item in
  `stages/02-interpretation.md`*: **s_uv(corr) = μ·s_uv**

  | PI (%) | <20 | 30 | 40 | 50 | 60 | 70 | 80 | ≥90 |
  |---|---|---|---|---|---|---|---|---|
  | μ | 1.0 | 0.9 | 0.85 | 0.75 | 0.70 | 0.70 | 0.65 | 0.65 |

### 2.1(f) DCP & in-situ test selection → tasks 2.1.1 / 2.1.3
- DCP read two ways: **blows/100 mm** (profiling) vs **mm/blow** (strength → CBR); Tbl 4.15.
- **Test-vs-application matrix — Tbl 4.5 (Bowles 1996).** This grid garbled in the txt, so it is
  saved as a rendered image (authoritative): [`../Sources/Handbooks/figures/Look2007_Table4.5_insitu-test-application_Bowles1996.png`](../Sources/Handbooks/figures/Look2007_Table4.5_insitu-test-application_Bowles1996.png).
  Code: **A** most applicable · **B** may be used · **C** least applicable, across 13 ground-interest
  columns (soil ID, profile, D_r, φ, s_u, u, OCR/K₀, E_s/G, m_v/C_c, c_v/c_h, k, stress-strain,
  liquefaction). **A-rated ("most applicable") uses** for the tests we use:

  | Test | Most-applicable (A) for |
  |---|---|
  | CPTu (electric piezocone) | soil ID, vertical profile, pore pressure u, stress history OCR/K₀, consolidation c_v/c_h, liquefaction |
  | Self-boring pressuremeter | D_r, φ, s_u, u, OCR/K₀, modulus, compressibility, consolidation, stress-strain, liquefaction (most versatile) |
  | DMT | vertical profile (B for s_u, OCR, modulus, D_r, liquefaction) |
  | Plate load / screw plate | modulus E_s/G (screw plate also compressibility) |
  | Seismic down-hole / cross-hole | small-strain modulus |
  | Field vane | undrained strength s_u |
  | SPT | liquefaction resistance (B for soil ID, profile, D_r) |
  | Cone (electric/mechanical friction) | vertical profile |

  Principle (Look §4.5): a test good for **profiling** may be poor for **modulus** — pick per the
  parameter wanted; the full B/C nuances are in the image.

---

## 2.2 — Classify & provisional layers
*Procedure tasks 2.2.1 (classify) · 2.2.2 (behaviour-type from in-situ) · 2.2.5 (special
materials).* The pack classification (EN ISO 14688/14689 or AS 1726) **governs**; these Look 2007
entries are **non-code cross-checks** and **in-situ behaviour-type charts** to fill log gaps.
*(Consistency→strength values, Tbl 2.14/2.15, are captured under §2.3.)*

### 2.2(a) Soil classification system — cross-check → task 2.2.1
- **USCS group symbols** (Tbl 2.7): GW/GP/GM/GC · SW/SP/SM/SC · ML/MH · CL/CH · OL/OH · Pt.
  Medium plasticity → intermediate symbols (CI, CL/CH). Borderline cases (silty sand vs sandy silt)
  **need lab**; without a lab test "any classification is an opinion."
- **Gradings** (Tbl 2.9): D₁₀, D₆₀; **U = C_u = D₆₀/D₁₀** (uniform U<5); **C_c = D₃₀²/(D₆₀·D₁₀)**
  (well graded U>5 and C_c=1–3).
- **Plasticity** field terms (Tbl 2.11): NP / L (no 3 mm thread) / I / H (greasy, cracks dry).
- **Atterberg** (Tbl 2.12): LL, PL, **PI = LL−PL**, SL, LS with **PI ≈ 2.13·LS**; tested on the
  **% passing 425 µm — report that %** (a "rock" site can show high PI when 90% was discarded).
- **Origin** (Tbl 2.17): residual · alluvial · colluvial · glacial · aeolian · organic · volcanic ·
  evaporites — each with a fabric signature (e.g. aeolian = uniform silt/fine sand; glacial gravels
  angular, broad grading). **Residual soils by parent rock** (Tbl 2.18, Hunt 2005): basalt/dolerite
  → high-activity clays; granite → low-activity clays + granular; shale → high-activity clays.

### 2.2(b) Behaviour-type from in-situ tests → task 2.2.2 (fill gaps between samples)
- **CPT (electric) soil behaviour** (Tbl 5.12, adapted Meigh 1987 / Robertson 1986): classify from
  the **combination** of friction ratio + cone resistance, cross-checked with B_q — *not one alone*.
  Guides: q_c<1.2 MPa → likely cohesive; >1.2 MPa → likely non-cohesive; full chart = Figs 5.3/5.4
  (mechanical cones differ). *Most useful in alluvial areas; ideal for thin layers/lensing.*
- **CPT soil type from friction ratio only** (Tbl 5.13) — *preliminary*:

  | F_R (%) | <1 | 1–2 | 2–5 | >5 |
  |---|---|---|---|---|
  | soil type | coarse–medium sand | fine/silty–clayey sand | sandy/silty clays, clays, organic | peat |

- **DMT soil type from material index I_D** (Tbl 5.17, Marchetti 1980): I_D<0.6 clay · 0.6–1.8 silt
  · >1.8 sand.

### 2.2(c) Structure & special-material flags → task 2.2.5 (carry to 2.3 reliability)
- **Structure** (Tbl 2.13): intact / fissured / slickensided / interbedded / fibrous / amorphous.
  **Fissured clay ≈ ⅔ the design strength of non-fissured** clay (also alters slope & permeability)
  → flag, propagates to 2.3 strength + reliability.
- Tag fill, organic/peat, sensitive, expansive (volcanic-derived), cemented, evaporite-crust per the
  origin/structure cues above.

### 2.2(d) Rock material classification → task 2.2.1 (rock)
- **Weathering** (Tbl 3.4): RS → XW (extremely) → DW [HW >50% soil / MW <50%] → SW → FR. *Use only
  calibrated against intact strength + defects, not standalone.*
- **RQD** (Tbl 3.7): 0–25 very poor · 25–50 poor · 50–75 fair · 75–90 good · >90 excellent;
  **RQD = Σ(sound core pieces >100 mm)/total run ×100**; exclude drilling-induced fractures;
  orientation-dependent.
- **Intact rock strength** field terms (Tbl 3.8): EL/VL/L/M/H/VH/EH (intact material, *not* mass).
- **Structure / defects** (Tbl 3.6, 3.11, 3.12): bedding & fracture spacing classes
  (crushed <20 mm … solid >6 m); defect descriptors (type, wall separation, roughness, infill —
  clay=low friction, calcite/gypsum may dissolve). **Hardness** ≠ strength (Tbl 3.9, Mohs).

### 2.2(e) Rock-mass classification → task 2.2.1 (rock mass, not material)
- **RMR** (Bieniawski 1989) = Σ ratings of {intact strength, RQD, discontinuity spacing,
  discontinuity condition, groundwater} ± orientation adjustment → **classes** I very good (81–100) ·
  II good (61–80) · III fair (41–60) · IV poor (21–40) · V very poor (<20). Tbls 18.2–18.5 give the
  rating points; discontinuity condition is the most influential parameter.
- **Q system** (Barton et al 1974): **Q = (RQD/Jₙ)·(J_r/J_a)·(J_w/SRF)** — relative block size ·
  relative frictional strength · active-stress term; **Q_c = Q·UCS/100**. Log scale → insensitive to
  any single parameter. (Parameter tables 18.10–18.14; extensive Q→support/strength correlations
  in Ch 18 — design-side, deferred.)

---

## 2.3 — Derive parameters (scenario-indexed)
*Procedure tasks 2.3.1–2.3.6.* The heart of the registry: each parameter is derived as a function
of **{ soil × drainage × data available }**. Every row is a method the engineer can run/compare;
reliability flagged; ←CP default still governs, these are non-code alternatives/fallbacks.
**Split:** **Part A — strength (below)** · Part B — stiffness/consolidation/permeability/unit
weight + rock properties (pending). All Look 2007; original authors noted.

### 2.3-A.1 Undrained shear strength s_u — clay/silt (undrained)
| Data available | Method / value | Reliability | Source |
|---|---|---|---|
| field consistency (tactile/thumb) | VS <12 · S 12–25 · F 25–50 · St 50–100 · VSt 100–200 · H >200 kPa | Low | Tbl 2.14 |
| pocket penetrometer | s_u = q_u/2; q_u = 0.8·PP (general), 1.15·PP (fills), 0.6·PP (fissured) — **never on SPT/disturbed samples** | Low–Med | Tbl 5.2 (Look 2004) |
| SPT | s_u ≈ f·N, f≈5 (range **2–8**); Sowers 4N (high PI)→15N (low PI); Stroud & Butler 4.5N (PI>30)→8N (PI≈15). **No overburden correction in clay.** | Low | Tbl 5.3 |
| CPTu | s_u = (q_c − P_o)/N_k (or q_c/N_k); **N_k 15–20** (NC 15–18, OC 18–20); also s_u = Δu/N_u (N_u 2–8) | Med (High if site-calibrated) | Tbl 5.14/5.15 |
| field vane | s_u = μ·s_uv, Bjerrum μ vs PI — see §2.1(e) | Med | Tbl 4.14 |
| DCP | n ≈ ⅓·(N₀)₆₀; VS 0–1 … Hard >12 blows/100 mm → s_u bands; top 0.5–1 m clay desiccated (low) | Low | Tbl 5.10 |
| effective overburden (NC clay) | **s_u = (0.11 + 0.0037·PI)·σ′ᵥ** (Skempton 1957); the s_u/σ′ᵥ ratio also flags OCR | Med (NC only) | Tbl 5.20 |
| cross-check | qc soil-strength bands: VS q_c<0.2 … Hard >4 MPa (N_k 17–20) | Med | Tbl 5.15 |

### 2.3-A.2 Effective friction φ′ — sand / granular (drained)
| Data available | Method / value | Reliability | Source |
|---|---|---|---|
| SPT (medium sand) | VL φ<28 · L 28–30 · MD 30–40 · D 40–45 · VD 45–50° | Med | Tbl 5.4 |
| corrected SPT by grain size | **(N₀)₆₀/D_r² = 55 fine / 60 medium / 65 coarse** (Skempton 1988); fine ↓, coarse ↑ vs medium | Med | Tbl 5.5 |
| aging adjustment | (N₀)₆₀/D_r² = 35 lab · 40 recent fill · 55 natural deposit (Skempton 1988) — interpret fills as looser | Med | Tbl 5.6 |
| grain shape + grading | **φ_crit = 30 + A + B** (A: rounded 0 / sub-ang 2 / angular 4; B: uniform 0 / moderate 2 / well 4) | Med | Tbl 5.7/5.8 (BS 8002) |
| + density (peak) | **φ_peak = 30 + A + B + C**; C = 0 (VL) · 0 (L) · 2 (MD) · 6 (D) · 9 (VD) — full grid image: [`Look2007_Table5.9...png`](../Sources/Handbooks/figures/Look2007_Table5.9_peak-friction-angle-sands_BS8002.png) | Med | Tbl 5.9 |
| CPT | D_r<15 (q_c<2.5) φ<30 · 15–35 (2.5–5) 30–35 · 35–65 (5–10) 35–40 · 65–85 (10–20) 40–45 · >85 (>20) >45° | Med | Tbl 5.16 |
| DMT | K_D <1.5 φ<30 · 1.5–2.5 30–35 · 2.5–4.5 35–40 · 4.5–9 40–45 · >9 >45° | Med | Tbl 5.19 |
| DCP | sand VL 0–1 φ<30 … VD >15 φ>45; gravel/cobbles >10 φ>30, >20 φ>40 | Low | Tbl 5.10 |
| typical by material/state | gravels 30–50°, sands 27–47° by density+grading; broken rock 27–42° | Low–Med | Tbl 7.8 |
| **corrections** | clayey sand **−5°**, gravelly **+5°** (Tbl 5.4); rounded↔angular **±4°**; **fines >30% → fines govern strength** | — | Tbl 5.4/7.8 |

### 2.3-A.3 Effective strength c′, φ′ — clay (drained)
| Data available | Method / value | Reliability | Source |
|---|---|---|---|
| typical by state | soft-organic c′ 5–10 kPa / φ′ 10–20° · soft c′ 10–20 / 15–25 · stiff c′ 20–50 / 20–30 · hard c′ 50–100 / 25–30 | Low (typical) | Tbl 7.9 |
| notes | long-term **softening → loss of c′** (use softened/residual in cuttings); remoulded/residual reduce both; coarse >30% adds friction | — | Tbl 7.9 |
| **not recommended** | clay **φ′ from SPT** — handbook gives no SPT route for clay φ′ (consistent with the NR rule) | **NR** | — |

### 2.3-A.4 Relative density D_r (state; feeds φ′)
| Data | Value | Source |
|---|---|---|
| SPT N | VL <15% (N<4) · L 15–35 (4–10) · MD 35–65 (10–30) · D 65–85 (30–50) · VD >85% (>50) | Tbl 2.15/5.4 |
| CPT q_c | per Tbl 5.16 bands above | Tbl 5.16 |

### 2.3-A.5 Intact rock strength (UCS)
| Data available | Method / value | Reliability | Source |
|---|---|---|---|
| field (hand/hammer) | EL/VL/L/M/H/VH/EH terms ↔ SPT N ↔ I_s(50); **UCS ≈ 20·I_s(50)** (multiplier varies by rock) | Low | Tbl 6.4 |
| point load index | UCS = m·I_s(50); m typically **23** (hard), **10** as general first conversion; weak rock (UCS<20) m≪20 — site-specific table 4–25 | Med (site-specific) | Tbl 6.5 (Tomlinson 1995; Look & Griffiths 2004) |
| Schmidt hammer | UCS: Low <6 · Med 6–20 · High 20–60 · V.High 60–200 · E.High >200 MPa (rebound bands); R_L=0.605+0.677·R_N; ≥10 readings, use 5 highest | Low–Med | Tbl 6.6 |

> **Note:** intact UCS ≠ rock-mass strength — combine with RQD/defects/RMR/Q (§2.2e). Rock *shear*
> strength & joint friction angles go in Part B (rock properties).

---

### 2.3-B.1 Unit weight γ
| Material | Value | Source |
|---|---|---|
| Cohesionless sand/gravel | dry ≈14–21, saturated ≈17–22 kN/m³ (VL→VD) | Tbl 7.3 |
| Cohesive clay | soft ≈14–16, stiff ≈18–20, hard ≈20–21 kN/m³ (saturated) | Tbl 7.3 |
| Rock (intact, dry) | ≈20–27 kN/m³ by origin/weathering; G_s typ **2.70** (range 2.3–5.0) | Tbl 9.2 |
| rules | use **saturated** γ below WT & in capillary fringe; **buoyant γ = γ_sat − 9.81** | Tbl 7.3 |

### 2.3-B.2 Stiffness / modulus (E, M, m_v) — drainage-specific
| Data / route | Method / value | Reliability | Source |
|---|---|---|---|
| typical E by soil (secant, foundations) | gravel 25–200 · sand <5–100 · silt <10–20 · **clay** VS<3 … hard 40–80 MPa (short-term; long-term lower) | Low–Med | Tbl 11.6/11.7 |
| m_v (oedometer) | heavily-OC clay <0.05 → NC alluvial/peat high (10⁻³ kPa⁻¹); **constrained modulus M = 1/m_v** (ν≈0) | Med (lab) | Tbl 11.8 (Carter 1983) |
| CPT (NC/LOC) | **m_v = 1/(α·q_c)** (α=5 CH/MH/ML, 6 CL/OL, 1.5 OH w>100%); **M = 3·q_c**; **E = 2.5·q_c** (pad) / **3.5·q_c** (strip) | Med | Tbl 11.10 (Fugro/Meigh) |
| CPT drained E — sand | VL (q_c<2.5) E<10 … VD (q_c>20) E>60 MPa | Med | Tbl 11.11 |
| SPT — clay | **E′/N = 0.6–0.7**; **E_u/N = 1.0–1.2** (CIRIA 1995); footing E_u/N=1, raft=2, friction pile=3 | Low–Med | Tbl 11.12 |
| Cu+PI — clay drained | **E′/C_u** ≈ 270 (low PI) → 110 (PI 50–60) | Med | Tbl 11.13 (Stroud 1975) |
| OCR+PI — clay undrained | **E_u/C_u** (secant @ FoS 2): PI<30 OCR<2 600–1500 … PI>50 50–300 | Med | Tbl 11.14 (Jamiolkowski 1979) |
| SPT+PI (large strain, pavements) | **E_s/N** = 3.5 sand/gravel · 2.5 low PI · 1.5 med · 1.0 high · 0.5 extreme — *not for soft clays* | Low | Tbl 11.15 |
| long vs short term | **E_long = β·E_short**: β = 0.9 gravel · 0.8 sand · 0.7 silt/silty clay · 0.6 stiff clay · 0.4 soft clay | — | Tbl 11.16 |
| Poisson ν | undrained clay **0.5**, oedometer **0**; sand 0.30 · low PI 0.35 · med 0.40 · high 0.45 (short-term) | — | Tbl 11.17 |

### 2.3-B.3 Consolidation — c_v, OCR, preconsolidation
| Parameter / route | Method / value | Reliability | Source |
|---|---|---|---|
| c_v from k, m_v | **c_v = k/(m_v·γ_w)**; **c_h = 2–10·c_v**; k_h = 2–10·k_v | Med | Tbl 8.12 |
| typical c_v | Boston blue clay 12±6 · glacial lake 2–2.7 · organic silt 0.6–3 · Mexico City 0.3–0.5 m²/yr | Low | Tbl 8.13 (Carter & Bentley 1991) |
| c_v vs LL | decreases with liquid limit (and with remoulding) | Low | Tbl 8.14 (NAVFAC 1988) |
| c_v / c_h from CPTu | c_h = 300/t₅₀ (min); c_v = c_h/2 — see §2.3-A / §2.1(c) | Med | Tbl 5.14 / 8.15 (Mayne 2002) |
| OCR | **OCR = P_c/P_o**; NC ~1 (<1.5) · LOC 1.5–4 · HOC >4; aged glacial clay 1.5–2 (PI>20, Bjerrum) | Med | Tbl 7.10 |
| preconsolidation P_c (CPTu) | **P_c ≈ 0.33·(q_T − P_o)** — intact clays only | Med | Tbl 7.11 (Mayne 2002) |

### 2.3-B.4 Permeability k
| Route | Method / value | Reliability | Source |
|---|---|---|---|
| typical by soil | gravels ~10⁻¹–10⁻² · clean sand 10⁻²–10⁻⁵ · silt 10⁻⁶–10⁻⁹ · clay <10⁻⁹ m/s (drainage: very good → practically impermeable) | Low–Med | Tbl 8.1 |
| grain size (Hazen) | **k = C·d₁₀²** (coarse 0.1–3 mm, **uniform U<5**); inaccurate for gap-graded/stratified | Med (coarse only) | Tbl 8.3 |
| classification | first-order magnitude check from USCS class | Low | Tbl 8.4 |
| from c_v | k = c_v·m_v·γ_w (small-sample; misses mass structure) | Med | Tbl 8.12 |
| flags | granular **not free-draining if fines >15%**; **low-k if fines >30%** | — | Tbl 8.1 |

### 2.3-B.5 Rock properties (mass strength, joint friction)
| Parameter / route | Method / value | Reliability | Source |
|---|---|---|---|
| rock-mass design compressive strength | RQD 0–70 → **0.33·q_u**; RQD 70–100 → 0.33–0.8·q_u; cohesion ≈0.1·q_u; φ 30° (0–70), 30–60° (70–100) | Med | Tbl 9.13 (Kulhawy & Goodman 1988) |
| joint friction angle | low **20–27°** (schist, shale) · med **27–34°** (sandstone, siltstone, chalk, gneiss, slate) · high **34–40°** (basalt, granite, limestone) | Med | Tbl 9.15 (TRB 1990) |
| joint roughness | **φ_eff = φ_basic + i**; asperity i = 10–15° (1st order, 500 mm) / 20–30° (2nd order, <50–100 mm) | Med | Tbl 9.16 (Patton 1966) |
| filled joints | use **residual** φ if clay infill has moved | Low–Med | Tbl 9.17 (Barton 1974) |

> q_u = UCS of intact core. Mass strength ≠ intact strength — combine with RQD/RMR/Q (§2.2e).

---

## 2.4 — Consistency & cross-location reconciliation
*Procedure tasks 2.4.2 (reliability-adjusted tolerance) · 2.4.3 (outlier/lateral-variability) ·
2.4.5 (reconciliation).* Ch 10 supplies the **empirical COV + distribution backbone** that turns
the consistency engine's qualitative logic into numbers. **This directly advances open item (c)**
(the least code-defined step) — see the note at the end. *(Non-code; engineer/founder confirms the
methodology; these are candidate references, not adopted code.)*

### 2.4(a) Coefficient-of-variation (COV) reference values
**COV = SD/Mean.** Total variability = **inherent (natural) + testing** variability (Tbl 10.5).
Use these to size the *expected* scatter envelope (genuine variability) vs an *error* (beyond it).

**Inherent soil-property COV** (mean, outliers removed; Kulhawy 1992, Tbl 10.2) — index params are
tighter than strength/deformation:

| γ | LL | PL | w_n | e₀ | φ′ | tan φ′ | rock q_u | C_u | C_c |
|---|---|---|---|---|---|---|---|---|---|
| 7% | 11% | 11% | 18% | 20% | 13% | 11% | 23% | 34% | 37% |

**In-situ test COV — the reliability ranking** that sets the **tolerance-band widening in 2.4.2**
(Poon & Kulhawy 1999, Tbl 10.3):

| DMT | electric CPT | vane | prebored PMT | SBPMT | mechanical CPT | SPT |
|---|---|---|---|---|---|---|
| 5–15% | 5–15% | 10–20% | 10–20% | 15–25% | 15–25% | **15–45% (most variable)** |

**Lab-test COV** (Poon & Kulhawy 1999, Tbl 10.4/10.5): density 1–2% · LL 7% · PL 10% ·
C_u 8–38% (mean ~19) · φ′ (triaxial) 7–56% (mean ~24) · PI 5–51% (mean ~24); lab strength on clay
UC 20–55 (40) · CIUC 20–40 (30) · UU 10–30 (20).

**Process/site COV** (Kay 1993, Tbl 10.11): natural variation over a site **5–15% typical**; design
model 0–25%, design decision 15–45%, construction 0–15%. Expert vs industry prediction COV
**14% vs 32%** (Tbl 10.12) — i.e. effort halves scatter.

### 2.4(b) Distribution & central tendency — for outlier detection & the reconciliation statistic
- **Normal distribution is *not* appropriate for soil/rock strength** — it yields **negative values
  at the lower 5% fractile** (Tbl 10.10). The assumed distribution can shift the probability of
  failure by a **factor of ~10** (Tbl 10.9).
- **Lognormal recommended** for soil & rock strength (highly ranked + simple; Tbl 10.10).
- **Use the median, not the mean**, for site characterisation given non-normality (Tbl 10.9) — and
  robust spread (MAD) to stop one bad point distorting the picture.
- FoS↔P_f via lognormal (Duncan & Wright 2005, Tbl 10.16): e.g. FoS 1.5 → P_f ≈ 0.1% (COV 10%) but
  ≈ 20% (COV 40%) — quantifies how variability, not just FoS, drives risk.

### How it feeds the procedure
- **2.4.2** reliability-adjusted tolerance band ← the in-situ test COV ranking (SPT wide, CPT/DMT
  tight): a fallback disagreeing with a lab value within its COV is *not* a conflict.
- **2.4.3** outlier/anomaly envelope ← inherent param COV (Tbl 10.2) + site natural variation
  (5–15%); deviations beyond → flag; classify with median/MAD robust stats.
- **2.4.5** reconciliation ← weight toward the lower-COV test; treat within-envelope spread as
  *genuine variability* (preserve for 2.5/2.6), beyond-envelope as *error*.
- **Pre-feeds 2.6:** the same COV + lognormal + 5%-fractile drive the cautious-estimate statistic.

> **Open item (c) — CONFIRMED (founder, 2026-06-29, D30):** the 2.4/2.6 statistical methodology
> (COV + lognormal/median + the reconciliation ladder) and its references (Kulhawy 1992; Poon &
> Kulhawy 1999; Look & Griffiths 2004; Duncan & Wright 2005) are the **accepted default**.

---

## 2.5 — Ground-model assembly
*Procedure tasks 2.5.2 (interpolation uncertainty) · 2.5.4 (representative attributes) · 2.5.5
(design-critical features).* **Lighter section:** the *assembly logic* (zoning, sections,
groundwater regime) is procedure/code-driven; the handbook contributes **derived ground-model
attributes** and **site-scale hazard features** to attach to units, plus a representativeness caveat.

### 2.5(a) Excavatability / rippability — construction-critical unit attributes → task 2.5.5
Derivable from interpretation params (strength, RQD, weathering, seismic velocity) and attached per
rock unit; flags the hard-rock / blast-required features that govern constructability.
- **Excavatability** (Franklin 1971, Tbl 12.5): easy dig I_s(50)<0.1 MPa / spacing <0.02 m / RQD<10%
  → marginal (<0.3 / <0.2 m / <90%) → **blast to loosen / fracture** (>0.3 MPa, spacing >0.2–0.6 m).
- **Rippability** (Weaver 1975, Tbl 12.9): rating from seismic velocity (>2150 m/s = very good rock)
  + hardness + weathering + joint spacing/continuity/gouge + strike/dip → class I–V → easy ripping →
  **unrippable; total rating >75 ⇒ unrippable without pre-blasting**.
- **Bulking factor** (Tbl 12.10): in-situ bulk density + bulk-up on excavation — granular 10–15%,
  cohesive 20–40%, peat/topsoil 25–45% (earthworks-volume attribute).

### 2.5(b) Site-scale terrain & landslide features → task 2.5.5
- **Terrain category** by slope (Tbl 15.6): steep hill >30% (>16.7°) · high undulating 20–30% ·
  moderate 10–20% · gently undulating–level <10%.
- **Landslide susceptibility** (Skempton & Hutchinson 1969, Tbl 15.7): debris slides D/L 5–10% on
  22–38° · slumps 15–30% on 8–16° · flows 0.5–3% on 3–20° — flags landslide-prone zones as
  design-critical features.

### 2.5(c) Representativeness → task 2.5.2 (interpolation uncertainty)
- **Volume sampled/loaded ratio** ≈10⁴–10⁶ (Tbl 1.9, §2.0b): interpolation between sparse soundings
  carries wide uncertainty; widen uncertainty bands where the sampled fraction is small / spacing
  exceeds Tbl 1.8 guidance.

---

## 2.6 — Characteristic / design value
*Procedure tasks 2.6.2 (choose statistic per limit state & footprint) · 2.6.3 (propose cautious
value).* Builds directly on the **COV + distribution backbone in §2.4** (lognormal for strength;
median/MAD; in-situ & inherent COV); this section adds the **fractile / cautious-estimate** specifics
that turn a population into a design value. *(Non-code; informs the EC "cautious estimate" §2.4.5.2
and the AU "cautiously assessed strength" — the engineer owns the value.)*

### 2.6(a) Characteristic-value fractile by project & limit state (Tbl 10.17)
| Project | Ultimate (ULS) | Serviceability (SLS) |
|---|---|---|
| Structure | 1% fractile | 5% fractile |
| Road | 5% fractile | 10% fractile |
- Tighter at **interfaces** (e.g. embankment next to a bridge): 1–5% SLS (major→minor roads).
- "Lowest value" is **not** used unless samples are very limited → otherwise use the fractile; this is
  the empirical handle on **local-governing (low fractile) vs volume-averaged (toward the mean/median)**
  in procedure 2.6.2.

### 2.6(b) Distribution caveat — normal vs lognormal fractile equivalence
The fractile only means what you think *if the distribution is right* (and for strength it is
**lognormal**, not normal — §2.4b):
- a **5%** value on a *normal* fit ≈ **10–30%** on a *lognormal* fit;
- a **10%** value on normal ≈ **30–50%** on lognormal; **~20% ≈ the median**.

→ compute the cautious value on the **lognormal** distribution; a normal-distribution 5% fractile can
even go **negative** for strength (§2.4b, Tbl 10.10).

### 2.6(c) Reliability targets (Tbl 10.18, roads)
Highway 90–97.5% (typ 95%) · main roads 85–95% (90%) · local roads 80–90% (85%) — vary by authority;
ULS vs SLS use different acceptance (10.17).

### How it feeds the procedure
- **2.6.2** statistic choice ← fractile from Tbl 10.17 (project × limit state) + local-vs-averaged
  rule; distribution = lognormal (§2.4b).
- **2.6.3** cautious value ← apply the chosen fractile to the lognormal population (COV from §2.4a),
  report range (mean/median → fractile) and sensitivity.

---

## 2.7 — Preliminary design recommendations
*Procedure task 2.7.1 (design-ready profiles).* The GIR bridge to design: bearing, pile fₛ/q_b,
rock sockets, earth pressures, settlement/tolerable-movement limits. **Equal depth across all four
chapters.** ←CP safety format still governs how these are factored downstream; values are
*indicative* (geometry-independent), engineer selects/compares. All Look 2007; authors noted.

### 2.7(a) Shallow foundation — bearing capacity (Ch 21)
- **Bearing equation** (Terzaghi): **q_ult = c·N_c + q·N_q + 0.5·γ·B·N_γ** (strip); square
  1.3·c·N_c + q·N_q + 0.4·γ·B·N_γ; circular 1.3·c·N_c + q·N_q + 0.3·γ·B·N_γ (Tbl 21.4).
- **Bearing-capacity factors** (Vesic 1973 / Hansen 1970, Tbl 21.5):

  | φ° | 0 | 5 | 10 | 15 | 20 | 25 | 30 | 35 | 40 | 45 |
  |---|---|---|---|---|---|---|---|---|---|---|
  | N_c | 5.14 | 6.5 | 8.3 | 11.0 | 14.8 | 20.7 | 30.1 | 46.1 | 75 | 134 |
  | N_q | 1.0 | 1.57 | 2.47 | 3.94 | 6.4 | 10.7 | 18.4 | 33.3 | 64 | 135 |
  | N_γ (Vesic) | 0 | 0.45 | 1.22 | 2.65 | 5.4 | 10.9 | 22.4 | 48.0 | 109 | 272 |
  | N_γ (Hansen) | 0 | 0.07 | 0.39 | 1.18 | 2.95 | 6.76 | 15.1 | 33.9 | 80 | 201 |

- **Undrained clay** φ=0 → N_c=5.14; **N_c with embedment** z/B (Skempton 1951): strip 5.14→7.5,
  square/circular 6.28→9.0 (z/B 0→5).
- **Granular allowable bearing from N** (Meyerhof 1956, Tbl 21.7): function of B and N (e.g. B=1 m:
  ~50 kPa N=5 → ~600 kPa N=50). **Halve if water within B**; FS=3 limits settlement to 25 mm
  (×2 for 50 mm, ×½ for 12 mm); use corrected N. *(Meyerhof 1965 ~50% higher.)*
- **FoS** (Vesic 1975, Tbl 21.9): loading×consequence × SI extent → 3.0 / 4.0 (high), 2.5 / 3.5,
  2.0 / 3.0; industry default **FS=3**; shear governs narrow footings, settlement governs B≥~2 m.

### 2.7(b) Shallow foundation — settlement / SLS (Ch 23)
- **Granular settlement from N** (Meyerhof 1965, Tbl 21.8, ρ in mm, q kPa): B<1.25 m **ρ=1.9q/N**;
  B>1.25 m **ρ=2.84q/N·[B/(B+0.33)]²**; large raft **ρ=2.84q/N**.
- **Immediate/total ratio** (Burland 1978): soft yielding ρ_u=0.1ρ_oed (ρ_u/ρ_T <10–15%); stiff
  elastic ρ_u=0.6ρ_oed, ρ_c=0.4ρ_oed (33–67%).
- **Consolidation correction μ** (Skempton-Bjerrum, Tomlinson 1995): very sensitive 1.0–1.2 · NC
  0.7–1.0 · OC 0.5–0.7 · heavily OC 0.2–0.5 → ρ_c = μ·ρ_oed.
- **Self-weight (fill)** (Goodger & Leach 1990): well-compacted granular/rock/clay ≈0.5%H; refuse
  30%H; pumped clay 12%H.

### 2.7(c) Axial pile — shaft fₛ & base q_b in soil (Ch 21)
- **Shaft** (Poulos 1980, Tbl 21.14):
  - clay **f_s = α·C_u**: bored α 0.45 (non-fissured)/0.3 (fissured), cap 100 kPa; driven α 1.0
    (soft–firm)/0.75 (stiff)/0.25 (v.stiff–hard).
  - sand **f_s = k_s·tanδ·σ′ᵥ**, with k_s·tanδ: bored NR(loose)/0.1(MD)/0.2(D)/0.3(VD);
    driven 0.3/0.5/0.8/1.2. Low-displacement (steel H) **×0.5**.
- **Base** (Tbl 21.16): clay **q_b = N_c·C_u·ω** (N_c=9, ω 1.0/0.75 fissured); sand
  **q_b = N_q·σ′ᵥ**, N_q bored 20/30/60/100, driven 70/90/150/200 (loose→VD); **q_b cap 10 MPa**.
- **fₛ/q_b from SPT N** (Meyerhof 1976): shaft (Tbl 21.17) driven high-disp **2N**, low-disp **N**,
  bored **0.67N** (kPa); base (Tbl 21.18) driven fine–med sand **40N·L/D ≤400N**, coarse/gravel
  **≤300N**, bored **14N·L/D** (kPa).
- **Installation φ shift** (Poulos 1980, Tbl 21.15): shaft uses φ₁ (bored) / ¾φ₁+10 (driven);
  base N_q uses φ₁−3 (bored) / (φ₁+40)/2 (driven).

### 2.7(d) Rock foundations — bearing & rock-socket shaft (Ch 22)
- **Allowable bearing from RQD** (Peck, Hansen & Thorburn 1974, Tbl 22.1): RQD 0–25 → 1–3 MPa ·
  25–50 → 3–6 · 50–75 → 6–12 · 75–90 → 12–20 · >90 → 20–30 MPa (**lesser of this, UCS, or concrete
  allowable**; first approximation, not detailed design).
- **Bored pile shaft in rock** (Seidel & Haberfield 1995, Tbl 22.14): **τ = ψ·(q_u·P_a)^0.5**
  (P_a≈100 kPa), ψ = 0.5/1.0/2.0/3.0 → τ = 0.1/0.225/0.45/0.70·√q_u; others: lesser of 0.15·q_u
  (Carter & Kulhawy) and 0.2·√q_u (Horvath & Kenney). q_u = intact UCS (MPa).
- **Design shaft resistance** (Tbl 22.17, combined): soil/low-rock RQD<<25% → **0.1√q_u**; low
  quality RQD<25% → **0.2√q_u**; medium/high RQD>25% → **0.45√q_u** (adjust for slurry/grooving;
  roughness classes R1–R4, Pells 1980).

### 2.7(e) Earth-pressure coefficients (Ch 19)
- **At-rest K₀** (Brooker & Ireland 1965, Tbl 19.3): NC **1−sinφ** (granular), **0.95−sinφ** (clay),
  **0.4+0.007·PI** (PI 0–40), **0.64+0.001·PI** (PI 40–80); OC **(1−sinφ)·OCR^sinφ** (granular),
  **(1−sinφ)·OCR^0.5** (clay); elastic **ν/(1−ν)**.
- **Active Kₐ** & **passive K_p** (Caquot & Kerisel 1948) — i=0, δ=0 (match Rankine):

  | φ° | 20 | 25 | 30 | 35 | 40 |
  |---|---|---|---|---|---|
  | Kₐ | 0.49 | 0.41 | 0.33 | 0.27 | 0.22 |
  | K_p | 2.0 | 2.5 | 3.0 | 3.7 | 4.6 |

  Wall friction (δ up to ½φ) and back-slope i strongly affect K_p (e.g. φ40, δ=½φ → K_p≈7.5); full
  grids: [Kₐ image](../Sources/Handbooks/figures/Look2007_Table19.12_active-earth-pressure_CaquotKerisel.png)
  · [K_p image](../Sources/Handbooks/figures/Look2007_Table19.13_passive-earth-pressure_CaquotKerisel.png).
  *Back-slope raises Kₐ by 1.5–3×; the effect is more pronounced on K_p.*

### 2.7(f) Tolerable / limiting movements (Ch 23) — SLS acceptance for recommendations
- **Limiting settlement** (Skempton & Macdonald 1955, Tbl 23.6): isolated footing clay 65 mm / sand
  40 mm; raft clay 65–100 / sand 40–65 mm.
- **Limiting angular distortion δ/L** (Wahls 1981, Tbl 23.7): machinery-sensitive 1/750 · diagonals
  1/600 · **no cracking 1/500** · first panel-wall cracking 1/300 · visible tilt 1/250 ·
  **structural-damage danger 1/150**.
- **Distortion + horizontal strain** (Boscardin & Cording 1989, Tbl 23.8): damage from combined
  angular distortion × ε_h (negligible <1.6×10⁻³ → severe ≥6.6×10⁻³).

---

## Status — interpretation (2.0–2.7) seeded from Look 2007 ✓
All interpretation steps now carry their handbook methods/ranges/recommendations. 2.8 is packaging
(no method content). Next sources (other handbooks, codes, AS pack) extend the same per-step
structure; design-module chapters (Ch 12–22 design side) feed the design-stage libraries later.

---

# Stage 5 — Shallow foundation design

Seeded here as the design library grows (`stages/05-shallow-foundation.md` = the stage spec; this
is the method registry it queries). Organized **by procedure step → method**. It now **carries both
packs and both source classes from the start**, mirroring interpretation §2.7:
- **EC pack code default** — **JRC 2013 *(EC7 guidance)*** (EN 1997-1 §6 direct method, transcribed
  clean and DA-worked);
- **AU pack** — **AS 5100.3 §10** (same bearing/settlement *mechanics*, swapped safety format:
  risk-based φg) *(code)*;
- **Non-code alternatives/fallbacks** — **Look 2007 Ch 21/23** (SPT-N-only routes, presumed-bearing
  tables, Meyerhof N-charts, FoS/tolerable-movement tables) — much already transcribed in §2.7(a)/(b)
  and cross-linked here rather than duplicated.

> **Selection-rule reminder for this stage:** JRC 2013 entries are ***the EC-pack code default*** and
> AS 5100.3 entries the ***AU-pack code default*** (not comparison alternatives) — their methods *are*
> the respective codes' own. Look 2007 entries stay *(non-code)* alternatives/fallbacks per the header
> rule (offer + compare; SPT-N routes are fallbacks with reliability flags). **Key structural point
> (see stage doc):** the two packs share the *same mechanics* (bearing equation, corrections,
> settlement methods) but differ entirely in **safety format** — EC applies partial factors
> (DA1/2/3), AU applies one risk-based φg to the ultimate resistance. So most method entries below are
> **pack-shared**; only the safety-format subsections (§5.x) fork.

## 5.1 — Preliminary / presumed bearing (prescriptive method)
*Stage task 5.1 (trial sizing) — a quick, geometry-light check before the full direct method.* All
three packs recognize a **prescriptive** route (EC7 §6.5.2.4 presumed bearing; AS 5100.3 permits
Rug from comparable experience; Look tabulates presumed values).

### 5.1(a) Presumed bearing value by soil description — *(non-code, Look Tbl 21.3)*
Quick sizing / sanity band before computing; **not** for final design.

| Material | Description (strength) | Presumed bearing (kPa) |
|---|---|---|
| Clay | V.soft <12 · soft 12–25 · firm 25–50 · stiff 50–100 · v.stiff 100–200 · hard >200 kPa | <25 · 25–50 · 50–100 · 100–200 · 200–400 · >400 |
| Sand | v.loose (D_r<15%, φ<30°) · loose (15–35%, 30–35°) · med.dense (35–65%, 35–40°) · dense (65–85%, 40–45°) · v.dense (>85%, φ>45°) | <50 · 50–100 · 100–300 · 300–500 · >500 |

- **EC pack:** the prescriptive method (§6.5.2.4); on **rock**, use EN 1997-1 **Annex G** presumed
  bearing (weak/broken rock groups) — see §5.4 in the stage doc.
- **AU pack:** §10.3.3.3 allows Rug from field/lab testing or empirical correlations; the table above
  serves as a first-pass presumed value, then confirmed by the direct method + φg.
- Reliability **Low** (preliminary only); always followed by the §5.2 direct check.

## 5.2 — ULS bearing resistance (EC7 direct method, Annex D)
*Stage task 5.2.* Clean transcription of the EN 1997-1 Annex D sample analytical method (the raw
code `.txt` is garbled here) + the eccentricity rules. **Governing inequality:** `V_d ≤ R_d`, where
`V_d` includes foundation self-weight + backfill + water actions. *(JRC 2013 §3.3.1, Ch 3 pp. 22–25.)*

### 5.2(a) Bearing resistance equation
- **Drained:** `R/A′ = c′·N_c·b_c·s_c·i_c + q′·N_q·b_q·s_q·i_q + ½·γ′·B′·N_γ·b_γ·s_γ·i_γ`
- **Undrained:** `R/A′ = (2+π)·c_u·b_c·s_c·i_c + q`
- `A′ = B′·L′` = **effective area** with the load acting at its centre (see eccentricity below).

### 5.2(b) Bearing-capacity factors
- `N_q = e^(π·tanφ′)·tan²(45°+φ′/2)` · `N_c = (N_q − 1)·cotφ′` · `N_γ = 2·(N_q − 1)·tanφ′`
  (Annex D form, δ ≥ φ′/2 rough base). *(Cross-check the tabulated Vesic/Hansen values in §2.7(a),
  Look Tbl 21.5 — same factors, handy for spot-checks.)*

### 5.2(c) Correction factors — drained (rectangular unless noted)
| Effect | Factor |
|---|---|
| Shape | `s_q = 1 + (B′/L′)·sinφ′` (square/circ: `1 + sinφ′`) · `s_c = (s_q·N_q − 1)/(N_q − 1)` · `s_γ = 1 − 0,3·(B′/L′)` (square/circ: 0,70) |
| Load inclination (H) | `i_q = [1 − 0,7H/(V + A′·c′·cotφ′)]^m` · `i_c = (i_q·N_q − 1)/(N_q − 1)` · `i_γ = [1 − H/(V + A′·c′·cotφ′)]³` |
| — exponent m | `m_B = [2+(B′/L′)]/[1+(B′/L′)]` · `m_L = [2+(L′/B′)]/[1+(L′/B′)]` · `m_θ = m_L·cos²θ + m_B·sin²θ` (θ = angle of H to L′) |
| Base inclination (α to horizontal) | `b_q = b_γ = (1 − α·tanφ′)²` · `b_c = b_q − (1 − b_q)/(N_c·tanφ′)` |

### 5.2(d) Correction factors — undrained
`s_c = 1 + 0,2·(B′/L′)` (square/circ 1,2) · `i_c = ½·(1 + √(1 − H/(A′·c_u)))` · `b_c = 1 − 2α/(π+2)`.
Requires `H ≤ A′·c_u`.

### 5.2(e) Eccentricity → effective dimensions
- **Effective width/length:** `B′ = B − 2e_B`, `L′ = L − 2e_L` (load re-centred on `A′ = B′·L′`).
- **§6.5.4 large-eccentricity precaution:** special care when `e_B > B/3` or `e_L > L/3` — **note:
  this is *not* the middle-third rule** (`e > B/6`, which only avoids a base–soil gap in elastic
  soil). Include construction **tolerance up to 0,10 m** in foundation dimensions.
- **Load-case permutation** (eccentricity couples action factoring to `A′`): re-analyse with the
  permanent vertical load taken **both favourable and unfavourable**, and with the **leading
  variable action switched** (V vs H). Worked γ-sets in JRC 2013 Fig.3.3.4.

### 5.2(f) Pack & non-code notes
- **AU pack — *(code, AS 5100.3 §10.3.3.3)*.** Same mechanics: AS 5100.3 prescribes **no** bearing
  equation — it requires `R_ug` (ultimate geotechnical strength) to be "established by field or
  laboratory testing", analytically or via empirical/quasi-analytical relationships from SPT, CPT,
  plate-load, vane, or pressuremeter tests (§10.3.3.3 NOTE). **So the engine reuses the equation +
  factor set in 5.2(a)–(e) above to produce `R_ug`;** only the *safety format* differs — see §5.x(AU)
  φg below (governing `S* ≤ φg·R_ug`, `R_ug` from **characteristic/unfactored** strength). Allowances
  required by §10.3.3.3: GWT variation & rapid drawdown, weak/soft zones, unfavourable rock
  bedding/jointing, time/cyclic effects, load eccentricity & inclination, sloping ground/excavations.
- **Non-code fallbacks/cross-checks — *(Look 2007)*, already transcribed in §2.7(a):** the tabulated
  **Vesic/Hansen N_c/N_q/N_γ** (Tbl 21.5) as a factor spot-check; **Meyerhof allowable-bearing-from-N**
  chart (Tbl 21.7, "**halve if water within B**") as the **SPT-N-only fallback** when no lab strength
  exists (reliability **Low–Med**); **FoS table** (Vesic 1975, Tbl 21.9) for the non-code
  global-FoS route (industry default **FS = 3**; shear governs narrow footings, settlement governs
  B ≳ 2 m).

## 5.3 — ULS sliding resistance
*Stage task 5.3.* Check `H_d ≤ R_d + R_{p,d}` (`R_{p,d}` = passive thrust in front — see caveat).
*(JRC 2013 §3.3.2.)*
- **Drained:** `R_d = V′_d·tanδ_d` (factor parameters) **or** `R_d = V′_d·tanδ_k/γ_Rh` (factor
  resistance). Interface remoulded → `δ_d = φ′_cv,d` (cast-in-situ concrete) or `⅔·φ′_cv,d` (smooth
  precast). **Neglect effective cohesion c′.**
- **Undrained:** `R_d = A_c·c_{u,d}` or `A_c·c_{u,k}/γ_Rh`; where the vertical load can't ensure full
  base contact, **cap R_d at 0,4·V_d**.
- **Passive-resistance caveat:** peak sliding and peak passive mobilise at *different* displacements
  (passive needs large movement), and excavation/erosion/shrinkage can remove the front soil —
  **usually neglect R_{p,d}**.
- **AU pack — *(code, AS 5100.3 §10.3.3.4)*.** Same physics, φg format: `H_ug·φg + E_pr·φg ≥ S*`
  (`H_ug` = ultimate base shear, `E_pr` = ultimate passive resistance in front). Same caveat baked
  into the code NOTE — relate both `H_ug·φg` and `E_pr·φg` to the anticipated movement scale,
  consider post-peak softening, and account for the front soil being removed by erosion/excavation.

## 5.4 — Indirect (SLS-controlled) method — Terzaghi & Peck
*Stage task 5.2/5.5 boundary.* An **indirect method** (Table 3.2.1): allowable bearing pressure for
**granular soil** as f(corrected SPT `N`, breadth `B`, embedment ratio `D/B`), calibrated to keep
settlement **< 25 mm**. *(JRC 2013 §3.4.3; Terzaghi & Peck 1967.)*
- Charts: [Terzaghi–Peck allowable bearing (imperial Fig.3.3.8 + SI Fig.3.3.9)](../Sources/Handbooks/figures/JRC2013_Fig3.3.8-9_Terzaghi-Peck_allowable-bearing_granular-SPT.png)
  — SI chart reads `q_all` (kPa) vs `B` (m) for `N` = 5…60.
- **Key design insight (governing-limit-state shift):** the curves rise linearly with `B` (bearing/**ULS**
  governs) then flatten (settlement/**SLS** governs) — so for pads the governing check *changes with
  footing size*; large `B` is settlement-controlled. Confirms why both checks are always required.
  *(This is the design-side analogue of the Look Tbl 21.7 "halve if water within B" N-chart in §2.7(a).)*

## 5.5 — SLS settlement & tolerable movements
*Stage task 5.5.* EC7 §6.6 mandates the *requirement*, not a formula; JRC 2013 gives the worked
method set + a code-anchored (Annex H) limits table. Components: `s_0` immediate + `s_1`
consolidation + `s_2` creep; SLS partial factors = 1; combination ψ = ψ₂ (quasi-permanent).

### 5.5(a) Limiting deformations — Annex H aligned (Table 3.4.1)
| Structure | Concern | Criterion | Limiting value |
|---|---|---|---|
| Framed / reinf. load-bearing walls | structural damage | angular distortion | 1/150–1/250 |
| | cracking in walls/partitions | angular distortion | 1/500 (1/1000–1/1400 end bays) |
| | visual appearance | tilt | 1/300 |
| | connection to services | total settlement | 50–75 mm (sands) · 75–135 mm (clays) |
| Tall buildings | lift/elevator operation | tilt after installation | 1/1200–1/2000 |
| Unreinforced load-bearing walls | cracking (sagging) | deflection ratio | 1/2500 (L/H=1) → 1/1250 (L/H=5) |
| | cracking (hogging) | deflection ratio | 1/5000 (L/H=1) → 1/2500 (L/H=5) |
| Bridges — general | ride / distress / function | total settl. / horiz. | 100 mm / 63 mm / 38 mm |
| Bridges — multi-span / single-span | structural damage | angular distortion | 1/250 / 1/200 |

Rules of thumb (JRC §3.4.1): relative rotation **1/500 acceptable** for many structures; **1/150 ≈
ULS onset**; isolated-footing total settlement up to **50 mm** often acceptable. *(Complements the
non-code Look Tbl 23.6–23.8 limits in §2.7(f) — use this as the EC-pack default, Look as cross-check.)*

### 5.5(b) Immediate settlement — elasticity method (Annex F.2 adjusted elasticity)
- Compact form: `w = p·b·f/E_m` (`p` = mean base pressure, `b` = breadth, `f` = influence coeff.,
  `E_m` = operative/design modulus).
- Layer-sum worked form: `s = q·B·Σ_j[(1 − ν_j²)/E_j · (I·H_i)]` with influence factor
  **`I = μ₀·μ₁`** from Christian-Carrier-type charts:
  [μ₀ chart (embedment D/B)](../Sources/Handbooks/figures/JRC2013_FigA.3.14_mu0_elastic-settlement-influence-factor.png)
  · [μ₁ chart (layer H/B, shape L/B)](../Sources/Handbooks/figures/JRC2013_FigA.3.15_mu1_elastic-settlement-influence-factor.png).
- **Worked calibration:** for the strip (B=1,5 m) `δ ≈ 14 mm`; the equivalent whole-building raft
  (B=15,5 m) `δ ≈ 21 mm` — bigger footprint stresses deeper, larger settlement (§2.5c volume-effect
  in action).

### 5.5(c) Consolidation settlement (oedometric)
`S_ed = ΣΔH_i`, `ΔH_i = H_i/(1+e₀)·[C_s·log(σ′_c/σ′_v0) + C_c·log((σ′_v0+Δσ)/σ′_c)]`; stress
increments via Boussinesq/Newmark influence factors. **Depth-of-influence choice is decisive:**
limiting integration to Δσ < 10% σ′_v0 gave 3,3 cm vs 24,3 cm when extended to ~building width —
flag the assumption (small OC/cementation) explicitly. *(JRC 2013 §A.3.5.)*

### 5.5(d) Pack & non-code notes
- **AU pack — *(code, AS 5100.3 §10.3.5)*.** Same as EC in structure: the code mandates the
  **requirement** (single-footing displacement, group/raft differential displacement, and vibration
  from cyclic/dynamic loads — §10.3.5.1), computed at **serviceability loads**, checked against
  serviceability limit values chosen so as not to damage the supported structure — but prescribes
  **no method**. So the elasticity/consolidation methods in 5.5(b)/(c) are **pack-shared**; the
  engineer selects, exactly as under EC7 §6.6. Limit values: use the Table 3.4.1 set in 5.5(a) as the
  default, refined by the structure-specific SLS values the bridge/structural code sets.
- **Non-code — *(Look 2007)*, already in §2.7(b)/(f):** the **Meyerhof settlement-from-N** charts
  (Tbl 21.8, `ρ = f(q, N, B)`) as the **SPT-N-only settlement fallback** for granular soils (both
  packs, since neither code names a formula); **Burland immediate/total ratios** + **Skempton–Bjerrum
  μ** consolidation correction; and the **tolerable-movement limits** (Wahls Tbl 23.7 δ/L;
  Skempton–Macdonald Tbl 23.6; Boscardin–Cording Tbl 23.8) as a cross-check on 5.5(a).

## 5.x(EC) — Safety format: DA1/DA2/DA3 (+ DA2*) — EC pack
*Feeds the stage-doc "per-pack safety format" split.* Recommended EN 1997-1 partial-factor sets,
design-ready. National Annex may override. *(JRC 2013 §3.3, Tables 3.3.1–3.3.3.)*

| Combination | Actions | Params | Resist. |
|---|---|---|---|
| DA1-1 | A1 | M1 | R1 |
| DA1-2 | A2 | M2 | R1 |
| DA2 | A1 | M1 | R2 |
| DA3 | (A1 struct / A2 geo) | M2 | R3 |

- **Actions γ_F (A1/A2):** perm. unfav 1,35/1,0 · perm. fav 1,0/1,0 · var. unfav 1,5/1,3 · var. fav 0/0.
- **Resistances γ_R spread ftg (R1/R2/R3):** bearing γ_Rv 1,0/1,4/1,0 · sliding γ_Rh 1,0/1,1/1,0.
  (Overall stability earth resistance γ_Re: 1,0/1,1/1,0, Table A.14.)
- **Params γ_M (M1/M2):** tanφ′ 1,0/1,25 · c′ 1,0/1,25 · c_u 1,0/1,4 · q_u 1,0/1,4 · γ 1,0/1,0.
- **Actions side (from EN 1990/1991, not the geotech code):** two input modes (D38). **Default —**
  the engineer enters the structural engineer's **already-combined design actions** directly (ULS
  `V_d`/`H_d`/`M_d` + SLS), consumed as-is. **Fallback —** enter **characteristic** `G_k`/`Q_k` and let
  the tool apply the factors here (γ_G/γ_Q above; ψ₀ from EN 1990; SLS partial factors = 1 with
  quasi-permanent ψ₂). **DA1 caveat:** because DA1 factors actions **two ways** (A1 for Comb 1, A2 for
  Comb 2), direct entry needs *both* factored sets — otherwise use the characteristic fallback. When
  design actions are entered directly, record which combination/A-set they carry so the correct M/R
  sets pair and no double-factoring occurs. Full model in the stage doc "Actions & load combinations".
- **DA2 vs DA2\* (matters for eccentric loads):** DA2 factors the **actions at source** → design
  eccentricity `e_d` (large) → smaller `B′`; **DA2\*** runs the whole calc on characteristic values
  and factors **only the effect at the end** → characteristic eccentricity `e_k` (small) → larger
  `B′`. `e_d ≫ e_k`, so **DA2\* is the more economic** (less conservative) route. This nuance is a
  genuine refinement of the "different factors" story — the *place* the factor enters changes the
  geometry, not just the margin.
- **Worked calibration (φ′_k = 38°, strip, size B to give R_d = V_d):** DA2 → **B = 1,21 m**
  (smallest) · DA1 (C2 governs) → **B = 1,50 m** · DA3 → **B = 1,74 m** (largest). Ranking holds for
  all φ′ not-too-small: [R_d/V_d vs φ′_k for DA1/DA2/DA3](../Sources/Handbooks/figures/JRC2013_FigA.3.12_utilization-ratio-Rd-Vd_vs_phi_DA1-DA2-DA3.png).
  Use this worked set as a **validation/test case** for the bearing engine.

## 5.x(AU) — Safety format: risk-based φg — AU pack
*The AU fork of the same split.* One **geotechnical strength reduction factor φg** multiplies the
**ultimate** geotechnical strength; actions (`S*`) come factored per **AS 5100.2** for ULS (not
re-tabulated here), and **serviceability loads** are used for the SLS displacement check (§10.3.5).
Governing: bearing `S* ≤ φg·R_ug` (§10.3.3.3); sliding `H_ug·φg + E_pr·φg ≥ S*` (§10.3.3.4).
*(AS 5100.3 §7.3.5 general principle; §10.3.3 values, Tables 10.3.3(A)/(B).)*

### φg range by method of assessing R_ug — Table 10.3.3(A)
| Method of assessing ultimate geotechnical strength | φg range |
|---|---|
| Analysis using parameters from appropriate **advanced in-situ** tests | 0,50–0,65 |
| Analysis using parameters from appropriate **advanced laboratory** tests | 0,45–0,60 |
| Analysis using **CPT** | 0,40–0,50 |
| Analysis using **SPT** | 0,35–0,40 |

### Where in the range — Table 10.3.3(B) guide (lower end ← → upper end)
Limited SI ↔ comprehensive SI · simple calc ↔ sophisticated method · limited construction control ↔
rigorous control · severe consequence of failure ↔ less severe · significant cyclic loading ↔ mainly
static · permanent structure ↔ temporary · published correlations ↔ site-specific correlations.

- **The structural contrast with EC (why this stress-tests the abstraction):** φg is **not fixed
  upstream** — it is chosen **per bearing/sliding check, inside Stage 5**, from *how* `R_ug` was
  assessed and *how good* the evidence is. Same ultimate `R_ug` (from the pack-shared 5.2 mechanics),
  but a single, explicit, **gated** reduction at the end — versus EC's front-loaded, project-wide
  DA factor sets. This is the design-side of divergence #2 (interpretation value basis). See the
  stage doc's "per-pack safety format" section.
- **AU worked/validation set:** *(none in the sources yet — AS 5100.3 gives no worked example; the EC
  worked calibration above is the test case. A parallel AU calibration is a candidate to author.)*

## Status — Stage 5 shallow foundation — step C complete ✓
Now carries **both packs and both source classes**:
- **EC pack default** *(JRC 2013)* — direct method: presumed bearing (5.1), bearing (factors +
  corrections + eccentricity, 5.2), sliding (5.3), Terzaghi–Peck indirect (5.4), SLS (Annex-H limits +
  elasticity + consolidation, 5.5), DA1/2/3(+DA2\*) format with a worked calibration + 4 chart figures.
- **AU pack** *(AS 5100.3 §10)* — pack-shared mechanics reused for `R_ug`; forked safety format §5.x(AU)
  (φg Tables 10.3.3(A)/(B)); sliding §10.3.3.4; SLS §10.3.5. Per-step pack notes in 5.2(f)/5.3/5.5(d).
- **Non-code** *(Look 2007 Ch 21/23)* — presumed-bearing Tbl 21.3 (5.1a), Meyerhof N-fallbacks for
  bearing (Tbl 21.7) and settlement (Tbl 21.8), Vesic/Hansen factor cross-check (Tbl 21.5), FoS
  (Tbl 21.9), tolerable-movement limits (Tbl 23.6–23.8) — cross-linked to §2.7(a)/(b)/(f).

**Stage 5 complete (A+B+C).** Procedures in `stages/05-shallow-foundation-procedures.md`. One
candidate follow-up flagged in §5.x(AU): author a parallel **AU φg worked calibration** as a test
case (AS 5100.3 ships none).

---

# Stage 6 — Axial pile design

Method registry for the axial-pile module (`stages/06-axial-pile.md` = the stage spec). Organized
**by procedure step → method**. **First seeding is the API method** (added on founder request); the
full step-C seeding (JRC 2013 Ch 8 / Annex A.8 *(EC guidance)*, AS 2159 *(code)* risk-φg, Look Ch 21
*(non-code)* α/β/N-fallbacks — much already in §2.7c, and the CPT methods once sourced) slots into the
same steps.

> **Selection-rule reminder (piles):** capacity **methods** (α, β, CPT, N-method, API) are largely
> **pack-agnostic** — they each compute an ultimate shaft/base resistance `R_cal`/`R_ug`. The
> **safety format** is what forks (and here it is **four**-way): EC7 partial factors + ξ (LRFD),
> AS 2159 risk-φg (LSD), **API global FoS (WSD)**, and **AASHTO method-keyed φstat (US LRFD)**. So a
> method entry says *how to compute resistance*; the pack says *how to make it safe*. Offer + compare
> per the header rule; the engineer selects the method and owns the reliability judgment (ξ / risk
> assessment / FoS / φstat).

## 6.2 — Axial capacity: shaft f_s & base q_b — API RP 2A-WSD method
*Stage task 6.2.* The offshore-standard α/β formulation. **Ultimate capacity** `Q_d = Q_f + Q_p =
f·A_s + q·A_p` (§6.4.1); shaft f acts inside + outside; base capped by the internal-plug capacity
(plugged → full section, unplugged → annulus only). *(API RP 2A-WSD §6.4; High reliability — extensive
offshore load-test calibration; **pack-agnostic capacity**, WSD safety format is API's own — see §6.x.)*

### 6.2(a) Cohesive soil (clay) — α method  §6.4.2
- **Shaft:** `f = α·c` (c = undrained shear strength at the point).
- **α from ψ = c/p′₀** (p′₀ = effective overburden at the point):
  `α = 0,5·ψ^(−0,5)` for ψ ≤ 1,0 · `α = 0,5·ψ^(−0,25)` for ψ > 1,0 · **α ≤ 1,0**.
  (Underconsolidated clay → α ≈ 1,0; apply judgment for ψ > 3, sparse load-test data.)
- **Base:** `q = 9·c` (N_c = 9).

### 6.2(b) Cohesionless soil (sand/silt) — β / K method  §6.4.3
- **Shaft:** `f = K·p₀·tanδ` — K = **0,8** (open-ended, driven unplugged, tension & compression),
  **1,0** (full-displacement, plugged/closed-end). δ from Table 6.4.3-1.
- **Base:** `q = p₀·N_q` (N_q from Table 6.4.3-1); both f and q **capped** at the table limits.
- **Table 6.4.3-1 — design parameters, cohesionless siliceous soil** *(guideline; CPT/lab may justify
  other values; **not for carbonate / micaceous / volcanic sands** — those need site-specific tests):*

  | Density / description | δ (°) | limiting f, kPa | N_q | limiting q, MPa |
  |---|---|---|---|---|
  | Very loose sand · loose sand-silt · medium silt | 15 | 47,8 | 8 | 1,9 |
  | Loose sand · medium sand-silt · dense silt | 20 | 67,0 | 12 | 2,9 |
  | Medium sand · dense sand-silt | 25 | 81,3 | 20 | 4,8 |
  | Dense sand · very dense sand-silt | 30 | 95,7 | 40 | 9,6 |
  | Dense gravel · very dense sand | 35 | 114,8 | 50 | 12,0 |

  [Rendered table (authoritative)](../Sources/Handbooks/figures/API-RP-2A_Table6.4.3-1_cohesionless-design-params.png).

### 6.2(c) Grouted piles in rock  §6.4.4
- Shaft ≤ triaxial shear strength of rock or grout (usually **much less**, per installation
  disturbance; limit may be the steel-grout bond, §7.4.3). Base ≤ **9,58 MPa** (100 tsf).

### 6.2(d) Pullout / tension  §6.5
- Ultimate pullout ≤ **Q_f** (total skin friction only; no base). Clay f per (a), sand/silt f per (b),
  rock per (c). Include effective pile weight + hydrostatic uplift + soil-plug weight.

### 6.2(e) CPT direct methods — *(FHWA GEC-12 Ch 7)*
Two direct CPT correlations — **pack-agnostic capacity** (compute R_s + R_p from CPT profiles); the
AASHTO φstat that GEC-12 pairs with them is one safety-format option (§6.x(US) below).

**Eslami–Fellenius (1997) — CPTu, mixed soils** *(GEC-12 §7.2.1.3.6; High for CPTu):*
- **Shaft:** `f_s = C_s·q_E` (Eq 7-24), with **effective (Eslami) cone stress** `q_E = q_t − u₂`
  (Eq 7-25) and pore-corrected tip `q_t = q_c + u₂·(1−a)` (Eq 7-26). `C_s` = shaft coefficient by soil
  type (Tbl 7-11): soft-sensitive **8,0%** · clay **5,0%** · silty/stiff clay & silt **2,5%** · sandy
  silt/silt **1,5%** · fine/silty sand **1,0%** · sand **0,4%**.
- **Toe:** `q_p = C_p·q_Eg` (Eq 7-27); `q_Eg` = **geometric mean** of q_E over the toe influence zone
  (4b below → 8b above through weak-into-dense; 4b below → 2b above dense-into-weak); `C_p = 1,0`
  except piles > 16 in (Eq 7-28). *(No AASHTO φstat — use the field-verification φdyn.)*

**Nottingham–Schmertmann (1975) — CPT, mixed soils** *(GEC-12 §7.2.1.3.7; AASHTO **φstat = 0,50**):*
- **Shaft, cohesionless:** `R_s = K_s·[Σ(f_s·A_s)₀–₈b + Σ(f_s·A_s)₈b–D]` (Eq 7-29); `K_s` = f(D/b, pile
  material, cone type) from Fig 7-19. No-sleeve-data fallback: `R_s = C_f·q_c·A_s` (Eq 7-30), `C_f`
  (Tbl 7-12): precast concrete 0,012 · timber 0,018 · steel displacement 0,012 · open-end steel pipe
  0,008.
- **Shaft, cohesive:** `R_s = α′·f_s·A_s` (Eq 7-31), `α′` = f(f_s) from Fig 7-20 (Tomlinson-α patterned).
- **Toe:** weighted average of q_c from 8b above to 3,75b below the toe, favouring lower q_c (Fig 7-21);
  **limit q_p 105–315 ksf** (≈5–15 MPa); mechanical cone in cohesive → reduce q_p **40%**.
- Figures: [Fig 7-19/7-20 shaft K_s & α′ curves](../Sources/Handbooks/figures/FHWA-GEC12_Fig7.19-20_Nottingham-Schmertmann_shaft-Ks-alpha-curves.png)
  · [Fig 7-21 toe averaging](../Sources/Handbooks/figures/FHWA-GEC12_Fig7.21_Nottingham-Schmertmann_toe-resistance-averaging.png).

*(ICP-05 and the other offshore CPT-2005 methods — UWA/Fugro/NGI — are candidates to add from the
API/ISO 19901-4 commentary or Jardine et al. 2005 when sourced.)*

## 6.5 — SLS: axial load-transfer (t-z) & tip (Q-z) curves — API  §6.7
*Stage task 6.5.* Non-linear soil springs for pile settlement/load-transfer analysis. `t_max = f`
(unit skin friction from §6.4); full tip mobilisation needs **z ≈ 0,10·D**. *(Non-carbonate soils;
recommended in the absence of site-specific curves.)*

- **t-z (shaft), clay:** z/D → t/t_max = 0,0016→0,30 · 0,0031→0,50 · 0,0057→0,75 · 0,0080→0,90 ·
  0,0100→1,00 · 0,0200→(0,70–0,90) · ∞→(0,70–0,90 residual).
- **t-z (shaft), sand:** z (in) → t/t_max = 0→0 · 0,10→1,00 · ∞→1,00.
- **Q-z (tip), sand & clay:** z/D → Q/Q_p = 0,002→0,25 · 0,013→0,50 · 0,042→0,75 · 0,073→0,90 ·
  0,100→1,00. Residual adhesion ratio t_res/t_max = **0,70–0,90**.
- [Rendered t-z figure 6.7.2-1](../Sources/Handbooks/figures/API-RP-2A_Fig6.7.2-1_t-z_axial-load-transfer-curves.png).
- *Lateral p-y curves (§6.8, Matlock soft clay etc.) — the lateral analysis is adjacent to the axial
  MVP core (stage doc 6.5.2 scope note); noted, not seeded here.*

## 6.x(API) — Safety format: Working Stress Design (global FoS) — API
*The third safety-format paradigm* (alongside EC7 LRFD §ξ/partial factors and AS 2159 LSD φg). API
sizes the pile so **allowable capacity = ultimate/FoS ≥ working load**; FoS by load condition
(§6.3.4):

| Load condition | FoS |
|---|---|
| Design environmental + appropriate drilling loads | 1,5 |
| Operating environmental during drilling | 2,0 |
| Design environmental + appropriate producing loads | 1,5 |
| Operating environmental during producing | 2,0 |
| Design environmental + minimum loads (pullout) | 1,5 |

- **Architectural note:** because API is **WSD**, its capacity methods §6.2 pair with **unfactored
  (working) actions** and a **global FoS** — *not* with the EC/AU factored-action machinery. When the
  engineer selects API capacity methods *inside* an EC or AS project, only the **capacity numbers**
  (`R_cal`) transfer; the safety format stays the active pack's (ξ/φg), **never mix FoS with partial
  factors**. API's own WSD format is used when **API is the governing standard** (offshore/marine).
- FoS ranges are lower than typical onshore (1,5–2,0) because they pair with **environmental load
  conditions** already defined conservatively — another reason not to transplant them across packs.

## 6.x(US) — Safety format: AASHTO method-keyed φstat (US LRFD) — FHWA GEC-12
*The fourth safety-format paradigm.* US LRFD: `R_r = φstat·R_n` — a **resistance factor calibrated
per static-analysis method**, so the factor *travels with the method*, not with a project-wide choice
(EC DA) or a site risk score (AS 2159). Examples (GEC-12 Tbl 7-3, AASHTO 2014): Meyerhof **0,30** ·
α-method **0,35** · λ-method **0,40** · Nordlund **0,45** · Schmertmann/CPT **0,50**. Methods without a
calibrated φstat (e.g. Eslami–Fellenius) fall back to the **field-verification** factor φdyn.
- **Contrast with AS 2159:** both are LRFD-style single resistance factors, but AS derives φg from a
  **site/design/installation risk assessment** (project-specific), whereas AASHTO φstat is a
  **reliability-calibrated constant per method** (method-specific). Same equation shape, opposite
  source of the number — reinforcing that the safety format is a *gated procedure whose form is
  pack-specific*, the abstraction the pile module exists to surface.
- [Methods summary + φstat table (GEC-12 Tbl 7-3)](../Sources/Handbooks/figures/FHWA-GEC12_Table7.3_static-methods-summary-AASHTO-phistat.png).

## Status — Stage 6 axial pile (seeding in progress)
**Seeded so far:**
- **API RP 2A-WSD** *(offshore, WSD)* — α (clay), K·p₀·tanδ + N_q (sand, Tbl 6.4.3-1), rock, pullout,
  t-z/Q-z curves, WSD FoS (§6.x(API)).
- **FHWA GEC-12** *(US LRFD)* — **CPT direct methods** Eslami–Fellenius & Nottingham–Schmertmann
  (§6.2e) + the AASHTO method-keyed **φstat** safety format (§6.x(US)).

**Safety-format paradigms now covered: four** — EC7 partial-factors+ξ, AS 2159 risk-φg, API WSD FoS,
AASHTO φstat.

**Still to add (finish step C):** JRC 2013 Ch 8/Annex A.8 *(EC guidance)*, AS 2159 risk-φg machinery
*(code)*, Look Ch 21 α/β/N-method *(non-code, much in §2.7c)*; **LCPC/Bustamante** and **ICP-05 / UWA /
Fugro / NGI** offshore CPT-2005 methods once their sources are added.
