# Team enablement plan — demonstrate BMad and work together

A plan you can run with your team using **this repo as the teaching kit**. It covers:

1. A live demo of capabilities (not a slide dump)
2. How the team works together after the demo
3. **Brownfield:** existing repos where BMad is not installed, and you want it for *new changes going forward*
4. **Greenfield:** fresh projects from day one

You do not “BMad the whole legacy system.” You install it, teach agents the house rules, and use it on the **next** change.

---

## What “done” looks like

| Horizon | Brownfield (existing product) | Greenfield (new product) |
|---|---|---|
| **Demo day** | Team can name 5 agents, size a change, and say when *not* to write a PRD | Same, plus they have seen a full think → spec → build path |
| **Week 1** | One pilot repo has `_bmad/` + `AGENTS.md` + a team `_bmad/custom/` | New repo installed on day 0 |
| **Week 2–3** | First real change shipped with `bmad-build` (or spec + build) | First epic has `SPEC.md` + at least one Build |
| **Week 4** | Team has used `bmad-code-review` on a **different model** than the builder | Team has run one `bmad-party-mode` on a real disagreement |
| **Steady state** | New work on that repo goes through BMad by default; old code is not rewritten “for BMad” | Planning artifacts live in the repo; Build reads `SPEC.md` |

---

## Roles (map to BMad agents, not job titles)

One person can wear more than one hat. The agent is the **hat**, not a headcount.

| Team hat | BMad agent | Owns these artifacts | Does not own |
|---|---|---|---|
| Product / founder | John (`bmad-agent-pm`) | `prd.md` updates, epics, readiness | Implementation |
| Research / discovery | Mary (`bmad-agent-analyst`) | recon, brief, PRFAQ | Architecture spine |
| Design | Sally (`bmad-agent-ux-designer`) | `DESIGN.md`, `EXPERIENCE.md` | Code |
| Tech lead | Winston (`bmad-agent-architect`) | `ARCHITECTURE-SPINE.md` | Ticket trivia |
| Engineer | Amelia (`bmad-agent-dev`) | code, impl records, tests | Silently rewriting the PRD |
| Whole team | `bmad-party-mode` | keepsake + memlog of a fight | Fake consensus slides |

**Shared, not owned by one person:** `AGENTS.md` (pitfalls anyone can record), `sprint-status.yaml` (the tracking file), `_bmad/custom/*.toml` (team overrides — PR the change).

---

## Demo day (90 minutes)

**Prep (you, the day before):** [install](install-bmad-method.md) BMad on a **throwaway** folder *and* have this repo open. Do not demo on a production app the first time.

**Leave-behind:** this repo + the decision tree. Do not invent extra slides.

### Minute 0–10 — The one picture

Open [`bmad-skill-decision-diagrams.md`](bmad-skill-decision-diagrams.md) §1 (master tree) and §3 (size the work).

Say out loud:

> BMad is named skills, not a religion. A one-sitting fix goes to `bmad-build`. An epic goes to `bmad-spec`. A product goes through brief/PRD. We skip boxes we do not need.

Show the `<< in` / `>> out` notation so people stop thinking “the AI just chats.”

### Minute 10–25 — Capability 1: tiny change (live)

In the throwaway repo:

1. `/bmad-help` — “add a health endpoint”
2. `/bmad-build` — implement it
3. Optional: `/bmad-code-review` in a **new chat on a different model**

**What they should take away:** you can use BMad tomorrow on a 2-hour ticket. No PRD.

**Story to point at:** [use-case 03](use-cases/03-greenfield-invoicediff-cli.md) (spec-first, skip theater) and the tiny-change sequence in the master file §4.

### Minute 25–40 — Capability 2: greenfield path (walk the paper)

Do **not** live-run the whole of MealPlan. Walk [use-case 01](use-cases/01-greenfield-mealplan-ai.md) on a projector:

- Think: brainstorm → forge → recon → short party
- Write: brief → PRD → validate (human supplies `prd.md`; **skill** supplies the rubric)
- Slice: spec → epics → sprint-planning
- Ship: build → CR → QA → retro

Pause on: “we skipped PRFAQ because the founders were already sure.” Skipping is a feature.

### Minute 40–55 — Capability 3: brownfield (the one they actually need)

Walk [use-case 04](use-cases/04-brownfield-northwind-sso.md) then [use-case 07](use-cases/07-brownfield-mealplan-course-correct.md):

