# 01 — MealPlan AI (greenfield product)

**Field:** greenfield  
**Size:** project (~20+ build sessions, several epics)  
**Why this path:** UI is the product, more than one person must agree, several engineers will build in parallel.

A two-founder team wants a family meal-planning app: pantry + dietary constraints → weekly dinners + grocery list. Concept is fuzzy on day one. They need a signed-off PRD, UX, and an architecture spine before slicing work.

---

## Skills used (and skipped)

Used: `bmad-help`, `bmad-brainstorming`, `bmad-forge-idea`, `bmad-deep-recon`, `bmad-party-mode`, `bmad-product-brief`, `bmad-prd`, `bmad-advanced-elicitation`, `bmad-review`, `bmad-ux`, `bmad-architecture`, `bmad-spec`, `bmad-create-epics-and-stories`, `bmad-sprint-planning`, `bmad-build`, `bmad-code-review`, `bmad-checkpoint-preview`, `bmad-qa-generate-e2e-tests`, `bmad-build-auto`, `bmad-retrospective`, `bmad-project-context`, `bmad-customize`.

Skipped: `bmad-prfaq` (founders already sure of the concept — brief is enough), `bmad-correct-course` (no mid-flight explosion). Agents optional: any `bmad-agent-*` can sit on top and dispatch the same workflows.

---

## Sequence (artifacts on the right)

```
YOU          SKILL / ROOM                         ARTIFACTS WRITTEN
 |                |                                      |
 |-- "what first?" ------------------------------------->|
 |                 bmad-help                             |  (none — recommends brainstorm)
 |                |                                      |
 |== THINK ==============================================|
 |-- "help me ideate" ---------------------------------->|
 |                 bmad-brainstorming                    |
 |                |                                      |  brainstorming/brainstorm.html
 |                |                                      |  brainstorming/brainstorm-intent.md
 |                |                                      |
 |-- "pressure-test pantry-scan + plan" ---------------->|
 |                 bmad-forge-idea                       |
 |                |                                      |  forge/forge-report.html
 |                |                                      |  forge/forged-idea.md
 |                |                                      |
 |-- evidence, not vibes ------------------------------->|
 |                 bmad-deep-recon  type=market          |  research/market-family-meal-planning.md
 |                 bmad-deep-recon  type=user-voice      |  research/user-voice-home-cooks.md
 |                 bmad-deep-recon  type=competitive     |  research/competitive-mealime-paprika.md
 |                 bmad-deep-recon  type=technical       |  research/technical-vision-vs-barcode.md
 |                 bmad-deep-recon  shape=select         |  research/select-vision-vendor.md
 |                |                                      |  (+ optional HTML briefings)
 |                |                                      |
 |-- "put Mary, John, Winston, Sally, Amelia in a room ->|
 |     Should v1 even attempt pantry vision?"            |
 |                 bmad-party-mode                       |
 |                 party=installed  mode=session         |  party-mode/memories/installed/.memlog.md
 |                |                                      |  party-mode/2026-08-31-mealplan-v1-scope.html
 |                |  CLASH: Sally wants delightful scan; |
 |                |  Amelia wants barcode-only in v1;    |
 |                |  Winston refuses a vision pipeline   |
 |                |  without an architecture invariant.  |
 |                |  Outcome: v1 = manual pantry +       |
 |                |  barcode; vision is a later epic.    |
 |                |                                      |
 |== WRITE THE PRODUCT ==================================|
 |-- concept is now solid ------------------------------->|
 |                 bmad-product-brief                    |  planning/brief.md
 |                |                                      |  planning/addendum.md
 |                |                                      |
 |-- org (two founders + advisor) needs sign-off ------->|
 |                 bmad-prd   intent=create              |  planning/prd.md
 |                |                                      |  planning/addendum.md
 |                |                                      |  planning/.memlog.md
 |                |                                      |
 |-- draft feels like LLM first pass ------------------->|
 |                 bmad-advanced-elicitation             |  (rewrites sections of prd.md in place)
 |                 method=pre-mortem + red-team          |
 |                |                                      |
 |-- "review the PRD" ---------------------------------->|
 |                 bmad-review                           |  review/prd-findings.json
 |                 lenses=adversarial,structure,prose    |  review/prd-findings.md
 |                |                                      |
 |-- validate against checklist ------------------------>|
 |                 bmad-prd   intent=validate            |  planning/prd-validation.html
 |                |                                      |  planning/prd-validation.md
 |                |                                      |
 |-- UI is the product --------------------------------->|
 |                 bmad-ux                               |  planning/DESIGN.md
 |                |                                      |  planning/EXPERIENCE.md
 |                |                                      |  planning/.memlog.md
 |                |                                      |
 |-- several streams will build in parallel ------------>|
 |                 bmad-architecture                     |  planning/ARCHITECTURE-SPINE.md
 |                |                                      |
 |-- lock WHAT for epic-1 pantry-to-plan --------------->|
 |                 bmad-spec                             |  specs/spec-pantry-to-plan/SPEC.md
 |                |  + story breakdown                   |  specs/spec-pantry-to-plan/stories.yaml
 |                |                                      |  specs/spec-pantry-to-plan/<companions>
 |                |                                      |
 |== SLICE + GATE =======================================|
 |                 bmad-create-epics-and-stories         |  planning/epics/*.md
 |                |                                      |  (story files with AC / BDD)
 |                |                                      |
 |                 bmad-sprint-planning                  |  implementation/sprint-status.yaml
 |                |                                      |  readiness: PASS | CONCERNS | FAIL
 |                |                                      |
 |== TEACH THE REPO =====================================|
 |                 bmad-project-context  setup           |  AGENTS.md  (managed block)
 |                 bmad-customize                        |  _bmad/custom/bmad-build.user.toml
 |                |  (always run pnpm, never npm)        |
 |                |                                      |
 |== SHIP (repeat per story) ============================|
 |                 bmad-build                            |  code in repo
 |                |                                      |  specs/.../impl/<story> record
 |                |                                      |
 |                 bmad-code-review                      |  review findings + optional patches
 |                 bmad-checkpoint-preview               |  (walkthrough — usually no file)
 |                |                                      |
 |  later stories, patterns stable:                      |
 |                 bmad-build-auto                       |  code + impl record + terminal status
 |                |                                      |
 |                 bmad-qa-generate-e2e-tests            |  e2e/ + api test suite
 |                |                                      |
 |== CLOSE EPIC =========================================|
 |                 bmad-retrospective                    |  implementation/retro-epic-1.md
 |                |                                      |  action items + acceptance verdict
 v                v                                      v
```

