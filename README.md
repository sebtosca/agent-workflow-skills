# Agent workflow skills

Seven agent skills that take a project from idea to finished product, each step leaving a document the next one builds on:

| Step | Skill | Produces | What happens |
|---|---|---|---|
| 1 | `discovery` | `DISCOVERY.md` (+ `CONTEXT.md`, `docs/adr/`) | Matt Pocock's `grill-with-docs`, plus a log: the agent grills you in rounds, logs every question and answer, and keeps a glossary and decision records as it goes. |
| 2 | `research` | `RESEARCH.md` | Parallel sub-agents each research one domain; their findings are compiled verbatim. |
| 3 | `plan` | `PLAN.md` | Discovery and research are synthesized into phases and tasks with checkable "done when" criteria. |
| 4 | `prd` | `PRD.md` | What you're building, who it's for, and what it must do: goals, non-goals, metrics, and user stories with Given/When/Then acceptance criteria, each mapped to plan tasks. Template adapted from Matt Pocock's `to-spec`. |
| 5 | `architecture` | `ARCHITECTURE.md` | How the app is structured and how its systems connect: codemap, data flow, boundaries, invariants. Follows matklad's ARCHITECTURE.md convention. |
| 6 | `agents-md` | `AGENTS.md` | Instructions every coding agent reads on every turn: commands, conventions, rules, and pointers to the other docs. Links itself into `CLAUDE.md` if one exists. |
| 7 | `progress` | `PROGRESS.md` | A checklist of every task that the agent updates as it builds, so a fresh session resumes exactly where the last one stopped. |

## Requirements

`discovery` uses Matt Pocock's `grilling` and `domain-modeling` skills when they're installed, and falls back to its own interview format when they aren't:

```bash
npx skills add mattpocock/skills --skill grilling domain-modeling -g -y
```

## Install

Works with Claude Code, Cursor, and other agents supported by the [skills CLI](https://skills.sh). The repo is private, so each machine must be logged in to GitHub first (`gh auth login`):

```bash
npx skills add sebtosca/agent-workflow-skills -g -y
```

Update to the latest version on any machine:

```bash
npx skills update
```

## Use

In a project folder, run the skills in order: `/discovery`, `/research`, `/plan`, `/prd`, `/architecture`, `/agents-md`, `/progress`.

Steps 4–6 may append tasks to `PLAN.md` when they find gaps. Task IDs are never renumbered, so every reference stays valid.

After a context reset or in a new session, run `/progress` (or just say "continue"). `AGENTS.md` tells every new session to read `PROGRESS.md` first. While building, the agent keeps `ARCHITECTURE.md` and `AGENTS.md` in sync with the code and checks each finished phase against the PRD's acceptance criteria.
