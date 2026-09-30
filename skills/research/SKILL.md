---
name: research
description: Research a project by splitting it into domains, dispatching one parallel sub-agent per domain, and compiling their verbatim findings into RESEARCH.md. Use after DISCOVERY.md is complete, or when the user says "research".
---

# Parallel research

Turn the settled decisions in `DISCOVERY.md` into researched facts in `RESEARCH.md`. You are the coordinator: you pick the domains, brief the sub-agents, and assemble their output. You never summarize or rewrite their findings.

## Steps

1. **Read `DISCOVERY.md`.** If it is missing or its Status is not `complete`, tell the user and offer to run the `discovery` skill first.
2. **Pick the domains.** Start from its "Suggested research areas". Choose 3–8 domains that do not overlap, each narrow enough for one agent. Examples: framework/stack, database and data model, auth, payments, a specific third-party API, hosting/deployment, competitor products, UI patterns, legal/compliance. Show the list to the user and confirm it before you dispatch.
3. **Dispatch every sub-agent in one message so they run in parallel.** Give each one the brief below. Each writes to its own file, `research/NN-<domain-slug>.md`. Separate files mean parallel agents never overwrite each other.
4. **Check the results.** Every domain file must exist, have sources, and answer its questions. Re-dispatch any agent that failed or came back thin.
5. **Assemble `RESEARCH.md` verbatim.** Write the header and index yourself. Then append each domain file byte-for-byte with a shell command such as `cat research/*.md >> RESEARCH.md`, so nothing passes through your own rewording. Keep the `research/` folder as the source.
6. **Add the cross-cutting section** at the top: conflicts between domains, risks, and anything that should change a decision in DISCOVERY.md. Mark this section as coordinator-written. Tell the user the next step is the `plan` skill.

## Sub-agent brief

Fill in the brackets for each agent:

> You are researching **[domain]** for this project: [2–4 sentence product summary]. Relevant decisions from DISCOVERY.md: [the decisions that touch this domain, with their Q numbers].
>
> Answer these questions: [3–6 specific questions, e.g. "Which auth library fits a Next.js 15 app router project, and how is it set up?"].
>
> Use primary sources: official docs, source repos, changelogs, pricing pages. Record the version and date of anything that changes over time. Quote the key passages exactly, and give a URL for every claim. Include working code snippets and setup steps where they exist. Flag anything that contradicts a decision above.
>
> Write your findings to `research/[NN]-[slug].md` using this structure: `## [NN]. [Domain]`, then `### Questions`, `### Findings` (one sub-heading per question), `### Recommendation`, `### Risks and gotchas`, `### Sources`. When you finish, reply only with the file path and a one-line status.

## RESEARCH.md header

```markdown
# Research: <project name>

Generated: <YYYY-MM-DD> from DISCOVERY.md
Domains: <numbered list linking to each section>

## Cross-cutting findings (coordinator)
<conflicts, risks, suggested changes to discovery decisions>

---
<domain sections appended verbatim below>
```
