# 07 — MealPlan AI year 2 (brownfield + late course-correct)

**Field:** brownfield  
**Size:** living product; one new epic ships, the *next* epic detonates  
**Why this path:** same shape as [01](01-greenfield-mealplan-ai.md), but the repo already exists. `bmad-correct-course` is **not** the first skill — it arrives after a later-stage partner API dies mid-sprint.

Year 1 shipped pantry-to-plan (manual pantry + barcode). The product is in production. A new engineer joins to add **household sharing**, then **Instacart grocery export**. Sharing ships. Export is in week 2 of 3, two stories merged, when Instacart changes ToS and kills the partner API. That is when course-correct earns its keep.

Pair with [05 LedgerLite](05-brownfield-ledgerlite-course-correction.md) if you want to *start* at the bomb. This file walks the brownfield path **until** the bomb.

---

## Skills used (and skipped)

Used: `bmad-project-context`, `bmad-help`, `bmad-customize`, `bmad-deep-recon`, `bmad-prd` (update + validate), `bmad-review`, `bmad-ux`, `bmad-architecture` (ratify), `bmad-spec`, `bmad-create-epics-and-stories`, `bmad-sprint-planning`, `bmad-build`, `bmad-code-review`, `bmad-checkpoint-preview`, `bmad-qa-generate-e2e-tests`, `bmad-retrospective`, `bmad-party-mode`, `bmad-correct-course`, `bmad-build-auto`.

Skipped: brainstorm, forge, brief, PRFAQ (the product already exists). Agents optional.

---

## Sequence (`<< in` / `>> out`)

