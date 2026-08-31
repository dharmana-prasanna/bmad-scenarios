# 06 — Atlas Health: `bmad-party-mode` end to end

**Field:** mixed (greenfield product decision on top of a brownfield clinic OS)  
**Skill in the spotlight:** `bmad-party-mode`  
**Why this path:** a single agent will flatten the tradeoff. The decision needs a fight, then a second fight with the people who will sit in the waiting room.

**Atlas Health** is a clinic operating system (scheduling, charting, billing) already in production at 11 mid-size practices. The next bet is **video visits**. Three options are on the table and the founders are splitting:

| Option | Pitch | Hidden cost |
|---|---|---|
| **A. Twilio Video** | Fast, known, metered | PHI / BAA, per-minute surprise bills, UX is "a Twilio room" |
| **B. In-house WebRTC** | Control, no per-minute tax | 6+ months, TURN, iOS Safari, on-call |
| **C. White-label Doxy.me** | Clinicians already know it | You do not own the session; brand and data gravity leak |

This file is the party-mode deep dive. Other skills appear only as **handoffs** after the rooms close.

---

## When party mode is the right skill

```
Need more than one mind in the room?
│
├─ One specialist, sequential menus          → bmad-agent-pm / architect / …
├─ A facilitated critique of a draft         → bmad-advanced-elicitation
├─ A structured multi-lens report            → bmad-review
└─ People talking to each other, clashing,
   pulling YOU into the argument,
   leaving a history                         → bmad-party-mode
```

Party mode is not a panel that files answers. If the transcript reads like five memos, the party is dead. The skill's own bar: short turns, unequal voices, unresolved clash, one woven conversation, the user is a guest in the argument.

---

## What party mode reads and writes

```
<< in  (any run)
    a topic ("Twilio vs WebRTC vs Doxy — pick a lane")
    optional --party <id> / --mode <session|auto|subagent|agent-team>
    optional --non-interactive
    existing memories/<party>/.memlog.md   (if that room's memory is on)
    for authoring: interview notes, a cast idea, or a live ad-hoc roster to save

>> out (when they apply)
    memories/<party>/.memlog.md            live append if memory on
    {date}-*.html keepsake                 on wrap, if you accept
    _bmad/custom/bmad-party-mode*.toml     only when authoring / saving a cast
```

## Artifacts party mode actually writes

```
{output_folder}/party-mode/
├── 2026-06-18-atlas-video-strategy.html      ← end-of-session keepsake (offered, not forced)
├── 2026-06-19-waiting-room-panel.html
└── memories/
    ├── installed/.memlog.md                  ← default BMad-agent room
    └── waiting-room/.memlog.md               ← custom group (only if memory=true)

_bmad/custom/
└── bmad-party-mode.user.toml                 ← custom members + groups (via bmad-customize)
```

| Artifact | When it appears | What it is |
|---|---|---|
| `{output}/party-mode/memories/<party>/.memlog.md` | Live, if that party's memory is on | Append-only beats: clashes, alliances, decisions. **Not a transcript.** |
| `{output}/party-mode/{date}-*.html` | On wrap-up, if you accept the keepsake | Self-contained remembrance, laid out by persona |
| `_bmad/custom/bmad-party-mode.user.toml` | When you author or save a cast | `party_members`, `party_groups`, optional `default_party` |
| `_bmad/custom/bmad-party-mode.toml` | Same, if the party is meant for the whole team | Team-scoped override |

Conversation-only until wrap-up: the live dialogue itself is not a file.

---

## Casts used in this story

```
ROOM A  "installed"          ROOM B  "waiting-room"         ROOM C  "video-red-team"
default BMad agents          custom focus group             custom lens panel
memory = true                memory = true                  memory = false

Mary    Analyst              Dee      67, rural patient     Nix     assumes it is broken
John    PM                   Omar     34, night-shift RN    Pat     edge-case hunter
Winston Architect            Priya    clinic ops manager    Cal     hates cleverness
Sally   UX                   Kenji    41, anxious first-    Prag    ships Tuesday
Amelia  Dev                           time telehealth user
```

Room B is **distilled from real interview notes** (the "focus group from data" authoring path). Room C is a **panel of lenses**.

---

## Sequence 1 — author the waiting-room party

Use `bmad-party-mode` itself (create/configure intent), which writes through `bmad-customize`. Do not hand-edit TOML.

