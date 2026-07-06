# 05 — MVP scope

## Definition
MVP = **investigation interpretation** + **two design modules**: **shallow foundation bearing capacity** and **axial pile capacity**.

## Rationale
- Both sit directly on top of the ground model and are universally needed.
- Axial pile is heavily CPT-method-driven, which ties tightly to interpretation (CPT ingestion) — the two modules share a lot of plumbing.
- Both have clean, code-traceable formulas to showcase the validated-engine + step-by-step sign-off loop.

## v1 method set (what we build first)
- **Shallow foundation:** Terzaghi, Meyerhof, Hansen, Vesić + EN 1997-1 Annex D (with shape/depth/inclination/groundwater corrections, drained vs undrained). Plus a basic SLS settlement check.
- **Axial pile:** α (Tomlinson), β (effective stress) + 2–3 CPT methods (e.g. LCPC/Bustamante, Eslami-Fellenius, ICP), with EC7 ξ correlation & resistance factors.

> Note: a *basic* settlement (SLS) check rides along with the shallow footing. The full **Settlement Analysis** module (embankment, pile groups) stays Priority 3 — keep it out of the MVP to avoid scope creep.

## Open questions
- **OPEN/Proposed — advanced constitutive models in the MVP.** Recommended: make the data model constitutive-agnostic from day one (never get boxed into c-φ), but only wire the **derivation methods** the two MVP modules consume (mostly strength + stiffness), and add full Hardening Soil / Cam-Clay derivations alongside the settlement/deformation modules. _Awaiting founder confirmation: agnostic data model now with derivations deferred, vs. advanced derivations active in MVP._

## Next planned step
Fully specify the **interpretation → ground-model** stage: its sub-steps, the data each needs, the v1 derivation methods, what the AI proposes, and what the engineer sees at each gate. This stage also hosts the consistency engine, multi-constitutive soil models, characteristic-value judgment, and the preliminary design recommendations — so it is the richest and highest-leverage part to detail first.
