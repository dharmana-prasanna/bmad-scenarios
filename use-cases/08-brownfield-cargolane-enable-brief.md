# 08 — CargoLane: enable BMad, brief a portal, then add a requirement after it ships

**Field:** brownfield (BMad is **not** installed yet)  
**Size:** existing product + new customer surface; a **second** requirement after the first change is in production  
**Why this path:** the codebase already ships; the next bet is a **new offering**, not a ticket. You turn BMad on, write a **product brief** for the portal, ship book-and-track — then a **new requirement** arrives. You update the brief and PRD. You do not start over, reopen the ACCEPTed epic, or run `bmad-correct-course` (nothing exploded).

**CargoLane** is a 6-year-old freight TMS: Rails 7 API, React dispatcher console, Postgres, Sidekiq. Dispatchers book pickups. Shippers still email CSVs to ops. Sales has been selling a **shipper self-serve portal** for a quarter (book a load, see ETA, download POD). The concept is solid. Two teams must agree the v1 story before engineering slices it.

The repo has tribal knowledge that is not in the code: `bin/dev` (not `rails s`), Docker Postgres on `:5434`, `bundle exec rspec`, and a hard rule that `shipments.pro_number` is the customer-facing id. Billing’s nightly Sidekiq job must not be touched.

**After** epic 1 is in production, Acme Foods (largest shipper) will only go live if clerks can **request a warehouse pickup appointment** (window + dock) before the load is confirmed. Dispatch already does this in the console against `appointments`. The portal shipped without it. That is an additive requirement, not a bomb.

Pair with [04](04-brownfield-northwind-sso.md) if the next change is a **bounded ticket** (skip brief/PRD). Pair with [07](07-brownfield-mealplan-course-correct.md) if the plan **dies** mid-flight. This file is enable → brief → ship → **add a requirement**.

---

## Skills used (and skipped)

Used: install (human), `bmad-help`, `bmad-project-context`, `bmad-customize`, `bmad-deep-recon` (user-voice + competitive + later technical), `bmad-product-brief` (create, then again after the new requirement), `bmad-advanced-elicitation`, `bmad-prd` (**create**, then **update** + validate), `bmad-review`, `bmad-ux`, `bmad-architecture` (ratify + extend), `bmad-spec` (new folder per epic), `bmad-create-epics-and-stories`, `bmad-sprint-planning`, `bmad-build`, `bmad-code-review`, `bmad-checkpoint-preview`, `bmad-qa-generate-e2e-tests`, `bmad-retrospective`.

Skipped: brainstorm, forge (sales already sold the portal — concept is not fuzzy), PRFAQ (not a press-release / civic gauntlet), party-mode (the brief is the alignment artifact; if sales vs dispatch later detonates, that is [06](06-party-mode-atlas-health.md)), `bmad-document-project` (deprecated), build-auto (customer-facing booking + auth is high-risk on story 1), **`bmad-correct-course`** (the plan did not explode — scope grew after a clean ship).

Deprecated names someone might still type: `bmad-document-project`, `bmad-generate-project-context` → `bmad-project-context`. `bmad-create-brief` / old brief aliases → `bmad-product-brief`. `bmad-edit-prd` → `bmad-prd` intent=update.

---

## Sequence (`<< in` / `>> out`)

