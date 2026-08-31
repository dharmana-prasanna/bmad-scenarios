# 05 — LedgerLite: mid-sprint explosion (brownfield + correct-course)

**Field:** brownfield  
**Size:** project already in flight; one epic detonates  
**Why this path:** significant change during sprint execution — the defining job of `bmad-correct-course`.

**LedgerLite** is a 3-year-old Node + MongoDB bookkeeping SaaS for EU freelancers. Epic 4 ("multi-currency journals") is in week 2 of 3. Two stories are merged. Legal then drops a bomb: a German customer’s DPA requires **Postgres in `eu-central-1`**, no document store, and a 30-day deletion SLA. Mongo-on-Atlas-US is now a ship blocker. The team also disagrees on whether to pause the epic or dual-write.

---

## Skills used (and skipped)

Used: `bmad-help`, `bmad-sprint-planning` (status), `bmad-party-mode`, `bmad-deep-recon` (domain, technical, select), `bmad-correct-course`, `bmad-prd` (update), `bmad-architecture`, `bmad-spec`, `bmad-create-epics-and-stories`, `bmad-sprint-planning` (re-gate), `bmad-review`, `bmad-project-context`, `bmad-build`, `bmad-build-auto`, `bmad-code-review`, `bmad-qa-generate-e2e-tests`, `bmad-retrospective`.

Skipped: brainstorm, forge, brief, PRFAQ, UX (no UI change), checkpoint-preview (team prefers CR + CI).

Deprecated name someone might type: `bmad-sprint-status` → `bmad-sprint-planning` status view.

---

## Sequence (artifacts on the right)

```
YOU          SKILL / ROOM                         ARTIFACTS WRITTEN
 |                |                                      |
 |== BEFORE THE BOMB (already on disk) ==================|
 |  planning/prd.md                                      |
 |  planning/ARCHITECTURE-SPINE.md   (Mongo is "the DB") |
 |  specs/spec-multi-currency/SPEC.md                    |
 |  implementation/sprint-status.yaml                    |
 |  two stories already merged to main                   |
 |                |                                      |
 |== THE BOMB ===========================================|
 |-- "legal just killed Mongo for EU tenants" ---------->|
 |                 bmad-help                             |  (none — "status, then correct-course")
 |                 bmad-sprint-planning  view=status     |  (status summary in chat)
 |                |  reads sprint-status.yaml            |  risks: 2 done, 4 ready, epic in-progress
 |                |                                      |
 |== ARGUE THE BLAST RADIUS BEFORE REWRITING PAPER ======|
 |                 bmad-party-mode                       |
 |                 --party installed --mode auto         |
 |                |                                      |
 |                |  John   : pause epic 4, we cannot    |
 |                |           ship multi-currency on a   |
 |                |           store legal just banned    |
 |                |  Winston: dual-write is a trap;      |
 |                |           cut a migration epic first |
 |                |  Amelia : two merged stories assume  |
 |                |           Mongo session semantics;   |
 |                |           revert or isolate them     |
 |                |  Mary   : DPA text ≠ "no Mongo";     |
 |                |           read the clause            |
 |                |  Sally  : (quiet) no UX change       |
 |                |                                      |
 |                |  CLASH STAYS OPEN on purpose:        |
 |                |  John wants a customer-visible pause;|
 |                |  Amelia wants a feature-flag and     |
 |                |  keeps coding. Winston refuses both  |
 |                |  until the store invariant is reset. |
 |                |                                      |  party-mode/memories/installed/.memlog.md
 |                |                                      |  party-mode/2026-05-04-ledger-store-fight.html
 |                |                                      |
 |== EVIDENCE FOR THE COURSE-CORRECT ====================|
 |                 bmad-deep-recon  type=domain          |  research/domain-gdpr-dpa-stores.md
 |                 bmad-deep-recon  type=technical       |  research/technical-mongo-to-pg.md
 |                 bmad-deep-recon  shape=select         |  research/select-prisma-pg-vs-drizzle.md
 |                |                                      |
 |== THE SKILL THIS USE CASE EXISTS FOR =================|
 |                 bmad-correct-course                   |  planning/change-proposal-store-cutover.md
 |                |                                      |
 |                |  recommendation tree it actually     |
 |                |  considers (and we take the bold one)|
 |                |                                      |
 |                |     start over?           no         |
 |                |     update PRD?           YES        |
 |                |     redo architecture?    YES        |
 |                |     rewrite epics/stories YES        |
 |                |     redo sprint planning  YES        |
 |                |     just patch the spec?  no — too   |
 |                |                           small      |
 |                |                                      |
 |== REWRITE THE UPSTREAM ARTIFACTS =====================|
 |                 bmad-prd   intent=update              |  planning/prd.md          (rewritten)
 |                |  change signal = the proposal        |  planning/addendum.md
 |                |                                      |  planning/.memlog.md
 |                 bmad-architecture                     |  planning/ARCHITECTURE-SPINE.md
 |                |  new invariants: Postgres is the     |  (Mongo demoted to "legacy read replica
 |                |  system of record; ledger rows are   |   until cutover epic closes")
 |                |  append-only; tenant region pinned   |
 |                |                                      |
 |                 bmad-create-epics-and-stories         |  planning/epics/epic-4b-pg-cutover.md
 |                |                                      |  planning/epics/epic-4-multi-currency.md
 |                |                                      |  (4 now blocked-on 4b)
 |                 bmad-spec   epic=pg-cutover           |  specs/spec-pg-cutover/SPEC.md
 |                |                                      |  specs/spec-pg-cutover/stories.yaml
 |                 bmad-review                           |  review/cutover-spec-findings.md
 |                 lenses=verification-gap,edge-case     |
 |                 bmad-sprint-planning                  |  implementation/sprint-status.yaml
 |                |  re-gate: CONCERNS (dual-running     |  (repaired + resorted)
 |                |  window, rollback story missing)     |
 |                |  add rollback story, re-run → PASS   |
 |                |                                      |
 |                 bmad-project-context  record          |  AGENTS.md
 |                |  pitfall: do not add Mongo indexes   |
 |                |  on new ledger collections           |
 |                |                                      |
 |== SHIP THE NEW COURSE ================================|
 |                 bmad-build          (schema, migrate, |  code + impl records
 |                                     dual-run, cut)    |
 |                 bmad-code-review                      |  findings + patches
 |  repetitive migration scripts, pattern now stable:    |
 |                 bmad-build-auto     (backfill chunks) |  code + impl + terminal status
 |                 bmad-qa-generate-e2e-tests            |  e2e/ledger-pg-invariants.spec.ts
 |                 bmad-retrospective                    |  implementation/retro-pg-cutover.md
 |                |  verdict: ACCEPT — DPA clause met;   |
 |                |  action: un-block epic 4 currency    |
 v                v                                      v
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
