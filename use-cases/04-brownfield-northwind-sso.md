# 04 — Northwind Shop: add SSO (brownfield)

**Field:** brownfield  
**Size:** one epic inside a living product  
**Why this path:** the codebase already exists; agents keep running the wrong commands; the change is well-bounded (Auth0 SSO beside the current email/password stack).

**Northwind Shop** is a 4-year-old Next.js 14 + Prisma + Postgres commerce app. A new engineer joins. Ticket: enterprise buyers must sign in with their IdP (OIDC). Email/password must keep working. The repo has tribal knowledge that is not in the code: `pnpm`, a private npm registry, Playwright against a seeded Docker DB, and a hard rule that `users.email` is the account key.

---

## Skills used (and skipped)

Used: `bmad-project-context`, `bmad-help`, `bmad-deep-recon` (technical + select), `bmad-spec`, `bmad-architecture` (ratify, don't redesign), `bmad-review`, `bmad-build`, `bmad-code-review`, `bmad-checkpoint-preview`, `bmad-qa-generate-e2e-tests`, `bmad-retrospective`, `bmad-customize`.

Skipped: brainstorm, forge, brief, PRFAQ, PRD, UX (login already has a design system), create-epics-and-stories, sprint-planning (team already tracks in Linear), party-mode, build-auto (auth is high-risk), correct-course. If the next change is a **new offering** that needs a written vision, use [08 CargoLane](08-brownfield-cargolane-enable-brief.md) instead (enable + `bmad-product-brief`).

Deprecated names someone on the team might still type: `bmad-document-project`, `bmad-generate-project-context` → both forward here.

---

## Sequence (`<< in` / `>> out`)

```
YOU          SKILL
 |                |
 |== DAY 0: MAKE AGENTS USEFUL IN THIS REPO =============
 |                 bmad-project-context  setup
 |                      << in:  the living Next.js repo (pnpm, docker compose,
 |                              Playwright :5433, email-is-account-key policy)
 |                      >> out: AGENTS.md  (managed BMad block)
 |
 |                 bmad-customize
 |                      << in:  "always pnpm; never touch users.email"
 |                      >> out: _bmad/custom/bmad-build.toml  (team)
 |
 |                 bmad-help
 |                      << in:  Linear ticket + AGENTS.md + repo
 |                      >> out: (none — "spec the SSO epic")
 |
 |== CHOOSE THE IDP SHAPE ===============================
 |                 bmad-deep-recon  type=technical
 |                      << in:  "OIDC beside existing NextAuth + Prisma User"
 |                      >> out: research/technical-oidc-nextauth.md
 |                 bmad-deep-recon  shape=select
 |                      << in:  Auth0 vs Clerk vs Ory
 |                      >> out: research/select-auth0-vs-clerk-vs-ory.md
 |
 |== RATIFY HOW, THEN LOCK WHAT =========================
 |                 bmad-architecture
 |                      << in:  the existing codebase + recon select
 |                      >> out: planning/ARCHITECTURE-SPINE.md
 |                 BROWNFIELD: ratifies NextAuth as the only auth entry;
 |                 User.id stays PK; email stays unique; new Account rows.
 |
 |                 bmad-spec
 |                      << in:  Linear ticket + spine + select writeup
 |                      >> out: specs/spec-sso-oidc/SPEC.md
 |                      >> out: specs/spec-sso-oidc/stories.yaml
 |                      >> out: specs/spec-sso-oidc/id-mapping.md
 |
 |                 bmad-review  lenses=adversarial,edge-case,verification-gap
 |                      << in:  SPEC.md
 |                      >> out: review/spec-sso-findings.json + .md
 |                 bmad-spec   intent=update
 |                      << in:  existing SPEC.md + review findings
 |                      >> out: SPEC.md (stable capability IDs)
 |
 |== SHIP =================================================
 |                 bmad-build   story=1-nextauth-auth0
 |                      << in:  stories.yaml #1 + SPEC.md + existing NextAuth
 |                      >> out: app/api/auth/[...nextauth]/ + impl record
 |                 bmad-build   story=2-account-link
 |                      << in:  stories.yaml #2 + SPEC.md + story-1 code
 |                      >> out: lib/auth/link-account.ts + impl record
 |                 bmad-build   story=3-admin-sso-flag
 |                      << in:  stories.yaml #3 + SPEC.md + linking code
 |                      >> out: prisma migration + admin UI hook + impl record
 |
 |                 bmad-code-review
 |                      << in:  the SSO diff (second model, not the builder)
 |                      >> out: findings + patches
 |                 bmad-checkpoint-preview
 |                      << in:  the linking PR
 |                      >> out: walkthrough of account-link edge cases
 |
 |                 bmad-qa-generate-e2e-tests
 |                      << in:  implemented SSO + SPEC success conditions
 |                      >> out: e2e/sso-login.spec.ts, e2e/sso-account-link.spec.ts
 |
 |== AGENT TRIPPED; RECORD IT ===========================
 |  Amelia ran `npm test` and broke the lockfile
 |                 bmad-project-context  record
 |                      << in:  the observed mistake (npm vs pnpm)
 |                      >> out: AGENTS.md  (+ pitfall line)
 |
 |                 bmad-retrospective
 |                      << in:  spec folder + stories.yaml + impl + code
 |                      >> out: implementation/retro-sso-oidc.md + ACCEPT
 v                v
```

---

## What each phase left on disk

```
northwind-shop/
├── AGENTS.md                          ← the brownfield starting artifact
├── _bmad/custom/bmad-build.toml
├── app/api/auth/[...nextauth]/
├── lib/auth/link-account.ts
├── e2e/sso-login.spec.ts
├── e2e/sso-account-link.spec.ts
├── {planning_artifacts}/
│   ├── research/technical-oidc-nextauth.md
│   ├── research/select-auth0-vs-clerk-vs-ory.md
│   └── ARCHITECTURE-SPINE.md          ← ratifies the living system
├── specs/spec-sso-oidc/
│   ├── SPEC.md
│   ├── stories.yaml
│   ├── id-mapping.md
│   └── impl/
└── {implementation_artifacts}/retro-sso-oidc.md
```

No new `prd.md`. The Linear ticket + recon + ratified spine were enough intent.

---

## What this use case teaches

- First skill in a strange repo is `bmad-project-context`, not `bmad-build`.
- Brownfield `bmad-architecture` **ratifies** invariants; it does not propose a rewrite.
- `bmad-customize` is how the squad makes "always pnpm" survive the next session.
- After an agent repeats a mistake, `bmad-project-context record` is the skill — not a Slack rant.
- Skip PRD/UX/sprint-planning when the org already has a ticket tracker and a design system.