---

## What each phase left on disk

```
repo/
├── AGENTS.md
├── _bmad/custom/bmad-build.user.toml
├── {output}/
│   ├── brainstorming/brainstorm.html
│   ├── brainstorming/brainstorm-intent.md
│   ├── forge/forge-report.html
│   ├── forge/forged-idea.md
│   ├── party-mode/2026-08-31-mealplan-v1-scope.html
│   └── party-mode/memories/installed/.memlog.md
├── {planning_artifacts}/
│   ├── research/market-family-meal-planning.md
│   ├── research/user-voice-home-cooks.md
│   ├── research/competitive-mealime-paprika.md
│   ├── research/technical-vision-vs-barcode.md
│   ├── research/select-vision-vendor.md
│   ├── brief.md
│   ├── addendum.md
│   ├── prd.md
│   ├── prd-validation.html
│   ├── prd-validation.md
│   ├── DESIGN.md
│   ├── EXPERIENCE.md
│   ├── ARCHITECTURE-SPINE.md
│   ├── epics/
│   └── .memlog.md
├── {output}/specs/spec-pantry-to-plan/
│   ├── SPEC.md
│   ├── stories.yaml
│   └── impl/
├── {implementation_artifacts}/
│   ├── sprint-status.yaml
│   └── retro-epic-1.md
└── (application code + e2e tests)
```

---

## Why party mode appears here (and stays short)

The founders were about to pour a month into pantry vision. One default-room party forced the clash into the open: UX wanted magic, engineering wanted a shippable v1. The keepsake HTML is the paper trail of *why vision slipped to epic 3*. The long party-mode treatment — custom casts, two rooms, memory — is in [06 Atlas Health](06-party-mode-atlas-health.md).