```
YOU          PARTY MODE / CUSTOMIZE
 |                    |
 |-- "build me a patient + clinician focus group             |
 |    from these 14 interview notes" ----------------------->|
 |                    |
 |             bmad-party-mode
 |             intent = create / configure
 |                      << in:  14 interview notes (or a spreadsheet / export)
 |                      << in:  "patient + clinician focus group"
 |             loads references/create-party.md
 |                    |                                      |
 |             clusters notes by behavior                    |
 |             (not demographics alone):                     |
 |               rural bandwidth, night-shift RN,            |
 |               ops-manager bottleneck, first-time fear     |
 |                    |                                      |
 |             proposes 4 personas, you correct the cut      |
 |                    |                                      |
 |             bmad-customize
 |                      << in:  drafted party_members + party_groups
 |                      >> out: _bmad/custom/bmad-party-mode.user.toml
 |             target = bmad-party-mode [workflow]
 |             writes sparse tables:                         |
 |               [[workflow.party_members]]  ×4              |
 |               [[workflow.party_groups]]                   |
 |                 id      = "waiting-room"                  |
 |                 scene   = "after-hours clinic lobby,      |
 |                            fluorescent hum, someone       |
 |                            is 20 minutes past their slot" |
 |                 members = ["dee","omar","priya","kenji"]  |
 |                 memory  = true                            |
 |                    |                                      |
 |-- "also a red-team room for the video stack" ------------>|
 |             same skill, second group
 |                      << in:  "lens panel for the video stack"
 |                      >> out: same .user.toml (merged)
 |               id      = "video-red-team"                  |
 |               memory  = false                             |
 |               members = ["nix","pat","cal","prag"]        |
 |                    |                                      |
 |             how to summon later:                          |
 |               bmad-party-mode --party waiting-room        |
 |               bmad-party-mode --party video-red-team      |
 v                    v                                      v
```

Offer `--mode subagent` (or set `party_mode = "subagent"` on the group) for a focus group. Otherwise one mind voices every patient and they bleed together.

---

## Sequence 2 — Room A: the founders' fight (installed agents)

Topic: A vs B vs C. Mode `auto` — inline banter, spawn real agents when independent thinking changes the outcome.

```
YOU          ROOM A (installed)
 |                    |
 |-- bmad-party-mode --mode auto --------------------------->|
 |                      << in:  --mode auto (no --party → installed roster)
 |                      << in:  memories/installed/.memlog.md (empty first run)
 |             resolve_party.py → roster = Mary, John,
 |             Winston, Sally, Amelia
 |             memory_enabled = true
 |                      >> out: party-mode/memories/installed/.memlog.md  (init)
 |                    |                                      |
 |             welcome: icons, names, one-line roles         |
 |             "what's the thing?"                           |
 |                    |                                      |
 |-- "Video visits. Twilio, our own WebRTC, or Doxy.         |
 |    Pick a lane. Don't be polite." ----------------------->|
 |                    |                                      |
 |   {rounds — one woven conversation, not five memos}       |
 |                    |                                      |
 |   John   : Doxy. We sell a clinic OS, not a video co.     |
 |   Winston: Doxy is a data-gravity leak. If the session    |
 |            lives there, our charting timeline is a lie.   |
 |   Sally  : Twilio looks like a Twilio room. Patients      |
 |            already bounce when the chrome is wrong.       |
 |   Amelia : In-house WebRTC is 6 months of TURN and        |
 |            Safari. I will not on-call that for 11 clinics.|
 |   Mary   : Nobody has asked what "visit completed"        |
 |            means for billing. That's the real product.    |
 |   John   : (snaps) Billing is my problem.                 |
 |   Mary   : Then why is it missing from every option?      |
 |                    |                                      |
 |   ALLIANCE FORMS: Winston + Mary (own the session event)  |
 |   FACTION: John + Amelia (do not become a video company)  |
 |   SALLY is unaligned — she will sink Twilio on UX alone.  |
 |                    |                                      |
 |   memlog appends (silent):                                |  .memlog.md  += clash / alliance / outcome
 |     type=clash    Winston vs John on data gravity         |
 |     type=moment   Mary reframes to "visit completed"      |
 |     type=callback Sally will sink Twilio chrome           |
 |                    |                                      |
 |-- you, in the room: "What if Twilio for media,            |
 |    but we own the session record?" ----------------------->|
 |                    |                                      |
 |   Winston: That is option D. Say it.                      |
 |   Amelia : I can wrap Twilio and write our own            |
 |            VisitSession. I will not write a SFU.          |
 |   John   : Fine. Doxy is dead if we own the event.        |
 |   Sally  : Then the waiting-room chrome is ours. Good.    |
 |                    |                                      |
 |   STILL UNRESOLVED (correct):                             |
 |   cost model (Twilio minutes vs engineering months)       |
 |   and HIPAA BAA surface. Do not bow-tie it.               |
 |                    |                                      |
 |-- you: "park it. I want the waiting room next." --------->|
 |             wrap Room A
 |             << in:  you signaled done
 |             >> out: memories/installed/.memlog.md  (top-up)
 |             >> out: party-mode/2026-06-18-atlas-video-strategy.html  (keepsake accepted)
 |             on_complete (empty)                           |
 v                    v                                      v
```

