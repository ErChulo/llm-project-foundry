# How to Start a Project with an LLM — Step by Step (v1.0.0)


**Standard version:** `1.0.0`  
**Revision date:** 2026-10-18  
**Canonical filename:** `START-A-PROJECT-STEP-BY-STEP-v1.0.0.md`

### Version control for this living standard

- Use semantic versioning (`MAJOR.MINOR.PATCH`): increment **MAJOR** for incompatible workflow changes or significant reordering; **MINOR** for backward-compatible new steps, skills, or MCP guidance; **PATCH** for corrections and clarifications that do not change the workflow.
- For every revision, update the version in **both the document title and filename**, the metadata above, and the revision history below. Do not silently overwrite an earlier version; retain previous numbered files in Git history or a versioned archive.
- Commit each revision to Git with a descriptive message (example: `docs(standard): release v1.0.1`) and tag releases when useful (example: `project-standard-v1.0.1`).
- Treat version `1.0.0` as the first formally versioned baseline; prior unversioned drafts are historical predecessors, not automatically numbered releases.

### Revision history

| Version | Date | Change |
|---|---|---|
| **1.0.0** | 2026-10-18 | Established the initial versioned standard from the existing project-start guide, including skill and MCP selection, conflict audits, installation guidance, and agent setup. |

---

Follow these in order. Don't skip steps even if they feel small.

---

### STEP 1 — Write down your fuzzy idea, badly, in your own words
Just a few sentences. What is it? Who is it for? What's the one main thing
it does? Don't worry about sounding smart. Bad writing is fine here.

**You do:** type 2-5 sentences somewhere (notes app, chat, whatever).

---

### STEP 2 — Turn the fuzzy idea into a real spec
Take what you wrote in Step 1 and paste it into an LLM chat (like this one).
Ask it to turn your idea into:
- a clear project description
- a list of core features (keep it small — 3 to 6 things max)
- a suggested tech stack
- a suggested folder structure

**You say something like:**
> "Here's my rough idea: [paste Step 1]. Turn this into a clear spec: what
> it does, core features, a tech stack, and a folder structure. Keep it
> simple, this is a first version."

**You get:** a written plan. Save it somewhere (a doc, a note).

---

### STEP 3 — Ask the LLM for two more small files
Same conversation. Ask for:
- `DESIGN.md` — only if your project has any screen/UI a person will look at.
  Colors, fonts, spacing, vibe.
- `ARCHITECTURE.md` — the important rules that must never be broken (e.g.
  "always store money as whole cents, never decimals").

**You say something like:**
> "Now write me a DESIGN.md with basic colors/fonts/spacing, and an
> ARCHITECTURE.md with the 3-5 most important rules this project must
> always follow."

**You get:** two more short files.

---

### STEP 3A-0 — Establish a reusable baseline of general-purpose skills
Before choosing specialist skills, consider this **standard candidate set** for
software projects. These are widely reusable, but installation is not a
requirement for every project: verify source, permissions, compatibility and
usefulness first. **Do not indiscriminately install every skill in a repository.**

Prerequisite: Node.js/npm (`npx`) and a coding agent that supports agent
skills. Run these commands in a terminal; execute project-local installs
**inside the project directory after Step 5**. At this planning stage,
record the commands and have the scaffold optionally execute them after
review. `--skill` selects specific skills; omit `-g` to install for the
current project. To list choices before installation:

```bash
npx skills@latest add mattpocock/skills --list
npx skills@latest add vercel-labs/skills --list
```

**Suggested baseline installs (individual commands):**

```bash
# 1. Structured requirements interview with persistent project documentation
npx skills@latest add mattpocock/skills --skill grill-with-docs

# 2. Discover additional task-specific skills
npx skills@latest add vercel-labs/skills --skill find-skills

# 3. Product requirements document -> actionable issues (two skills)
npx skills@latest add mattpocock/skills --skill to-prd --skill to-issues

# 4. Review and improve an existing codebase's architecture
npx skills@latest add mattpocock/skills --skill improve-codebase-architecture

# Required setup for Matt Pocock's engineering workflow
npx skills@latest add mattpocock/skills --skill setup-matt-pocock-skills
```

**Correct usage and timing:**