```
YOU          SKILL / ROOM
 |                |
 |== ALREADY ON DISK (year 1 — see use-case 01) =========
 |  AGENTS.md                         (maybe stale)
 |  planning/prd.md                   (v1: no sharing, no grocery partner)
 |  planning/ARCHITECTURE-SPINE.md    (barcode-only pantry)
 |  specs/spec-pantry-to-plan/        (done)
 |  implementation/sprint-status.yaml (epic 1 done)
 |  production Next.js + Prisma + pnpm
 |
 |== DAY 0: MAKE AGENTS USEFUL IN THIS REPO =============
 |                 bmad-project-context  refresh
 |                      << in:  the living repo (pnpm, docker compose,
 |                              Playwright against :5433, year-1 pitfalls)
 |                      >> out: AGENTS.md  (managed block refreshed)
 |                 bmad-customize
 |                      << in:  "always pnpm; never rewrite pantry SKU ids"
 |                      >> out: _bmad/custom/bmad-build.toml
 |                 bmad-help
 |                      << in:  "add household sharing then Instacart export"
 |                      << in:  existing prd.md + spine + AGENTS.md
 |                      >> out: (none — "update PRD, ratify arch, spec epic 2")
 |
 |== EVIDENCE FOR THE NEW BET ===========================
 |                 bmad-deep-recon  type=user-voice
 |                      << in:  "families fighting over one plan"
 |                      >> out: research/user-voice-household-sharing.md
 |                 bmad-deep-recon  type=technical
 |                      << in:  existing Prisma User + Plan models
 |                      >> out: research/technical-household-tenancy.md
 |                 bmad-deep-recon  shape=select
 |                      << in:  Instacart vs Amazon Fresh vs CSV-only
 |                      >> out: research/select-grocery-partner.md
 |
 |== UPDATE THE PRODUCT PAPER (not a new brief) =========
 |                 bmad-prd   intent=update
 |                      << in:  existing prd.md          ← year-1 create run
 |                      << in:  change signal            ← you: "sharing + Instacart"
 |                      << in:  recon/*.md
 |                      >> out: planning/prd.md          (rewritten)
 |                      >> out: planning/addendum.md
 |                      >> out: planning/.memlog.md
 |
 |                 bmad-review  lenses=adversarial,verification-gap
 |                      << in:  updated prd.md
 |                      >> out: review/prd-y2-findings.json + .md
 |
 |                 bmad-prd   intent=validate
 |                      << in:  finished prd.md          ← you / the update run
 |                      << in:  shipped rubric           ← bmad-prd, unless the org overrode it
 |                      >> out: validation-report.html + .md
 |                 shipped rubric = assets/prd-validation-checklist.md
 |                 org override   = _bmad/custom/bmad-prd.toml
 |
 |                 bmad-ux
 |                      << in:  updated prd.md (invite / accept / shared list)
 |                      >> out: planning/DESIGN.md
 |                      >> out: planning/EXPERIENCE.md
 |
 |                 bmad-architecture
 |                      << in:  updated prd.md + DESIGN.md + the living codebase
 |                      >> out: planning/ARCHITECTURE-SPINE.md
 |                 BROWNFIELD: ratifies Plan as household-scoped;
 |                 invite tokens; grocery partner is an adapter, not a core.
 |
 |== SLICE EPIC 2 (sharing) — THIS SHIPS CLEANLY ========
 |                 bmad-spec   epic=household-share
 |                      << in:  updated prd + spine + UX (condensed)
 |                      >> out: specs/spec-household-share/SPEC.md
 |                      >> out: specs/spec-household-share/stories.yaml
 |                 bmad-create-epics-and-stories
 |                      << in:  new spine + updated prd.md
 |                      >> out: planning/epics/epic-2-household-share.md
 |                      >> out: planning/epics/epic-3-grocery-export.md
 |                 bmad-sprint-planning
 |                      << in:  new epics + old sprint-status.yaml
 |                      >> out: implementation/sprint-status.yaml  (PASS)
 |
 |                 bmad-build          (invite, accept, shared pantry)
 |                      << in:  one story + SPEC.md + year-1 codebase
 |                      >> out: code + impl records
 |                 bmad-code-review
 |                      << in:  the sharing diff
 |                      >> out: findings + optional patches
 |                 bmad-checkpoint-preview
 |                      << in:  the invite PR
 |                      >> out: walkthrough (usually no file)
 |                 bmad-qa-generate-e2e-tests
 |                      << in:  implemented sharing + SPEC success signal
 |                      >> out: e2e/household-invite.spec.ts
 |                 bmad-retrospective
 |                      << in:  spec-household-share/ + stories.yaml + impl + code
 |                      >> out: implementation/retro-epic-2.md
 |                      >> out: ACCEPT — sharing is in production
 |
 |== LATER: EPIC 3 IS IN FLIGHT =========================
 |  specs/spec-grocery-export/SPEC.md     (Instacart is "the" adapter)
 |  sprint-status.yaml                    epic 3 in-progress
 |  stories 3-1 cart-map and 3-2 oauth    MERGED to main
 |  stories 3-3 webhook, 3-4 retry        ready
 |
 |== THE BOMB (this is when course-correct appears) =====
 |-- "Instacart killed the partner API in 19 days" ----->|
 |                 bmad-help
 |                      << in:  ToS email + current planning artifacts
 |                      >> out: (none — "status, then correct-course")
 |                 bmad-sprint-planning  view=status
 |                      << in:  implementation/sprint-status.yaml
 |                      >> out: status summary
 |                      2 done (merged), 2 ready, epic 3 in-progress
 |
 |                 bmad-party-mode  --party installed --mode auto
 |                      << in:  "partner API is dead" + prd/spine/sprint
 |                      << in:  memories/installed/.memlog.md (year-1 scope fight)
 |                      >> out: party-mode/memories/installed/.memlog.md
 |                      >> out: party-mode/{date}-instacart-killed.html
 |                 CLASH: John wants to pause the epic in the app;
 |                 Amelia wants a feature-flag and to keep the cart mapper;
 |                 Winston refuses any new Instacart types in the spine.
 |
 |                 bmad-deep-recon  type=technical
 |                      << in:  merged cart-map + oauth vs a CSV/email fallback
 |                      >> out: research/technical-grocery-without-instacart.md
 |                 bmad-deep-recon  shape=select
 |                      << in:  pause vs CSV export vs new partner (6 months)
 |                      >> out: research/select-export-fallback.md
 |
 |== COURSE-CORRECT (late — after epic 2 shipped) =======
 |                 bmad-correct-course
 |                      << in:  change signal            ← you: Instacart ToS email
 |                      << in:  current prd.md           ← year-2 update
 |                      << in:  ARCHITECTURE-SPINE.md
 |                      << in:  epics + sprint-status.yaml
 |                      << in:  party takeaway + recon
 |                      >> out: planning/change-proposal-drop-instacart.md
 |                 take: update PRD, redo arch, restory epic 3, re-gate.
 |                 do not start over. do not touch shipped epic 2.
 |
 |                 bmad-prd   intent=update
 |                      << in:  existing prd.md          ← year-2 paper
 |                      << in:  change signal            ← the proposal
 |                      >> out: planning/prd.md          (Instacart demoted)
 |                      >> out: planning/addendum.md
 |                      >> out: planning/.memlog.md
 |
 |                 bmad-architecture
 |                      << in:  updated prd.md + living codebase (incl. merged 3-1/3-2)
 |                      >> out: planning/ARCHITECTURE-SPINE.md
 |                 new invariant: grocery export is a file/email port;
 |                 partner adapters are optional; no vendor in the spine.
 |
 |                 bmad-create-epics-and-stories
 |                      << in:  new spine + updated prd.md
 |                      >> out: planning/epics/epic-3-grocery-export.md  (rewritten)
 |                      epic 2 left alone (already ACCEPTed)
 |
 |                 bmad-spec   epic=grocery-export  intent=update
 |                      << in:  existing SPEC.md + proposal + new spine
 |                      >> out: specs/spec-grocery-export/SPEC.md
 |                      >> out: specs/spec-grocery-export/stories.yaml
 |                      (3-1/3-2 kept as "cart mapper"; oauth story parked)
 |
 |                 bmad-sprint-planning
 |                      << in:  rewritten epic 3 + old sprint-status.yaml
 |                      >> out: repaired sprint-status.yaml  (PASS)
 |                 3-2 oauth → blocked; 3-3 becomes csv-export; 3-4 email-send
 |
 |                 bmad-project-context  record
 |                      << in:  observed mistake: "do not add Instacart types"
 |                      >> out: AGENTS.md  (+ pitfall line)
 |
 |== SHIP THE NEW COURSE ================================
 |                 bmad-build          (csv-export, email-send)
 |                      << in:  new stories + updated SPEC.md + codebase
 |                      >> out: code + impl records
 |                 bmad-code-review
 |                      << in:  the fallback diff
 |                      >> out: findings + optional patches
 |                 bmad-build-auto     (csv fixtures, pattern now stable)
 |                      << in:  one bounded fixture story + spec + proven pattern
 |                      >> out: code + impl + terminal status
 |                 bmad-qa-generate-e2e-tests
 |                      << in:  csv/email export + SPEC success signal
 |                      >> out: e2e/grocery-csv-export.spec.ts
 |                 bmad-retrospective
 |                      << in:  spec-grocery-export/ + stories.yaml + impl + code
 |                      >> out: implementation/retro-epic-3.md
 |                      >> out: ACCEPT — export ships without Instacart
 v                v
```

