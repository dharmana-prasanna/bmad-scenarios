# When to Use Which BMAD-Method Skill

ASCII trees and sequence diagrams for every installed BMad Method skill.

Catalog source: a current BMad Method install (`skill-manifest.csv`).
**49 skills total: 29 current + 20 deprecated shims.**

Running example: **MealPlan AI** — a family meal-planning app that turns pantry + dietary constraints into a weekly plan and grocery list.

**End-to-end projects (artifacts on every arrow):** see [`use-cases/README.md`](use-cases/README.md). Six files — three greenfield, two brownfield, one party-mode deep dive.

| # | Project | Field |
|---|---|---|
| [01](use-cases/01-greenfield-mealplan-ai.md) | MealPlan AI | Greenfield product |
| [02](use-cases/02-greenfield-harborwatch.md) | HarborWatch flood alerts | Greenfield, research-heavy |
| [03](use-cases/03-greenfield-invoicediff-cli.md) | InvoiceDiff CLI | Greenfield, spec-first |
| [04](use-cases/04-brownfield-northwind-sso.md) | Northwind Shop SSO | Brownfield |
| [05](use-cases/05-brownfield-ledgerlite-course-correction.md) | LedgerLite store cutover | Brownfield + correct-course |
| [06](use-cases/06-party-mode-atlas-health.md) | Atlas Health video visits | **Party mode** showcase |

---

## 0a. What each current skill writes

Right-hand notes in the sequences below are **artifacts**, not extra steps.

```
CORE
  bmad-help                    (none — next-skill recommendation)
  bmad-brainstorming           {output}/brainstorming/brainstorm.html
                               optional brainstorm-intent.md
  bmad-forge-idea              {output}/forge/forge-report.html
                               optional forged-idea.md
  bmad-deep-recon              {planning}/research/*.md  (+ optional HTML briefing)
  bmad-advanced-elicitation    in-place edits to the draft just produced
  bmad-review                  findings.json + findings.md
  bmad-party-mode              {output}/party-mode/{date}-*.html     (keepsake, on wrap)
                               {output}/party-mode/memories/<party>/.memlog.md
                               optional _bmad/custom/bmad-party-mode*.toml
  bmad-customize               _bmad/custom/<skill>.toml  or  <skill>.user.toml

AGENTS
  bmad-agent-*                 none of their own — they dispatch workflows

PLAN
  bmad-product-brief           brief.md + addendum.md
  bmad-prfaq                   prfaq-*.md
  bmad-prd                     create/update: prd.md, addendum.md, .memlog.md
                               validate: HTML + .md findings
  bmad-ux                      DESIGN.md, EXPERIENCE.md, .memlog.md
  bmad-spec                    specs/spec-{slug}/SPEC.md + companions
                               optional stories.yaml
  bmad-architecture            ARCHITECTURE-SPINE.md
  bmad-create-epics-and-stories  planning/epics/*.md + story files
  bmad-sprint-planning         PASS|CONCERNS|FAIL + sprint-status.yaml
  bmad-project-context         AGENTS.md managed block

SHIP
  bmad-build                   code + per-story implementation record
  bmad-build-auto              same + terminal status for an orchestrator
  bmad-code-review             findings + optional applied patches
  bmad-checkpoint-preview      walkthrough (usually conversation-only)
  bmad-qa-generate-e2e-tests   API / E2E test suite
  bmad-correct-course          change proposal (then may rewrite PRD/arch/epics/sprint)
  bmad-retrospective           retro doc + action items + acceptance verdict
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
 |                     context            |  >> AGENTS.md managed block     |
 |                            |           |               |                 |
 |-- "build this" ----------------------->|               |                 |
 |                                 bmad-build             |                 |
 |                                 clarify→plan→          |  >> code
 |                                 implement→review       |  >> spec/.../impl/<story> record
 |                            |           |               |                 |
 |  [want a second pass?]     |           |               |                 |
 |-- optional ----------------------------|-------------->|                 |
 |                                                 bmad-code-review         |
 |                                                            >> findings + optional patches
 |                            |           |               |                 |
 |  [I want to walk the PR]   |           |               |                 |
 |-- optional ---------------------------------------------------------->   |
 |                                                            bmad-checkpoint-preview
 |                                                            >> walkthrough (usually no file)
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
 |       >> brainstorm.html + optional brainstorm-intent.md    |
 |     and/or bmad-forge-idea    |          |         |        |
 |       >> forge-report.html + optional forged-idea.md        |
 |          |          |         |          |         |        |
 |-- need evidence? ----------->|         |          |         |        |
 |                    bmad-deep-recon     |          |         |        |
 |                      >> research/*.md (+ optional HTML)     |
 |          |          |         |          |         |        |
 |-- intent is now defined -------------->|          |         |        |
 |                              bmad-spec            |         |        |
 |                              >> SPEC.md + companions        |
 |                              >> optional stories.yaml       |
 |          |          |         |          |         |        |
 |-- each story ----------------------------|------->|         |        |
 |                                   bmad-build      |         |        |
 |                                   >> code + impl record     |
 |          |          |         |          |         |        |
 |  [patterns stable? unattended later stories]      |         |        |
 |-- optional -----------------------------|-------> |         |        |
 |                                   bmad-build-auto |         |        |
 |                                   >> code + impl + terminal status
 |          |          |         |          |         |        |
 |-- feature exists, need E2E ----------------------|-------->|        |
 |                                            bmad-qa-generate-e2e-tests
 |                                            >> API/E2E test suite    |
 |          |          |         |          |         |        |
 |-- epic done ------------------------------------------------------>|
 |                                                         bmad-retrospective
 |                                                         >> retro.md
 |                                                         >> actions + verdict
 v          v          v         v          v         v        v
```