```
YOU          SKILL
 |                |
 |== ALREADY ON DISK (no BMad) ==========================
 |  Rails 7 API + React dispatcher console + Postgres
 |  Sidekiq nightly billing (do not touch)
 |  Tribal: bin/dev, Docker :5434, pro_number is the id
 |  No _bmad/, no AGENTS.md, no planning/prd.md
 |
 |== DAY 0: TURN BMAD ON IN THIS REPO ===================
 |  (human — not a skill)
 |                 npx bmad-method install
 |                      << in:  this repo; modules=bmm; tools=cursor
 |                      >> out: _bmad/  .agents/skills/  (empty custom/)
 |
 |                 bmad-help
 |                      << in:  "we want a shipper portal; BMad is brand new here"
 |                      << in:  the living Rails repo (no planning artifacts)
 |                      >> out: (none — "project-context first, then brief the portal")
 |
 |                 bmad-project-context  setup
 |                      << in:  the living TMS (bin/dev, :5434, rspec,
 |                              pro_number is customer id, never touch billing job)
 |                      >> out: AGENTS.md  (managed BMad block)
 |
 |                 bmad-customize
 |                      << in:  "always bin/dev; never rename shipments.pro_number;
 |                              never enqueue on the billing queue"
 |                      >> out: _bmad/custom/bmad-build.toml  (team — commit this)
 |
 |  PR the install: _bmad/, .agents/skills/, AGENTS.md, team custom TOML.
 |  Do not run bmad-prd create for the whole TMS. Do not document-project.
 |
 |== EVIDENCE FOR THE NEW SURFACE (not a rewrite) =======
 |                 bmad-deep-recon  type=user-voice
 |                      << in:  "shippers who email CSVs; CS tickets about ETA"
 |                      << in:  AGENTS.md  (so recon stays in this domain)
 |                      >> out: research/user-voice-shipper-csv-pain.md
 |                 bmad-deep-recon  type=competitive
 |                      << in:  shipper portals: Freightos, project44, carrier tools
 |                      >> out: research/competitive-shipper-portals.md
 |
 |== WRITE THE VISION — THIS IS WHY BRIEF IS HERE =======
 |                 bmad-product-brief
 |                      << in:  "shipper self-serve: book load, ETA, POD"
 |                      << in:  user-voice + competitive recon
 |                      << in:  living product facts (dispatcher console stays;
 |                              portal is additive; same Shipment source of truth)
 |                      >> out: planning/brief.md
 |                      >> out: planning/addendum.md
 |                 BROWNFIELD: the brief is about the *portal*, not a
 |                 six-year history of CargoLane. Legacy TMS is context,
 |                 not the document you are writing.
 |
 |                 bmad-advanced-elicitation  method=pre-mortem
 |                      << in:  the just-written brief.md
 |                      >> out: brief.md rewritten in place
 |                 (catches: "v1 shows all contract rates" would leak
 |                  competitor pricing to every clerk at the shipper)
 |
 |                 bmad-review  lenses=adversarial,structure
 |                      << in:  planning/brief.md
 |                      >> out: review/brief-findings.json + .md
 |
 |== ORG NEEDS SIGN-OFF ON THE NEW OFFERING =============
 |                 bmad-prd   intent=create
 |                      << in:  brief.md + addendum.md + recon
 |                      >> out: planning/prd.md          ← portal only
 |                      >> out: planning/addendum.md
 |                      >> out: planning/.memlog.md
 |
 |                 bmad-prd   intent=validate
 |                      << in:  finished prd.md          ← you / the create run
 |                      << in:  shipped rubric           ← bmad-prd
 |                      >> out: validation-report.html + .md
 |
 |-- new customer UI ----------------------------------->|
 |                 bmad-ux
 |                      << in:  prd.md (portal is a new surface)
 |                      >> out: planning/DESIGN.md
 |                      >> out: planning/EXPERIENCE.md
 |                      >> out: planning/.memlog.md
 |
 |== RATIFY HOW + GENERATE ADRs =========================
 |                 bmad-architecture   intent=create
 |                      << in:  prd.md + DESIGN.md + the existing Rails codebase
 |                      << in:  Coaching path (default) — you choose the
 |                              load-bearing calls; Fast path tags [ASSUMPTION]
 |                      >> out: planning/ARCHITECTURE-SPINE.md
 |                              (paradigm + AD-1…AD-n + conventions + seed)
 |                      >> out: planning/architecture/.memlog.md
 |                              (rationale — not copied into the AD blocks)
 |                 HOW ADRs ARE MADE (no separate bmad-adr skill):
 |                 1. Skill appends each call to the memlog
 |                    (what it binds + the divergence it prevents).
 |                 2. Finalize *distills* survivors into AD-n blocks
 |                    (stable ID, Binds / Prevents / Rule).
 |                 3. [ADOPTED] = already true in the living TMS.
 |                 BROWNFIELD ADs this run (IDs stay forever):
 |                   AD-1 [ADOPTED] Shipment / Stop / Carrier are the
 |                        only booking source of truth
 |                   AD-2 [ADOPTED] pro_number is the customer-facing id
 |                   AD-3          portal is a Next.js app on the existing
 |                        API — no second booking table
 |                   AD-4 [ADOPTED] billing Sidekiq job is out of bounds
 |                 Optional at Finalize: a human-facing deck/HTML if you
 |                 asked for one. That is a rendering. The ADRs are the
 |                 AD-n blocks. Specs cite those IDs.
 |
 |                 bmad-spec  + story breakdown
 |                      << in:  prd.md + UX + spine + brief (v1 scope)
 |                      >> out: specs/spec-shipper-book-and-track/SPEC.md
 |                      >> out: specs/spec-shipper-book-and-track/stories.yaml
 |                      >> out: specs/spec-shipper-book-and-track/id-mapping.md
 |
 |                 bmad-review  lenses=adversarial,edge-case,verification-gap
 |                      << in:  SPEC.md
 |                      >> out: review/spec-portal-findings.json + .md
 |                 bmad-spec   intent=update
 |                      << in:  existing SPEC.md + review findings
 |                      >> out: SPEC.md (stable capability IDs)
 |
 |== SLICE + GATE =======================================
 |                 bmad-create-epics-and-stories
 |                      << in:  ARCHITECTURE-SPINE.md + prd.md
 |                      >> out: planning/epics/epic-1-shipper-book-and-track.md
 |
 |                 bmad-sprint-planning
 |                      << in:  epics/*.md + stories
 |                      >> out: implementation/sprint-status.yaml
 |                      >> out: readiness PASS | CONCERNS | FAIL
 |
 |== SHIP EPIC 1 (this change is done) ==================
 |                 bmad-build   story=1-shipper-auth-tenant
 |                      << in:  stories.yaml #1 + SPEC.md + existing User/Account
 |                      >> out: portal auth + tenant scope + impl record
 |                 bmad-build   story=2-book-load
 |                      << in:  stories.yaml #2 + SPEC.md + story-1 + Rails API
 |                      >> out: book-load flow hitting existing Shipment + impl
 |                 bmad-build   story=3-eta-and-pod
 |                      << in:  stories.yaml #3 + SPEC.md + booking code
 |                      >> out: status + POD download + impl record
 |
 |                 bmad-code-review
 |                      << in:  the portal diff (second model, not the builder)
 |                      >> out: findings + optional patches
 |                 bmad-checkpoint-preview
 |                      << in:  the book-load PR
 |                      >> out: walkthrough of tenant isolation + pro_number
 |
 |                 bmad-qa-generate-e2e-tests
 |                      << in:  implemented book-and-track + SPEC success signal
 |                      >> out: e2e/shipper-book.spec.ts, e2e/shipper-pod.spec.ts
 |
 |                 bmad-retrospective
 |                      << in:  spec folder + stories.yaml + impl + code
 |                      >> out: implementation/retro-shipper-portal-epic-1.md
 |                      >> out: ACCEPT — book / ETA / POD are in production
 |
 |== ON DISK AFTER THE FIRST CHANGE =====================
 |  AGENTS.md + _bmad/custom/            (BMad is on)
 |  planning/brief.md                    (v1: book, ETA, POD — no appointments)
 |  planning/prd.md                      (created from that brief)
 |  specs/spec-shipper-book-and-track/   (ACCEPTed)
 |  portal auth + book + track           (shipped)
 |  sprint-status.yaml                   epic 1 done
 |
 |== NEW REQUIREMENT (additive — not a bomb) ============
 |  Acme Foods will not go live without warehouse
 |  appointment windows (request + confirm) before
 |  the load is confirmed. Dispatch already has this
 |  in the console. Portal v1 shipped without it.
 |
 |                 bmad-help
 |                      << in:  "Acme needs appointment windows; epic 1 shipped"
 |                      << in:  brief.md + prd.md + spine + sprint-status.yaml
 |                      >> out: (none — "update the brief, then PRD update,
 |                              new spec; do not reopen ACCEPTed epic 1;
 |                              this is not correct-course")
 |
 |                 bmad-deep-recon  type=technical
 |                      << in:  existing appointments table + dispatcher UI
 |                      << in:  shipped portal book-load flow
 |                      >> out: research/technical-appointments-already-in-tms.md
 |                 BROWNFIELD: the model exists. You are exposing it,
 |                 not inventing a second appointments table.
 |
 |                 bmad-product-brief
 |                      << in:  existing brief.md + addendum.md
 |                      << in:  "appointment request + confirmation before book confirms"
 |                      << in:  living portal (auth / book / track already shipped)
 |                      >> out: planning/brief.md          (vision now includes appointments)
 |                      >> out: planning/addendum.md
 |                 Same skill as Day 0. You do not write a second product.
 |                 You update the portal vision now that v1 exists.
 |
 |                 bmad-review  lenses=adversarial,structure
 |                      << in:  updated brief.md
 |                      >> out: review/brief-appointments-findings.json + .md
 |
 |                 bmad-prd   intent=update
 |                      << in:  existing prd.md          ← the create run above
 |                      << in:  change signal            ← updated brief + Acme ask
 |                      << in:  technical recon
 |                      >> out: planning/prd.md          (appointments in scope)
 |                      >> out: planning/addendum.md
 |                      >> out: planning/.memlog.md
 |
 |                 bmad-prd   intent=validate
 |                      << in:  finished prd.md
 |                      << in:  shipped rubric
 |                      >> out: validation-report.html + .md
 |
 |                 bmad-ux
 |                      << in:  updated prd.md (request window / confirmation states)
 |                      >> out: planning/DESIGN.md
 |                      >> out: planning/EXPERIENCE.md
 |
 |                 bmad-architecture   intent=update
 |                      << in:  existing ARCHITECTURE-SPINE.md + its .memlog.md
 |                      << in:  updated prd.md + DESIGN.md + living codebase
 |                      << in:  (shipped portal + existing Appointment)
 |                      >> out: planning/ARCHITECTURE-SPINE.md  (re-distilled)
 |                      >> out: planning/architecture/.memlog.md (new lines appended)
 |                 Resume the memlog. Keep AD-1…AD-4. Never renumber.
 |                 New ADR only:
 |                   AD-5 [ADOPTED] Appointment is already-in-TMS;
 |                        portal calls the same API; book-load (AD-3)
 |                        stays the only create-Shipment path;
 |                        confirm waits on a dock window.
 |                 Amend a Rule in place if AD-3's wording must mention
 |                 confirm-vs-book; do not invent AD-1b.
 |
 |                 bmad-spec  + story breakdown
 |                      << in:  updated prd + UX + spine + updated brief
 |                      >> out: specs/spec-shipper-appointments/SPEC.md
 |                      >> out: specs/spec-shipper-appointments/stories.yaml
 |                      >> out: specs/spec-shipper-appointments/id-mapping.md
 |                 NEW spec folder. Do not reopen
 |                 spec-shipper-book-and-track (already ACCEPTed).
 |
 |                 bmad-create-epics-and-stories
 |                      << in:  new spine + updated prd.md
 |                      >> out: planning/epics/epic-2-shipper-appointments.md
 |                      epic 1 left alone (already ACCEPTed)
 |
 |                 bmad-sprint-planning
 |                      << in:  new epic + existing sprint-status.yaml
 |                      >> out: implementation/sprint-status.yaml
 |                      >> out: readiness PASS | CONCERNS | FAIL
 |
 |                 bmad-build   story=1-request-window
 |                      << in:  stories.yaml #1 + new SPEC.md + shipped portal
 |                      >> out: appointment request UI + impl record
 |                 bmad-build   story=2-confirm-after-dock
 |                      << in:  stories.yaml #2 + SPEC.md + story-1 + Rails Appointment
 |                      >> out: confirm-load waits on dock + impl record
 |
 |                 bmad-code-review
 |                      << in:  the appointments diff (second model, not the builder)
 |                      >> out: findings + optional patches
 |                 bmad-checkpoint-preview
 |                      << in:  the confirm-after-dock PR
 |                      >> out: walkthrough of "book is not confirm"
 |
 |                 bmad-qa-generate-e2e-tests
 |                      << in:  implemented appointments + SPEC success signal
 |                      >> out: e2e/shipper-appointment.spec.ts
 |
 |                 bmad-retrospective
 |                      << in:  spec-shipper-appointments/ + stories + impl + code
 |                      >> out: implementation/retro-shipper-portal-epic-2.md
 |                      >> out: ACCEPT — Acme can request a window
 v                v
```

