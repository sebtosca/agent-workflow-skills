---
name: discovery
description: Interview the user about a product idea and log every question and answer to DISCOVERY.md. Use when starting a new project or feature, when the user says "discovery", or to resume an unfinished DISCOVERY.md.
---

# Discovery

Interview the user until every decision about the product is settled, and record the whole interview in `DISCOVERY.md` at the project root. This file is the first of four: DISCOVERY.md → RESEARCH.md → PLAN.md → PROGRESS.md. Later agents read it cold, with no access to this chat, so it must stand on its own.

## Steps

1. **Load state.** If `DISCOVERY.md` exists, read it and continue from its open questions; the answered ones are settled. Otherwise create it from the template below, filling in the idea as the user stated it.
2. **Explore facts yourself.** If there is existing code, docs, or a repo, read them before asking anything. Facts are your job; decisions are the user's. Only ask what you cannot look up.
3. **Ask in rounds.** Treat the product as a design tree: each decision unlocks the decisions beneath it. The _frontier_ is every question whose prerequisites are already answered. Ask the whole frontier in one round, numbered, each with your recommended answer:

   ```
   ❓ **Q7 - <title>**: <question, with options if useful>
   ➡️ Recommended: <your answer and a one-line reason>
   ```

   Cover the branches a builder will need: goal and users, core features and scope (MVP vs later), user flows, data, platforms and tech stack, integrations, auth, design/brand, constraints (budget, timeline, hosting), and what "done" looks like.
4. **Log after every round, before asking the next.** Append each question to the Q&A log with the user's answer verbatim, then update the Decisions summary and Open questions. Never batch logging to the end: the context window may compact at any time.
5. **Finish** when the frontier is empty: every branch visited and nothing silently assumed. Set Status to `complete`, list the research areas you think RESEARCH.md should cover, and tell the user the next step is the `parallel-research` skill.

## DISCOVERY.md template

```markdown
# Discovery: <project name>

Status: in progress | complete
Last updated: <YYYY-MM-DD>

## The idea
<The user's own description, lightly cleaned up.>

## Decisions summary
<One line per settled decision, grouped by topic, each citing its question: "Auth: email magic links (Q7)".>

## Open questions
<Questions not yet answered. Empty when Status is complete.>

## Suggested research areas
<Filled in at the end: the domains RESEARCH.md should investigate, and why.>

## Q&A log

### Round 1 — <date>
**Q1 - <title>**
- Asked: <question>
- Recommended: <your recommendation>
- Answer: <user's answer, verbatim>
```
