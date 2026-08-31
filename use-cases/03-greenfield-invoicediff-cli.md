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

## Sequence (`<< in` / `>> out`)

```
YOU          SKILL
 |                |
 |-- "I know exactly what this CLI must do" ------------>|
 |                 bmad-help
 |                      << in:  "Go CLI, flags and exit codes already known"
 |                      >> out: (none — "you are on spec + build")
 |
 |== LOCK WHAT ==========================================
 |                 bmad-spec  + story breakdown
 |                      << in:  2-page brain dump + desired --flags + exit codes
 |                      >> out: specs/spec-invoicediff/SPEC.md
 |                      >> out: specs/spec-invoicediff/cli-flags.md  (companion)
 |                      >> out: specs/spec-invoicediff/stories.yaml
 |
 |                 bmad-review  lenses=edge-case,verification-gap
 |                      << in:  specs/spec-invoicediff/SPEC.md
 |                      >> out: review/spec-findings.json + .md
 |                 bmad-spec   intent=update
 |                      << in:  existing SPEC.md + review findings
 |                      >> out: SPEC.md rewritten in place (stable capability IDs)
 |
 |== THIN HOW (optional, but two parsers will diverge) ==
 |                 bmad-architecture
 |                      << in:  SPEC.md (two parsers will otherwise diverge)
 |                      >> out: planning/ARCHITECTURE-SPINE.md
 |
 |                 bmad-project-context  setup
 |                      << in:  the Go repo (go test ./..., never go test .)
 |                      >> out: AGENTS.md
 |
 |== SHIP ONE STORY AT A TIME ===========================
 |                 bmad-build   story=1-parse
 |                      << in:  stories.yaml #1 + SPEC.md + empty tree
 |                      >> out: cmd/invoicediff/, internal/parse/, impl/1-parse.md
 |                 bmad-code-review
 |                      << in:  the parser diff
 |                      >> out: findings + patches
 |                 bmad-checkpoint-preview
 |                      << in:  the parser commit / PR
 |                      >> out: walkthrough (usually no file)
 |
 |                 bmad-build   story=2-normalize
 |                      << in:  stories.yaml #2 + SPEC.md + parser code
 |                      >> out: internal/normalize/ + impl record
 |                 bmad-build   story=3-diff
 |                      << in:  stories.yaml #3 + SPEC.md + normalize code
 |                      >> out: internal/diff/ + impl record
 |                 bmad-build   story=4-json
 |                      << in:  stories.yaml #4 + SPEC.md + diff code
 |                      >> out: internal/report/ + impl record
 |
 |                 bmad-qa-generate-e2e-tests
 |                      << in:  implemented CLI + SPEC success signal
 |                      >> out: testdata/*.pdf, testdata/*.csv, e2e/invoicediff_test.go
 |
 |                 bmad-retrospective
 |                      << in:  spec folder + stories.yaml + impl records + code
 |                      >> out: implementation/retro-invoicediff.md + ACCEPT verdict
 v                v
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
