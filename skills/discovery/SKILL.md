---
name: discovery
description: "Grill the user about a product idea (grill-with-docs style) and log every question and answer to DISCOVERY.md, plus a CONTEXT.md glossary and ADRs. Use when starting a new project or feature, when the user says \"discovery\", or to resume an unfinished DISCOVERY.md."
---

# Discovery

Interview the user until every decision about the product is settled, and record the whole interview in `DISCOVERY.md` at the project root. This is the first step of nine: DISCOVERY → RESEARCH → PLAN → PRD → ARCHITECTURE → THREAT-MODEL → SECURITY → AGENTS → PROGRESS.

This is Matt Pocock's `grill-with-docs` with a log added. Load the `grilling` and `domain-modeling` skills (in Claude Code they may be named `mattpocock-skills:grilling` and `mattpocock-skills:domain-modeling`) and follow both. `grilling` drives the interview; `domain-modeling` keeps the `CONTEXT.md` glossary and `docs/adr/` decision records. This skill adds the DISCOVERY.md log on top. If those skills aren't installed, use the round format in step 3 and write CONTEXT.md and ADRs yourself as described in step 4. Later agents read it cold, with no access to this chat, so it must stand on its own.

## Steps

1. **Load state.** If `DISCOVERY.md` exists, read it and continue from its open questions; the answered ones are settled. Otherwise create it from the template below, filling in the idea as the user stated it.
2. **Explore facts yourself.** If there is existing code, docs, or a repo, read them before asking anything. Facts are your job; decisions are the user's. Only ask what you cannot look up.
3. **Ask in rounds.** Treat the product as a design tree: each decision unlocks the decisions beneath it. The _frontier_ is every question whose prerequisites are already answered. Ask the whole frontier in one round, numbered, each with your recommended answer:

   ```
   ❓ **Q7 - <title>**: <question, with options if useful>
   ➡️ Recommended: <your answer and a one-line reason>
   ```

   Challenge fuzzy or overloaded words ("account": customer or user?) and probe edge cases with concrete scenarios.

   Cover the branches a builder will need: goal and users, core features and scope (MVP vs later), user flows, data, platforms and tech stack, integrations, auth, design/brand, constraints (budget, timeline, hosting), and what "done" looks like.
4. **Log after every round, before asking the next.** Never batch logging to the end: the context window may compact at any time.
   - `DISCOVERY.md`: append each question with the user's answer verbatim, then update the Decisions summary and Open questions.
   - `CONTEXT.md`: add each term the moment its meaning is settled. It is a glossary only: definitions, no implementation details.
   - `docs/adr/NNNN-<slug>.md`: offer an ADR only for a decision that is hard to reverse, would surprise a future reader, *and* came from a real trade-off. Link each ADR from the Decisions summary.
5. **Finish** when the frontier is empty: every branch visited and nothing silently assumed. Set Status to `complete`, list the research areas you think RESEARCH.md should cover, and tell the user the next step is the `research` skill.

## DISCOVERY.md template

```markdown
# Discovery: <project name>

Status: in progress | complete
Last updated: <YYYY-MM-DD>

## The idea
<The user's own description, lightly cleaned up.>

## Decisions summary
<One line per settled decision, grouped by topic, each citing its question: "Auth: email magic links (Q7)". Add the ADR link when one exists: "(Q7, ADR-0002)".>

## Glossary
Terms are defined in `CONTEXT.md`. Decision records are in `docs/adr/`.

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