---

## How artifacts moved

```
YEAR 1 (use-case 01)          YEAR 2 EARLY (this file)         AFTER COURSE-CORRECT
────────────────────          ────────────────────────         ────────────────────
prd.md  v1 barcode            prd.md  +sharing +Instacart      prd.md  Instacart demoted
ARCHITECTURE-SPINE.md         spine  household + adapter       spine  file/email port
spec-pantry-to-plan/  done    spec-household-share/  ACCEPT    (untouched)
                              spec-grocery-export/   in flight spec-grocery-export/  rewritten
sprint-status.yaml  epic 1    epic 2 done, epic 3 live         epic 3 repaired
                              3-1, 3-2 merged                  3-2 oauth blocked
                                                               change-proposal-drop-instacart.md
```

---

## What each phase left on disk

```
mealplan-ai/
├── AGENTS.md
├── _bmad/custom/bmad-build.toml
├── {planning_artifacts}/
│   ├── prd.md
│   ├── addendum.md
│   ├── .memlog.md
│   ├── DESIGN.md
│   ├── EXPERIENCE.md
│   ├── ARCHITECTURE-SPINE.md
│   ├── change-proposal-drop-instacart.md
│   ├── epics/epic-2-household-share.md
│   ├── epics/epic-3-grocery-export.md
│   └── research/
│       ├── user-voice-household-sharing.md
│       ├── technical-household-tenancy.md
│       ├── select-grocery-partner.md
│       ├── technical-grocery-without-instacart.md
│       └── select-export-fallback.md
├── specs/spec-household-share/     (done — not rewritten)
├── specs/spec-grocery-export/      (rewritten after CC)
├── {implementation_artifacts}/
│   ├── sprint-status.yaml
│   ├── retro-epic-2.md
│   └── retro-epic-3.md
├── {output}/party-mode/{date}-instacart-killed.html
└── e2e/household-invite.spec.ts
    e2e/grocery-csv-export.spec.ts
```

---

## What this use case teaches

- Brownfield starts at `bmad-project-context`, not brainstorm. The product already has a PRD — **update** it.
- `bmad-architecture` **ratifies** the living system; it does not propose a rewrite on day 0.
- `bmad-correct-course` is a **later-stage** skill. You have already shipped an epic and merged two stories of the next one. A footnote in a stand-up is not enough.
- Course-correct is allowed to rewrite PRD, spine, epic 3, spec, and sprint status. It is **not** allowed to reopen an ACCEPTed epic without a reason.
- Merged work is an input: the cart mapper stays; the Instacart oauth story is parked.
- [05](05-brownfield-ledgerlite-course-correction.md) is the same skill if you walk in *after* the bomb. This file is the bomb arriving in week 8 of a brownfield engagement.
