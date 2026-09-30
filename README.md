# Agent workflow skills

Four agent skills that take a project from idea to finished product, each step leaving a document the next one builds on:

| Step | Skill | Produces | What happens |
|---|---|---|---|
| 1 | `discovery` | `DISCOVERY.md` | The agent interviews you in rounds and logs every question and answer. |
| 2 | `parallel-research` | `RESEARCH.md` | Parallel sub-agents each research one domain; their findings are compiled verbatim. |
| 3 | `master-plan` | `PLAN.md` | Discovery and research are synthesized into phases and tasks with checkable "done when" criteria. |
| 4 | `progress-tracker` | `PROGRESS.md` | A checklist of every task that the agent updates as it builds, so a fresh session resumes exactly where the last one stopped. |

## Install

Works with Claude Code, Cursor, and other agents supported by the [skills CLI](https://skills.sh):

```bash
npx skills add sebtosca/agent-workflow-skills -g -y
```

Update to the latest version on any machine:

```bash
npx skills update
```

## Use

In a project folder, run the skills in order: `/discovery`, `/parallel-research`, `/master-plan`, `/progress-tracker`.

After a context reset or in a new session, run `/progress-tracker` (or just say "continue"). The skill also adds a pointer to `CLAUDE.md` / `AGENTS.md` so new sessions read `PROGRESS.md` on their own.
