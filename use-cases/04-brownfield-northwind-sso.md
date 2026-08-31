# 04 — Northwind Shop: add SSO (brownfield)

**Field:** brownfield  
**Size:** one epic inside a living product  
**Why this path:** the codebase already exists; agents keep running the wrong commands; the change is well-bounded (Auth0 SSO beside the current email/password stack).

**Northwind Shop** is a 4-year-old Next.js 14 + Prisma + Postgres commerce app. A new engineer joins. Ticket: enterprise buyers must sign in with their IdP (OIDC). Email/password must keep working. The repo has tribal knowledge that is not in the code: `pnpm`, a private npm registry, Playwright against a seeded Docker DB, and a hard rule that `users.email` is the account key.

---

## Skills used (and skipped)

Used: `bmad-project-context`, `bmad-help`, `bmad-deep-recon` (technical + select), `bmad-spec`, `bmad-architecture` (ratify, don't redesign), `bmad-review`, `bmad-build`, `bmad-code-review`, `bmad-checkpoint-preview`, `bmad-qa-generate-e2e-tests`, `bmad-retrospective`, `bmad-customize`.

Skipped: brainstorm, forge, brief, PRFAQ, PRD, UX (login already has a design system), create-epics-and-stories, sprint-planning (team already tracks in Linear), party-mode, build-auto (auth is high-risk), correct-course.

Deprecated names someone on the team might still type: `bmad-document-project`, `bmad-generate-project-context` → both forward here.

---

## Sequence (artifacts on the right)

```
YOU          SKILL                                ARTIFACTS WRITTEN
 |                |                                      |
 |== DAY 0: MAKE AGENTS USEFUL IN THIS REPO =============|
 |                 bmad-project-context  setup           |
 |                |  verifies: pnpm test, docker compose |
 |                |  up db, playwright against :5433     |
 |                |  records policy: email is account key|
 |                |  records pitfall: never npm install  |
 |                |                                      |  AGENTS.md   (managed BMad block)
 |                |                                      |
 |                 bmad-customize                        |  _bmad/custom/bmad-build.toml
 |                |  team override: always pnpm;         |  (shared with the squad)
 |                |  never touch schema.prisma users.email
 |                |                                      |
 |                 bmad-help                             |  (none — "spec the SSO epic")
 |                |                                      |
 |== CHOOSE THE IDP SHAPE ===============================|
 |                 bmad-deep-recon  type=technical       |  research/technical-oidc-nextauth.md
 |                 bmad-deep-recon  shape=select         |  research/select-auth0-vs-clerk-vs-ory.md
 |                |  decision: Auth0, NextAuth already   |
 |                |  in the tree, keep Prisma User       |
 |                |                                      |
 |== RATIFY HOW, THEN LOCK WHAT =========================|
 |                 bmad-architecture                     |  planning/ARCHITECTURE-SPINE.md
 |                |  BROWNFIELD MODE: does not invent a  |
 |                |  new stack. Ratifies: NextAuth is    |
 |                |  the only auth entry; User.id stays  |
 |                |  internal PK; email remains unique;  |
 |                |  new Account rows for OIDC links     |
 |                |                                      |
 |                 bmad-spec                             |  specs/spec-sso-oidc/SPEC.md
 |                |  sources: Linear ticket + spine +    |  specs/spec-sso-oidc/stories.yaml
 |                |  recon select writeup                |  specs/spec-sso-oidc/id-mapping.md
 |                |                                      |
 |                 bmad-review                           |  review/spec-sso-findings.json
 |                 lenses=adversarial,edge-case,         |  review/spec-sso-findings.md
 |                        verification-gap               |
 |                |  found: account-linking when the     |
 |                |  same email already has a password   |
 |                 bmad-spec   intent=update             |  SPEC.md (stable capability IDs)
 |                |                                      |
 |== SHIP =================================================|
 |                 bmad-build   story=1-nextauth-auth0   |  app/api/auth/[...nextauth]/
 |                |                                      |  specs/.../impl/1-nextauth-auth0.md
 |                 bmad-build   story=2-account-link     |  lib/auth/link-account.ts
 |                |                                      |  + impl record
 |                 bmad-build   story=3-admin-sso-flag   |  prisma migration + admin UI hook
 |                |                                      |  + impl record
 |                |                                      |
 |                 bmad-code-review                      |  findings + patches
 |                |  (second model, not the builder)     |
 |                 bmad-checkpoint-preview               |  (walk the linking edge cases)
 |                |                                      |
 |                 bmad-qa-generate-e2e-tests            |  e2e/sso-login.spec.ts
 |                |                                      |  e2e/sso-account-link.spec.ts
 |                |                                      |
 |== AGENT TRIPPED; RECORD IT ===========================|
 |  Amelia ran `npm test` and broke the lockfile         |
 |                 bmad-project-context  record          |  AGENTS.md  (+ pitfall line)
 |                |                                      |
 |                 bmad-retrospective                    |  implementation/retro-sso-oidc.md
 |                |  verdict: ACCEPT with note —         |  action: staging Auth0 tenant runbook
 |                |  staging tenant was undocumented     |
 v                v                                      v
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