If mid-epic the pantry scanner is killed and you pivot to manual pantry entry:

```
You -----> bmad-correct-course -----> (update spec / restory / or restart planning)
              >> change proposal.md
```

---

## 6. Sequence — full greenfield product (MealPlan AI)

This is the long path. Planning tools are independent — skip any box you do not need.
`bmad-spec` is the contract Build actually reads.

```
     You        Mary/Analyst           John/PM         Sally/UX      Winston/Arch      Amelia/Dev
      |              |                    |                |               |                |
      |-- lost?      |                    |                |               |                |
      |  bmad-help   |                    |                |               |                |
      |              |                    |                |               |                |
      |== PHASE: THINK ================================================================     |
      |              |                    |                |               |                |
      |-- brainstorm -------------------->|                |               |                |
      |         bmad-brainstorming        |  >> brainstorm.html, brainstorm-intent.md       |
      |              |                    |                |               |                |
      |-- "is this even a good idea?" ---> |                |               |                |
      |         bmad-forge-idea           |  >> forge-report.html, forged-idea.md           |
      |              |                    |                |               |                |
      |-- research market / users / tech >|                |               |                |
      |         bmad-deep-recon           |  >> research/{market,user-voice,tech}.md        |
      |         (MR, UV, TR, …)           |                |               |                |
      |              |                    |                |               |                |
      |-- optional: all agents debate --------------------------------------------->        |
      |         bmad-party-mode  (Mary + John + Winston + Sally + Amelia)                   |
      |         >> memories/installed/.memlog.md + party-mode/{date}-*.html keepsake        |
      |              |                    |                |               |                |
      |== PHASE: WRITE THE PRODUCT =================================================        |
      |              |                    |                |               |                |
      |-- concept is clear, write brief ->|                |               |                |
      |         bmad-product-brief        |  >> brief.md, addendum.md      |                |
      |    OR stress-test working-backwards                 |               |                |
      |         bmad-prfaq                |  >> prfaq-*.md                 |                |
      |              |                    |                |               |                |
      |-- org needs sign-off -------------------------------->|            |                |
      |                                  bmad-prd             |            |                |
      |                                  create|update|validate            |                |
      |                                  >> prd.md, addendum.md, .memlog.md                |
      |                                  >> validate: HTML + .md findings  |                |
      |              |                    |                |               |                |
      |-- UI is the product ---------------------------------------------->|                |
      |                                                 bmad-ux            |                |
      |                                                 >> DESIGN.md, EXPERIENCE.md         |
      |              |                    |                |               |                |
      |-- several people will build in parallel ------------------------------------>|      |
      |                                                              bmad-architecture      |
      |                                                              >> ARCHITECTURE-SPINE.md
      |              |                    |                |               |                |
      |-- lock WHAT (per epic) ------------------------------------------------------       |
      |         bmad-spec                 >> specs/spec-{slug}/SPEC.md + stories.yaml       |
      |              |                    |                |               |                |
      |== PHASE: SLICE + GATE ======================================================        |
      |              |                    |                |               |                |
      |-- break into epics/stories ------------------------>|              |                |
      |                         bmad-create-epics-and-stories              |                |
      |                         >> planning/epics/*.md + story files       |                |
      |              |                    |                |               |                |
      |-- ready to implement? -------------------------------------------->|                |
      |                         bmad-sprint-planning (IR + tracking)       |                |
      |                         >> PASS|CONCERNS|FAIL + sprint-status.yaml |                |
      |              |                    |                |               |                |
      |== PHASE: SHIP =================================================================     |
      |              |                    |                |               |                |
      |-- implement next story ---------------------------------------------------------->  |
      |                                                                    bmad-build       |
      |                                                                    >> code + impl record
      |              |                    |                |               |                |
      |-- extra review ------------------------------------------------------------------>  |
      |                                                                    bmad-code-review |
      |                                                                    >> findings + patches
      |              |                    |                |               |                |
      |-- walk me through the PR -------------------------------------------------------->  |
      |                                                            bmad-checkpoint-preview  |
      |                                                            >> walkthrough (rarely a file)
      |              |                    |                |               |                |
      |-- automate E2E ------------------------------------------------------------------>  |
      |                                                       bmad-qa-generate-e2e-tests    |
      |                                                       >> API/E2E test suite         |
      |              |                    |                |               |                |
      |-- epic closed ------------------------------------------------------------------>   |
      |                                                                    bmad-retrospective
      |                                                                    >> retro.md + verdict
      |              |                    |                |               |                |
      |== ANYTIME ESCAPES =============================================================     |
      |  draft feels thin .............. bmad-advanced-elicitation                          |
      |  review any artifact ........... bmad-review                                        |
      |  plan just exploded ............ bmad-correct-course                                |
      |  agents keep making same mistake bmad-project-context (record pitfall)              |
      |  change how BMad behaves ....... bmad-customize                                     |
      |  unattended later stories ...... bmad-build-auto                                    |
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
 |              setup | refresh |          |  >> AGENTS.md managed block     |
 |              record | audit             |                 |                |
 |                    |                   |                 |                |
 |-- "what next?" ------------------------>|                 |                |
 |                                 bmad-help                 |                |
 |                    |                   |                 |                |
 |-- well-defined change ----------------------------------->|                |
 |                                              bmad-spec?   |  >> SPEC.md + companions
 |                                              then         |                |
 |                                              bmad-build   |  >> code + impl record
 |                    |                   |                 |                |
 |                    |                   |                 |                |
 |-- agent used npm test again, it is pnpm ----------------->|                |
 |              bmad-project-context                         |                |
 |              (record pitfall)                             |                |
 |                    |                   |                 |                |
 |-- before merge ---------------------------------------------------------->|
 |                                                            bmad-review    |
 |                                                            >> findings.json + .md
 |                                                         or bmad-code-review
 |                                                            >> findings + patches
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
                     (generate options)
                     >> brainstorm.html
                                |
                                v
                         pick a direction
                                |
                                v
                        bmad-forge-idea
                  (harden, prove, or kill)
                  >> forge-report.html
                  >> forged-idea.md (if it hardens)
                                |
              ┌─────────────────┴─────────────────┐
              |                                   |
         idea dies cheaply                  idea hardens
              |                                   |
              v                                   v
           STOP                         bmad-deep-recon
                                  market + user-voice +
                                  competitive + technical
                                  >> research/*.md
                                              |
                                              v
                               brief ──or── PRFAQ ──or── PRD
                               >> brief.md / prfaq-*.md / prd.md
                                              |
                                              v
                                          bmad-spec
                                          >> SPEC.md
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
 |                    assesses blast radius              |
 |                    >> change proposal.md              |
 |               |                    |                  |
 |               |                    |-- update PRD ---> bmad-prd            >> prd.md
 |               |                    |-- redo arch ----> bmad-architecture   >> ARCHITECTURE-SPINE.md
 |               |                    |-- restory ------> bmad-create-epics-and-stories >> epics/*.md
 |               |                    |-- replan -------> bmad-sprint-planning >> sprint-status.yaml
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
 |                     |                    |       |         |        |        |
 |              READ memories/installed/.memlog.md (if memory on)               |
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
 |              >> party-mode/{date}-*.html                                     |
 |              >> party-mode/memories/installed/.memlog.md                     |
 |                     |                    |       |         |        |        |
 |-- if it hardened, hand off to bmad-spec or bmad-prd                          |
 |              >> SPEC.md  or  prd.md                                          |
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

BMM AGENTS (5)
──────────────
bmad-agent-analyst      Mary
bmad-agent-pm           John
bmad-agent-architect    Winston
bmad-agent-ux-designer  Sally
bmad-agent-dev          Amelia
```

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

