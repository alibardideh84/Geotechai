# 03 — Workflow and the human/AI model

## The per-step loop (the differentiator)
Every stage runs the same loop:
1. **AI proposes** — runs the validated calc engine for the step, cites the code clause.
2. **Engineer reviews / overrides** — method and inputs are fully editable.
3. **Approve & sign** — unlocks the next step. (Or **revise / re-run** loops back to the AI.)

The engineer is the **expert of record and accountable**; the AI does ~95% of the work.

## Gate model — scheduled + exception gates
Control operates through two kinds of sign-off gate:

- **Step gates (scheduled):** one at the end of every step — always present. The engineer reviews the step's output and approves before the next step runs.
- **Exception gates (conditional, event-driven):** fire *mid-task* whenever a trigger condition is met. The AI pauses and surfaces the issue with options, its recommended option + rationale, a confidence level, and the impact; the engineer decides. Every decision is logged in the audit trail.

**Triggers** (any one opens an exception gate):

| Trigger | Example | Default handling |
|---|---|---|
| Error / validation failure | out-of-range value, corrupt input, calc error | blocking |
| Missing / insufficient data | required input absent | blocking (AI may offer an estimate to approve) |
| Multiple routes / methods | ≥2 applicable methods, consequential choice | decision (AI recommends a default) |
| Methods disagree materially | scatter/divergence beyond threshold | decision |
| Low confidence / accuracy | reliability flag, no-lab fallback, wide scatter | advisory → decision if it affects a governing value |
| Inconsistency / conflict | cross-test, cross-location, vs prior/regional | decision |
| Out-of-applicability | method used outside valid conditions | blocking |
| Assumption required | AI had to assume a value/condition | decision (accept/edit) |
| Design-critical threshold | result governs a limit state / crosses a critical value | decision (force review) |
| Code Pack silent / no method | selected standard has no recommended method for this case | decision — AI proposes a method from another reference (other code / research), flagged *outside selected standard*, for approval |

**At an exception gate** the engineer can: choose a method · override · supply data · accept/edit an assumption · request more investigation · accept-with-note.

**Fatigue management:** severity is **blocking** (must resolve to continue) or **advisory** (flagged, batch-reviewed at the step gate). Thresholds are engineer-configurable; default conservative (surface more rather than less). Code-agnostic — the applicability conditions and value basis behind several triggers come from the selected Code Pack.

## Notices register (compiled into the report)
Every gate event and decision is captured and **compiled into a register at the end of the report** — cautions, assumptions, reminders, and notices — so the deliverable is self-documenting and the engineer's accountability is traceable. Entries include:
- **Assumptions** made and approved.
- **Low-confidence / low-accuracy** derivations (reliability flags, no-lab fallbacks).
- **Method selections** where ≥2 routes existed — which was chosen and why.
- **Conflicts / inconsistencies** and how they were resolved.
- **Missing-data estimates** and their basis.
- **Out-of-applicability** overrides.
- **Out-of-code methods** — selected standard silent, alternative reference adopted (prominently flagged with the external reference).
- **Design-critical items** and outstanding recommendations (e.g. further investigation).

Each entry records: *item · location/context · trigger · options considered · decision · rationale · approver · date.*

## The MVP workflow spine (8 stages)
Everything funnels into the **ground model**, which feeds the two design modules **in parallel**; both feed one report. Every transition is an engineer sign-off gate.

```
setup & code → data intake → interpretation ─┐
                                             ▼
                                        ground model
                                        (characteristic values)
                                        ┌──────┴──────┐
                                        ▼             ▼
                                 shallow foundation  axial pile
                                        └──────┬──────┘
                                               ▼
                                          report / GDR
```

| # | Stage | What the AI does (≈95%) | Engineer sign-off gate |
|---|---|---|---|
| 0 | Setup & code | Reads project type; proposes geotechnical category, design approach + partial-factor set from the selected code/national annex; sets units, seismicity | Code, national annex, category & design approach confirmed |
| 1 | Data intake | Lists data needed; ingests uploads (any format); digitizes BH logs, in-situ (SPT/CPT/CPTu/DMT/PMT) & lab tests; validates; flags gaps & outliers | Verified dataset |
| 2 | Interpretation | Derives parameters per test via chosen correlations; shows scatter + source; produces preliminary design recommendations (below) | Approved derived parameters + recommendations |
| 3 | Ground model | Proposes stratigraphy across boreholes, groundwater regime, and a cautious **characteristic value** per layer/parameter with rationale; supports multiple constitutive models | Approved ground model (the master input) |
| 4 | Design basis & actions | Sets geometry options; assembles actions & load combinations; applies partial factors per the design approach | Loads & combinations confirmed |
| 5 | Shallow foundation | Runs selected method(s): ULS bearing (+ sliding, + basic SLS settlement), eccentricity/inclination, utilization & governing case, sizing | Approved footing design |
| 6 | Axial pile | Runs α/β/CPT methods for shaft + base; applies EC7 ξ correlation & resistance factors; compares methods; resistance vs. action | Approved pile design |
| 7 | Report / GDR | Assembles calc package: inputs, ground model, methods + citations, results, and every gate approval as the audit trail | Final sign-off |

## Flexible entry & input contracts (Accepted)
The engineer is **not forced to start from scratch** — they can start at **any stage**. Stages are modular; each declares an **input contract**. Inputs come either from the upstream in-app stage **or** are uploaded/entered directly. If earlier stages are skipped, the system requests those deliverables as direct inputs.

| If you start at… | …you provide directly (instead of the upstream stage) |
|---|---|
| 1 Data intake | raw field/lab files |
| 2 Interpretation | digitized test data |
| 3 Ground model | derived parameters + logs, or an existing ground model |
| 5 Shallow foundation | characteristic parameters + footing geometry + actions |
| 6 Axial pile | parameters / fs–qb profiles + pile geometry + actions |

## Consistency engine (Accepted) — cross-cutting, mainly stages 1–3
When prior site investigations, nearby/adjacent boreholes, regional geology, or published data exist, the AI compares the new data's consistency against them, flags conflicts, and recommends a preferred dataset/approach to reconcile — the engineer decides.

## Preliminary design recommendations in interpretation (Accepted)
Interpretation outputs design-ready profiles before the design stage: unit skin friction f_s vs depth, pile base capacity q_b vs depth, and typical bearing capacity for general shallow-foundation types (Geotechnical Interpretive Report style).

## Three must-stay-human judgment gates (EC7)
1. **Design approach & partial factors** (stage 0) — DA1/DA2/DA3 changes the whole answer.
2. **Characteristic values** (stage 3) — EC7 §2.4.5.2 "cautious estimate" is judgment, not a formula.
3. **Pile ξ / model factors** (stage 6) — number of test profiles, correlation factor.
