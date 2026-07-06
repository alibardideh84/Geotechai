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
  *Source* column. File: `Sources/Handbooks/Handbook+of+Geotechnical+Investigation&DesignTables 1`.
  - **Figures/garbled tables:** tables that don't extract cleanly from the txt are rendered from
    the PDF (PyMuPDF) and saved to `Sources/Handbooks/figures/` as named PNGs (authoritative); the
    library links them and transcribes the decision-relevant content.

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