---

## What each phase left on disk

```
cargolane/
├── AGENTS.md                              ← first BMad artifact (house rules)
├── _bmad/                                 ← installer
│   └── custom/bmad-build.toml             ← team: bin/dev, pro_number, billing
├── .agents/skills/
├── (existing) app/  spec/  config/        ← Rails TMS, unchanged billing job
├── (new)      portal/                     ← Next.js shipper app (epic 1 + 2)
├── e2e/shipper-book.spec.ts
├── e2e/shipper-pod.spec.ts
├── e2e/shipper-appointment.spec.ts        ← after the new requirement
├── {planning_artifacts}/
│   ├── research/user-voice-shipper-csv-pain.md
│   ├── research/competitive-shipper-portals.md
│   ├── research/technical-appointments-already-in-tms.md
│   ├── brief.md                           ← updated after Acme (was book/ETA/POD)
│   ├── addendum.md
│   ├── prd.md                             ← created from first brief, then updated
│   ├── prd-validation.html
│   ├── prd-validation.md
│   ├── DESIGN.md
│   ├── EXPERIENCE.md
│   ├── ARCHITECTURE-SPINE.md              ← AD-1…AD-4, then AD-5 (IDs stable)
│   ├── architecture/.memlog.md            ← ADR rationale (append-only)
│   ├── epics/
│   │   ├── epic-1-shipper-book-and-track.md   ← ACCEPTed — do not reopen
│   │   └── epic-2-shipper-appointments.md
│   └── .memlog.md                         ← PRD run memlog (separate)
├── {output}/specs/spec-shipper-book-and-track/
│   ├── SPEC.md
│   ├── stories.yaml
│   ├── id-mapping.md
│   └── impl/                              ← epic 1 — left alone
├── {output}/specs/spec-shipper-appointments/
│   ├── SPEC.md                            ← new folder for the new requirement
│   ├── stories.yaml
│   ├── id-mapping.md
│   └── impl/
└── {implementation_artifacts}/
    ├── sprint-status.yaml
    ├── retro-shipper-portal-epic-1.md
    └── retro-shipper-portal-epic-2.md
```

