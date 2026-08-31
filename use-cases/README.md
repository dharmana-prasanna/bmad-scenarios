# End-to-end use cases

Eight projects that walk BMad Method from a messy question to shipped work. Each file is a standalone ASCII sequence with **what each skill reads** and **what it writes**.

Skill catalog, “when to use which,” and **recommended models** live in [`../bmad-skill-decision-diagrams.md`](../bmad-skill-decision-diagrams.md#2a-recommended-models-agents-and-skills).

| # | Project | Field | Why it exists | Party mode? |
|---|---|---|---|---|
| [01](01-greenfield-mealplan-ai.md) | **MealPlan AI** — family meal planning SaaS | Greenfield | Full product path: think → brief/PRD → UX → arch → spec → sprint → build | Light (one default-room debate) |
| [02](02-greenfield-harborwatch.md) | **HarborWatch** — coastal flood early-warning | Greenfield | Research-heavy, civic, PRFAQ instead of a gentle brief | Yes (ethics / liability roundtable) |
| [03](03-greenfield-invoicediff-cli.md) | **InvoiceDiff** — CLI that diffs vendor invoices | Greenfield | Epic-sized, spec-first; skip PRD/UX/sprint theater | No |
| [04](04-brownfield-northwind-sso.md) | **Northwind Shop** — add SSO to existing Next.js commerce | Brownfield | Join a live repo, teach agents the house rules, ship one epic | No |
| [05](05-brownfield-ledgerlite-course-correction.md) | **LedgerLite** — mid-sprint Mongo → Postgres + GDPR | Brownfield | You walk in *at* the bomb; `bmad-correct-course` first | Yes (data-residency fight) |
| [06](06-party-mode-atlas-health.md) | **Atlas Health** — build vs buy vs partner video visits | Mixed | Dedicated `bmad-party-mode` deep dive: custom focus group, two rooms, memory, keepsake | **Primary** |
| [07](07-brownfield-mealplan-course-correct.md) | **MealPlan AI year 2** — sharing ships, then Instacart dies | Brownfield | Same shape as 01; `bmad-correct-course` only *after* an epic shipped | Yes (late, after epic 2) |
| [08](08-brownfield-cargolane-enable-brief.md) | **CargoLane** — enable BMad, brief a portal, then add appointments after it ships | Brownfield | BMad not on yet; `bmad-product-brief` + later **additive** requirement (not course-correct) | No |

## How to read a sequence

Right-hand notes are **inputs and outputs**, not extra steps.

```
You -----> skill-name
              << in:  what it consumes (your words, prior artifacts, the repo)
              >> out: what it produces
```

Conversation-only skills (`bmad-help`, `bmad-advanced-elicitation`, `bmad-checkpoint-preview` mid-walkthrough) still appear; they may write nothing durable.

## Input / output cheat sheet (current skills)

| Skill | Reads (`<< in`) | Writes (`>> out`) |
|---|---|---|
| `bmad-help` | your question + existing artifacts | none (next-skill recommendation) |
| `bmad-brainstorming` | a topic or "I'm stuck" | `{output}/brainstorming/brainstorm.html`, optional `brainstorm-intent.md` |
| `bmad-forge-idea` | the idea (sentence or short brief) | `{output}/forge/forge-report.html`, optional `forged-idea.md` |
| `bmad-deep-recon` | subject + type, or a report to process, or candidates | `{planning}/research/*.md` (+ optional HTML briefing) |
| `bmad-advanced-elicitation` | the just-produced draft + a method | in-place improvements to that draft |
| `bmad-review` | path to a doc/diff/spec + optional lenses | findings JSON + markdown report |
| `bmad-party-mode` | topic; optional `--party`/`--mode`; memlog; or interview notes to author a cast | keepsake HTML; `memories/<party>/.memlog.md`; optional custom TOML |
| `bmad-customize` | skill name + the override | `_bmad/custom/<skill>.toml` or `<skill>.user.toml` |
| `bmad-product-brief` | a relatively clear concept; optional recon / forged-idea | `brief.md` + `addendum.md` |
| `bmad-prfaq` | product concept to stress-test; optional recon / forge | `prfaq-*.md` |
| `bmad-prd` | create: brief/PRFAQ/brain dump; update: `prd.md` + change signal; validate: finished PRD | create/update: `prd.md`, `addendum.md`, `.memlog.md`; validate: HTML + `.md` |
| `bmad-ux` | PRD or spec | `DESIGN.md`, `EXPERIENCE.md`, `.memlog.md` |
| `bmad-spec` | any intent (brief, PRD, transcript, dump, folder); or existing `SPEC.md` + a change | `specs/spec-{slug}/SPEC.md` + companions; optional `stories.yaml` |
| `bmad-architecture` | PRD or spec; UX if present; brownfield: the codebase | `ARCHITECTURE-SPINE.md` (`AD-n` blocks) + `.memlog.md` |
| `bmad-create-epics-and-stories` | architecture + PRD/spec | epic files + stories |
| `bmad-sprint-planning` | epics/stories, or existing `sprint-status.yaml` | PASS/CONCERNS/FAIL + `sprint-status.yaml` |
| `bmad-project-context` | the repo / an observed mistake / current `AGENTS.md` | managed block in repo-root `AGENTS.md` |
| `bmad-build` | sentence, issue, `SPEC.md`, or one story + the codebase | code + per-story implementation record |
| `bmad-build-auto` | one bounded story + spec + codebase | same as build + terminal status |
| `bmad-code-review` | a diff / branch / PR | findings + optional applied patches |
| `bmad-checkpoint-preview` | a commit / branch / PR | guided walkthrough (usually conversation-only) |
| `bmad-qa-generate-e2e-tests` | implemented feature/code (+ SPEC success conditions) | API / E2E test suite |
| `bmad-correct-course` | change signal + current PRD / arch / epics / sprint | change proposal (then may rewrite those artifacts) |
| `bmad-retrospective` | spec folder, `stories.yaml`, impl records, code | retro doc + action items + acceptance verdict |
| `bmad-agent-*` | "talk to Mary/John/…" or a menu code | none — they dispatch the workflows above |
