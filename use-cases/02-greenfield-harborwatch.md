# 02 — HarborWatch (greenfield, research-heavy, civic)

**Field:** greenfield  
**Size:** project  
**Why this path:** the idea might deserve to die; liability and evidence matter more than a cute brief; a city partner must sign a PRD.

A coastal-resilience nonprofit wants **HarborWatch**: SMS + app alerts when a neighborhood will flood in the next 6 hours, using public tide/NOAA data plus a cheap sensor mesh. The founder is emotionally attached. A city CIO will only fund it if the product survives Working Backwards *and* a liability debate.

---

## Skills used (and skipped)

Used: `bmad-help`, `bmad-forge-idea`, `bmad-deep-recon` (academic-lit, domain, technical, user-voice, competitive, select), `bmad-party-mode`, `bmad-prfaq`, `bmad-prd`, `bmad-review`, `bmad-architecture`, `bmad-spec`, `bmad-create-epics-and-stories`, `bmad-sprint-planning`, `bmad-project-context`, `bmad-build`, `bmad-qa-generate-e2e-tests`, `bmad-retrospective`.

Skipped: `bmad-brainstorming` (they already have one idea), `bmad-product-brief` (PRFAQ is the right gauntlet), `bmad-ux` (v1 is SMS-first; app shell is later), `bmad-build-auto` (life-safety code stays human-gated), `bmad-correct-course`.

---

## Sequence (artifacts on the right)

```
YOU          SKILL / ROOM                         ARTIFACTS WRITTEN
 |                |                                      |
 |                 bmad-help                             |  (none — "forge it, it might die")
 |                |                                      |
 |== WILL THIS IDEA SURVIVE? ============================|
 |                 bmad-forge-idea                       |  forge/forge-report.html
 |                |  personas: city CIO, NOAA hydrologist|  forge/forged-idea.md
 |                |  insurance counsel, neighborhood lead|
 |                |  HARDENS — but only as "advisory     |
 |                |  nowcast + human confirm", not a     |
 |                |  guaranteed warning system.          |
 |                |                                      |
 |== DECISION-GRADE RESEARCH ============================|
 |                 bmad-deep-recon  type=academic-lit    |  research/lit-flood-nowcast.md
 |                 bmad-deep-recon  type=domain          |  research/domain-tide-datums.md
 |                 bmad-deep-recon  type=technical       |  research/technical-sensor-mesh.md
 |                 bmad-deep-recon  type=user-voice      |  research/user-voice-flooded-wards.md
 |                 bmad-deep-recon  type=competitive     |  research/competitive-fema-ibwc.md
 |                 bmad-deep-recon  shape=select         |  research/select-alert-channel.md
 |                |  (SMS vs push vs NOAA weather radio) |  (+ optional HTML briefings)
 |                |                                      |
 |== PARTY: LIABILITY + ETHICS ==========================|
 |                 bmad-party-mode                       |
 |                 --party installed --mode subagent     |
 |                |                                      |
 |                |  Mary   : false-negative rates       |
 |                |  John   : city will not fund a       |
 |                |           product that implies 911   |
 |                |  Winston: no ML black box in the     |
 |                |           alert path; deterministic  |
 |                |           thresholds only            |
 |                |  Sally  : alert copy must not sound  |
 |                |           like an evacuation order   |
 |                |  Amelia : sensor firmware OTA is a   |
 |                |           separate trust boundary    |
 |                |                                      |
 |                |  UNRESOLVED CLASH (correct):         |
 |                |  John wants a disclaimer-heavy UX;   |
 |                |  Sally says that trains people to    |
 |                |  ignore alerts. They do not hug.     |
 |                |                                      |  party-mode/memories/installed/.memlog.md
 |                |                                      |  party-mode/2026-03-12-harborwatch-liability.html
 |                |                                      |
 |== WORKING BACKWARDS, NOT A GENTLE BRIEF ==============|
 |                 bmad-prfaq                            |  planning/prfaq-harborwatch.md
 |                |  press release written as if the     |
 |                |  city launched yesterday; FAQ kills  |
 |                |  "we predict floods" language        |
 |                |                                      |
 |                 bmad-prd   intent=create              |  planning/prd.md
 |                |           reads PRFAQ + recon        |  planning/addendum.md
 |                |                                      |  planning/.memlog.md
 |                 bmad-review                           |  review/prd-findings.json
 |                 lenses=adversarial,verification-gap   |  review/prd-findings.md
 |                 bmad-prd   intent=validate            |  planning/prd-validation.html
 |                |                                      |  planning/prd-validation.md
 |                |                                      |
 |== HOW, THEN WHAT =====================================|
 |                 bmad-architecture                     |  planning/ARCHITECTURE-SPINE.md
 |                |  invariants: deterministic threshold |
 |                |  engine; SMS provider swappable;     |
 |                |  no model in the page-out path       |
 |                |                                      |
 |                 bmad-spec   epic=sms-nowcast          |  specs/spec-sms-nowcast/SPEC.md
 |                |                                      |  specs/spec-sms-nowcast/stories.yaml
 |                 bmad-create-epics-and-stories         |  planning/epics/*.md
 |                 bmad-sprint-planning                  |  implementation/sprint-status.yaml
 |                |                                      |  readiness: CONCERNS
 |                |  (sensor hardware lead time)         |  (gate still proceeds with noted risk)
 |                |                                      |
 |                 bmad-project-context  setup           |  AGENTS.md
 |                |  pitfalls: never page on estimated   |
 |                |  tide alone; always attach station id|
 |                |                                      |
 |== SHIP (human-gated — life safety) ===================|
 |                 bmad-build          (per story)       |  code + impl records
 |                 bmad-qa-generate-e2e-tests            |  tests/e2e/alert-pipeline.spec.ts
 |                 bmad-retrospective                    |  implementation/retro-sms-nowcast.md
 v                v                                      v
```

---

## What this use case teaches

- `bmad-forge-idea` before research when the founder is attached — cheap death is a success.
- `bmad-prfaq` instead of `bmad-product-brief` when the customer (a city) must feel the launch day.
- `bmad-party-mode --mode subagent` when independent thinking matters (liability vs UX copy). Session mode would blend the voices.
- `bmad-deep-recon type=academic-lit` is a first-class research type, not an afterthought.
- Skip `bmad-ux` when the primary surface is SMS, not a screen.
- Do **not** use `bmad-build-auto` on the page-out path.

Party-mode mechanics (custom casts, focus groups, room switching) are expanded in [06](06-party-mode-atlas-health.md).
