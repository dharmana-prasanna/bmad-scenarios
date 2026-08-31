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

## Sequence (`<< in` / `>> out`)

```
YOU          SKILL / ROOM
 |                |
 |                 bmad-help
 |                      << in:  "city flood SMS alerts" + empty project
 |                      >> out: (none — "forge it, it might die")
 |
 |== WILL THIS IDEA SURVIVE? ============================
 |                 bmad-forge-idea
 |                      << in:  the founder's attached idea
 |                      >> out: forge/forge-report.html
 |                      >> out: forge/forged-idea.md
 |                 HARDENS — but only as "advisory nowcast +
 |                 human confirm", not a guaranteed warning system.
 |
 |== DECISION-GRADE RESEARCH ============================
 |                 bmad-deep-recon  type=academic-lit
 |                      << in:  flood nowcast literature question
 |                      >> out: research/lit-flood-nowcast.md
 |                 bmad-deep-recon  type=domain
 |                      << in:  tide datums / floodplain terms
 |                      >> out: research/domain-tide-datums.md
 |                 bmad-deep-recon  type=technical
 |                      << in:  cheap sensor mesh + SMS page-out
 |                      >> out: research/technical-sensor-mesh.md
 |                 bmad-deep-recon  type=user-voice
 |                      << in:  residents of flooded wards
 |                      >> out: research/user-voice-flooded-wards.md
 |                 bmad-deep-recon  type=competitive
 |                      << in:  FEMA / IBWC / city siren apps
 |                      >> out: research/competitive-fema-ibwc.md
 |                 bmad-deep-recon  shape=select
 |                      << in:  SMS vs push vs NOAA weather radio
 |                      >> out: research/select-alert-channel.md
 |
 |== PARTY: LIABILITY + ETHICS ==========================
 |                 bmad-party-mode  --party installed --mode subagent
 |                      << in:  "can we ship a warning that isn't 911?"
 |                      << in:  forged-idea.md + recon/*.md
 |                      >> out: party-mode/memories/installed/.memlog.md
 |                      >> out: party-mode/2026-03-12-harborwatch-liability.html
 |                 UNRESOLVED CLASH (correct): John wants disclaimer-
 |                 heavy UX; Sally says that trains people to ignore alerts.
 |
 |== WORKING BACKWARDS, NOT A GENTLE BRIEF ==============
 |                 bmad-prfaq
 |                      << in:  hardened idea + recon + party takeaway
 |                      >> out: planning/prfaq-harborwatch.md
 |
 |                 bmad-prd   intent=create
 |                      << in:  prfaq-harborwatch.md + recon/*.md
 |                      >> out: planning/prd.md, addendum.md, .memlog.md
 |                 bmad-review  lenses=adversarial,verification-gap
 |                      << in:  planning/prd.md
 |                      >> out: review/prd-findings.json + .md
 |                 bmad-prd   intent=validate
 |                      << in:  finished prd.md + shipped rubric (not a user file)
 |                      >> out: planning/prd-validation.html + .md
 |
 |== HOW, THEN WHAT =====================================
 |                 bmad-architecture
 |                      << in:  prd.md (no UX — SMS-first)
 |                      >> out: planning/ARCHITECTURE-SPINE.md
 |
 |                 bmad-spec   epic=sms-nowcast
 |                      << in:  prd.md + spine (condensed)
 |                      >> out: specs/spec-sms-nowcast/SPEC.md + stories.yaml
 |                 bmad-create-epics-and-stories
 |                      << in:  spine + prd.md
 |                      >> out: planning/epics/*.md
 |                 bmad-sprint-planning
 |                      << in:  epics + stories
 |                      >> out: sprint-status.yaml + readiness CONCERNS
 |
 |                 bmad-project-context  setup
 |                      << in:  the repo; pitfall: never page on estimated tide
 |                      >> out: AGENTS.md
 |
 |== SHIP (human-gated — life safety) ===================
 |                 bmad-build          (per story)
 |                      << in:  one story + SPEC.md + codebase
 |                      >> out: code + impl records
 |                 bmad-qa-generate-e2e-tests
 |                      << in:  implemented alert pipeline + SPEC success signal
 |                      >> out: tests/e2e/alert-pipeline.spec.ts
 |                 bmad-retrospective
 |                      << in:  spec-sms-nowcast/ + stories.yaml + impl + code
 |                      >> out: implementation/retro-sms-nowcast.md + verdict
 v                v
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
