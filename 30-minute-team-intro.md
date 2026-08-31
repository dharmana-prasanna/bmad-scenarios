# Introduce BMad to the team — 30 minutes

Facilitator script. One screen, this repo, no extra slides.

**Goal:** after 30 minutes the team can (1) say what BMad is, (2) size a change, (3) turn it on for an **existing** repo and a **new** repo.

**Prep (you, the night before):**

- This repo open
- [install-bmad-method.md](install-bmad-method.md) bookmarked
- Optional: a throwaway folder with BMad already installed so `/bmad-help` works if someone asks “does it actually run?”
- Name one **pilot existing repo** and, if you have one this quarter, one **new project**

**Leave-behind:** this file + [team-enablement-plan.md](team-enablement-plan.md) (90-minute version and week-by-week playbooks). Do not invent a slide deck.

---

## Minute map

| Min | What | Open |
|---|---|---|
| 0–6 | What BMad is | This file, “The one picture” |
| 6–10 | Size first | [diagrams §3](bmad-skill-decision-diagrams.md#3-size-the-work-first--then-pick-a-path) |
| 10–20 | Enable an **existing** project | Commands below + [use-case 04](use-cases/04-brownfield-northwind-sso.md) Day 0 |
| 20–27 | Enable a **new** project | Commands below + [use-case 01](use-cases/01-greenfield-mealplan-ai.md) / [03](use-cases/03-greenfield-invoicediff-cli.md) |
| 27–30 | How we work this week | Working agreement + assign steward + first ticket |

If you run long, cut the new-project walk to the command block only. Existing-project enable is the one they need.

---

## 0–6 — What BMad is

Say this, then stop talking:

> BMad is a set of **named skills** that live **in the project**, not a global chatbot. Each skill has a job, takes inputs, and writes artifacts. Agents are **hats** (PM, architect, engineer), not extra headcount. We skip skills we do not need.

Show the notation so people stop thinking “the AI just chats”:

```
You -----> skill-name
              << in:  ticket, repo, prior artifacts
              >> out: a file the next session can read
```

**Five hats** (one person can wear more than one):

| Hat | Agent | Typical artifact |
|---|---|---|
| Research | Mary | recon, brief |
| Product | John | `prd.md`, epics |
| Design | Sally | `DESIGN.md` |
| Tech lead | Winston | `ARCHITECTURE-SPINE.md` |
| Engineer | Amelia | code + impl record |

**The sentence that prevents a rewrite:**

> We do not BMad last year’s code. We install it, teach the house rules, and use it on the **next** change.

Point at [use-case 04](use-cases/04-brownfield-northwind-sso.md) if someone asks “what about our four-year-old app?” — Day 0 is `project-context`, not a generated novel of the whole product.

---

## 6–10 — Size first, then pick a skill

Open [diagrams §3](bmad-skill-decision-diagrams.md#3-size-the-work-first--then-pick-a-path). Put this table on the last slide of the meeting (or leave this file up):

| The change is… | Default | Do not |
|---|---|---|
| One sitting, intent is clear | `bmad-build` | Write a PRD |
| Several sittings, one outcome | `bmad-spec` → Build per story | Invent sprint theater if Linear already is the board |
| New product / multi-epic | Brief or PRFAQ → PRD → spec per epic | Run every skill “because it exists” |
| Existing repo, first week | `bmad-project-context` **then** a row above | Generate a full “document this project” dump |
| Existing repo, new offering (concept is solid) | Enable, then `bmad-product-brief` for **that** surface — [08](use-cases/08-brownfield-cargolane-enable-brief.md) | Write a fake PRD of the last six years |
| Plan just exploded | `bmad-correct-course` | Keep coding with a Slack footnote |

Say:

> A 2-hour ticket tomorrow is `/bmad-build`. An SSO epic is `/bmad-spec`. A new product is a PRD. Same toolkit, different boxes.

If someone asks about the 49 skills: **29 current, 20 deprecated shims.** You will not demo all of them. Point at the [shim map](bmad-skill-decision-diagrams.md) and move on.

---

## 10–20 — Enable an existing project

This is the path for repos that already ship. Walk it as a script; do not live-install on production in the meeting.

### What you are doing

Install BMad **in that repo**. Teach agents the house rules. Use it on the **next** ticket. Do not recreate four years of PRDs.

### Day 0 (steward + one engineer, later this week — ~half a day)

```bash
# 1. Prerequisites once per machine: Node 20.12+, Python 3.10+, uv, Git
#    Details: install-bmad-method.md

cd /path/to/existing-app

# 2. Install into this repo (Cursor + Agile suite)
npx bmad-method install \
  --directory . \
  --modules bmm \
  --tools cursor \
  --yes
```

Then, in Cursor on that repo:

| Step | Skill | `<< in` | `>> out` |
|---|---|---|---|
| Prove it loaded | `/bmad-help` | “what should we do next?” | (none — a recommendation) |
| Teach the house | `/bmad-project-context` **setup** | real repo facts: package manager, how tests run, the one rule that is not in the code | `AGENTS.md` managed block |
| Lock 2–3 team facts | `/bmad-customize` | e.g. always `pnpm`; never touch `users.email` | `_bmad/custom/*.toml` — **commit** |
| First real change | size the ticket, then `bmad-build` **or** `bmad-spec` | ticket + `AGENTS.md` + repo | code / `SPEC.md` |

**PR the install:** `_bmad/`, `.agents/skills/` (or your tool’s copies), `AGENTS.md`, team `custom/*.toml`. Decide as a team whether `_bmad-output/` is gitignored.

### What you refuse on an existing repo

- `bmad-prd` **create** for the whole product on day 1
- `bmad-document-project` (deprecated; it made agents worse)
- Rewriting last year’s features “so they are BMad”
- `bmad-build-auto` on auth, money, or the first story of a new pattern
- Installing on every repo the same week — **one pilot**, then copy team TOML

### First ticket (week 1)

| Ticket size | Run |
|---|---|
| Clear, one session | `bmad-build` only |
| Several sessions, one outcome | `bmad-spec` (ticket + docs you already have) → Build per story |
| Two teams must sign the same “what” | `bmad-prd` **update** if a PRD exists; else a short spec + your existing one-pager |

Paper walk if you have 2 extra minutes: [use-case 04](use-cases/04-brownfield-northwind-sso.md) Day 0 only (project-context → customize → help). Skip the SSO build in this meeting. If the next bet is a **new offering** (not a ticket), point at [08 CargoLane](use-cases/08-brownfield-cargolane-enable-brief.md) — enable, brief the new surface, and after it ships **update** the brief/PRD for the next requirement (do not course-correct unless the plan died).

---

## 20–27 — Enable a new project

### Day 0

```bash
mkdir my-product && cd my-product && git init

npx bmad-method install \
  --directory . \
  --modules bmm \
  --tools cursor \
  --yes
```

Copy the **same** team `_bmad/custom/*.toml` you already use on the pilot (package manager, “never commit secrets”). Invoke `/bmad-help`.

### Size the idea, then pick a path

| Starting point | Follow |
|---|---|
| Fuzzy — “we want something with X” | [01 MealPlan](use-cases/01-greenfield-mealplan-ai.md): brainstorm → forge → recon → brief/PRD |
| Engineer already knows the flags | [03 InvoiceDiff](use-cases/03-greenfield-invoicediff-cli.md): spec + thin spine + build — **skip PRD** |
| Idea might deserve to die (legal / ethics) | [02 HarborWatch](use-cases/02-greenfield-harborwatch.md): PRFAQ + party |

Say:

> New project does **not** mean run every skill. InvoiceDiff never writes a PRD. MealPlan skips PRFAQ because the founders were already sure. Skipping is a feature.

### Think → write → slice → ship (only if it is actually a product)

```
Think    brainstorm / forge / recon     (skip if the idea is already sharp)
Write    brief or PRFAQ → PRD → validate
Slice    spec → stories  (+ UX / architecture if you need them)
Ship     build → code-review on a *different* model → retro
```

Human supplies the finished `prd.md`. The skill supplies the validation **rubric**. Details are in use-case 01.

If day 1 is already a well-defined CLI, skip to spec. Do not perform MealPlan for sport.

---

## 27–30 — How we work this week

Read the agreement once. Assign names. End.

### Working agreement (keep)

1. **Size first.** One sitting → build. Epic → spec. Product → PRD. Explosion → correct-course.
2. **Artifacts are the handoff.** If it is not in a file, the next session will guess.
3. **Team customize is committed** (`_bmad/custom/*.toml`). Personal `*.user.toml` is gitignored.
4. **Review on a different model** than the one that built. Humans still walk scary diffs.
5. **Party mode is for fights**, not status. Then hand off to PRD or spec — the party does not write the contract.

### Assign before you leave

| Role | Name (fill in) | This week |
|---|---|---|
| **Steward** | | Rerun installer, review `_bmad/custom/` PRs. Not a gatekeeper. |
| **Pilot existing repo** | | Install + `project-context` + first real ticket |
| **First ticket** | | Named, sized (build vs spec) |
| **New project** (if any) | | Day 0 install; do **not** install two repos on the same afternoon |

### Models in one line

```
Frontier thinks  →  Workhorse builds  →  other-family reviews
```

Do not let a fast model write `ARCHITECTURE-SPINE.md`. Do not let the same model implement and review. See [diagrams §2a](bmad-skill-decision-diagrams.md#2a-recommended-models-agents-and-skills).

---

## If someone asks (30-second answers)

| Question | Answer |
|---|---|
| Do we rewrite the monolith? | No. Next change only. |
| Do we need a PRD for a bugfix? | No. `bmad-build`. |
| Where do skills live? | In the **project** (`_bmad/`, `.agents/skills/`). Not a company-wide install. |
| What if the plan dies mid-sprint? | `bmad-correct-course`. [Use-case 07](use-cases/07-brownfield-mealplan-course-correct.md) if work already shipped; [05](use-cases/05-brownfield-ledgerlite-course-correction.md) if you walk in *at* the bomb. |
| 49 skills? | 29 current. 20 are shims. You will use ~8 in the first month. |
| How do I install on my laptop? | [install-bmad-method.md](install-bmad-method.md) — Node 20.12+, `uv`, then `npx bmad-method install`. |

---

## After the meeting

- Steward: install on the pilot this week. Script is the existing-project block above.
- Everyone: keep this repo. Next meeting if you want the live 90-minute tour: [team-enablement-plan.md](team-enablement-plan.md).
- First merged PR that used a named skill + one `AGENTS.md` pitfall if an agent tripped = enablement actually started.

Official docs: [Install](https://docs.bmad-method.org/start/install-bmad/) · [Existing codebases](https://docs.bmad-method.org/existing-codebases/start-in-an-existing-codebase/) · [Choose a planning path](https://docs.bmad-method.org/plan/choose-a-planning-path/).
