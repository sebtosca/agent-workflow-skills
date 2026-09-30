---
name: agents-md
description: "Write or update AGENTS.md, the instruction file that tells AI coding agents how to understand and work on this project: commands, conventions, rules, and where the project docs are. Use after ARCHITECTURE.md exists, when commands or conventions change, or when the user asks to set up agent instructions."
---

# AGENTS.md

Write `AGENTS.md` at the project root: a README for coding agents. Cursor, Codex, Copilot, and other agents read it automatically. Claude Code reads it too, as long as the project has no `CLAUDE.md` (step 5 handles that case).

The file is loaded into the agent's context on **every turn**, so each line has to earn its place. Aim for under 150 lines. Include what an agent would get wrong without being told. Leave out what it can find by looking: directory listings, dependency lists, and architecture explanations belong in ARCHITECTURE.md, and AGENTS.md just points there.

## Steps

1. **Read `PRD.md`, `ARCHITECTURE.md`, `THREAT-MODEL.md`, `SECURITY.md`, `PLAN.md`, `RESEARCH.md`, and `CONTEXT.md`**, and the repo's config files (`package.json`, `pyproject.toml`, `Makefile`, CI config) if they exist. If ARCHITECTURE.md is missing, point the user to the `architecture` skill.
2. **Get the commands exactly right.** Take them from config files when they exist. Before any code exists, write the commands the chosen stack will use (from RESEARCH.md) and mark them `(planned)`. The `progress` skill replaces them once they're real.
3. **Copy the security rules.** Put THREAT-MODEL.md's Always / Ask first / Never rules into the Security section word for word; agents act on rules they can see every turn. If THREAT-MODEL.md is missing, point the user to the `threat-model` skill.
4. **Write `AGENTS.md` from the template below.** Make every instruction concrete enough to check: "Run `pnpm test` before committing", not "test your changes". Explain the *why* of any rule that isn't obvious; agents follow rules better when they understand them.
5. **Connect Claude Code.** If a `CLAUDE.md` or `.claude/CLAUDE.md` exists, make sure its first line is `@AGENTS.md`, which imports the file. Otherwise Claude Code ignores AGENTS.md. If neither exists, don't create one: Claude Code reads AGENTS.md directly.
6. **Tell the user** the next step is the `progress` skill, which sets up PROGRESS.md and starts building.

## Template

```markdown
# AGENTS.md

<Project name>: <one sentence on what it is and who it is for>.

## Project docs
Read these before large changes; don't duplicate them here.
- `PRD.md`: what we are building and why (user stories US-n)
- `ARCHITECTURE.md`: structure, codemap, invariants
- `THREAT-MODEL.md`: threats (TM-nnn) and mitigations; read before touching auth, input handling, data storage, or external services
- `SECURITY.md`: public vulnerability-reporting policy
- `PLAN.md`: phases and tasks (IDs like P2.T4)
- `PROGRESS.md`: current state of the build
- `CONTEXT.md`: glossary; use these terms in code, comments, and commits
- `docs/adr/`: decision records; don't reverse one without asking
- `DISCOVERY.md`, `RESEARCH.md`: original decisions and research

## Implementation progress
This project is being built from PLAN.md. At the start of every session, and after any context reset, read PROGRESS.md and continue from its "Current state" using the `progress` skill. Update PROGRESS.md after every task.

## Commands
- Install: `...`
- Dev server: `...`
- Test (all / one file): `...` / `...`
- Lint + typecheck: `...`
- Build: `...`

## Conventions
<Only what differs from the language's or framework's defaults, or what an agent would guess wrong: naming, file placement for new features, state management, error handling, how data is fetched.>

## Rules
<Hard rules with a one-line reason each. Include the key invariants from ARCHITECTURE.md, e.g. "UI components never import from `db/`: all data goes through `api/`.">

## Security
<THREAT-MODEL.md's Always / Ask first / Never rules, copied verbatim. For security-sensitive work, also use the `security-and-hardening` skill if it is installed.>

## Testing
<Where tests live, what must have tests, how to run a single test, what "done" requires (tests + lint + typecheck green).>

## Git workflow
<Branch naming, commit message format (include the task ID: `P2.T4: add login form`), commit after every task.>

## Gotchas
<Non-obvious things that waste time: env setup, flaky services, version pins, and why. Add a line whenever an agent makes the same mistake twice.>
```