There was no legacy `prd.md` to update on Day 0. The brief is the first product artifact. After epic 1 ships, the same `brief.md` and `prd.md` are **updated** — you do not create a second product history.

---

## What this use case teaches

- **Enable first, then plan.** On a repo that never had BMad, Day 0 is install → `bmad-help` → `bmad-project-context` → `bmad-customize`. A brief written before house rules is how agents invent `rails s` and a second booking table.
- **`bmad-product-brief` belongs on brownfield** when the next bet is a **new offering** with a clear concept and more than one stakeholder. It is not a greenfield-only skill.
- The brief describes the **new surface**. It does not pretend to be the product history of the TMS.
- After the first change **ships**, a new requirement is still a brief + PRD job — same files, `prd` **update**, new spec folder. You already have artifacts; use them.
- **Do not reopen** an ACCEPTed epic. Book-and-track stays. Appointments get `spec-shipper-appointments/`.
- **Do not run `bmad-correct-course`** for additive scope. Course-correct is for when the plan dies (partner API, legal, v1 was wrong). Acme asking for appointments is a new requirement, not a bomb. That contrast is [07](07-brownfield-mealplan-course-correct.md).
- [04](04-brownfield-northwind-sso.md) skipped brief/PRD because SSO was a Linear ticket. If your next change is that small, do 04, not this.
- [01](01-greenfield-mealplan-ai.md) uses the same brief skill after brainstorm + forge. Here the concept is already sold — skip the think block.
- Architecture **ratifies** `Shipment` / `Stop` / `Carrier` / (later) `Appointment`. The portal does not get its own source of truth.
- **ADRs come from `bmad-architecture`**, not a separate skill. See the next section.
- Do not run `bmad-document-project`. Do not install BMad on every repo the same week.

