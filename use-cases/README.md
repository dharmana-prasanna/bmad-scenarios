# End-to-end use cases

Six projects that walk BMad Method from a messy question to shipped work. Each file is a standalone ASCII sequence with the artifacts each skill writes.

Skill catalog and “when to use which” live in [`../bmad-skill-decision-diagrams.md`](../bmad-skill-decision-diagrams.md).

| # | Project | Field | Why it exists | Party mode? |
|---|---|---|---|---|
| [01](01-greenfield-mealplan-ai.md) | **MealPlan AI** — family meal planning SaaS | Greenfield | Full product path: think → brief/PRD → UX → arch → spec → sprint → build | Light (one default-room debate) |
| [02](02-greenfield-harborwatch.md) | **HarborWatch** — coastal flood early-warning | Greenfield | Research-heavy, civic, PRFAQ instead of a gentle brief | Yes (ethics / liability roundtable) |
| [03](03-greenfield-invoicediff-cli.md) | **InvoiceDiff** — CLI that diffs vendor invoices | Greenfield | Epic-sized, spec-first; skip PRD/UX/sprint theater | No |
| [04](04-brownfield-northwind-sso.md) | **Northwind Shop** — add SSO to existing Next.js commerce | Brownfield | Join a live repo, teach agents the house rules, ship one epic | No |
| [05](05-brownfield-ledgerlite-course-correction.md) | **LedgerLite** — mid-sprint Mongo → Postgres + GDPR | Brownfield | Plan explodes; `bmad-correct-course` reroutes the train | Yes (data-residency fight) |
| [06](06-party-mode-atlas-health.md) | **Atlas Health** — build vs buy vs partner video visits | Mixed | Dedicated `bmad-party-mode` deep dive: custom focus group, two rooms, memory, keepsake | **Primary** |

## How to read a sequence

Right-hand notes are **artifacts produced**, not extra steps.

```
You -----> skill-name                 >> writes: path/or-file
                                      >> reads:  earlier artifact
```

Conversation-only skills (`bmad-help`, `bmad-advanced-elicitation`, `bmad-checkpoint-preview` mid-walkthrough) still appear; they may write nothing durable.

## Artifact cheat sheet (current skills)

| Skill | Typical artifacts |
|---|---|
| `bmad-help` | None (next-skill recommendation) |
| `bmad-brainstorming` | `{output}/brainstorming/brainstorm.html`, optional `brainstorm-intent.md` |
| `bmad-forge-idea` | `{output}/forge/forge-report.html`, optional `forged-idea.md` |
| `bmad-deep-recon` | `{planning}/research/*.md` (+ optional HTML briefing) |
| `bmad-advanced-elicitation` | In-place improvements to the draft just produced |
| `bmad-review` | Findings JSON + markdown report |
| `bmad-party-mode` | `{output}/party-mode/YYYY-MM-DD-*.html` keepsake; `{output}/party-mode/memories/<party>/.memlog.md`; optional `_bmad/custom/bmad-party-mode.user.toml` |
| `bmad-customize` | `_bmad/custom/<skill>.toml` or `<skill>.user.toml` |
| `bmad-product-brief` | `brief.md` + `addendum.md` |
| `bmad-prfaq` | `prfaq-*.md` |
| `bmad-prd` | Create/update: `prd.md`, `addendum.md`, `.memlog.md`. Validate: HTML + `.md` findings |
| `bmad-ux` | `DESIGN.md`, `EXPERIENCE.md`, `.memlog.md` |
| `bmad-spec` | `specs/spec-{slug}/SPEC.md` + companions; optional `stories.yaml` |
| `bmad-architecture` | `ARCHITECTURE-SPINE.md` (brownfield: ratifies the existing system) |
| `bmad-create-epics-and-stories` | Epic files + stories under planning artifacts |
| `bmad-sprint-planning` | Readiness verdict (PASS/CONCERNS/FAIL) + `sprint-status.yaml` |
| `bmad-project-context` | Managed block in repo-root `AGENTS.md` |
| `bmad-build` | Code + per-story implementation record under the spec folder |
| `bmad-build-auto` | Same as build, plus terminal status for an orchestrator |
| `bmad-code-review` | Findings + optional applied patches |
| `bmad-checkpoint-preview` | Guided walkthrough (usually conversation-only) |
| `bmad-qa-generate-e2e-tests` | API / E2E test suite in the repo |
| `bmad-correct-course` | Change proposal (then may rewrite PRD / arch / epics / sprint) |
| `bmad-retrospective` | Retro doc + action items + acceptance verdict |
| `bmad-agent-*` | None of their own — they dispatch the workflows above |
