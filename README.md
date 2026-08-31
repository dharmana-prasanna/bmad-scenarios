# BMad Method — skill decision diagrams and scenarios

ASCII trees and sequence diagrams for **when to use which [BMad Method](https://docs.bmad-method.org/) skill**, plus seven end-to-end project scenarios (greenfield and brownfield).

Catalog source: a current BMad Method install (`skill-manifest.csv`). **49 skills: 29 current + 20 deprecated shims.**

## Start here

| File | What it is |
|---|---|
| [`bmad-skill-decision-diagrams.md`](bmad-skill-decision-diagrams.md) | Master decision tree, lifecycle sequences, agent dispatch map, recommended models, deprecated-shim map, skill → input/output cheat sheet |
| [`use-cases/README.md`](use-cases/README.md) | Index of the seven scenarios |

## Scenarios

| # | Project | Field |
|---|---|---|
| [01](use-cases/01-greenfield-mealplan-ai.md) | **MealPlan AI** — family meal planning SaaS | Greenfield product |
| [02](use-cases/02-greenfield-harborwatch.md) | **HarborWatch** — coastal flood early-warning | Greenfield, research-heavy |
| [03](use-cases/03-greenfield-invoicediff-cli.md) | **InvoiceDiff** — CLI that diffs vendor invoices | Greenfield, spec-first |
| [04](use-cases/04-brownfield-northwind-sso.md) | **Northwind Shop** — add SSO to existing Next.js commerce | Brownfield |
| [05](use-cases/05-brownfield-ledgerlite-course-correction.md) | **LedgerLite** — mid-sprint Mongo → Postgres + GDPR | Brownfield + correct-course |
| [06](use-cases/06-party-mode-atlas-health.md) | **Atlas Health** — build vs buy vs partner video visits | `bmad-party-mode` deep dive |
| [07](use-cases/07-brownfield-mealplan-course-correct.md) | **MealPlan AI year 2** — sharing ships, then Instacart dies | Brownfield + late course-correct |

## How to read a sequence

Right-hand notes are **inputs and outputs**, not extra steps.

```
You -----> skill-name
              << in:  what it consumes (your words, prior artifacts, the repo)
              >> out: what it produces
```

## Repo

This is the public copy of the local working folder. Issues and PRs welcome if a skill, artifact name, or path has drifted from current BMad Method.
