# 03 — InvoiceDiff CLI (greenfield, spec-first, skip-heavy)

**Field:** greenfield  
**Size:** one epic (several build sessions, one coherent outcome)  
**Why this path:** the intent is already well defined; no org sign-off; no UI; one engineer.

A freelance backend engineer wants **InvoiceDiff**: a Go CLI that takes two vendor invoice PDFs (or CSVs), normalizes line items, and prints a colored diff plus a JSON report for CI. They already know the flags, the exit codes, and the non-goals. A PRD would be theater.

---

## Skills used (and skipped)

Used: `bmad-help`, `bmad-spec`, `bmad-review`, `bmad-architecture` (thin spine), `bmad-build`, `bmad-code-review`, `bmad-checkpoint-preview`, `bmad-qa-generate-e2e-tests`, `bmad-retrospective`, `bmad-project-context`.

Skipped: brainstorm, forge, recon, brief, PRFAQ, PRD, UX, epics-and-stories, sprint-planning, party-mode, build-auto, correct-course, customize, all `bmad-agent-*`.

This is the "well-defined intent → spec → build" path from BMad's planning docs.

---

## Sequence (artifacts on the right)

```
YOU          SKILL                                ARTIFACTS WRITTEN
 |                |                                      |
 |-- "I know exactly what this CLI must do" ------------>|
 |                 bmad-help                             |  (none — "you are on spec + build")
 |                |                                      |
 |== LOCK WHAT ==========================================|
 |                 bmad-spec                             |
 |                |  input: a 2-page brain dump +        |
 |                |  desired --flags and exit codes      |
 |                |                                      |  specs/spec-invoicediff/SPEC.md
 |                |                                      |    Why / Capabilities / Constraints
 |                |                                      |    Non-goals / Success signal
 |                |                                      |  specs/spec-invoicediff/cli-flags.md
 |                |                                      |    (companion — tables live beside SPEC)
 |                |  + story breakdown                   |  specs/spec-invoicediff/stories.yaml
 |                |                                      |    1. parse PDF/CSV
 |                |                                      |    2. normalize SKUs
 |                |                                      |    3. diff + exit codes
 |                |                                      |    4. JSON reporter
 |                |                                      |
 |                 bmad-review                           |  review/spec-findings.json
 |                 lenses=edge-case,verification-gap     |  review/spec-findings.md
 |                |  (found: Unicode invoices,           |
 |                |   duplicate SKUs, $ vs cents)        |
 |                 bmad-spec   intent=update             |  SPEC.md rewritten in place
 |                |                                      |  (capability IDs stay stable)
 |                |                                      |
 |== THIN HOW (optional, but two parsers will diverge) ==|
 |                 bmad-architecture                     |  planning/ARCHITECTURE-SPINE.md
 |                |  invariants: one Invoice IR;         |
 |                |  parsers are adapters; money is      |
 |                |  integer cents; diff is pure         |
 |                |                                      |
 |                 bmad-project-context  setup           |  AGENTS.md
 |                |  go test ./..., never go test .      |
 |                |                                      |
 |== SHIP ONE STORY AT A TIME ===========================|
 |                 bmad-build   story=1-parse            |  cmd/invoicediff/, internal/parse/
 |                |                                      |  specs/.../impl/1-parse.md
 |                 bmad-code-review                      |  findings + patches
 |                 bmad-checkpoint-preview               |  (human walkthrough of the parser)
 |                |                                      |
 |                 bmad-build   story=2-normalize        |  internal/normalize/ + impl record
 |                 bmad-build   story=3-diff             |  internal/diff/ + impl record
 |                 bmad-build   story=4-json             |  internal/report/ + impl record
 |                |                                      |
 |                 bmad-qa-generate-e2e-tests            |  testdata/*.pdf, testdata/*.csv
 |                |                                      |  e2e/invoicediff_test.go
 |                |                                      |
 |                 bmad-retrospective                    |  implementation/retro-invoicediff.md
 |                |  verdict: ACCEPT — exit codes match  |  action: add golden-file corpus
 |                |  SPEC success signal                 |
 v                v                                      v
```

---

## What each phase left on disk

```
invoicediff/
├── AGENTS.md
├── cmd/invoicediff/
├── internal/{parse,normalize,diff,report}/
├── testdata/
├── e2e/invoicediff_test.go
├── {output}/specs/spec-invoicediff/
│   ├── SPEC.md
│   ├── cli-flags.md
│   ├── stories.yaml
│   └── impl/
│       ├── 1-parse.md
│       ├── 2-normalize.md
│       ├── 3-diff.md
│       └── 4-json.md
├── {planning_artifacts}/ARCHITECTURE-SPINE.md
└── {implementation_artifacts}/retro-invoicediff.md
```

No `prd.md`. No `DESIGN.md`. No `sprint-status.yaml`. That is correct.

---

## What this use case teaches

- If someone else could build it without guessing, start at `bmad-spec`, not at a brief.
- `bmad-spec` writes `SPEC.md`; companions hold tables/flags so the kernel stays short.
- A one-person CLI still earns a thin `ARCHITECTURE-SPINE.md` the moment two parsers exist.
- `bmad-sprint-planning` is a coordination tool. One engineer with `stories.yaml` can skip it.
- Story breakdown on the spec *replaces* `bmad-create-epics-and-stories` at this size.
