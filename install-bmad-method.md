# What it takes to install BMad Method

Short guide for putting [BMad Method](https://docs.bmad-method.org/) into a project and connecting it to Cursor, Claude Code, or another supported AI coding tool.

Official source of truth (flags and tool IDs change): [How to Install BMad](https://docs.bmad-method.org/start/install-bmad/). After install, use [`bmad-skill-decision-diagrams.md`](bmad-skill-decision-diagrams.md) to pick skills.

---

## Prerequisites

| Need | Version / notes |
|---|---|
| [Node.js](https://nodejs.org) | **20.12+** — required for the installer (`npx`) |
| npm / npx | Ships with Node |
| [Python](https://www.python.org) | **3.10+** (some docs say 3.11+ for rendered skills) |
| [uv](https://docs.astral.sh/uv/) | Required for skills that run Python scripts. A missing-`uv` warning does **not** stop install, but those skills will not work until you add it |
| Git | Needed to clone official / custom modules |
| An AI coding tool | Cursor, Claude Code, Windsurf, etc. Run `npx bmad-method install --list-tools` for current IDs |

Install `uv` (macOS / Linux):

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Confirm versions:

```bash
node -v          # v20.12.0 or newer
python3 --version
uv --version
git --version
```

---

## What you get

The installer writes two kinds of things into **the project directory** (not a global app):

```
your-project/
├── _bmad/                 shared config, module sources, scripts
│   ├── core/
│   ├── bmm/               if you selected the Agile suite
│   ├── _config/
│   └── custom/            your overrides later (via bmad-customize)
├── .agents/skills/        Cursor skill copies (if you selected cursor)
├── .claude/               Claude Code skills/commands (if you selected claude-code)
└── _bmad-output/          created as skills run (planning, party keepsakes, …)
```

Skills are copied into each selected tool’s skill directory. `_bmad/` is the shared backend those skills call.

This repo’s diagrams assume **core + BMM** (the Agile suite): Mary, John, Winston, Sally, Amelia, plus the plan/ship workflows.

---

## Install (interactive) — recommended first time

1. `cd` into the project (new or existing).
2. Run:

```bash
npx bmad-method install
```

3. Follow the prompts: modules, config, AI tools.
4. Wait for **BMAD is ready to use!** and note the path plus any warnings.

Typical first-time choices:

- **Modules:** BMad Method / BMM (Agile suite). Core is added automatically.
- **Tools:** `cursor` if you work in Cursor; `claude-code` if you work in Claude Code. You can select more than one.
- **Languages / output folder:** defaults are fine until you have a team convention.

---

## Install (headless) — CI or “just do it”

Flags can change. Check the live list first:

```bash
npx bmad-method install --help
npx bmad-method install --list-tools
```

**Cursor, current directory, BMM:**

```bash
npx bmad-method install \
  --directory . \
  --modules bmm \
  --tools cursor \
  --yes
```

**Claude Code** (from the official first-change tutorial):

```bash
npx bmad-method install \
  --directory . \
  --modules bmm \
  --tools claude-code \
  --yes
```

**Both tools:**

```bash
npx bmad-method install --yes --modules bmm --tools cursor,claude-code
```

`--modules` is the **exact set you want kept**, not a delta. Core is auto-added. `--yes` / `-y` skips prompts and takes flag values plus defaults. Fresh `--yes` installs need `--tools`.

---

## Verify

From the project that contains `_bmad/`:

1. Open the project in the tool you selected.
2. Invoke **`bmad-help`** (Cursor: `/bmad-help` or ask for the skill).
3. Ask what to do next.

If the tool runs the skill, integration is ready.

Brownfield next step: `bmad-project-context` (see [use-case 07](use-cases/07-brownfield-mealplan-course-correct.md)).  
Small known change: `bmad-build`.  
Lost: stay in `bmad-help`.

---

## Update or change the install

From the same project:

```bash
npx bmad-method install
```

The installer detects `_bmad/` and offers **update** (refresh in place) or **modification** (modules, tools, config).

Coming from an older BMad: remove stale `bmad-*` entries in legacy command directories so the tool does not show duplicate commands.

---

## Prerelease

```bash
npx bmad-method@next install
```

Use the same `@next` tag for `--help` / `--list-tools` if that is what you installed. Prefer stable (`npx bmad-method install`) for ordinary project work.

---

## Extra modules

Official extras (Builder, Test Architect, Loop, Game Dev, Creative Intelligence, …) are picked in the same installer. See [Add Modules](https://docs.bmad-method.org/customize/add-modules/).

Custom / community source:

```bash
npx bmad-method install \
  --directory . \
  --modules bmm \
  --custom-source https://github.com/org/my-bmad-module \
  --tools cursor \
  --yes
```

`--custom-source` without `--modules` installs only core + that custom module. The installer warns that third-party Git modules are unverified.

---

## Common gotchas

| Symptom | What to do |
|---|---|
| Installer warns about `uv` | Install `uv`; skills that shell out to Python will fail until you do |
| Cursor does not see skills | Reopen the project from the directory that contains `_bmad/` and `.agents/skills/` |
| Duplicate `bmad-*` commands | Leftover legacy command files from an older install — delete the stale copies |
| Skills feel “empty” or scripts fail | `_bmad/` is missing or incomplete; rerun the installer as **update** |
| Want team rules (always pnpm, …) | Do **not** edit installed files. Use `bmad-customize` → `_bmad/custom/` |
| Planning on ChatGPT / Gemini | [Web bundles](https://bmadcode.com/web-bundles/) — then bring artifacts into the IDE to build |

---

## After it is installed

| Goal | Open |
|---|---|
| Which skill, when | [`bmad-skill-decision-diagrams.md`](bmad-skill-decision-diagrams.md) |
| Recommended models | [§2a](bmad-skill-decision-diagrams.md#2a-recommended-models-agents-and-skills) |
| Greenfield walkthrough | [use-case 01](use-cases/01-greenfield-mealplan-ai.md) |
| Brownfield + late course-correct | [use-case 07](use-cases/07-brownfield-mealplan-course-correct.md) |
| Official first change | [Build your first change](https://docs.bmad-method.org/start/build-your-first-change/) |

---

## Official links

- [Install BMad](https://docs.bmad-method.org/start/install-bmad/)
- [GitHub: bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD)
- [npm: bmad-method](https://www.npmjs.com/package/bmad-method)
- [Add modules](https://docs.bmad-method.org/customize/add-modules/)
- [Customize BMad](https://docs.bmad-method.org/customize/customize-bmad/)
