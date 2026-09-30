---
name: plan
description: "Synthesize DISCOVERY.md and RESEARCH.md into PLAN.md, a fully scoped plan of phases and tasks that takes the product from empty repo to finished goal. Use after research is done, or when the user asks for the build plan."
---

# Master plan

Write `PLAN.md`: the complete route from the current state to the finished product, broken into phases and tasks. An agent with fresh context should be able to pick up any single task and build it using only PLAN.md, the two source documents, and the repo.

## Steps

1. **Read `DISCOVERY.md` and `RESEARCH.md` in full**, plus `CONTEXT.md`, `docs/adr/`, and the existing code if there is any. ADRs are settled: plan within them, and ask the user before contradicting one. Name tasks with CONTEXT.md terms. If either document is missing, stop and point the user to the `discovery` or `research` skill.
2. **Settle conflicts first.** Where research contradicts a discovery decision, or leaves a choice open, ask the user before planning around it. Record each resolution in the plan's Decisions section.
3. **Draft the phases.** Each phase ends in something working and demonstrable: a running skeleton, then the first real feature end to end, then more features, then hardening and launch. Order them by dependency. Build the riskiest and most foundational parts early.
4. **Break every phase into tasks.** A task is one focused change that one agent session can finish and verify. Every task needs all the fields in the template. Be exhaustive: a large product has dozens of tasks, and a plan that stops at the MVP leaves the end goal unplanned. Include setup, tests, deployment, and docs tasks. They are real work.
5. **Check coverage.** Every feature and requirement in DISCOVERY.md must map to at least one task. List anything deliberately left out under Out of scope.
6. **Show the user the phase list with task counts** and adjust it based on their feedback. Then tell them the next step is the `prd` skill. The PRD, ARCHITECTURE, THREAT-MODEL, and AGENTS steps may append tasks to this plan, which is why task IDs are never renumbered.

## Task rules

- IDs are stable: `P<phase>.T<task>` (e.g. `P2.T4`). PROGRESS.md and commits refer to them, so never renumber. To add a task later, append it with the next free number.
- **Source** cites where the task comes from: `DISCOVERY Q12`, `RESEARCH §3`.
- **Done when** is checkable: a command that passes, a screen that shows X, a test that is green. "Works well" does not count.
- Name concrete files, libraries, and versions from RESEARCH.md. The implementer should not have to research again.

## PLAN.md template

```markdown
# Plan: <project name>

Generated: <YYYY-MM-DD> from DISCOVERY.md + RESEARCH.md

## Goal
<What the finished product is and does, in one paragraph.>

## Tech stack
<Each choice, with its RESEARCH section.>

## Architecture overview
<Main components and how data flows between them; a short diagram if useful.>

## Decisions
<Choices made during planning, including resolved conflicts.>

## Out of scope
<Deliberately excluded, with the reason.>

## Phases at a glance
| Phase | Name | Outcome | Tasks |
|---|---|---|---|

---

## Phase 1 — <name>
**Outcome:** <what works at the end of this phase>
**Depends on:** <earlier phases or none>

### P1.T1 — <task title>
- **What:** <the change, specific enough to build>
- **Files:** <files to create or modify>
- **Depends on:** <task IDs or none>
- **Source:** <DISCOVERY Qn / RESEARCH §n>
- **Done when:** <checkable criteria>

### Phase 1 checkpoint
<How to verify the whole phase: commands to run, flows to click through.>
```