**Option D is the point of the party.** No single agent would have named "Twilio for media, Atlas owns VisitSession." The clash earned a fourth option.

---

## Sequence 3 — switch rooms mid-thread (waiting-room focus group)

Same skill, different `--party`. Carry the thread: "We are leaning toward Twilio-media + our session. What happens to you?"

```
YOU          ROOM B (waiting-room)
 |                    |
 |-- bmad-party-mode --party waiting-room                    |
 |                   --mode subagent ----------------------->|
 |                      << in:  --party waiting-room --mode subagent
 |                      << in:  "Twilio-media + our session — what happens to you?"
 |                      << in:  Room A takeaway / option D
 |                      << in:  memories/waiting-room/.memlog.md (if any)
 |             each persona thinks independently
 |                      >> out: memories/waiting-room/.memlog.md
 |                    |                                      |
 |   scene plays: fluorescent lobby, 20 minutes late         |
 |                    |                                      |
 |   Dee    : I am on a farm LTE. If this needs a            |
 |            "modern browser" I am driving 90 minutes.      |
 |   Kenji  : I will close the tab if I see a Twilio         |
 |            logo. I already think this is a scam.          |
 |   Omar   : I have 7 minutes between rooms. If join        |
 |            takes more than two taps I will call instead.  |
 |   Priya  : No-show rate is the only number I watch.       |
 |            If grandma can't join, I eat the slot.         |
 |                    |                                      |
 |   CLASH: Kenji wants a magic-link SMS with no app.        |
 |   Omar wants the join button inside the chart, period.    |
 |   They do not agree. Priya sides with Omar because        |
 |   front-desk SMS already fails 1 in 5 rural numbers.      |
 |                    |                                      |
 |   walk-on (open moment): a clinic IT guy, "Vince",        |
 |   leans in from the hallway — "our iPads are stuck        |
 |   on iOS 16." The room keeps him for this scene.          |
 |                    |                                      |
 |   memlog: Dee/LTE, Kenji/trust, Omar/two-taps,            |  .memlog.md  += moments
 |           Vince/iOS16 walk-on                             |
 |                    |                                      |
 |-- you signal done -------------------------------------->|
 |             << in:  you signaled done
 |             >> out: memories/waiting-room/.memlog.md  (top-up)
 |             >> out: party-mode/2026-06-19-waiting-room-panel.html
 |             "Vince isn't in the roster. Save him?"
 |-- yes -------------------------------------------------->|
 |             create-party.md → bmad-customize
 |                      << in:  Vince as he played (code, persona, group)
 |                      >> out: bmad-party-mode.user.toml  (members += vince)
 v                    v                                      v
```

---

## Sequence 4 — Room C: 20-minute red team, then hand off

No memory. Attack option D.

```
YOU          ROOM C (video-red-team)
 |                    |
 |-- bmad-party-mode --party video-red-team                  |
 |                   --mode session  --non-interactive ----->|
 |                      << in:  --party video-red-team --mode session --non-interactive
 |                      << in:  option D (Twilio media + Atlas VisitSession)
 |             (only non-interactive path: run to a          |
 |              natural close, then wrap)                    |
 |                    |                                      |
 |   Nix  : Twilio outage = every clinic dark. Single        |
 |          vendor in the first sentence of the PRD.         |
 |   Pat  : waiting-room on iOS 16 + LTE + a proxy.          |
 |          Dee cannot join. Capability is a lie.            |
 |   Cal  : "VisitSession wrapper" is clever. I want         |
 |          one state machine, no adapter poetry.            |
 |   Prag : We can ship Twilio-media in a sprint if          |
 |          Safari is a known-broken badge, not a blocker.   |
 |                    |                                      |
 |             wrap → takeaways
 |             >> out: (none — you declined the keepsake; memory=false)
 |
 |== HANDOFF OUT OF PARTY MODE ==============================
 |             (party is done; these are other skills)
 |
 |                 bmad-deep-recon  shape=select
 |                      << in:  A vs B vs C vs D (option D now in the set)
 |                      >> out: research/select-twilio-vs-webrtc-vs-doxy.md
 |
 |                 bmad-prd   intent=update
 |                      << in:  existing prd.md + Room A+B+C takeaways
 |                      >> out: planning/prd.md, addendum.md
 |
 |                 bmad-architecture
 |                      << in:  updated prd.md + the living clinic OS
 |                      >> out: planning/ARCHITECTURE-SPINE.md
 |                      (Atlas owns VisitSession; media vendor is an adapter)
 |
 |                 bmad-spec   epic=video-visit
 |                      << in:  updated prd + spine + waiting-room takeaways
 |                      >> out: specs/spec-video-visit/SPEC.md + stories.yaml
 |
 |                 bmad-ux
 |                      << in:  updated prd + Dee/Kenji/Omar constraints
 |                      >> out: planning/DESIGN.md, EXPERIENCE.md
 |
 |                 bmad-build   (first story, human-gated)
 |                      << in:  first story + SPEC.md + brownfield codebase
 |                      >> out: code + impl record
 v                    v
```

