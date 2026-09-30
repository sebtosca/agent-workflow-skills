---
name: security-policy
description: "Write or update SECURITY.md, the public vulnerability-reporting policy that GitHub shows in the repo's Security tab: how to report privately, supported versions, response times, and disclosure rules. Use after THREAT-MODEL.md, before a repo goes public or ships a release, or when the user asks for a security policy or SECURITY.md."
---

# Security policy

Write `SECURITY.md` at the repo root: the public page that tells a security researcher how to report a vulnerability privately, and what happens next. GitHub finds it in the root, `docs/`, or `.github/`, shows it in the repo's Security tab, and links it from the new-issue page.

This file is **public**. It contains contact details and process only. Architecture, threats, and internal controls live in `THREAT-MODEL.md`, which this file never links to or quotes.

## Steps

1. **Gather the facts.** Read `PRD.md` and `PLAN.md` for the product name, what gets deployed, and how releases are versioned. Run `gh repo view --json nameWithOwner,visibility` if the project is on GitHub.
2. **Ask the user what only they can decide.** Recommend an answer for each:
   - **Reporting channel.** Recommend GitHub private vulnerability reporting: reports arrive as private drafts in the repo and no email address is exposed. The user turns it on under Settings → Code security → Private vulnerability reporting, or you can run `gh api -X PUT repos/<owner>/<repo>/private-vulnerability-reporting` once they approve. An email address is the alternative, or the fallback for repos that aren't on GitHub.
   - **Response times** they can realistically keep. Recommend: acknowledge within 3 business days, first assessment within 10, and a fix for critical issues within 30.
   - **Supported versions.** Recommend "latest release only" for a solo or small project.
   - **Disclosure window** before a reporter may publish. Recommend 90 days, the industry norm.
3. **Write `SECURITY.md` from the template.** Use only the answers the user gave. Never invent an email address or a promise they didn't make.
4. **Link it.** Add a "Security" line to `README.md` pointing to SECURITY.md. For a website, offer to also add `/.well-known/security.txt` (RFC 9116) with the same contact and an `Expires` date within a year.
5. **Remind the user** to turn on private vulnerability reporting if they chose it. Then tell them the next step is the `agents-md` skill.

## Rules

- Keep it short. Researchers skim it for three things: where to report, what to include, and what to expect.
- Tell people to never report a vulnerability in a public issue, pull request, or discussion.
- Keep the scope honest: list what counts, and what doesn't (for example, third-party services you don't control, social engineering, or denial of service by flooding).
- Update the supported-versions table on each major release. The `progress` skill checks it at phase checkpoints.

## SECURITY.md template

```markdown
# Security policy

## Reporting a vulnerability

Please **do not** report security vulnerabilities through public GitHub issues, pull requests, or discussions.

Report them privately through [GitHub's private vulnerability reporting](https://github.com/<owner>/<repo>/security/advisories/new)
<or: by email to <address>>.

Please include:
- The type of issue (e.g. XSS, SQL injection, authentication bypass)
- The affected URL, endpoint, or file path, and the version or commit
- Step-by-step instructions to reproduce it
- Proof of concept, if you have one
- The impact: what an attacker could do with it

## What to expect

- Acknowledgement within <3> business days
- An initial assessment within <10> business days
- Updates at least every <14> days until it's resolved
- Credit in the release notes if you'd like it

## Supported versions

| Version | Supported |
|---|---|
| <latest> | ✅ |
| < <latest> | ❌ |

## Scope

**In scope:** <the app at domain.com, the API, this repository's code>
**Out of scope:** <third-party services, social engineering, physical attacks, denial of service by volume, findings from automated scanners with no demonstrated impact>

## Disclosure policy

We follow coordinated disclosure. Please give us <90> days to release a fix before publishing details. We will tell you when the fix ships.

## Safe harbor

We will not take legal action against research done in good faith under this policy: avoid privacy violations, data destruction, and service disruption, and only test against accounts you own.
```