| Skill | Use when | Writes |
|---|---|---|
| `bmad-help` | You are lost, or asking "what next?" | (none) |
| `bmad-brainstorming` | You need options, not a decision | `brainstorm.html`, optional `brainstorm-intent.md` |
| `bmad-forge-idea` | You have an idea and want it hardened or killed | `forge-report.html`, optional `forged-idea.md` |
| `bmad-deep-recon` | A decision needs evidence (any research type) | `research/*.md` (+ optional HTML) |
| `bmad-advanced-elicitation` | A just-produced draft needs a second, harder pass | In-place edits |
| `bmad-review` | Any artifact needs multi-lens critique before it ships | findings JSON + markdown |
| `bmad-party-mode` | You want several agents / personas in one room | keepsake HTML, per-party `.memlog.md`, optional custom TOML |
| `bmad-customize` | You want lasting behavior/template/menu changes | `_bmad/custom/*.toml` |
| `bmad-agent-analyst` | You want Mary facilitating analysis work | (none — dispatches) |
| `bmad-agent-pm` | You want John facilitating product work | (none — dispatches) |
| `bmad-agent-architect` | You want Winston facilitating architecture work | (none — dispatches) |
| `bmad-agent-ux-designer` | You want Sally facilitating UX work | (none — dispatches) |
| `bmad-agent-dev` | You want Amelia facilitating implementation work | (none — dispatches) |
| `bmad-product-brief` | Concept is clear; write the vision before a PRD | `brief.md`, `addendum.md` |
| `bmad-prfaq` | You want Working-Backwards stress-test, not a gentle brief | `prfaq-*.md` |
| `bmad-prd` | Create, update, or validate a Product Requirements Document | `prd.md` / validation HTML |
| `bmad-ux` | UI/UX is a primary surface and needs written design | `DESIGN.md`, `EXPERIENCE.md` |
| `bmad-spec` | Lock WHAT into a short machine contract Build can read | `SPEC.md` + companions, optional `stories.yaml` |
| `bmad-architecture` | Lock HOW so independently built parts stay consistent | `ARCHITECTURE-SPINE.md` |
| `bmad-create-epics-and-stories` | Slice a PRD/architecture into an implementation backlog | epic + story files |
| `bmad-sprint-planning` | Gate readiness and/or track/repair sprint status | verdict + `sprint-status.yaml` |
| `bmad-project-context` | Teach agents this repo (commands, policy, pitfalls) | `AGENTS.md` managed block |
| `bmad-build` | Implement one intent/story with you in the loop | code + impl record |
| `bmad-build-auto` | Implement one unit unattended for an orchestrator | code + impl + terminal status |
| `bmad-code-review` | Extra adversarial review of a code change | findings + optional patches |
| `bmad-checkpoint-preview` | You want a guided human walkthrough of a change | walkthrough (usually no file) |
| `bmad-qa-generate-e2e-tests` | Feature exists; you want API/E2E automation | API / E2E test suite |
| `bmad-correct-course` | Mid-flight change is big enough to threaten the plan | change proposal |
| `bmad-retrospective` | An epic is done; judge the evidence and extract lessons | retro doc + verdict |