| Skill | Invoke / use when | Expected result |
|---|---|---|
| `grill-me`, `grilling` | Optional simpler interview before the specification; no persistent artifacts needed | Questions that expose missing decisions; it is an **alternative**, not an additional mandatory pass |
| `grill-with-docs` | Before finalizing the specification, preferably once a repository/workspace exists; it may write `CONTEXT.md` and architectural decision records (ADRs) | Clarified requirements and stable terminology; use this **instead of** the simpler grill when you want written decisions |
| `find-skills` | During Steps 3A/3B and whenever a new capability is needed | Identified candidate skills with verified provenance, **not automatic approval to install** |
| `to-prd` | After the interview and scope decisions are settled; use to finalize/revise Step 2's spec | Product Requirements Document (PRD) |
| `to-issues` | Only after the PRD or plan is approved and issue-tracker setup is complete | Reviewable, independently implementable issues; approve before publishing |
| `improve-codebase-architecture` | After meaningful code exists, or while assessing an inherited codebase; not a substitute for initial `ARCHITECTURE.md` | Architecture improvement proposals/refactoring issues; review before implementation |
| `setup-matt-pocock-skills` | Once per repository after installation and initial project setup | Issue tracker, labels and domain-document configuration for downstream skills |

Use `/grill-with-docs`, `/to-prd`, `/to-issues`,
`/improve-codebase-architecture`, or `/setup-matt-pocock-skills` inside an
agent **only if that agent supports slash-command invocation**; otherwise ask
the agent to invoke the installed skill by name. Start with
`/setup-matt-pocock-skills` after the repository has been created, before
`to-issues` or architecture issue publishing. You can run the initial
interview before scaffolding with `grill-me`, or revisit the original draft
spec through `grill-with-docs` after scaffolding.

**Important naming correction:** `prd-to-issues` describes a workflow, but
`mattpocock/skills` currently exposes **`to-prd` and `to-issues`** as two
separate skills. Do not try `--skill prd-to-issues` unless a repository actually
lists that exact name. `grill-me`, `grilling`, and `grill-with-docs` likewise
are not three mandatory successive activities.

**Optional:** If you specifically prefer the standalone interview, install
`grill-me` instead of (or alongside, with distinct invocation rules)
`grill-with-docs`:

```bash
npx skills@latest add mattpocock/skills --skill grill-me
```

**Installation and security notes:** The broad command
`npx skills@latest add mattpocock/skills` opens a selection flow; it does
**not** mean all skills are necessary. Check the selected target agent,
installation scope, source contents and transitive commands before approving.
Avoid installing the same skills through both a managed agent plugin and
`npx skills` without checking for duplicate instructions. Once installed,
verify the agent can discover and invoke each required skill. Record sources
and tested revisions in `SKILLS.md` and include all baseline selections in
the conflict audit in Step 3C. If source revisions are not pinned, re-audit
when upgrading.

**Primary references:**
- https://www.skills.sh/mattpocock/skills
- https://www.skills.sh/vercel-labs/skills/find-skills
- https://www.npmjs.com/package/skills

---

### STEP 3A — List the skills YOU want to include
Include the Step 3A-0 baseline candidates you want, then list any other
skills you have already chosen. Include their names and installation sources (links, repositories, or
local paths), if known. Your choices take precedence over recommendations.

**You say something like:**
> "These are the skills I want this project to use: [names and sources].
> Keep them in the plan. Check whether they work with my coding agent and
> project, and flag any conflicts instead of silently replacing them."

**You get:** a list of required, user-selected skills (including any approved
baseline skills) and any compatibility questions to resolve.

---

### STEP 3B — Ask the LLM to recommend any additional skills
Give the LLM your project spec, `DESIGN.md`, `ARCHITECTURE.md`, tech stack,
and your user-selected skill list. Ask for only complementary skills, not
duplicates. Recommendations are optional; you decide which to accept.

**You say something like:**
> "Given these project documents and my required skills, suggest any
> additional skills that would materially help. Explain each skill's role,
> installation source, and compatibility. Do not replace my selected skills."

**You get:** one approved skill list combining your selections and any
recommendations you accept.

---

### STEP 3C — Detect and resolve skill conflicts BEFORE installation
Review **all user-selected and LLM-recommended skills together**, before
installing anything. Read each skill's actual documentation and configuration
when available; distinguish verified findings from possible risks.

Check for duplicate or overlapping functionality, contradictory instructions,
incompatible versions or dependencies, competing configuration files, unsafe
permissions, and conflicting workflows. Classify findings as **Critical**,
**Moderate**, or **Minor**. For each, explain the impact and a proposed
resolution: retain both with distinct responsibilities, establish precedence,
configure coexistence, change versions, replace, or remove a skill.

**You say something like:**
> "Audit all my selected and recommended skills before installation.
> Inspect their documentation and dependencies where accessible. Find
> duplicates, overlapping responsibilities, contradictory instructions,
> dependency/version conflicts, configuration collisions, permission risks,
> and workflow conflicts. For each finding, report Skill A, Skill B,
> evidence, severity (Critical/Moderate/Minor), consequences, and a
> recommended resolution. Give my selected skills precedence unless doing
> so creates a technical or security problem. Do not silently discard or
> substitute a required skill. Flag anything unverified. Save the findings
> in SKILL_CONFLICT_REPORT.md. If none are detected, explicitly write
> 'No skill conflicts detected' and state what was actually checked.
> Do not install any unresolved critically conflicting skills; ask for my
> decision when the alternatives affect my requirements."

