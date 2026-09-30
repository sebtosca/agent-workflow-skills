---
name: progress
description: "Create and maintain PROGRESS.md, the checklist of every phase and task in PLAN.md that lets an agent resume implementation after a context reset. Use when starting to implement PLAN.md, when resuming work in a fresh session, or when the user says \"continue\", \"where did we leave off\", or \"progress\"."
---

# Progress

`PROGRESS.md` is the implementing agent's memory. Context windows compact and sessions end; this file is how the next session knows exactly where the work stands. Treat it as a save file: if it is not written down, the next session will not know it happened.

It is the last of the project documents, and the only one that changes on every task:

| Doc | Answers | Role while building |
|---|---|---|
| `PLAN.md` | What to build next | Task list, with "Done when" for each task |
| `PRD.md` | What it must do for the user | Acceptance criteria for each user story |
| `ARCHITECTURE.md` | Where the code goes and which rules must hold | Codemap and invariants |
| `AGENTS.md` | How to work in this repo | Commands, conventions, and the resume pointer |

Pick the branch that matches the repo:

- **No PROGRESS.md yet** → Set up.
- **PROGRESS.md exists** → Resume.

## Set up

1. Read `PLAN.md`, `PRD.md`, `ARCHITECTURE.md`, and `AGENTS.md`. If one is missing, point the user to the skill that makes it: `plan`, `prd`, `architecture`, or `agents-md`.
2. Create `PROGRESS.md` from the template below. Include every phase and every task ID from PLAN.md, each with a one-line summary and the user stories it serves, taken from PRD.md's "Story → plan map". Details stay in PLAN.md; PROGRESS.md points to them by ID.
3. Make sure `AGENTS.md` has the "Implementation progress" section (the resume pointer below). If a `CLAUDE.md` exists, make sure it imports `@AGENTS.md`. This is what makes a brand-new session find PROGRESS.md before it does anything else.
4. Commit, then start the Work loop at the first task.

## Resume

1. Read `PROGRESS.md` top to bottom: **Current state** first, then the most recent Session log entries.
2. Check it against reality with `git log --oneline -15` and `git status`. If there are commits or uncommitted changes the log doesn't mention, the previous session was cut off mid-task. Record what you find in PROGRESS.md before continuing.
3. Read the PLAN.md section for the current task, plus the PRD.md stories it serves.
4. Tell the user in 2–3 lines where things stand and what you will do next, then start the Work loop.

## Work loop

For each task:

1. **Start**: mark it `[~]` and set **Current state** to this task, with a line on your approach. Read the PLAN.md task, the acceptance criteria of its PRD stories, and the ARCHITECTURE.md codemap entry and invariants for the area you'll touch.
2. **Build** it until the PLAN.md "Done when" criteria pass, and the story's acceptance criteria too once the task finishes that story. Put code where ARCHITECTURE.md's codemap says it goes, and follow AGENTS.md's conventions.
3. **Keep the docs true, in the same commit.** If the task added a module, changed data flow, or added an external service, update ARCHITECTURE.md. If it added or changed a command, a convention, or a gotcha, update AGENTS.md (replace any `(planned)` command with the real one). If it changed product behaviour, stop and ask the user before editing PRD.md.
4. **Finish**: mark it `[x]` with the date and commit hash. Update Current state to the next task. Add a Session log line. Commit the code and docs together, with the task ID in the message (`P2.T4: add login form`).
5. Anything that changed along the way goes in **Deviations**: a changed approach, a task you added (give it the next free ID in PLAN.md too), or a task you skipped. Put anything that stops you in **Blockers**, and ask the user.

Update PROGRESS.md at each of these moments, not at the end of the session: the context can compact without warning, and anything left unwritten is lost.

## Phase checkpoint

When the last task of a phase is done, before starting the next phase:

1. Run the phase checkpoint from PLAN.md.
2. Check every PRD story this phase completes against its acceptance criteria, and mark it verified in the PRD coverage line.
3. Compare ARCHITECTURE.md with the actual code. Fix any drift, and set its Status to `built` once no Planned changes remain. Check that every AGENTS.md command runs.
4. Record the result on the phase heading, then commit.

## Resume pointer

The `agents-md` skill puts this block in AGENTS.md. Add it if it's missing:

```markdown
## Implementation progress
This project is being built from PLAN.md. At the start of every session, and after any context reset, read PROGRESS.md and continue from its "Current state" using the `progress` skill. Update PROGRESS.md after every task.
```

## PROGRESS.md template

```markdown
# Progress: <project name>

Plan: PLAN.md · Last updated: <YYYY-MM-DD HH:MM>
Overall: <done>/<total> tasks · Phase <n> of <m>
PRD coverage: <verified>/<total> stories verified

## Current state
- **Working on:** P2.T4 — <title> (or "next up" if between tasks)
- **Status / approach:** <where it stands mid-task: what is done, what's left, files touched>
- **Next after this:** P2.T5 — <title>

## Blockers
<Anything waiting on the user or on outside input. "None" if empty.>

## Phases
Legend: [ ] todo · [~] in progress · [x] done · [-] skipped

### Phase 1 — <name> (3/3) ✅ checkpoint passed <date> · stories verified: US-1, US-2
- [x] P1.T1 — <summary> (US-1) · <date> · <commit>
- [x] P1.T2 — <summary> · <date> · <commit>

### Phase 2 — <name> (1/6)
- [x] P2.T1 — <summary> (US-3) · <date> · <commit>
- [~] P2.T2 — <summary> (US-3, US-4)
- [ ] P2.T3 — <summary>

## Deviations from plan
<Date · task ID · what changed and why.>

## Session log
<Newest first. One line per task finished or significant event: date · task · outcome.>
```
