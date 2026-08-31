# 05 — LedgerLite: mid-sprint explosion (brownfield + correct-course)

**Field:** brownfield  
**Size:** project already in flight; one epic detonates  
**Why this path:** significant change during sprint execution — the defining job of `bmad-correct-course`. You walk in *at* the bomb. For the same skill after a full brownfield path has already shipped an epic, see [07](07-brownfield-mealplan-course-correct.md).

**LedgerLite** is a 3-year-old Node + MongoDB bookkeeping SaaS for EU freelancers. Epic 4 ("multi-currency journals") is in week 2 of 3. Two stories are merged. Legal then drops a bomb: a German customer’s DPA requires **Postgres in `eu-central-1`**, no document store, and a 30-day deletion SLA. Mongo-on-Atlas-US is now a ship blocker. The team also disagrees on whether to pause the epic or dual-write.

---

## Skills used (and skipped)

Used: `bmad-help`, `bmad-sprint-planning` (status), `bmad-party-mode`, `bmad-deep-recon` (domain, technical, select), `bmad-correct-course`, `bmad-prd` (update), `bmad-architecture`, `bmad-spec`, `bmad-create-epics-and-stories`, `bmad-sprint-planning` (re-gate), `bmad-review`, `bmad-project-context`, `bmad-build`, `bmad-build-auto`, `bmad-code-review`, `bmad-qa-generate-e2e-tests`, `bmad-retrospective`.

Skipped: brainstorm, forge, brief, PRFAQ, UX (no UI change), checkpoint-preview (team prefers CR + CI).

Deprecated name someone might type: `bmad-sprint-status` → `bmad-sprint-planning` status view.

---

## Sequence (`<< in` / `>> out`)

```
YOU          SKILL / ROOM
 |                |
 |== BEFORE THE BOMB (already on disk) ==================
 |  planning/prd.md
 |  planning/ARCHITECTURE-SPINE.md   (Mongo is "the DB")
 |  specs/spec-multi-currency/SPEC.md
 |  implementation/sprint-status.yaml
 |  two stories already merged to main
 |
 |== THE BOMB ===========================================
 |-- "legal just killed Mongo for EU tenants" ---------->|
 |                 bmad-help
 |                      << in:  legal DPA + current planning artifacts
 |                      >> out: (none — "status, then correct-course")
 |                 bmad-sprint-planning  view=status
 |                      << in:  implementation/sprint-status.yaml
 |                      >> out: status summary (2 done, 4 ready, epic in-progress)
 |
 |== ARGUE THE BLAST RADIUS BEFORE REWRITING PAPER ======
 |                 bmad-party-mode  --party installed --mode auto
 |                      << in:  "Mongo is now a ship blocker" + prd/spine/sprint
 |                      << in:  memories/installed/.memlog.md (if any)
 |                      >> out: party-mode/memories/installed/.memlog.md
 |                      >> out: party-mode/2026-05-04-ledger-store-fight.html
 |
 |== EVIDENCE FOR THE COURSE-CORRECT ====================
 |                 bmad-deep-recon  type=domain
 |                      << in:  the German DPA clause
 |                      >> out: research/domain-gdpr-dpa-stores.md
 |                 bmad-deep-recon  type=technical
 |                      << in:  current Mongo ledger + Postgres cutover options
 |                      >> out: research/technical-mongo-to-pg.md
 |                 bmad-deep-recon  shape=select
 |                      << in:  Prisma+PG vs Drizzle
 |                      >> out: research/select-prisma-pg-vs-drizzle.md
 |
 |== THE SKILL THIS USE CASE EXISTS FOR =================
 |                 bmad-correct-course
 |                      << in:  change signal + prd.md + spine + epics + sprint-status
 |                      << in:  party takeaway + recon
 |                      >> out: planning/change-proposal-store-cutover.md
 |                 take: update PRD, redo arch, restory, re-gate. Do not start over.
 |
 |== REWRITE THE UPSTREAM ARTIFACTS =====================
 |                 bmad-prd   intent=update
 |                      << in:  existing prd.md + the change proposal
 |                      >> out: planning/prd.md (rewritten), addendum.md, .memlog.md
 |                 bmad-architecture
 |                      << in:  updated prd.md + the living Mongo codebase
 |                      >> out: planning/ARCHITECTURE-SPINE.md  (Postgres SoR)
 |                 bmad-create-epics-and-stories
 |                      << in:  new spine + updated prd.md
 |                      >> out: epics/epic-4b-pg-cutover.md
 |                      >> out: epics/epic-4-multi-currency.md  (now blocked-on 4b)
 |                 bmad-spec   epic=pg-cutover
 |                      << in:  proposal + new spine + updated prd
 |                      >> out: specs/spec-pg-cutover/SPEC.md + stories.yaml
 |                 bmad-review  lenses=verification-gap,edge-case
 |                      << in:  spec-pg-cutover/SPEC.md
 |                      >> out: review/cutover-spec-findings.md
 |                 bmad-sprint-planning
 |                      << in:  new epics + old sprint-status.yaml
 |                      >> out: repaired + resorted sprint-status.yaml (PASS)
 |
 |                 bmad-project-context  record
 |                      << in:  "do not add Mongo indexes on new ledger collections"
 |                      >> out: AGENTS.md  (+ pitfall line)
 |
 |== SHIP THE NEW COURSE ================================
 |                 bmad-build          (schema, migrate, dual-run, cut)
 |                      << in:  one cutover story + SPEC.md + codebase
 |                      >> out: code + impl records
 |                 bmad-code-review
 |                      << in:  the cutover diff
 |                      >> out: findings + patches
 |                 bmad-build-auto     (backfill chunks, pattern now stable)
 |                      << in:  one bounded backfill unit + spec + proven pattern
 |                      >> out: code + impl + terminal status
 |                 bmad-qa-generate-e2e-tests
 |                      << in:  cutover code + SPEC invariants
 |                      >> out: e2e/ledger-pg-invariants.spec.ts
 |                 bmad-retrospective
 |                      << in:  spec-pg-cutover/ + stories.yaml + impl + code
 |                      >> out: implementation/retro-pg-cutover.md + ACCEPT
 v                v
```

---

## How artifacts moved (before → after)

```
BEFORE (invalid)                         AFTER (course-corrected)
─────────────────                        ────────────────────────
prd.md  "Mongo is fine"                  prd.md  "Postgres, EU pin, 30-day delete"
ARCHITECTURE-SPINE.md                    ARCHITECTURE-SPINE.md  (store invariant flipped)
spec-multi-currency/                     spec-multi-currency/   (blocked)
                                         spec-pg-cutover/       (new, in front)
sprint-status.yaml                       sprint-status.yaml     (repaired, resorted)
                                         change-proposal-store-cutover.md
                                         party-mode keepsake + memlog
                                         research/* gdpr / pg / select
```

---

## What this use case teaches

- `bmad-correct-course` is not "keep coding with a footnote." It is allowed to tell you to update the PRD, redo architecture, restory, and re-gate the sprint.
- Run `bmad-sprint-planning` status *first* so the proposal is grounded in what is already merged.
- A party before the proposal surfaces factions; the proposal is the decision object.
- `bmad-prd intent=update` takes a change signal (the proposal), not a blank page.
- `bmad-sprint-planning` can **repair** a tracking file, not only create one.
- `bmad-build-auto` is appropriate only after the cutover pattern is proven by a human `bmad-build`.