Use this report structure:

| Skill A | Skill B | Conflict and evidence | Severity | Resolution | Status |
|---|---|---|---|---|---|
| Example X | Example Y | Contradictory instructions | Critical | Set explicit precedence | Pending |

**You get:** `SKILL_CONFLICT_REPORT.md`, with the conflict matrix,
proposed resolutions, unresolved decisions, and verification limitations.
Proceed only after critical conflicts are resolved or excluded from setup.

---

### STEP 3D — Plan installation, then install and verify the skills
Ask the LLM for the correct installation method **for your actual coding
agent** and each approved skill. Verify sources and commands before running
them. Some skills install globally; others live inside a project folder.

- Install global or user-level skills now, if appropriate.
- For project-local skills, add their reviewed installation steps and configuration
  to the scaffold script in Step 4, and execute them after creating the
  project folder in Step 5. For baseline skills, use the exact commands from
  Step 3A-0 only after passing Step 3C.
- Check that each skill is discoverable by the agent and that there are no
  conflicts or duplicate installations.
- Record skill names, sources, versions or revisions (when available),
  installation scope, and purpose in `SKILLS.md`. Do not commit secrets.

**You say something like:**
> "Prepare exact installation and verification instructions for every
> approved skill. Put project-local setup into scaffold.sh, create a
> SKILLS.md manifest, and show me how to verify the agent finds them."

**You get:** verified installation steps and a reproducible skills manifest.

---

### STEP 3E — List the MCP servers YOU want to include
List your required Model Context Protocol (MCP) servers before requesting
recommendations. Provide server names, official sources, and the tools or
data each must expose. Your choices take precedence, subject to security
and compatibility checks.

**You say something like:**
> "These MCP servers are required: [names and sources]. Keep them in the
> plan, identify what each connects to, and flag incompatible or unsafe
> configurations rather than silently replacing them."

**You get:** an explicit user-selected MCP server list.

---

### STEP 3F — Ask the LLM to recommend additional MCP servers
Using the project spec, agent, environment, and required servers, request
only complementary MCP servers. Do not install recommendations without
approval. Distinguish MCP servers (external tools and data connections) from
skills (instructions or workflows).

**You say something like:**
> "Recommend any additional MCP servers that materially support this
> project. Give the purpose, official source, required permissions, and
> whether they duplicate tools I already selected. Wait for my approval."

**You get:** an approved combined MCP server list.

---

### STEP 3G — Detect and resolve MCP server conflicts BEFORE configuration
Review all **user-selected and LLM-recommended MCP servers** as a single
set before configuring them. Check overlapping tools or tool names,
conflicting server identifiers, ports, transports, dependencies, runtime
requirements, authentication schemes, permission scopes, competing writes to
the same resources, and insecure or excessive access. Distinguish actual
conflicts from benign functional overlap.

**You say something like:**
> "Audit my approved MCP servers and their interaction with the coding
> agent before configuration. Identify duplicate or colliding tools,
> server names, ports, transports, version and dependency conflicts,
> authentication and permission risks, and conflicting access to the
> same files or services. For every issue, give both server names,
> supporting evidence, severity (Critical/Moderate/Minor), consequences,
> and a safe resolution. Preserve my required servers when practical;
> flag any substitution or removal for my decision. Create
> MCP_CONFLICT_REPORT.md with a conflict matrix, verification status,
> and unresolved items. If no conflicts are detected, explicitly write
> 'No MCP conflicts detected' and describe the scope of checking.
> Do not enable servers with unresolved critical conflicts."

Use a matrix with columns **Server A**, **Server B**, **Conflict and
evidence**, **Severity**, **Resolution**, and **Status**. Do not claim
compatibility was tested merely because configuration appears valid.

**You get:** `MCP_CONFLICT_REPORT.md`, with safe resolutions and open
decisions before server configuration.

---

### STEP 3H — Configure, secure, and verify the MCP servers
For each approved server, determine whether it runs locally or remotely,
which MCP transport and client configuration your agent supports, and
whether installation is user-global or project-local.

- Verify the official source and pin versions or revisions when feasible.
- Grant only necessary tools, scopes, and filesystem/network access.
- Configure authentication using environment variables or a secret manager;
  never store tokens, API keys, or passwords in Git.
- Configure global servers now when suitable. Put reproducible project-local
  installation and **non-secret** configuration into Step 4's scaffold.