---

## Full evening, one picture

```
  author cast                    Room A                 Room B              Room C           handoff
  (create-party)
       |                           |                      |                   |                |
YOU----bmad-party-mode             |                      |                   |                |
       + bmad-customize            |                      |                   |                |
       |                           |                      |                   |                |
       |  .user.toml               |                      |                   |                |
       |                           |                      |                   |                |
       |-------------- party=installed --mode auto ------>|                   |                |
       |                           |  memlog/installed    |                   |                |
       |                           |  keepsake HTML       |                   |                |
       |                           |  OPTION D appears    |                   |                |
       |                           |                      |                   |                |
       |---------------------- party=waiting-room ----------------------->    |                |
       |                           |                 --mode subagent          |                |
       |                           |                 memlog/waiting-room      |                |
       |                           |                 Vince walk-on saved      |                |
       |                           |                 keepsake HTML            |                |
       |                           |                      |                   |                |
       |-------------------------------- party=video-red-team --non-interactive -->|           |
       |                           |                      |           no memlog, no HTML       |
       |                           |                      |                   |                |
       |                           |                      |                   |-- recon select |
       |                           |                      |                   |-- prd update   |
       |                           |                      |                   |-- architecture |
       |                           |                      |                   |-- spec + ux    |
       |                           |                      |                   |-- bmad-build   |
       v                           v                      v                   v                v
```

---

## Modes (so the diagram stays honest)

```
bmad-party-mode --mode <...>

session      one mind voices every persona inline
             default floor; every other mode degrades here

auto         inline for banter; spawn a real agent when
             independent thinking would change the outcome
             (Room A)

subagent     a real agent behind each persona every
             substantive round — use for focus groups
             (Room B)

agent-team   persistent team who address each other
             directly (Claude Code only)

--non-interactive
             the ONLY path that closes when the opening
             intent is served. Otherwise the party runs
             until YOU signal done. (Room C)
```

`--party <id>` / `--group <id>` picks the room. `--list-groups` prints the menu. An inline-named cast is the roster for that session and is ephemeral until you save it.

---

## What the rooms changed in the product (the point)

```
BEFORE THE PARTIES                         AFTER
──────────────────                         ─────
A / B / C as a three-way religious war     Option D: Twilio media + Atlas VisitSession
"pick a video vendor"                      "own the session event; vendor is an adapter"
PRD silent on join UX                      DESIGN.md: magic-link + in-chart join; no vendor chrome
iOS / LTE invisible                        SPEC constraint: iOS 16 + farm LTE is a success case
Doxy still tempting for speed              Doxy killed by data-gravity + Kenji's trust
```

Those outcomes are why you run a party instead of asking one agent "what's best?"

---

## Anti-patterns (party mode)

```
DON'T                                              DO
─────────────────────────────────────────────      ──────────────────────────────────────────
Ask for a balanced panel and a consensus slide     Let factions form; leave clashes unresolved
Use session mode for a customer focus group        --mode subagent so patients don't bleed
Treat the first answered prompt as "we're done"    Keep going until you signal done
Skip memory on a recurring strategy room           memory=true; let grudges come back
Hand-edit customize.toml                           bmad-customize via create-party.md
Dump the memlog into the room                      Distill; play it in character
Run party mode to write the PRD                    Party, then bmad-prd / bmad-spec
Save every walk-on                                 Save Vince; leave the janitor in the memlog
```

---

## Skill coverage for this file

| Skill | Reads | Writes |
|---|---|---|
| `bmad-party-mode` | topic, `--party`/`--mode`, memlog, or interview notes | keepsake HTML, `.memlog.md`, optional custom TOML |
| `bmad-customize` | drafted members/groups | `_bmad/custom/bmad-party-mode.user.toml` |
| `bmad-deep-recon` | A/B/C/D candidates | select-shape research writeup |
| `bmad-prd` | existing `prd.md` + party takeaways | updated `prd.md` |
| `bmad-architecture` | updated PRD + living clinic OS | `ARCHITECTURE-SPINE.md` |
| `bmad-spec` | PRD + spine + waiting-room takeaways | `SPEC.md` + `stories.yaml` |
| `bmad-ux` | PRD + Dee/Kenji/Omar constraints | `DESIGN.md`, `EXPERIENCE.md` |
| `bmad-build` | first story + spec + brownfield codebase | code + impl record |

Agents (`bmad-agent-*`) are the default Room A roster. You do not invoke them separately when party mode is already voicing them.
