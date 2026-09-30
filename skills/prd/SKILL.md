---
name: prd
description: "Write PRD.md, the product requirements document that defines what is being built, who it is for, and what it must do, synthesized from DISCOVERY.md, RESEARCH.md, and PLAN.md. Use after PLAN.md exists, or when the user asks for a PRD, spec, or requirements."
---

# PRD

Write `PRD.md`: the single answer to "what are we building, for whom, and what must it do?" The template adapts Matt Pocock's `to-spec` skill, but writes a file instead of publishing to an issue tracker, and adds the standard PRD sections (users, goals, metrics, acceptance criteria).

This is synthesis, not an interview. Everything you need is already in the three source documents. Only ask the user when they contradict each other or leave a requirement open.

## Steps

1. **Read `DISCOVERY.md`, `RESEARCH.md`, and `PLAN.md` in full.** If any is missing, point the user to the `discovery`, `research`, or `plan` skill.
2. **Use the project's own words.** Use the terms defined in `CONTEXT.md` exactly as defined, for users, features, and objects alike. Later documents and code will reuse these names. If you need a term the glossary lacks, add it to CONTEXT.md. Cite ADRs in Implementation decisions.
3. **Write `PRD.md` from the template below.** The user stories must be exhaustive: every feature, role, edge case, error state, and admin or setup flow gets a story. Give each story acceptance criteria written as Given / When / Then, so they can be tested.
4. **Map every story to PLAN.md tasks.** Fill in the "Plan tasks" column. If a story has no task, append the missing tasks to PLAN.md using the next free IDs, and add a line to PLAN.md's Decisions section saying they came from the PRD. If the PRD drops something the plan includes, ask the user which one is right.
5. **Show the user** the goals, non-goals, and the number of stories per feature area. Adjust from their feedback. Then tell them the next step is the `architecture` skill.

## Writing rules

- Describe behaviour from the user's point of view. Put *what* in the PRD and *how* in ARCHITECTURE.md and PLAN.md. No file paths or code; they go stale quickly.
- Metrics must be measurable ("signup takes under 60 s"), not goals in disguise ("fast signup").
- Non-goals are as important as goals: they stop the scope from creeping.

## PRD.md template

```markdown
# PRD: <project name>

Status: draft | approved · Last updated: <YYYY-MM-DD>
Sources: DISCOVERY.md, RESEARCH.md, PLAN.md

## Problem statement
<The problem, from the user's point of view.>

## Solution
<What the product does about it, from the user's point of view.>

## Target users
<Each type of user: who they are, what they are trying to do, and their context (device, skill level, how often they use it).>

## Goals
<Numbered outcomes the product must achieve.>

## Non-goals
<What this product deliberately will not do, and why.>

## Success metrics
| Metric | Target | How it is measured |
|---|---|---|

## User stories
Grouped by feature area. IDs are stable: `US-<n>`.

### <Feature area>
**US-1** As a <user>, I want <capability>, so that <benefit>.
- Given <context>, when <action>, then <result>.
- Given ..., when ..., then ...

## Non-functional requirements
<Performance, security, privacy, accessibility, reliability, supported platforms, localization, each with a measurable target.>

## Implementation decisions
<Decisions already made that constrain the build: the main modules, the interfaces between them, data-model choices, external services, API contracts. Cite the DISCOVERY Q or RESEARCH § they come from.>

## Testing decisions
<What a good test looks like here (test behaviour you can see from outside, not internal details); which modules get tests; what kinds of tests (unit, integration, end-to-end).>

## Assumptions and dependencies
<What must be true or available: third-party services, accounts, data, approvals.>

## Out of scope
<Deferred to later versions, with the reason.>

## Open questions
<Anything not yet settled. Empty when Status is approved.>

## Story → plan map
| Story | Plan tasks |
|---|---|
| US-1 | P1.T3, P2.T1 |
```