- After Step 5, test each server's connection and tool discovery from the
  actual coding agent; a configuration file alone is not proof it works.
- Record the server, source, revision, scope, capabilities, required secret
  variable names (not values), and verification procedure in `MCP_SERVERS.md`.

**You say something like:**
> "Generate a safe configuration and a verification checklist for the
> approved MCP servers for my specific coding agent. Include project-local
> setup in scaffold.sh, document them in MCP_SERVERS.md, and keep all
> credentials out of the repository."

**You get:** a reproducible MCP configuration plan and verification checklist.

---

### STEP 4 — Ask the LLM for a script that builds the folders
Don't build the folders by hand. Ask for a script that does it.

**You say something like:**
> "Write me a bash script called scaffold.sh that creates the folder
> structure you suggested, plus empty starter files (package.json, a
> basic entry file, README.md, and the DESIGN.md and ARCHITECTURE.md
> content from before). Also include SKILLS.md, MCP_SERVERS.md, the approved
> project-local skills setup from Step 3D, and non-secret MCP configuration
> from Step 3H. Also create SKILL_CONFLICT_REPORT.md and
> MCP_CONFLICT_REPORT.md using the audit results from Steps 3C and 3G. Do not embed credentials or unverified commands."

**You get:** one script file.

---

### STEP 5 — Actually create the project folder and run the script
On your computer:

```bash
mkdir my-project
cd my-project
# paste in scaffold.sh here
bash scaffold.sh
```

**You get:** a real folder with real files in it — not empty, not fake.
Verify project-local skills and MCP server connections/tool discovery before
moving on. Run `/setup-matt-pocock-skills` in your agent (if installed) once
repository context exists; run `grill-with-docs` now if documenting the
interview and revise the spec/architecture accordingly. Keep local secret files out of Git (for example, in `.gitignore`).

---

### STEP 6 — Save your work with git, right away
```bash
git init
git add .
git commit -m "initial scaffold"
```
Do this now, before anything else. This is your save point.

---

### STEP 7 — Start your coding agent and run its init command
Open your coding agent (OpenCode, Claude Code, whichever you use) inside
that folder, and run its version of `/init`.

**What this does:** it looks at the real files you just made and writes
`AGENTS.md` (or your agent's equivalent) — instructions future AI sessions
can read so you don't have to re-explain your project every time. Check that
the agent discovers your installed skills and configured MCP server tools;
`/init` alone may not load or connect them.

---

### STEP 8 — Link the files together
Open `AGENTS.md` and add one line near the top:

```
See DESIGN.md for UI rules, ARCHITECTURE.md for system rules, and
SKILLS.md for approved skills and when to use them;
MCP_SERVERS.md for approved MCP tools, their permissions, and setup;
and SKILL_CONFLICT_REPORT.md and MCP_CONFLICT_REPORT.md for conflict decisions.
```

This way, whichever file the agent reads first, it knows the others exist.
Also specify in `AGENTS.md` when each installed skill should be used, without
copying its entire instructions into that file. Include any approved
precedence rules, tool ownership boundaries, and unresolved limitations. Reference MCP tools by
capability, and avoid placing connection secrets in agent instructions.

---

### STEP 9 — Commit again
```bash
git add .
git commit -m "agent context files added"
```

---

### STEP 10 — Start building, and re-run init later
Before building, complete the selected planning chain if not already done:
`grill-with-docs` (or `grill-me`) → `to-prd` → user approves PRD →
`to-issues` → user reviews issues → implement. Use
`improve-codebase-architecture` when existing code merits architectural
review, rather than automatically running it on an empty scaffold.

Now start actually building features with your coding agent.

Every so often (like once a week, or after a big change), re-run `/init`.
Review `AGENTS.md` after re-running `/init` so it still references
`DESIGN.md`, `ARCHITECTURE.md`, `SKILLS.md`, `MCP_SERVERS.md`, and both
conflict reports; some
agents may overwrite manual additions. Recheck skill discovery and MCP
server connectivity after major configuration changes. **Repeat Steps 3C
and 3G whenever skills or MCP servers are added, removed, or updated**,
and revise both conflict reports before committing configuration changes.

---

## The whole thing in one sentence
**Idea → draft spec → DESIGN.md/ARCHITECTURE.md → baseline skill candidates → your chosen skills → optional
LLM-recommended skills → skill conflict audit → approved skills setup →
your chosen MCP servers → optional LLM-recommended MCP servers → MCP conflict
audit → secure MCP configuration → scaffold (including SKILLS.md,
MCP_SERVERS.md, and both conflict reports) → verify → git commit → agent `/init`
→ link files and verify skills/MCP tools → git commit → build and periodically
review agent context.**