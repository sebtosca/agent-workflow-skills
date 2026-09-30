---
name: progress
description: Create and maintain PROGRESS.md, the checklist of every phase and task in PLAN.md that lets an agent resume implementation after a context reset. Use when starting to implement PLAN.md, when resuming work in a fresh session, or when the user says "continue", "where did we leave off", or "progress".
---

# Progress tracker

`PROGRESS.md` is the implementing agent's memory. Context windows compact and sessions end; this file is how the next session knows exactly where the work stands. Treat it as a save file: if it is not written down, the next session will not know it happened.

Pick the branch that matches the repo:

- **No PROGRESS.md yet** → Set up.
- **PROGRESS.md exists** → Resume.

## Set up

1. Read `PLAN.md`. If it is missing, point the user to the `plan` skill.
2. Create `PROGRESS.md` from the template below. Include every phase and every task ID from PLAN.md, each with a one-line summary. Details stay in PLAN.md; PROGRESS.md points to them by ID.
3. Add the resume pointer (below) to the project's agent instructions file: `CLAUDE.md` and/or `AGENTS.md`, whichever exists. Create `AGENTS.md` if neither does. This is what makes a brand-new session find PROGRESS.md before it does anything else.
4. Commit both files, then start the Work loop at the first task.

## Resume

1. Read `PROGRESS.md` top to bottom: **Current state** first, then the most recent Session log entries.
2. Check it against reality with `git log --oneline -15` and `git status`. If there are commits or uncommitted changes the log doesn't mention, the previous session was cut off mid-task. Record what you find in PROGRESS.md before continuing.
3. Read the PLAN.md section for the task in progress (or the next task).
4. Tell the user in 2–3 lines where things stand and what you will do next, then start the Work loop.

## Work loop

For each task:

1. **Start**: mark it `[~]` and set **Current state** to this task, with a line on your approach.
2. **Build** it until its PLAN.md "Done when" criteria pass.
3. **Finish**: mark it `[x]` with the date and commit hash. Update Current state to the next task. Add a Session log line. Commit the code and PROGRESS.md together, with the task ID in the message (`P2.T4: add login form`).
4. Anything that changed along the way goes in **Deviations**: a changed approach, a task you added (give it the next free ID in PLAN.md too), or a task you skipped. Put anything that stops you in **Blockers**, and ask the user.

Update PROGRESS.md at each of these moments, not at the end of the session: the context can compact without warning, and anything left unwritten is lost. When a phase finishes, run its PLAN.md checkpoint and record the result before starting the next phase.

## Resume pointer

Add this block to `CLAUDE.md` / `AGENTS.md`:

```markdown
## Implementation progress
This project is being built from PLAN.md. At the start of every session, and after any context reset, read PROGRESS.md and continue from its "Current state" using the progress skill. Update PROGRESS.md after every task.
```

## PROGRESS.md template

```markdown
# Progress: <project name>

Plan: PLAN.md · Last updated: <YYYY-MM-DD HH:MM>
Overall: <done>/<total> tasks · Phase <n> of <m>

## Current state
- **Working on:** P2.T4 — <title> (or "next up" if between tasks)
- **Status / approach:** <where it stands mid-task: what is done, what's left, files touched>
- **Next after this:** P2.T5 — <title>

## Blockers
<Anything waiting on the user or on outside input. "None" if empty.>

## Phases
Legend: [ ] todo · [~] in progress · [x] done · [-] skipped

### Phase 1 — <name> (3/3) ✅ checkpoint passed <date>
- [x] P1.T1 — <summary> · <date> · <commit>
- [x] P1.T2 — <summary> · <date> · <commit>

### Phase 2 — <name> (1/6)
- [x] P2.T1 — <summary> · <date> · <commit>
- [~] P2.T2 — <summary>
- [ ] P2.T3 — <summary>

## Deviations from plan
<Date · task ID · what changed and why.>

## Session log
<Newest first. One line per task finished or significant event: date · task · outcome.>
```
