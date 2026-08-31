# When to Use Which BMAD-Method Skill

ASCII trees and sequence diagrams for every installed BMad Method skill.

Catalog source: a current BMad Method install (`skill-manifest.csv`).
**49 skills total: 29 current + 20 deprecated shims.**

Running example: **MealPlan AI** — a family meal-planning app that turns pantry + dietary constraints into a weekly plan and grocery list.

**End-to-end projects (`<< in` / `>> out` on every arrow):** see [`use-cases/README.md`](use-cases/README.md). Six files — three greenfield, two brownfield, one party-mode deep dive.

| # | Project | Field |
|---|---|---|
| [01](use-cases/01-greenfield-mealplan-ai.md) | MealPlan AI | Greenfield product |
| [02](use-cases/02-greenfield-harborwatch.md) | HarborWatch flood alerts | Greenfield, research-heavy |
| [03](use-cases/03-greenfield-invoicediff-cli.md) | InvoiceDiff CLI | Greenfield, spec-first |
| [04](use-cases/04-brownfield-northwind-sso.md) | Northwind Shop SSO | Brownfield |
| [05](use-cases/05-brownfield-ledgerlite-course-correction.md) | LedgerLite store cutover | Brownfield + correct-course |
| [06](use-cases/06-party-mode-atlas-health.md) | Atlas Health video visits | **Party mode** showcase |

---

## 0a. What each current skill reads and writes

Sequence notation:

```
You -----> skill-name
              << in:  what it consumes (your words, prior artifacts, the repo)
              >> out: what it produces
```

`<< in` can be a spoken intent, a file, a folder, a diff, or the live codebase. Agents (`bmad-agent-*`) consume a persona request and dispatch; they do not write artifacts of their own.

```
CORE
  bmad-help
      << in:  your question + whatever planning/impl artifacts already exist
      >> out: (none — next-skill recommendation)

  bmad-brainstorming
      << in:  a topic, stuck question, or "help me ideate"
      >> out: {output}/brainstorming/brainstorm.html
              optional brainstorm-intent.md

  bmad-forge-idea
      << in:  an idea (sentence or short brief) to harden or kill
      >> out: {output}/forge/forge-report.html
              optional forged-idea.md

  bmad-deep-recon
      << in:  subject + type (market|domain|technical|competitive|
              user-voice|academic-lit|select)
              OR a finished research report to process
              OR a candidate set for choose-between
      >> out: {planning}/research/*.md  (+ optional HTML briefing)

  bmad-advanced-elicitation
      << in:  the just-produced draft + a method (pre-mortem, red team, …)
      >> out: in-place edits to that draft

  bmad-review
      << in:  path to a diff, branch, PRD, spec, story, arch, or prose
              + optional lens names
      >> out: findings.json + findings.md

  bmad-party-mode
      << in:  a topic; optional --party --mode
              optional interview notes / cast idea (authoring)
              existing memories/<party>/.memlog.md if memory is on
      >> out: {output}/party-mode/{date}-*.html     (keepsake, on wrap)
              {output}/party-mode/memories/<party>/.memlog.md
              optional _bmad/custom/bmad-party-mode*.toml

  bmad-customize
      << in:  which skill + the behavior/template/menu/facts to persist
      >> out: _bmad/custom/<skill>.toml  or  <skill>.user.toml

AGENTS
  bmad-agent-*
      << in:  "talk to Mary/John/…" or a menu code (PRD, BD, CU, …)
      >> out: (none — dispatches a workflow skill)

PLAN
  bmad-product-brief
      << in:  a relatively clear concept; optional recon / forged-idea
      >> out: brief.md + addendum.md

  bmad-prfaq
      << in:  a product concept to stress-test; optional recon / forge
      >> out: prfaq-*.md

  bmad-prd
      << in:  create:   brief and/or PRFAQ and/or brain dump + recon
              update:   existing prd.md + a change signal
              validate: finished prd.md + the skill's shipped rubric
                        (assets/prd-validation-checklist.md; org can override)
      >> out: create/update: prd.md, addendum.md, .memlog.md
              validate: HTML + .md findings

  bmad-ux
      << in:  PRD or spec (UI is a primary surface); optional brand notes
      >> out: DESIGN.md, EXPERIENCE.md, .memlog.md

  bmad-spec
      << in:  any intent: brief, PRD, transcript, brain dump,
              design folder, mixed sources (condense if huge)
              update: existing SPEC.md + the change
      >> out: specs/spec-{slug}/SPEC.md + companions
              optional stories.yaml

  bmad-architecture
      << in:  PRD or spec; UX if present
              brownfield: the existing codebase
      >> out: ARCHITECTURE-SPINE.md

  bmad-create-epics-and-stories
      << in:  architecture + PRD/spec
      >> out: planning/epics/*.md + story files

  bmad-sprint-planning
      << in:  epics/stories (create/gate)
              existing sprint-status.yaml (status / repair)
      >> out: PASS|CONCERNS|FAIL + sprint-status.yaml

  bmad-project-context
      << in:  setup/refresh: the repo (scripts, package manager, CI)
              record: an observed agent mistake
              audit: current AGENTS.md block
      >> out: AGENTS.md managed block

SHIP
  bmad-build
      << in:  a sentence, issue, SPEC.md, or one story from stories.yaml
              + the codebase (+ AGENTS.md if present)
      >> out: code + per-story implementation record

  bmad-build-auto
      << in:  one already-bounded unit (story) + spec + codebase
      >> out: code + impl record + terminal status

  bmad-code-review
      << in:  a code change (diff / branch / PR)
      >> out: findings + optional applied patches

  bmad-checkpoint-preview
      << in:  a commit, branch, or PR
      >> out: guided walkthrough (usually conversation-only)

  bmad-qa-generate-e2e-tests
      << in:  implemented feature/code (+ SPEC success conditions)
      >> out: API / E2E test suite

  bmad-correct-course
      << in:  the change signal + current PRD / arch / epics / sprint-status
      >> out: change proposal (then may rewrite those same artifacts)

  bmad-retrospective
      << in:  completed epic: spec folder, stories.yaml, impl records, code
      >> out: retro doc + action items + acceptance verdict
```

---

## 0. How the catalog is organized

