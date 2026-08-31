# 01 — MealPlan AI (greenfield product)

**Field:** greenfield  
**Size:** project (~20+ build sessions, several epics)  
**Why this path:** UI is the product, more than one person must agree, several engineers will build in parallel.

A two-founder team wants a family meal-planning app: pantry + dietary constraints → weekly dinners + grocery list. Concept is fuzzy on day one. They need a signed-off PRD, UX, and an architecture spine before slicing work.

---

## Skills used (and skipped)

Used: `bmad-help`, `bmad-brainstorming`, `bmad-forge-idea`, `bmad-deep-recon`, `bmad-party-mode`, `bmad-product-brief`, `bmad-prd`, `bmad-advanced-elicitation`, `bmad-review`, `bmad-ux`, `bmad-architecture`, `bmad-spec`, `bmad-create-epics-and-stories`, `bmad-sprint-planning`, `bmad-build`, `bmad-code-review`, `bmad-checkpoint-preview`, `bmad-qa-generate-e2e-tests`, `bmad-build-auto`, `bmad-retrospective`, `bmad-project-context`, `bmad-customize`.

Skipped: `bmad-prfaq` (founders already sure of the concept — brief is enough), `bmad-correct-course` (no mid-flight explosion — that is [07](07-brownfield-mealplan-course-correct.md), year 2 of this product). Agents optional: any `bmad-agent-*` can sit on top and dispatch the same workflows.

---

## Sequence (`<< in` / `>> out`)

