---
name: locals-crossrepo-request
description: Use when Locals legal documents work needs a change in another Locals repository, such as a new or changed backend endpoint, a design system component, a shared contract, or a product decision; covers the follow-up request comment format that the create-follow-up-issue workflow turns into an issue in the target repository.
metadata:
  short-description: Locals cross-repository follow-up requests
---

# Locals Cross-Repository Request

Locals spans six repositories. You work only in the one you were started in.
You never edit another repository, never push to it, and never create issues or
pull requests in it directly.

When the current task needs work elsewhere, you post one follow-up request
comment on the current GitHub Issue. The `create-follow-up-issue` workflow
validates the comment and creates the issue in the target repository for you.

## Repository Map

| Area | Repository | Label |
|---|---|---|
| iOS application | `LLC-Locals/Locals-iOS` | `type:iOS` |
| Design system | `LLC-Locals/iOS-DesignSystem` | `type:design-system` |
| Legal documents | `LLC-Locals/iOS-legal` | `type:legal` |
| Backend | `LLC-Locals/Locals-Go` | `type:golang` |
| Website | `LLC-Locals/Locals-Web` | `type:web` |
| Product discovery | `LLC-Locals/locals-discovery` | `type:discovery` |

Use the exact repository name from this table. Any other value is rejected.
The table is checked against `repos.yml` in `locals-discovery` by CI.

Write the request in product language. `locals-discovery` →
`product/glossary.md` maps each product concept to the name the target
repository uses; put the product concept in the prose and that repository's own
identifiers in the requirements.

## When To Request

Request only when the change cannot be made in the current repository:

- A backend endpoint is missing, or its contract must change.
- A design system component is missing or must change.
- A shared model or protocol must change.
- The published legal documents must change.
- A product decision must be resolved in discovery.

Do not request anything that can be completed here. Do not request a change in
the repository you are already working in; such a request is rejected.

Before posting, read the existing comments on the issue. If the same request is
already there, do not post it again.

## Comment Format

Post the request as its own comment. One request per comment: the workflow
parses the first match and silently ignores anything after it.

```markdown
<!-- follow-up-request -->

Target repository: LLC-Locals/Locals-Go
Title: Add collaborators to the collection response
Key: guide-collaborators-endpoint
Blocking: Yes

## Description

The collection screen must render the list of collaborators, but the current
`GET /collections/{id}` response has no such field.

## Requirements

- Add `collaborators` to the collection response
- Each item carries `id`, `name` and `avatarUrl`
- Return an empty array when there are none, never `null`
- Document the field in the OpenAPI schema
```

Fields:

- `<!-- follow-up-request -->` is required. Without it nothing runs.
- `Target repository:` is required. Exact name from the repository map.
- `Title:` is required. One line, imperative, no issue numbers.
- `Key:` is optional but strongly preferred. Lowercase kebab-case. It is the
  deduplication key. When omitted it is derived from `Title`, so any later edit
  to the title creates a second issue instead of matching the first.
- `Blocking:` is optional, `Yes` or `No`. `Yes` records a native GitHub
  **blocked by** relationship on the current issue, pointing at the new one.
  There are no `blocked` / `blocker` labels: GitHub itself shows what blocks
  the issue and whether that task is still open.
- `## Description` is required. Everything after this heading becomes the issue
  body, so put `## Requirements` and any other sections below it.

Write concrete requirements. Do not include secrets, tokens or credentials. Do
not include file paths that only make sense in the current repository.

Do not add source links, labels, relationships or backlinks yourself: the
workflow injects the source issue, the type label and the blocked-by relation.

## After Posting

The workflow answers in the same issue:

- Created, with a link and the key. Done, never post that request again.
- Already exists, with a link. The key matched an existing issue, nothing to do.
- Rejected, with a reason. Fix exactly what it reports and post the corrected
  comment again with the same `Key`.

If no answer appears, the workflow did not trigger. Report that in your summary
instead of retrying or creating the issue by other means. The usual cause is
that the comment was posted by an account without write access in this
repository.

## Blocking Work

When a follow-up is marked `Blocking: Yes` and is still unresolved, stop
implementation at the boundary. The current issue carries a `blocked by` link to
it; check that link's state rather than guessing. Describe the gap in your summary. Do not invent
the missing endpoint, field, component or product decision, and do not mark the
task complete.
