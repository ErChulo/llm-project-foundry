# Project Bootstrap Kit — Universal Agent Context Layer

A checklist + directory template for starting any project so that any coding
agent (OpenCode, Claude Code, Codex, Cursor, Copilot, Aider, Windsurf, ...)
has everything it needs without re-deriving it every session.

Layers are ordered by how directly they should feed into a fresh scaffold.
Not every project needs all seven — see "Minimum viable set" at the bottom.

---

## 1. `AGENTS.md` — operational, cross-tool standard
**Answers:** "How do I build, test, and work in this repo?"
Build/lint/test commands, code style, PR/commit rules, files never to touch.
Read natively by 30+ agents (Codex, Claude Code via import, Copilot, Cursor,
Gemini CLI, Jules, Factory, Aider, Zed, VS Code, Windsurf). If you maintain
only one file, make it this one. Generated/updated by `/init` in OpenCode
and equivalents elsewhere — but only as good as the code it can observe, so
scaffold real files before running it.

## 2. `DESIGN.md` — visual/UI design system
**Answers:** "How should things look?"
YAML front matter with hard tokens (hex colors, font stack, spacing scale,
radii) + prose on when/why to use them. Prevents drift across sessions
(button radius changes screen to screen, "secondary gray" isn't the same
gray twice). Only relevant if the project has a UI.

## 3. `ARCHITECTURE.md` — system invariants and rationale
**Answers:** "Why is the code shaped this way?"
The decisions that are invisible in the code itself: "money is always an
integer of minor units," "no external calls inside a DB transaction," "the
Account aggregate is the only writer to the ledger." Without this, an agent
sees the pattern once and cheerfully adds a second, conflicting way to do
the same thing.

## 4. `skills/` — reusable procedures (SKILL.md per skill)
**Answers:** "How do we always do X, regardless of which project this is?"
Portable, loaded on demand: agent reads name + description first, only
pulls in the full body when a task matches. Good candidates: "how we write
a migration," "how we cut a release," "how we name commits." Distinct from
AGENTS.md — AGENTS.md is project-scoped, skills are capability-scoped and
meant to move between projects.

```
skills/
  writing-migrations/SKILL.md
  release-checklist/SKILL.md
  commit-conventions/SKILL.md
```

## 5. `schemas/` — data contracts
**Answers:** "What exact shape does this data have?"
Not a named industry convention like the others — just good practice.
Actual `.schema.json` files for API payloads, config shapes, or structured
LLM output. More precise than prose; agents can validate against it instead
of inferring field names from examples and getting them subtly wrong.

```
schemas/
  api/create-order.schema.json
  config/opencode.schema.json
```

## 6. `scripts/scaffold.sh` — the scaffolding script itself
**Answers:** "How do I regenerate or extend this structure?"
Commit the script that built the directory tree, not just its output.
Lets you rerun or extend the scaffold later without renegotiating the
whole structure with an LLM from scratch. Reference it from AGENTS.md
so agents know it exists and don't hand-roll a competing one.

## 7. `README.md` — human-facing, not agent-facing
**Answers:** "What is this and why does it exist?"
Keep this for humans skimming GitHub. Agents get their instructions from
the files above; don't overload the README with agent directives — that's
what AGENTS.md is for.

---

## Directory layout

```
project-root/
├── README.md
├── AGENTS.md              # points to DESIGN.md, ARCHITECTURE.md, skills/
├── DESIGN.md               # if UI exists
├── ARCHITECTURE.md
├── schemas/
│   └── ...
├── skills/
│   └── <skill-name>/SKILL.md
├── scripts/
│   └── scaffold.sh
└── src/
    └── ...
```

## Minimum viable set

- Tiny script / prototype: `README.md` + `AGENTS.md` only.
- Anything with a UI a client will see: add `DESIGN.md`.
- Anything with non-obvious invariants (money, concurrency, external
  integrations): add `ARCHITECTURE.md`.
- Anything you'll build more than once, or a team will touch: add
  `skills/` and `schemas/`.

## Workflow

1. Draft the idea into a spec with an LLM. Same pass, ask it to draft
   `DESIGN.md` and `ARCHITECTURE.md` stubs (skip DESIGN.md if no UI).
2. Generate `scripts/scaffold.sh` from that spec; run it to build the tree,
   including a few real files (package.json / pyproject.toml / entry
   point) — not just empty folders. Commit.
3. `cd` in, run the coding agent, run `/init` (or equivalent) to generate
   `AGENTS.md` from what actually exists on disk.
4. Edit `AGENTS.md` to add one line pointing at each other file:
   "See DESIGN.md for UI conventions, ARCHITECTURE.md for system
   invariants, skills/ for reusable procedures."
5. Commit everything. Re-run `/init` periodically as the codebase
   diverges from the original scaffold — it updates in place rather
   than overwriting blindly.