```
BMAD-METHOD skills (49)
├── CURRENT — use these (29)
│   ├── CORE anytime tools (8)
│   ├── BMM agents / personas (5)
│   ├── BMM plan workflows (9)
│   └── BMM ship workflows (7)
└── DEPRECATED SHIMS — do not pick on purpose (20)
    ├── old review lenses     → bmad-review
    ├── old research skills   → bmad-deep-recon
    ├── old PRD / arch names  → bmad-prd / bmad-architecture
    ├── old impl names        → bmad-build / bmad-build-auto
    └── old context / status  → bmad-project-context / bmad-sprint-planning
```

---

## 1. Master decision tree — "I have a situation, which skill?"

Start at the top. Every **current** skill appears once as a leaf or a named node.

```
YOU ARE HERE
│
├─ I don't know what to do next in BMad
│     └─ bmad-help
│
├─ I want a specialist persona in the room (menu + character)
│     ├─ talk to Mary  (research / brief / PRFAQ)     → bmad-agent-analyst
│     ├─ talk to John  (PRD / epics / readiness)      → bmad-agent-pm
│     ├─ talk to Winston (architecture / readiness)   → bmad-agent-architect
│     ├─ talk to Sally (UX)                           → bmad-agent-ux-designer
│     └─ talk to Amelia (build / QA / review / retro) → bmad-agent-dev
│
├─ I want several personas arguing at once
│     └─ bmad-party-mode
│
├─ I want BMad itself to behave differently next time
│     └─ bmad-customize
│
├─ I just got a draft and it feels like a first pass
│     └─ bmad-advanced-elicitation     (pre-mortem, red team, Socratic, …)
│
├─ I want a multi-lens critique of ANY artifact (doc, spec, diff)
│     └─ bmad-review
│           lenses: adversarial | edge-case | verification-gap | structure | prose
│
├─ THE INTENT IS STILL FUZZY
│     ├─ generate options / stuck ideating      → bmad-brainstorming
│     ├─ pressure-test until it hardens or dies → bmad-forge-idea
│     └─ I need evidence, not opinions
│           └─ bmad-deep-recon
│                 types: market | domain | technical | competitive
│                        user-voice | academic-lit | select (choose-between)
│
├─ THE INTENT IS CLEAR ENOUGH TO WRITE DOWN
│     ├─ concept is solid, write it gently      → bmad-product-brief
│     ├─ stress-test customer-first (press release) → bmad-prfaq
│     ├─ org needs a signed-off requirements doc → bmad-prd   (create|update|validate)
│     ├─ UI is a primary surface                → bmad-ux
│     ├─ lock WHAT before HOW (machine contract)→ bmad-spec
│     ├─ lock HOW so parallel work stays consistent → bmad-architecture
│     ├─ break work into epics + stories        → bmad-create-epics-and-stories
│     └─ gate readiness + track the sprint      → bmad-sprint-planning
│
├─ I AM IN AN EXISTING REPO and agents keep tripping
│     └─ bmad-project-context     (setup | refresh | record pitfalls | audit)
│
├─ I WANT CODE WRITTEN
│     ├─ one change, I will sit with it         → bmad-build
│     └─ one unit, unattended (orchestrator)    → bmad-build-auto
│
├─ CODE EXISTS — I need to inspect or harden it
│     ├─ adversarial code review                → bmad-code-review
│     ├─ walk ME through the change (human HITL)→ bmad-checkpoint-preview
│     └─ generate API / E2E tests for a feature → bmad-qa-generate-e2e-tests
│
├─ THE PLAN JUST BROKE mid-sprint
│     └─ bmad-correct-course
│
└─ AN EPIC JUST FINISHED
      └─ bmad-retrospective
```

---

## 2. Two ways to invoke work

```
                    WANT WORK DONE?
                           │
           ┌───────────────┴───────────────┐
           │                               │
   Invoke the WORKFLOW              Invoke an AGENT
   skill directly                   (persona + menu)
           │                               │
           v                               v
   bmad-prd, bmad-build, …         bmad-agent-*
                                           │
                                           │  menu code dispatches
                                           v
                                   same workflow skill
```

Agents never replace workflows. They **greet, stay in character, and dispatch**.

```
bmad-agent-analyst (Mary)
  BP  → bmad-brainstorming
  MR  → bmad-deep-recon (market)
  DR  → bmad-deep-recon (domain)
  TR  → bmad-deep-recon (technical)
  TS  → bmad-deep-recon (select / choose-between)
  CR  → bmad-deep-recon (competitive)
  UV  → bmad-deep-recon (user-voice)
  CB  → bmad-product-brief
  WB  → bmad-prfaq
  PC  → bmad-project-context

bmad-agent-pm (John)
  PRD → bmad-prd
  CE  → bmad-create-epics-and-stories
  IR  → bmad-sprint-planning
  CC  → bmad-correct-course

bmad-agent-architect (Winston)
  CA  → bmad-architecture
  IR  → bmad-sprint-planning

bmad-agent-ux-designer (Sally)
  CU  → bmad-ux

bmad-agent-dev (Amelia)
  BD  → bmad-build
  QA  → bmad-qa-generate-e2e-tests
  CR  → bmad-code-review
  SP  → bmad-sprint-planning
  ER  → bmad-retrospective
```

---

## 2a. Recommended models (agents and skills)

BMad does **not** pin a model SKU to an agent. Official guidance is tier-and-role, not “always use X.” The table below is a practical mapping as of 2026. Swap in whatever frontier / workhorse / fast models your harness actually offers.

**Official BMad rules (not ours):**