1. **Day 0 is `bmad-project-context`**, not a 40-page generated doc dump.
2. **Update** the PRD; do not create a fake history of the legacy system.
3. Architecture **ratifies** the living codebase.
4. Then show 07’s later bomb: Instacart dies **after** sharing shipped → `bmad-correct-course`.

Say:

> We will not BMad-rewrite last year’s code. We will BMad the next change, and we will course-correct when the plan explodes — not in stand-up footnotes.

[Use-case 05](use-cases/05-brownfield-ledgerlite-course-correction.md) is the backup if someone asks “what if we walk in *at* the bomb?”

### Minute 55–70 — Capability 4: party mode (live if you can)

Live: `/bmad-party-mode` on a **real team disagreement** you already have (pricing, vendor, “do we need a mobile app”). If you cannot go live, walk [use-case 06](use-cases/06-party-mode-atlas-health.md) Room A (option D appears from the clash).

Rules to state:

- Session mode = one brain in five hats (fine for banter)
- `--mode subagent` = independent minds (use for focus groups / reviews)
- Do not ask for a consensus slide
- Then hand off to `bmad-prd` / `bmad-spec` — the party does not write the contract

### Minute 70–80 — Capability 5: models and review

Open [§2a](bmad-skill-decision-diagrams.md#2a-recommended-models-agents-and-skills).

```
Frontier thinks  →  Workhorse builds  →  other-family reviews
```

Cheapest mistakes: Fast model writes `ARCHITECTURE-SPINE.md`; same model implements and reviews.

### Minute 80–90 — How we will work + Q&A

Put the working agreement (next section) on the last screen. Assign:

- One **pilot brownfield repo**
- One **greenfield** if you have a new effort this quarter
- One **BMad steward** (not a gatekeeper — the person who reruns the installer and reviews `_bmad/custom/` PRs)

---

## Working agreement (print this)

Use this as a one-pager after demo day. Adapt names; keep the rules.

### 1. Size first, then pick skills

| The change is… | Default path | Do not |
|---|---|---|
| One sitting, intent is clear | `bmad-build` | Write a PRD |
| Several sittings, one outcome | `bmad-spec` + story breakdown + Build × N | Invent sprint theater if you already have Linear |
| New product / multi-epic | 01-style: brief or PRFAQ → PRD → UX? → arch → spec per epic | Run every skill “because it exists” |
| Existing repo, first week | `bmad-project-context` then the row above | `bmad-document-project` (deprecated, and it made agents worse) |
| Plan just exploded | `bmad-sprint-planning` status → party (optional) → `bmad-correct-course` | Keep coding with a Slack footnote |

### 2. Artifacts are the handoff

- John does not “tell Amelia in chat.” John leaves `prd.md` / a change signal.
- Winston leaves `ARCHITECTURE-SPINE.md`. Amelia reads `SPEC.md`.
- If it is not in an artifact, the next session will guess.

### 3. Team customization is committed; personal is not

| File | Who | Git |
|---|---|---|
| `_bmad/custom/*.toml` | Team (pnpm, “never touch users.email”) | Commit |
| `_bmad/custom/*.user.toml` | Individual | Gitignore |
| `AGENTS.md` managed block | Anyone who sees an agent trip | Commit the pitfall |

Use `bmad-customize`. Do not hand-edit installed skill files.

### 4. Review

- `bmad-build` already reviews once.
- Extra `bmad-code-review`: **new chat, different model family.**
- Humans still walk scary diffs (`bmad-checkpoint-preview` or a normal PR).

### 5. Party mode is for fights, not status

Use it when two reasonable people would disagree. Do not use it for “what’s the next ticket?”

### 6. Course-correct is a skill, not a vibe

If a partner API dies, a DPA lands, or v1 scope was wrong: run `bmad-correct-course`. It is allowed to say “update the PRD, redo the spine, restory, re-gate.” It is not allowed to silently reopen an ACCEPTed epic.

---

## Brownfield playbook — BMad is not on the repo yet

You have a living product. You want BMad **going forward** for new work. You do not generate a PRD that pretends to be the last four years.

### Phase 0 — Pick a pilot (this week)

Choose **one** repo that:

- The team actually ships in weekly
- Has tests you can name (even if imperfect)
- Has a change coming in the next 2–3 weeks that is bigger than a typo and smaller than “rewrite the platform”

Name a steward. Book the 90-minute demo if you have not run it.

### Phase 1 — Install and teach the house (half a day)

On the pilot repo, together (or steward + one engineer):

1. Prerequisites: Node 20.12+, Python, `uv` — [install guide](install-bmad-method.md)
2. `npx bmad-method install` (Cursor: `--tools cursor --modules bmm --yes` if you want headless)
3. `/bmad-help` — prove skills load
4. `/bmad-project-context` **setup**  
   `<< in:` the real repo (package manager, how tests run, the one convention that is not in the code)  
   `>> out:` `AGENTS.md` managed block
5. `/bmad-customize` for 2–3 **team** facts (always `pnpm`, never npm; do not invent a new auth stack; …)  
   `>> out:` `_bmad/custom/bmad-build.toml` — **commit this**
6. PR: `_bmad/`, `.agents/skills/` (or your tool’s copies), `AGENTS.md`, team custom TOML. Decide as a team whether `_bmad-output/` is gitignored.

**Do not** run `bmad-prd` create for the whole product on day 1.

This is [use-case 04](use-cases/04-brownfield-northwind-sso.md) Day 0.

### Phase 2 — First change: smallest honest path (week 1–2)

Pick the next **real** ticket.

| If the ticket is… | Run |
|---|---|
| Clear, one session | `bmad-build` only |
| Several sessions, one outcome | `bmad-spec` (ticket + whatever docs you already have) → Build per story |
| Needs a signed-off “what” because two teams will build | `bmad-prd` **update** only if a PRD already exists; else a short spec + a one-pager in your existing tool |

Walk [use-case 04](use-cases/04-brownfield-northwind-sso.md) as the script: recon/select if you must choose a vendor → **ratify** architecture → spec → build → CR on another model → record any agent mistake in `AGENTS.md`.

**Definition of done for phase 2:** one merged PR that used BMad, plus one pitfall line if an agent tripped.

### Phase 3 — Make it the default for *new* work (week 2–4)

Add to the team’s PR template (or CONTRIBUTING):

```
- [ ] Change sized (one sitting / epic / project)
- [ ] BMad skill used (name it) or reason skipped
- [ ] If code: review on a different model than the builder, or human CR
- [ ] If an agent repeated a mistake: AGENTS.md pitfall recorded
```

Optional: `bmad-sprint-planning` if you want a file the agents share. Skip it if Linear already is the board and you are one squad.

### Phase 4 — First explosion (whenever it happens)

When the plan dies (vendor, legal, scope):

1. `bmad-sprint-planning` status (what is already merged)
2. Optional party — [06](use-cases/06-party-mode-atlas-health.md) / 07 Room
3. `bmad-correct-course`  
   `<< in:` change signal + current prd/spine/epics/sprint  
   `>> out:` change proposal, then update only what the proposal names
4. Do not reopen ACCEPTed epics without a reason ([07](use-cases/07-brownfield-mealplan-course-correct.md))

If you walk in *after* the bomb, use [05](use-cases/05-brownfield-ledgerlite-course-correction.md).

### What you refuse to do on brownfield

- Generate a full “document this project” novel (deprecated, and it goes stale)
- Recreate year-1 PRDs for features that already shipped
- Run `bmad-build-auto` on auth, money, or the first story of a new pattern
- Install BMad on every repo the same week — one pilot, then copy the `_bmad/custom` conventions

### Roll out to the next existing repo

Copy the **team** `custom/*.toml` conventions, run install + `bmad-project-context` setup. Each repo gets its own `AGENTS.md`. Do not copy another repo’s `AGENTS.md` blindly.

---

## Greenfield playbook — fresh project

### Day 0

```bash
mkdir my-product && cd my-product && git init
npx bmad-method install --directory . --modules bmm --tools cursor --yes
```

Invoke `bmad-help`. Install the same team `_bmad/custom/*.toml` you already use on the pilot (package manager, languages, “never commit secrets”).

### Size the idea, then pick a path

Walk [§3](bmad-skill-decision-diagrams.md) with the team.

| Starting point | Follow |
|---|---|
| “We want something with X” (fuzzy) | [01](use-cases/01-greenfield-mealplan-ai.md) think block: brainstorm → forge → recon |
| Idea might deserve to die; city/legal/ethics | [02](use-cases/02-greenfield-harborwatch.md): forge + PRFAQ + party |
| Engineer already knows the flags | [03](use-cases/03-greenfield-invoicediff-cli.md): spec + thin spine + build |
| Two people must sign the same “what” | 01 write block: brief **or** PRFAQ → PRD → validate |
| UI is the product | `bmad-ux` after PRD |
| Several people will build in parallel | `bmad-architecture` before epics |

### How the team splits work

```
Mary / anyone     recon and forge     (can be one afternoon)
John + stakeholders   PRD create/validate
Sally             UX if there is a UI
Winston           spine (Frontier model)
Whole team        party only if there is a real fork
Then              spec per epic (often John + Winston)
Amelia(s)         one Build session per story
Different person or different model   code-review
Team              retro at epic end
```

Independent epic streams can run in parallel **after** the spine exists. That is why Winston goes before “everyone start coding.”

### First two weeks on a new product (suggested)

| Day | Together | Artifact |
|---|---|---|
| 1 | Install, help, brainstorm or forge | `brainstorm.html` or `forged-idea.md` |
| 2 | Recon (only the types you need) | `research/*.md` |
| 2 optional | Party on the one fight | keepsake HTML |
| 3–4 | Brief or PRFAQ → PRD | `prd.md` |
| 4 | Validate (shipped rubric) | `validation-report.html` |
| 5 | UX if needed; architecture | `DESIGN.md`, `ARCHITECTURE-SPINE.md` |
| 6 | Spec epic 1 + stories + sprint-planning | `SPEC.md`, `sprint-status.yaml` |
| 7–10 | Build story 1 **human-gated**, then more stories | code + impl records |

If day 1 is already a well-defined CLI, skip to spec (03). Do not perform 01 for sport.

---

## Four-week calendar (one pilot + optional greenfield)

```
         WEEK 1                 WEEK 2                 WEEK 3                 WEEK 4
         ──────                 ──────                 ──────                 ──────
Demo 90m
Pilot install
+ project-context
+ team customize
         │                      │                      │                      │
         └─ first real ticket ──┴─ merge + CR on       ┴─ second ticket       ┴─ retro:
            (build or spec)        other model            (spec if needed)       copy custom
                                   record pitfalls        party if fighting      TOML to repo #2
```

If a greenfield starts in the same month, run its Day 0 in week 2 so the steward is not installing two things on demo day.

---

## Demo script cheat sheet (print)

| # | Show | File | Live? |
|---|---|---|---|
| 1 | Size the work | diagrams §1 + §3 | No — talk |
| 2 | Tiny build | diagrams §4 | Yes |
| 3 | Greenfield full path | [01](use-cases/01-greenfield-mealplan-ai.md) | Walk paper |
| 4 | Brownfield day 0 + SSO | [04](use-cases/04-brownfield-northwind-sso.md) | Walk paper |
| 5 | Late course-correct | [07](use-cases/07-brownfield-mealplan-course-correct.md) | Walk paper |
| 6 | Walk-in-at-the-bomb | [05](use-cases/05-brownfield-ledgerlite-course-correction.md) | Only if asked |
| 7 | Party / option D | [06](use-cases/06-party-mode-atlas-health.md) | Live if you have a fight |
| 8 | Models | diagrams §2a | No — talk |
| 9 | Install | [install-bmad-method.md](install-bmad-method.md) | Yes, on throwaway |

---

## Steward checklist

- [ ] Demo day booked; throwaway repo installed the night before
- [ ] Pilot brownfield repo chosen; first ticket named
- [ ] Team custom facts agreed (3 or fewer)
- [ ] `_bmad-output/` gitignore decision made
- [ ] PR template updated (or rejected with a reason)
- [ ] Model policy posted: Frontier for arch/spec/CC; Workhorse for build; other family for CR
- [ ] After first agent trip: `bmad-project-context record` done in public
- [ ] After first real explosion: `bmad-correct-course` used, not a hallway rewrite
- [ ] Week 4: copy team TOML to the next repo; do not copy `AGENTS.md` blindly

---

## What not to demonstrate

- Every one of the 49 skills. Twenty are deprecated shims. Say that once, point at the shim map, move on.
- A 40-minute generated “document this repo” run.
- Party mode that ends in “everyone agrees.” If they agree, you picked a bad topic.
- `bmad-build-auto` on the first story of a new pattern.

---

## Official docs to keep next to this repo

- [Install BMad](https://docs.bmad-method.org/start/install-bmad/)
- [Choose a planning path](https://docs.bmad-method.org/plan/choose-a-planning-path/)
- [Start in an existing codebase](https://docs.bmad-method.org/existing-codebases/start-in-an-existing-codebase/)
- [Customize BMad](https://docs.bmad-method.org/customize/customize-bmad/)
- [Review a change](https://docs.bmad-method.org/build/review-a-change/)