---

## How to generate ADRs

There is no `bmad-adr` skill. Architecture Decision Records are the **`AD-n` blocks** inside `ARCHITECTURE-SPINE.md`. `bmad-architecture` writes them.

### Run it

| When | Invoke | What happens |
|---|---|---|
| First spine (after PRD / UX, or from the living repo) | `/bmad-architecture` — create | Coaching path by default (you pick the load-bearing calls). Fast path drafts the whole spine with `[ASSUMPTION]` tags. |
| New requirement after a spine exists | `/bmad-architecture` — **update** | Reloads `.memlog.md`, appends new decisions, **keeps AD IDs**, adds the next `AD-n`. |
| Pressure-test without changing it | `/bmad-architecture` — **validate** | Reviewer gate + HTML report; offer to roll findings into an update. |

Brownfield: the skill **reads the real code** and marks living conventions `[ADOPTED]`. It does not invent a new stack.

### What lands on disk

```
planning/
├── ARCHITECTURE-SPINE.md         ← the ADRs (AD-1, AD-2, …) — terse, citable
└── architecture/.memlog.md       ← why you chose them — append-only working memory
```

The skill logs each call to the memlog first (`what it binds` + `the divergence it prevents`). **Finalize distills** survivors into the spine. Rationale stays in the memlog. The spine is decisions, not essays.