- Planning in [web bundles](https://github.com/bmad-code-org/BMAD-METHOD) (ChatGPT Custom GPTs / Gemini Gems): use the **best model available** in that subscription, then bring artifacts into the IDE for implementation.
- `bmad-code-review` after a build: start a **fresh chat on a different LLM** than the one that implemented the story ([first-project tutorial](https://bmad-code-org-bmad-method-6.mintlify.app/tutorials/first-project); also [Review a Change](https://docs.bmad-method.org/build/review-a-change/) — custom lenses on other models are worth doing).
- `bmad-party-mode`: match the model to the round — **quick for banter, stronger for deep work**; a per-member `model` in customize.toml is used when set; `--model` pins everyone. `--mode subagent` favors faster cheaper models for spawned voices, but independence is the point.
- `bmad-deep-recon`: lead stays on the **strongest** model; researcher subagents at most one capable tier down; **judgment never on the smallest tier**.
- Built-in review layers inside `bmad-build`: keep review subagents at the **same capability as the current session** — do not silently drop them to a cheap model.

**Tiers used below**

| Tier | Job | Typical 2026 families (examples, not a lock-in) |
|---|---|---|
| **Frontier** | Hard reasoning, architecture, adversarial review, course-correct | Claude Opus 4.x (thinking), GPT-5.x, Gemini 2.5/3 Pro |
| **Workhorse** | Long structured docs, most implementation | Claude Sonnet 4.x, GPT-5.x mid, Gemini Pro, Cursor Composer |
| **Fast** | Banter, status, mechanical extract, later `bmad-build-auto` | Claude Haiku / fast Sonnet, Gemini Flash, GPT mini |

### Named agents

```
Mary     bmad-agent-analyst
         WHY: synthesis + research judgment + long context
         USE:  Frontier for forge / recon / PRFAQ; Workhorse for brief + project-context
         AVOID: Fast models on deep-recon judgment or select-shape decisions
         PIN:   optional party member model = frontier when she is in a hard room

John     bmad-agent-pm
         WHY: long consistent PRDs; change-signal updates; readiness gates
         USE:  Frontier when the PRD is the org contract; Workhorse for CE / IR
         AVOID: Chatty fast models (they pad PRDs and invent requirements)
         PIN:   frontier for correct-course (CC) — blast radius is expensive

Winston  bmad-agent-architect
         WHY: a wrong invariant poisons every later story
         USE:  Frontier, always, for CA; Workhorse only to ratify a tiny brownfield spine
         AVOID: Fast / “auto” coding models — they optimize for code, not invariants
         PIN:   frontier in party mode; he is the one who should not fold

Sally    bmad-agent-ux-designer
         WHY: taste + edge-case rigor; often needs to see UI
         USE:  Workhorse with vision if you have mocks; Frontier when UX is the product
         AVOID: Pure-code models with weak product taste
         PIN:   workhorse+vision; frontier if she is fighting Winston/Amelia on v1 scope

Amelia   bmad-agent-dev
         WHY: implementation, then a second pair of eyes
         USE:  Workhorse (strong coding) for BD / QA / SP / ER
               Frontier for the first story of a new pattern, or a scary domain
         AVOID: Same model for BD and the later CR pass
               Fast models on auth, money, or life-safety stories
         PIN:   workhorse for BD; a *different family* for CR
```

| Agent | Default pick | Upgrade to Frontier when | Use a different model for |
|---|---|---|---|
| Mary | Frontier | Always for recon / forge / PRFAQ | — |
| John | Workhorse | Org-signed PRD, `bmad-correct-course` | — |
| Winston | Frontier | Always for `bmad-architecture` | — |
| Sally | Workhorse + vision | UI is the product, or a scope fight | — |
| Amelia | Workhorse coding | First story of a new pattern; regulated / money / auth | `bmad-code-review` (other family) |

### Workflow skills (when you invoke them directly)

```
THINK
  bmad-help                    Fast or Workhorse — routing, not creation
  bmad-brainstorming           Workhorse (creative). Frontier if the idea space is high-stakes
  bmad-forge-idea              Frontier — it must be willing to kill the idea
  bmad-deep-recon              Frontier lead; Workhorse researchers; never Fast for judgment
  bmad-advanced-elicitation    Frontier (red team / pre-mortem)
  bmad-review                  Different family than the author of the artifact
  bmad-party-mode              session: one Workhorse/Frontier
                               auto/subagent: Fast for banter, Frontier for hard rounds
                               focus groups: --mode subagent + Workhorse per patient
  bmad-customize               Workhorse — it writes TOML, not product

PLAN
  bmad-product-brief           Workhorse
  bmad-prfaq                   Frontier (Working Backwards is a gauntlet)
  bmad-prd                     Frontier for create/validate; Workhorse for a small update
  bmad-ux                      Workhorse + vision; Frontier if UX is the product
  bmad-spec                    Frontier — SPEC.md is the machine contract Build reads
  bmad-architecture            Frontier
  bmad-create-epics-and-stories  Workhorse (needs the spine + PRD already right)
  bmad-sprint-planning         Workhorse; Frontier if the gate is FAIL/CONCERNS
  bmad-project-context         Workhorse — must actually run the repo commands

SHIP
  bmad-build                   Workhorse coding; Frontier for story 1 of a new spine
  bmad-build-auto              Workhorse or Fast — only after a human Build proved the pattern
  bmad-code-review             Other family than the builder; Frontier if you can afford it
  bmad-checkpoint-preview      Workhorse
  bmad-qa-generate-e2e-tests   Workhorse coding
  bmad-correct-course          Frontier
  bmad-retrospective           Workhorse; Frontier if the verdict is contested
```

### How to pin a model (when your harness allows it)

```
Party member (spawned voice)
  _bmad/custom/bmad-party-mode.user.toml
  [[workflow.party_members]]
  code  = "winston"
  model = "<your-frontier-id>"     # session --model overrides this

Deep recon researchers
  _bmad/custom/bmad-deep-recon.toml
  subagent_models = ["<workhorse-id>", "<fast-id>"]
  # lead stays on the session model (keep that one Frontier)

Code-review extra lens on another LLM
  _bmad/custom/bmad-code-review.toml
  [[workflow.review_layers]]
  id = "blind-hunter-other-llm"
  instruction = """run <other-model> on {diff_file} …"""

Cursor / Claude Code
  pick the model in the session UI before invoking the agent
  then start a *new* session on a different model for CR
```

```
CHEAPEST MISTAKE
  Fast model writes ARCHITECTURE-SPINE.md or SPEC.md
  → every later Build inherits the error

SECOND CHEAPEST
  Same model implements and reviews
  → it grades its own homework

RIGHT SHAPE
  Frontier thinks  →  Workhorse builds  →  other-family reviews
```

---

## 3. Size the work first — then pick a path

```
                     HOW BIG IS THIS?
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
   ONE SESSION         ONE EPIC            A PROJECT
   (hours)             (several builds,    (~20+ builds,
                        one outcome)        multi-epic)
        │                   │                   │
        v                   v                   v
  skip most planning   bmad-spec            brief/PRFAQ
  go to bmad-build     + story breakdown    + bmad-prd
                       + bmad-build         + bmad-ux?
                       per story            + bmad-architecture
                                            + epics & stories
                                            + sprint-planning
                                            + spec per epic
                                            + build per story
```

---

## 4. Sequence — tiny change (one sitting)

Use case: "Add a 'servings' multiplier to the grocery list."

```
You              Help?     Context?      Build           Review          Checkpoint
 |                 |           |           |               |                 |
 |-- I know exactly           |           |               |                 |
 |   what to change --------->|           |               |                 |
 |                            |           |               |                 |
 |  [brownfield & agents      |           |               |                 |
 |   keep getting lost?]      |           |               |                 |
 |-- optional --------------->|           |               |                 |
 |                     bmad-project-      |               |                 |
 |                     context            |  << in:  the repo (scripts, pkg manager)
 |                                        |  >> out: AGENTS.md managed block
 |                            |           |               |                 |
 |-- "build this" ----------------------->|               |                 |
 |                                 bmad-build             |                 |
 |                                 << in:  "add servings multiplier" + codebase
 |                                 >> out: code + spec/.../impl/<story> record
 |                            |           |               |                 |
 |  [want a second pass?]     |           |               |                 |
 |-- optional ----------------------------|-------------->|                 |
 |                                                 bmad-code-review         |
 |                                              << in:  the change (diff/PR)
 |                                              >> out: findings + optional patches
 |                            |           |               |                 |
 |  [I want to walk the PR]   |           |               |                 |
 |-- optional ---------------------------------------------------------->   |
 |                                                            bmad-checkpoint-preview
 |                                              << in:  the commit / branch / PR
 |                                              >> out: walkthrough (usually no file)
 |                            |           |               |                 |
 v                            v           v               v                 v
```

Skills used: `bmad-build`, optionally `bmad-project-context`, `bmad-code-review`, `bmad-checkpoint-preview`.
Not used (correctly): PRD, architecture, sprint planning.

---

## 5. Sequence — epic-sized work (one coherent outcome)

Use case: "Ship pantry-to-plan: scan pantry, generate 7 dinners, export grocery list."

```
You    Forge/Brain   Recon     Spec      Build×N     QA      Retro
 |          |          |         |          |         |        |
 |-- idea fuzzy? ----->|         |          |         |        |
 |     bmad-brainstorming        |          |         |        |
 |       << in:  "pantry to weekly plan" (fuzzy)
 |       >> out: brainstorm.html + optional brainstorm-intent.md
 |     and/or bmad-forge-idea    |          |         |        |
 |       << in:  the chosen direction
 |       >> out: forge-report.html + optional forged-idea.md
 |          |          |         |          |         |        |
 |-- need evidence? ----------->|         |          |         |        |
 |                    bmad-deep-recon     |          |         |        |
 |                      << in:  subject + type (or a report to process)
 |                      >> out: research/*.md (+ optional HTML)
 |          |          |         |          |         |        |
 |-- intent is now defined -------------->|          |         |        |
 |                              bmad-spec            |         |        |
 |                              << in:  forged-idea + recon (condensed)
 |                              >> out: SPEC.md + companions + stories.yaml
 |          |          |         |          |         |        |
 |-- each story ----------------------------|------->|         |        |
 |                                   bmad-build      |         |        |
 |                                   << in:  one story + SPEC.md + codebase
 |                                   >> out: code + impl record
 |          |          |         |          |         |        |
 |  [patterns stable? unattended later stories]      |         |        |
 |-- optional -----------------------------|-------> |         |        |
 |                                   bmad-build-auto |         |        |
 |                                   << in:  one bounded story + spec + code
 |                                   >> out: code + impl + terminal status
 |          |          |         |          |         |        |
 |-- feature exists, need E2E ----------------------|-------->|        |
 |                                            bmad-qa-generate-e2e-tests
 |                                            << in:  implemented feature + SPEC
 |                                            >> out: API/E2E test suite
 |          |          |         |          |         |        |
 |-- epic done ------------------------------------------------------>|
 |                                                         bmad-retrospective
 |                                                         << in:  spec folder + stories.yaml + impl + code
 |                                                         >> out: retro.md + actions + verdict
 v          v          v         v          v         v        v
```

If mid-epic the pantry scanner is killed and you pivot to manual pantry entry:

```
You -----> bmad-correct-course -----> (update spec / restory / or restart planning)
              << in:  "kill pantry vision" + current SPEC/sprint
              >> out: change proposal.md
```

---

## 6. Sequence — full greenfield product (MealPlan AI)

This is the long path. Planning tools are independent — skip any box you do not need.
`bmad-spec` is the contract Build actually reads.

```
     You        Mary/Analyst           John/PM         Sally/UX      Winston/Arch      Amelia/Dev
      |              |                    |                |               |                |
      |-- lost?      |                    |                |               |                |
      |  bmad-help   |  << in:  "what first?" + empty repo                                  |
      |              |  >> out: (none — recommends brainstorm)                              |
      |              |                    |                |               |                |
      |== PHASE: THINK ================================================================     |
      |              |                    |                |               |                |
      |-- brainstorm -------------------->|                |               |                |
      |         bmad-brainstorming        |  << in:  "family food + AI" (fuzzy)             |
      |                                  |  >> out: brainstorm.html, brainstorm-intent.md   |
      |              |                    |                |               |                |
      |-- "is this even a good idea?" ---> |                |               |                |
      |         bmad-forge-idea           |  << in:  chosen direction from brainstorm       |
      |                                  |  >> out: forge-report.html, forged-idea.md       |
      |              |                    |                |               |                |
      |-- research market / users / tech >|                |               |                |
      |         bmad-deep-recon           |  << in:  subject + type (MR, UV, TR, …)         |
      |                                  |  >> out: research/{market,user-voice,tech}.md    |
      |              |                    |                |               |                |
      |-- optional: all agents debate --------------------------------------------->        |
      |         bmad-party-mode  (Mary + John + Winston + Sally + Amelia)                   |
      |         << in:  "should v1 attempt pantry vision?" + forged-idea + recon            |
      |         >> out: memories/installed/.memlog.md + {date}-*.html keepsake              |
      |              |                    |                |               |                |
      |== PHASE: WRITE THE PRODUCT =================================================        |
      |              |                    |                |               |                |
      |-- concept is clear, write brief ->|                |               |                |
      |         bmad-product-brief        |  << in:  forged-idea + recon + party takeaway   |
      |                                  |  >> out: brief.md, addendum.md  |                |
      |    OR stress-test working-backwards                 |               |                |
      |         bmad-prfaq                |  << in:  the product concept + recon            |
      |                                  |  >> out: prfaq-*.md             |                |
      |              |                    |                |               |                |
      |-- org needs sign-off -------------------------------->|            |                |
      |                                  bmad-prd             |            |                |
      |                                  create:  << in:  brief.md and/or prfaq + recon     |
      |                                  update:  << in:  prd.md + change signal            |
      |                                  validate:<< in:  finished prd.md + shipped rubric  |
      |                                  >> out: prd.md, addendum.md, .memlog.md            |
      |                                  >> out: validate HTML + .md findings               |
      |              |                    |                |               |                |
      |-- UI is the product ---------------------------------------------->|                |
      |                                                 bmad-ux            |                |
      |                                                 << in:  prd.md                      |
      |                                                 >> out: DESIGN.md, EXPERIENCE.md    |
      |              |                    |                |               |                |
      |-- several people will build in parallel ------------------------------------>|      |
      |                                                              bmad-architecture      |
      |                                                              << in:  prd.md + DESIGN.md
      |                                                              >> out: ARCHITECTURE-SPINE.md
      |              |                    |                |               |                |
      |-- lock WHAT (per epic) ------------------------------------------------------       |
      |         bmad-spec                 << in:  prd.md + UX + spine (condensed)           |
      |                                  >> out: specs/spec-{slug}/SPEC.md + stories.yaml   |
      |              |                    |                |               |                |
      |== PHASE: SLICE + GATE ======================================================        |
      |              |                    |                |               |                |
      |-- break into epics/stories ------------------------>|              |                |
      |                         bmad-create-epics-and-stories              |                |
      |                         << in:  ARCHITECTURE-SPINE.md + prd.md     |                |
      |                         >> out: planning/epics/*.md + story files  |                |
      |              |                    |                |               |                |
      |-- ready to implement? -------------------------------------------->|                |
      |                         bmad-sprint-planning (IR + tracking)       |                |
      |                         << in:  epics/stories                      |                |
      |                         >> out: PASS|CONCERNS|FAIL + sprint-status.yaml             |
      |              |                    |                |               |                |
      |== PHASE: SHIP =================================================================     |
      |              |                    |                |               |                |
      |-- implement next story ---------------------------------------------------------->  |
      |                                                                    bmad-build       |
      |                                                                    << in:  one story + SPEC.md + codebase
      |                                                                    >> out: code + impl record
      |              |                    |                |               |                |
      |-- extra review ------------------------------------------------------------------>  |
      |                                                                    bmad-code-review |
      |                                                                    << in:  the change (diff/PR)
      |                                                                    >> out: findings + patches
      |              |                    |                |               |                |
      |-- walk me through the PR -------------------------------------------------------->  |
      |                                                            bmad-checkpoint-preview  |
      |                                                            << in:  commit / branch / PR
      |                                                            >> out: walkthrough (rarely a file)
      |              |                    |                |               |                |
      |-- automate E2E ------------------------------------------------------------------>  |
      |                                                       bmad-qa-generate-e2e-tests    |
      |                                                       << in:  implemented feature + SPEC
      |                                                       >> out: API/E2E test suite    |
      |              |                    |                |               |                |
      |-- epic closed ------------------------------------------------------------------>   |
      |                                                                    bmad-retrospective
      |                                                                    << in:  spec folder + stories.yaml + impl + code
      |                                                                    >> out: retro.md + verdict
      |              |                    |                |               |                |
      |== ANYTIME ESCAPES =============================================================     |
      |  draft feels thin .............. bmad-advanced-elicitation                          |
      |     << in: just-written draft     >> out: in-place edits                            |
      |  review any artifact ........... bmad-review                                        |
      |     << in: path to artifact       >> out: findings.json + .md                       |
      |  plan just exploded ............ bmad-correct-course                                |
      |     << in: change + current plan  >> out: change proposal                           |
      |  agents keep making same mistake bmad-project-context (record pitfall)              |
      |     << in: observed mistake       >> out: AGENTS.md pitfall line                    |
      |  change how BMad behaves ....... bmad-customize                                     |
      |     << in: skill + override       >> out: _bmad/custom/*.toml                       |
      |  unattended later stories ...... bmad-build-auto                                    |
      |     << in: one bounded story      >> out: code + impl + terminal status             |
      v              v                    v                v               v                v
```

---

## 7. Sequence — brownfield / existing codebase

Use case: MealPlan AI already exists. You join the repo. Agents keep running the wrong test command.

```
You                 Context              Help           Spec/Build         Review
 |                    |                   |                 |                |
 |-- "document this    |                   |                 |                |
 |    repo / make      |                   |                 |                |
 |    agents useful" ->|                   |                 |                |
 |              bmad-project-context       |                 |                |
 |              setup | refresh |          |  << in:  the repo (scripts, pkg manager)
 |              record | audit             |  >> out: AGENTS.md managed block
 |                    |                   |                 |                |
 |-- "what next?" ------------------------>|                 |                |
 |                                 bmad-help                 |                |
 |                                 << in:  question + AGENTS.md + repo state
 |                                 >> out: (none — next skill)
 |                    |                   |                 |                |
 |-- well-defined change ----------------------------------->|                |
 |                                              bmad-spec?   |  << in:  ticket / brain dump
 |                                                           |  >> out: SPEC.md + companions
 |                                              then         |                |
 |                                              bmad-build   |  << in:  SPEC.md + codebase
 |                                                           |  >> out: code + impl record
 |                    |                   |                 |                |
 |                    |                   |                 |                |
 |-- agent used npm test again, it is pnpm ----------------->|                |
 |              bmad-project-context                         |                |
 |              (record pitfall)                             |                |
 |              << in:  the observed mistake                 |                |
 |              >> out: AGENTS.md (+ pitfall line)           |                |
 |                    |                   |                 |                |
 |-- before merge ---------------------------------------------------------->|
 |                                                            bmad-review    |
 |                                                            << in:  path to spec/diff
 |                                                            >> out: findings.json + .md
 |                                                         or bmad-code-review
 |                                                            << in:  the change
 |                                                            >> out: findings + patches
 v                    v                   v                 v                v
```

Deprecated names that still land here: `bmad-document-project`, `bmad-generate-project-context`.

---

## 8. Think-path detail — idea is not ready yet

Use case: "I want to do something with food and AI." That is too thin for a spec.

```
                    "something with food and AI"
                                |
                                v
                      bmad-brainstorming
                     << in:  "something with food and AI"
                     >> out: brainstorm.html
                                |
                                v
                         pick a direction
                                |
                                v
                        bmad-forge-idea
                  << in:  the chosen direction
                  >> out: forge-report.html
                  >> out: forged-idea.md (if it hardens)
                                |
              ┌─────────────────┴─────────────────┐
              |                                   |
         idea dies cheaply                  idea hardens
              |                                   |
              v                                   v
           STOP                         bmad-deep-recon
                                  << in:  subject + type
                                  >> out: research/*.md
                                              |
                                              v
                               brief ──or── PRFAQ ──or── PRD
                               << in:  forged-idea + recon
                               >> out: brief.md / prfaq-*.md / prd.md
                                              |
                                              v
                                          bmad-spec
                                          << in:  that written intent (condensed)
                                          >> out: SPEC.md
```

When to pick brief vs PRFAQ vs PRD:

```
Need a written product account?
│
├─ Concept is already clear, do not want to be talked out of it
│     └─ bmad-product-brief          (gentler)
│
├─ Want the Amazon Working-Backwards gauntlet
│     └─ bmad-prfaq                  (harsher, customer-first press release)
│
└─ More than one person must agree, or several epics must not diverge
      └─ bmad-prd                    (create / update / validate)
```

---

## 9. Research type tree — all of `bmad-deep-recon`

```
bmad-deep-recon
│
├─ HOW you run it
│     ├─ draft a prompt for ChatGPT / Gemini / Grok / Perplexity
│     ├─ process a finished report into a cited summary
│     └─ run research here (web fan-out)
│
└─ WHAT you research
      ├─ market         "Is there demand for family meal planning?"
      ├─ domain         "Dietary constraint + grocery taxonomy"
      ├─ technical      "Vision pantry scan vs barcode vs manual"
      ├─ competitive    "Tear down Mealime, Paprika, ChatGPT recipes"
      ├─ user-voice     "Reddit / reviews / JTBD of home cooks"
      ├─ academic-lit   "Nutrition adherence literature"
      ├─ select         "Choose between vision API vendors"
      └─ custom         via bmad-customize overrides
```

Deprecated names that still land here:

```
bmad-market-research      ──▶  bmad-deep-recon (market)
bmad-domain-research      ──▶  bmad-deep-recon (domain)
bmad-technical-research   ──▶  bmad-deep-recon (technical)
```

---

## 10. Review tree — `bmad-review` vs the ship reviewers

```
WHAT ARE YOU REVIEWING?
│
├─ Any artifact (PRD, spec, story, architecture, prose, a diff)
│     └─ bmad-review
│           ├─ adversarial        attack the claim
│           ├─ edge-case          hunt missed cases
│           ├─ verification-gap   "how would we know this is true?"
│           ├─ structure          cuts / merges / moves
│           └─ prose              copy-edit LLM slop
│
├─ Code change, adversarial + structured triage
│     └─ bmad-code-review          (calls review layers; ship-phase)
│
└─ You, the human, need a guided walkthrough of a commit/PR
      └─ bmad-checkpoint-preview
```

Deprecated names that still land on `bmad-review`:

```
bmad-editorial-review                 ──▶  bmad-review
bmad-editorial-review-prose           ──▶  bmad-review
bmad-editorial-review-structure       ──▶  bmad-review
bmad-review-adversarial-general       ──▶  bmad-review
bmad-review-edge-case-hunter          ──▶  bmad-review
bmad-review-verification-gap          ──▶  bmad-review
```

---

## 11. Implementation fork — `bmad-build` vs `bmad-build-auto`

```
NEED CODE?
│
├─ I will sit with it, checkpoints OK
│     └─ bmad-build          official Phase 4 loop
│
└─ Orchestrator / loop should run one unit with no human
      └─ bmad-build-auto     does NOT pick the next story
```

Deprecated names that still land here:

```
bmad-quick-dev     ──▶  bmad-build
bmad-dev-story     ──▶  bmad-build
bmad-create-story  ──▶  bmad-build
bmad-dev-auto      ──▶  bmad-build-auto
```

---

## 12. Sequence — mid-sprint explosion (`bmad-correct-course`)

Use case: grocery partner API is shutting down next month.

```
You          PM (John)         Correct Course        then maybe…
 |               |                    |                  |
 |-- "the API is dying" ------------->|                  |
 |                    bmad-correct-course                |
 |                    << in:  "API is dying" + prd/arch/epics/sprint
 |                    >> out: change proposal.md          |
 |               |                    |                  |
 |               |                    |-- update PRD ---> bmad-prd
 |               |                    |     << in:  prd.md + the proposal
 |               |                    |     >> out: prd.md
 |               |                    |-- redo arch ----> bmad-architecture
 |               |                    |     << in:  updated prd + codebase
 |               |                    |     >> out: ARCHITECTURE-SPINE.md
 |               |                    |-- restory ------> bmad-create-epics-and-stories
 |               |                    |     << in:  new spine + prd
 |               |                    |     >> out: epics/*.md
 |               |                    |-- replan -------> bmad-sprint-planning
 |               |                    |     << in:  new epics + old sprint-status.yaml
 |               |                    |     >> out: repaired sprint-status.yaml
 |               |                    |-- start over ---> brief / PRFAQ / spec
 v               v                    v                  v
```

---

## 13. Anytime skills — they sit beside every phase

```
  THINK          PLAN           SLICE          SHIP
    |              |              |              |
    +--------------+--------------+--------------+
                   |
                   |  available on every arrow
                   |
     ┌─────────────┼─────────────┬───────────────┐
     v             v             v               v
 bmad-help   bmad-review   bmad-customize   bmad-party-mode
                   |
                   v
         bmad-advanced-elicitation
                   |
                   v
         bmad-project-context
                   |
                   v
         bmad-correct-course     (when the plan itself is wrong)
```

---

## 14. Agent roundtable sequence (`bmad-party-mode`)

Short form here. The full treatment — custom focus group, two rooms, memory, keepsake, modes, handoff — is [use-case 06 Atlas Health](use-cases/06-party-mode-atlas-health.md).

Use case (short): "Should MealPlan AI be B2C subscription or B2B for grocery chains?"

```
You                Party Mode              Mary    John    Winston   Sally   Amelia
 |                     |                    |       |         |        |        |
 |-- "run a party" ---> |                    |       |         |        |        |
 |              bmad-party-mode              |       |         |        |        |
 |              --mode auto                  |       |         |        |        |
 |              << in:  topic + optional --party/--mode                         |
 |              << in:  memories/installed/.memlog.md (if memory on)             |
 |                     |                    |       |         |        |        |
 |                     |-- analyst -------->|       |         |        |        |
 |                     |-- PM ----------------------|->       |        |        |
 |                     |-- architect -----------------------|->        |        |
 |                     |-- UX --------------------------------------->|        |
 |                     |-- dev ------------------------------------------------>|
 |                     |                    |       |         |        |        |
 |              clash stays open — do not bow-tie it                            |
 |              silent memlog appends (clash / alliance / outcome)              |
 |                     |                    |       |         |        |        |
 |-- you signal done ->|                    |       |         |        |        |
 |              takeaways + optional HTML keepsake                              |
 |              >> out: party-mode/{date}-*.html                                |
 |              >> out: party-mode/memories/installed/.memlog.md                |
 |                     |                    |       |         |        |        |
 |-- if it hardened, hand off to bmad-spec or bmad-prd                          |
 |              << in:  party takeaways (+ keepsake / memlog)                   |
 |              >> out: SPEC.md  or  prd.md                                     |
 v                     v                    v       v         v        v        v
```

Authoring a custom party (focus group, red-team panel, open-cast) writes `_bmad/custom/bmad-party-mode.user.toml` via `bmad-customize`. See use-case 06.

---

## 15. Customize vs everything else

```
Want a different *answer this once*?
    └─ just tell the agent  (session only)

Want a different *behavior forever*?
    └─ bmad-customize
          writes _bmad/custom/*.toml
          (facts, templates, activation hooks, menus)
```

---

## 16. Complete current-skill map (29)

```
CORE (8)                          BMM PLAN (9)                         BMM SHIP (7)
────────                          ────────────                         ───────────
bmad-help                         bmad-product-brief                   bmad-build
bmad-brainstorming                bmad-prfaq                           bmad-build-auto
bmad-forge-idea                   bmad-prd                             bmad-code-review
bmad-deep-recon                   bmad-ux                              bmad-checkpoint-preview
bmad-advanced-elicitation         bmad-spec                            bmad-qa-generate-e2e-tests
bmad-review                       bmad-architecture                    bmad-correct-course
bmad-party-mode                   bmad-create-epics-and-stories        bmad-retrospective
bmad-customize                    bmad-sprint-planning
                                  bmad-project-context

BMM AGENTS (5)                          default model tier
──────────────                          ──────────────────
bmad-agent-analyst      Mary            Frontier (recon/forge/PRFAQ)
bmad-agent-pm           John            Workhorse; Frontier for org PRD / CC
bmad-agent-architect    Winston         Frontier
bmad-agent-ux-designer  Sally           Workhorse + vision
bmad-agent-dev          Amelia          Workhorse coding; other family for CR
```

See [§2a](#2a-recommended-models-agents-and-skills) for the full mapping.

---

## 17. Complete deprecated-shim map (20)

Do not choose these. If something still invokes them, they forward.

```
OLD NAME                              FORWARDS TO
─────────────────────────────────     ──────────────────────────────────
bmad-editorial-review                 bmad-review
bmad-editorial-review-prose           bmad-review
bmad-editorial-review-structure       bmad-review
bmad-review-adversarial-general       bmad-review
bmad-review-edge-case-hunter          bmad-review
bmad-review-verification-gap          bmad-review

bmad-market-research                  bmad-deep-recon (market)
bmad-domain-research                  bmad-deep-recon (domain)
bmad-technical-research               bmad-deep-recon (technical)

bmad-create-prd                       bmad-prd (create)
bmad-edit-prd                         bmad-prd (update)
bmad-validate-prd                     bmad-prd (validate)
bmad-create-architecture              bmad-architecture

bmad-generate-project-context         bmad-project-context
bmad-document-project                 bmad-project-context
bmad-sprint-status                    bmad-sprint-planning (status)

bmad-quick-dev                        bmad-build
bmad-dev-story                        bmad-build
bmad-create-story                     bmad-build
bmad-dev-auto                         bmad-build-auto
```

Tree form:

```
deprecated shims (20)
├── review family (6) ──────────────▶ bmad-review
├── research family (3) ────────────▶ bmad-deep-recon
├── PRD family (3) ─────────────────▶ bmad-prd
├── architecture (1) ───────────────▶ bmad-architecture
├── project context (2) ────────────▶ bmad-project-context
├── sprint status (1) ──────────────▶ bmad-sprint-planning
├── interactive build (3) ──────────▶ bmad-build
└── unattended build (1) ───────────▶ bmad-build-auto
```

---

## 18. Coverage checklist (all 49, none missing)

### Current — Core (8)
- [x] `bmad-help`
- [x] `bmad-brainstorming`
- [x] `bmad-forge-idea`
- [x] `bmad-deep-recon`
- [x] `bmad-advanced-elicitation`
- [x] `bmad-review`
- [x] `bmad-party-mode`
- [x] `bmad-customize`

### Current — Agents (5)
- [x] `bmad-agent-analyst`
- [x] `bmad-agent-pm`
- [x] `bmad-agent-architect`
- [x] `bmad-agent-ux-designer`
- [x] `bmad-agent-dev`

### Current — Plan (9)
- [x] `bmad-product-brief`
- [x] `bmad-prfaq`
- [x] `bmad-prd`
- [x] `bmad-ux`
- [x] `bmad-spec`
- [x] `bmad-architecture`
- [x] `bmad-create-epics-and-stories`
- [x] `bmad-sprint-planning`
- [x] `bmad-project-context`

### Current — Ship (7)
- [x] `bmad-build`
- [x] `bmad-build-auto`
- [x] `bmad-code-review`
- [x] `bmad-checkpoint-preview`
- [x] `bmad-qa-generate-e2e-tests`
- [x] `bmad-correct-course`
- [x] `bmad-retrospective`

### Deprecated shims (20)
- [x] `bmad-editorial-review`
- [x] `bmad-editorial-review-prose`
- [x] `bmad-editorial-review-structure`
- [x] `bmad-review-adversarial-general`
- [x] `bmad-review-edge-case-hunter`
- [x] `bmad-review-verification-gap`
- [x] `bmad-market-research`
- [x] `bmad-domain-research`
- [x] `bmad-technical-research`
- [x] `bmad-create-prd`
- [x] `bmad-edit-prd`
- [x] `bmad-validate-prd`
- [x] `bmad-create-architecture`
- [x] `bmad-generate-project-context`
- [x] `bmad-document-project`
- [x] `bmad-sprint-status`
- [x] `bmad-quick-dev`
- [x] `bmad-dev-story`
- [x] `bmad-create-story`
- [x] `bmad-dev-auto`

---

## 19. One-line "when" for every current skill

| Skill | Use when | Reads (`<< in`) | Writes (`>> out`) |
|---|---|---|---|
| `bmad-help` | You are lost, or asking "what next?" | your question + existing artifacts | (none) |
| `bmad-brainstorming` | You need options, not a decision | a topic or "I'm stuck" | `brainstorm.html`, optional `brainstorm-intent.md` |
| `bmad-forge-idea` | You have an idea and want it hardened or killed | the idea (sentence or short brief) | `forge-report.html`, optional `forged-idea.md` |
| `bmad-deep-recon` | A decision needs evidence (any research type) | subject + type, or a report to process, or candidates | `research/*.md` (+ optional HTML) |
| `bmad-advanced-elicitation` | A just-produced draft needs a second, harder pass | that draft + a method | in-place edits |
| `bmad-review` | Any artifact needs multi-lens critique before it ships | path to doc/diff/spec + optional lenses | findings JSON + markdown |
| `bmad-party-mode` | You want several agents / personas in one room | topic; optional `--party`/`--mode`; memlog; or interview notes to author a cast | keepsake HTML, `.memlog.md`, optional custom TOML |
| `bmad-customize` | You want lasting behavior/template/menu changes | skill name + the override | `_bmad/custom/*.toml` |
| `bmad-agent-analyst` | You want Mary facilitating analysis work | "talk to Mary" or a menu code | (none — dispatches) |
| `bmad-agent-pm` | You want John facilitating product work | "talk to John" or a menu code | (none — dispatches) |
| `bmad-agent-architect` | You want Winston facilitating architecture work | "talk to Winston" or a menu code | (none — dispatches) |
| `bmad-agent-ux-designer` | You want Sally facilitating UX work | "talk to Sally" or `CU` | (none — dispatches) |
| `bmad-agent-dev` | You want Amelia facilitating implementation work | "talk to Amelia" or `BD`/`CR`/`QA` | (none — dispatches) |
| `bmad-product-brief` | Concept is clear; write the vision before a PRD | the concept; optional recon / forged-idea | `brief.md`, `addendum.md` |
| `bmad-prfaq` | You want Working-Backwards stress-test, not a gentle brief | the product concept; optional recon / forge | `prfaq-*.md` |
| `bmad-prd` | Create, update, or validate a Product Requirements Document | create: brief/PRFAQ/brain dump; update: `prd.md` + change signal; validate: finished PRD | `prd.md` / validation HTML |
| `bmad-ux` | UI/UX is a primary surface and needs written design | PRD or spec | `DESIGN.md`, `EXPERIENCE.md` |
| `bmad-spec` | Lock WHAT into a short machine contract Build can read | any intent (brief, PRD, transcript, dump, folder) | `SPEC.md` + companions, optional `stories.yaml` |
| `bmad-architecture` | Lock HOW so independently built parts stay consistent | PRD or spec; UX if present; brownfield: the codebase | `ARCHITECTURE-SPINE.md` |
| `bmad-create-epics-and-stories` | Slice a PRD/architecture into an implementation backlog | architecture + PRD/spec | epic + story files |
| `bmad-sprint-planning` | Gate readiness and/or track/repair sprint status | epics/stories, or existing `sprint-status.yaml` | verdict + `sprint-status.yaml` |
| `bmad-project-context` | Teach agents this repo (commands, policy, pitfalls) | the repo / an observed mistake / current `AGENTS.md` | `AGENTS.md` managed block |
| `bmad-build` | Implement one intent/story with you in the loop | sentence, issue, `SPEC.md`, or one story + the codebase | code + impl record |
| `bmad-build-auto` | Implement one unit unattended for an orchestrator | one bounded story + spec + codebase | code + impl + terminal status |
| `bmad-code-review` | Extra adversarial review of a code change | a diff / branch / PR | findings + optional patches |
| `bmad-checkpoint-preview` | You want a guided human walkthrough of a change | a commit / branch / PR | walkthrough (usually no file) |
| `bmad-qa-generate-e2e-tests` | Feature exists; you want API/E2E automation | implemented code (+ SPEC success conditions) | API / E2E test suite |
| `bmad-correct-course` | Mid-flight change is big enough to threaten the plan | change signal + current PRD/arch/epics/sprint | change proposal |
| `bmad-retrospective` | An epic is done; judge the evidence and extract lessons | spec folder, `stories.yaml`, impl records, code | retro doc + verdict |