```
YOU          SKILL / ROOM
 |                |
 |-- "what first?" ------------------------------------->|
 |                 bmad-help
 |                      << in:  "what first?" + empty repo
 |                      >> out: (none — recommends brainstorm)
 |
 |== THINK ==============================================
 |-- "help me ideate" ---------------------------------->|
 |                 bmad-brainstorming
 |                      << in:  "family pantry → weekly plan" (fuzzy)
 |                      >> out: brainstorming/brainstorm.html
 |                      >> out: brainstorming/brainstorm-intent.md
 |
 |-- "pressure-test pantry-scan + plan" ---------------->|
 |                 bmad-forge-idea
 |                      << in:  chosen direction from brainstorm-intent.md
 |                      >> out: forge/forge-report.html
 |                      >> out: forge/forged-idea.md
 |
 |-- evidence, not vibes ------------------------------->|
 |                 bmad-deep-recon  type=market
 |                      << in:  "family meal planning demand"
 |                      >> out: research/market-family-meal-planning.md
 |                 bmad-deep-recon  type=user-voice
 |                      << in:  "home-cook Reddit / reviews / JTBD"
 |                      >> out: research/user-voice-home-cooks.md
 |                 bmad-deep-recon  type=competitive
 |                      << in:  Mealime, Paprika, ChatGPT recipes
 |                      >> out: research/competitive-mealime-paprika.md
 |                 bmad-deep-recon  type=technical
 |                      << in:  vision vs barcode vs manual pantry
 |                      >> out: research/technical-vision-vs-barcode.md
 |                 bmad-deep-recon  shape=select
 |                      << in:  vision-vendor candidates
 |                      >> out: research/select-vision-vendor.md
 |
 |-- "put Mary, John, Winston, Sally, Amelia in a room ->|
 |     Should v1 even attempt pantry vision?"            |
 |                 bmad-party-mode  party=installed  mode=session
 |                      << in:  the question + forged-idea.md + recon/*.md
 |                      << in:  memories/installed/.memlog.md (if any)
 |                      >> out: party-mode/memories/installed/.memlog.md
 |                      >> out: party-mode/2026-08-31-mealplan-v1-scope.html
 |                 CLASH: Sally wants delightful scan; Amelia wants
 |                 barcode-only in v1; Winston refuses a vision pipeline
 |                 without an architecture invariant.
 |                 Outcome: v1 = manual pantry + barcode; vision later.
 |
 |== WRITE THE PRODUCT ==================================
 |-- concept is now solid ------------------------------->|
 |                 bmad-product-brief
 |                      << in:  forged-idea.md + recon + party takeaway
 |                      >> out: planning/brief.md
 |                      >> out: planning/addendum.md
 |
 |-- org (two founders + advisor) needs sign-off ------->|
 |                 bmad-prd   intent=create
 |                      << in:  brief.md + addendum.md + recon
 |                      >> out: planning/prd.md
 |                      >> out: planning/addendum.md
 |                      >> out: planning/.memlog.md
 |
 |-- draft feels like LLM first pass ------------------->|
 |                 bmad-advanced-elicitation  method=pre-mortem + red-team
 |                      << in:  the just-written prd.md
 |                      >> out: prd.md rewritten in place
 |
 |-- "review the PRD" ---------------------------------->|
 |                 bmad-review  lenses=adversarial,structure,prose
 |                      << in:  planning/prd.md
 |                      >> out: review/prd-findings.json
 |                      >> out: review/prd-findings.md
 |
 |-- validate against checklist ------------------------>|
 |                 bmad-prd   intent=validate
 |                      << in:  finished prd.md          ← you / the create run
 |                      << in:  shipped rubric           ← bmad-prd, unless the org overrode it
 |                      >> out: validation-report.html + .md
 |                 shipped rubric = assets/prd-validation-checklist.md
 |                 org override   = _bmad/custom/bmad-prd.toml
 |                 (addendum.md is also read if present)
 |
 |-- UI is the product --------------------------------->|
 |                 bmad-ux
 |                      << in:  prd.md (UI is the product)
 |                      >> out: planning/DESIGN.md
 |                      >> out: planning/EXPERIENCE.md
 |                      >> out: planning/.memlog.md
 |
 |-- several streams will build in parallel ------------>|
 |                 bmad-architecture
 |                      << in:  prd.md + DESIGN.md
 |                      >> out: planning/ARCHITECTURE-SPINE.md
 |
 |-- lock WHAT for epic-1 pantry-to-plan --------------->|
 |                 bmad-spec  + story breakdown
 |                      << in:  prd.md + UX + spine + party takeaway (condensed)
 |                      >> out: specs/spec-pantry-to-plan/SPEC.md
 |                      >> out: specs/spec-pantry-to-plan/stories.yaml
 |                      >> out: specs/spec-pantry-to-plan/<companions>
 |
 |== SLICE + GATE =======================================
 |                 bmad-create-epics-and-stories
 |                      << in:  ARCHITECTURE-SPINE.md + prd.md
 |                      >> out: planning/epics/*.md  (stories with AC / BDD)
 |
 |                 bmad-sprint-planning
 |                      << in:  epics/*.md + stories
 |                      >> out: implementation/sprint-status.yaml
 |                      >> out: readiness PASS | CONCERNS | FAIL
 |
 |== TEACH THE REPO =====================================
 |                 bmad-project-context  setup
 |                      << in:  the new repo (scripts, package manager)
 |                      >> out: AGENTS.md  (managed block)
 |                 bmad-customize
 |                      << in:  "always pnpm, never npm"
 |                      >> out: _bmad/custom/bmad-build.user.toml
 |
 |== SHIP (repeat per story) ============================
 |                 bmad-build
 |                      << in:  one story from stories.yaml + SPEC.md + codebase
 |                      >> out: code in repo
 |                      >> out: specs/.../impl/<story> record
 |
 |                 bmad-code-review
 |                      << in:  the change (diff / PR)
 |                      >> out: findings + optional patches
 |                 bmad-checkpoint-preview
 |                      << in:  the commit / branch / PR
 |                      >> out: walkthrough (usually no file)
 |
 |  later stories, patterns stable:
 |                 bmad-build-auto
 |                      << in:  one bounded later story + spec + codebase
 |                      >> out: code + impl record + terminal status
 |
 |                 bmad-qa-generate-e2e-tests
 |                      << in:  implemented pantry-to-plan + SPEC success signal
 |                      >> out: e2e/ + api test suite
 |
 |== CLOSE EPIC =========================================
 |                 bmad-retrospective
 |                      << in:  spec folder + stories.yaml + impl records + code
 |                      >> out: implementation/retro-epic-1.md
 |                      >> out: action items + acceptance verdict
 v                v
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