### Shape of one ADR (from the shipped spine template)

```markdown
### AD-3 — Portal is an API client, not a second TMS

- **Binds:** shipper book-load, ETA, POD
- **Prevents:** a second `shipments` table / a second booking service
- **Rule:** every write goes through the existing Rails API; `Shipment` remains the source of truth
```

`[ADOPTED]` on the heading (or in the Rule) means the living system already settled it — typical on brownfield (AD-1, AD-2, AD-4, later AD-5).

### Rules the team should say out loud

1. **One test for what becomes an ADR:** if two teams built this independently, could they choose incompatibly? If no, put it under Deferred — do not mint an AD.
2. **IDs never get reused or renumbered.** New requirement → next `AD-n`. Retired decision stays; do not recycle `AD-3`.
3. **Specs and stories cite AD IDs.** Offer to attach the spine as a spec companion so Build can see `AD-3`.
4. **Do not hand-write `docs/adr/0001-*.md` unless the org already requires Nygard files.** If you need a walkthrough for humans, say so at Finalize — the skill can emit an extra HTML/md deck. That is a rendering of the spine, not a second set of decisions.
5. **Do not run the deprecated `bmad-create-architecture`.** It forwards here. The old architecture-decision-template is gone.

### CargoLane IDs (this file)

| ID | When minted | Status | Decision |
|---|---|---|---|
| AD-1 | First architecture run | `[ADOPTED]` | `Shipment` / `Stop` / `Carrier` are the only booking source of truth |
| AD-2 | First architecture run | `[ADOPTED]` | `pro_number` is the customer-facing id |
| AD-3 | First architecture run | new | Portal is Next.js on the existing API; no second booking table |
| AD-4 | First architecture run | `[ADOPTED]` | Billing Sidekiq job is out of bounds |
| AD-5 | After Acme appointments | `[ADOPTED]` | `Appointment` already lives in the TMS; confirm waits on a dock window |

Epic 2 **updates** the spine and adds AD-5. It does not rewrite AD-1–AD-4 and does not open a second spine.

---

## When to pick 04 vs 08 vs 07 vs 05

| Situation | File |
|---|---|
| BMad off; next change is a **bounded ticket** | [04 Northwind SSO](04-brownfield-northwind-sso.md) — project-context → spec → build |
| BMad off; next change is a **new offering**; later a **new requirement after something shipped** | **This file** — enable → brief → PRD create → ship → brief again + PRD update → new spec |
| BMad (or a PRD) already on disk; later epic **explodes** | [07 MealPlan year 2](07-brownfield-mealplan-course-correct.md) — `correct-course` |
| You walk in *at* the bomb | [05 LedgerLite](05-brownfield-ledgerlite-course-correction.md) |
