# AGENTS.md

Always-read policy for AI agents in the Locals legal documents repository. Keep
this file compact; load linked decisions and skills only when the current task
touches them.

## What This Repository Is

`LLC-Locals/iOS-legal` holds the published legal set of Locals: the terms of
service, the privacy policy, the copyright and takedown policy, and the
third-party services page. It is served by GitHub Pages from `main` at
`https://llc-locals.github.io/iOS-legal/`.

**Merging to `main` publishes.** There is no build, no staging and no release
step between a merge and what users, Apple and the app read.

The rules for these documents are product decisions, kept in `locals-discovery`:

- [0097](https://github.com/LLC-Locals/locals-discovery/blob/main/decisions/0097-the-app-launches-without-a-legal-review.md)
  — the owner owns the documents, the questions for counsel stay open, and each
  document shows a "Last updated" date.
- [0098](https://github.com/LLC-Locals/locals-discovery/blob/main/decisions/0098-the-published-privacy-policy-is-the-data-inventory.md)
  — the privacy policy and the third-party page are the data inventory.
- [0101](https://github.com/LLC-Locals/locals-discovery/blob/main/decisions/0101-the-policy-is-one-english-page-linked-from-the-finish-step-about-and-the-app-store.md)
  — the policy is one English page, linked from the app and the App Store.
- [0115](https://github.com/LLC-Locals/locals-discovery/blob/main/decisions/0115-the-support-page-moves-to-localsapp-app-support.md)
  — the Support Page lives on the website; `index.html` here only points to it.

## Rules for the Documents

- **A document describes the product that has shipped.** A change to what the
  product stores or sends goes into the privacy policy or the third-party page
  in step with the release that ships it, never ahead of it (0097, 0098). Text
  that is ready before its release waits in its pull request.
- **The addresses do not move.** The App Store listing, the app's Finish Step
  and About screen, and the Age Suitability URL link to these files by path.
  Never rename or move `TOS/eng/TOS.html`, `privacy/eng/privacy.html`,
  `copyright/eng/copyright.html` or `third-party/eng/third-party.html`.
- **Every change updates the document's "Last updated" date.** The terms of
  service also carry a version identifier, which accounts record at acceptance
  ([0067](https://github.com/LLC-Locals/locals-discovery/blob/main/decisions/0067-the-terms-are-accepted-once-on-the-finish-step-and-recorded.md)).
  Change the version only when the decision you implement says so.
- **Legal questions are not answered here.** The questions for counsel named in
  0097 stay open. If a task needs one answered, or a decision is missing,
  unclear or contradicts the task, request it in `locals-discovery` (see below)
  and stop at that boundary.
- **The Support Page is not edited here.** Addresses and the publisher are
  changed on the website, together with the documents that name them (0115).
- A material change to the privacy policy ships with an in-app notice (0101).
  That notice is work for `LLC-Locals/Locals-iOS`; request it.

## Product Language

Product terms come from
[`locals-discovery` → `product/glossary.md`](https://github.com/LLC-Locals/locals-discovery/blob/main/product/glossary.md).
Use them as the glossary defines them; do not coin synonyms. The documents speak
to users, so a term may be written out in plain words, but it must mean what the
glossary says.

## Working on an Issue

Open a **draft** pull request as the first step, before the work is done. Its
body says `Closes LLC-Locals/iOS-legal#N`, and its branch starts with the agent's
name: `claude/`, `codex/` or `cursor/`. The project board reads the pull request:
a draft is In Progress, ready for review is Review, merged is Done. Here it is
read by the nightly board sync in `locals-discovery`, not on each event, so the
board can be a day behind. Do not move cards on the board by hand.

## Cross-Repository Requests

Locals spans six repositories. You work only in this one.

| Area | Repository | Label |
|---|---|---|
| iOS application | `LLC-Locals/Locals-iOS` | `type:iOS` |
| Design system | `LLC-Locals/iOS-DesignSystem` | `type:design-system` |
| Legal documents | `LLC-Locals/iOS-legal` | `type:legal` |
| Backend | `LLC-Locals/Locals-Go` | `type:golang` |
| Website | `LLC-Locals/Locals-Web` | `type:web` |
| Product discovery | `LLC-Locals/locals-discovery` | `type:discovery` |

This table is generated from `repos.yml` in `locals-discovery`. Do not change it
here alone.

Never edit another repository, push to it, or create issues and pull requests in
it directly. When a task needs work there, such as an in-app notice, a change to
the Support Page, or an unresolved product decision, post one follow-up request
comment.

**Post it on the discovery issue the current task belongs to, not on the
current issue.** This repository is public, and the shared `follow-up-request`
workflow lives in the private `locals-discovery`, which shares its workflows
with private repositories only — so nothing here can run it. A comment on the
discovery runs it there. The discovery is the current issue's parent, or the
issue under `## Source issue` in its body. If the task has neither, do not post
the request: stop and ask the owner where it belongs.

The new issue becomes a sub-issue of that discovery, as everywhere else.
`Blocking: Yes` marks the **discovery** as blocked by it, not the task here; say
in the task that it waits on the new issue.

Use `.agents/skills/locals-crossrepo-request/SKILL.md` for the comment format,
the deduplication key, and how to react to the workflow answer. Where the skill
says "the current issue", read "the discovery issue".

## Repository Map

- `TOS/eng/TOS.html`, `privacy/eng/privacy.html`,
  `copyright/eng/copyright.html`, `third-party/eng/third-party.html`: the
  published documents, plain HTML, one English page each.
- `assets/legal.css`: the one stylesheet every document uses.
- `index.html`: a redirect to `localsapp.app/support`, kept so the old address
  still works (0115).
- `_config.yml`: GitHub Pages settings. It excludes the agent files below from
  the published site.
- `.agents/skills/`: repo-owned, agent-neutral skill sources.
- `.claude/skills/`: Claude Code discovery adapters symlinked to shared skills.
- No workflows. The shared ones in `locals-discovery` cannot be called from a
  public repository; do not add a caller, and do not copy their implementation
  here.
- `CLAUDE.md`: thin Claude Code adapter that imports this policy.

## Verification

- Open each changed page in a browser and follow its links. Paths are relative,
  so a page that renders from the repository root renders on GitHub Pages.
- The changed document's "Last updated" date is today's.
- Before finishing, state what was verified. If verification could not be run,
  explain why.
