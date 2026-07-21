# Roadmap discovery

How to find and present feature-request candidates from the
[Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1)
for use as a **demo exercise**. Items and linked issues are **read-only** —
do not comment, change labels, or edit project fields.

---

## Prerequisites

```bash
gh auth status   # must be authenticated
```

Project board access needs the `read:project` scope. If `gh project` fails with
a missing-scope error:

```bash
gh auth refresh -s read:project
```

Then retry. If the user cannot refresh scopes in this session, use the
[fallbacks](#fallbacks-when-project-api-is-unavailable) below or ask them to
paste an issue URL from the board.

---

## Primary query (GitHub Project)

List items on org project **61** (Diffusers Roadmap):

```bash
gh project item-list 61 --owner huggingface --limit 20 --format json
```

Prefer items that:

1. Are in a **Backlog** (or equivalent "not started") status when the field is present
2. Link to an open **issue** (not a PR)
3. Read as a **feature / enhancement** rather than a pure bug fix
4. Have enough body text to analyze (not a one-line title only)

When presenting candidates, include title, status (if available), URL, and one
line on why it is a good demo pick (e.g. "clear ask but missing API details").

Present the top 3–5 via `AskUserQuestion`. Allow "paste my own URL" as an option.

---

## Fallbacks when project API is unavailable

### A. Issues labeled for the roadmap / features

```bash
gh issue list --repo huggingface/diffusers \
  --label "roadmap" --state open --limit 15 \
  --json number,title,body,labels,assignees,url,createdAt
```

If that is empty or too thin, widen:

```bash
gh issue list --repo huggingface/diffusers \
  --label "feature-request" --state open --limit 15 \
  --json number,title,body,labels,assignees,url,createdAt
```

Optional third pass:

```bash
gh issue list --repo huggingface/diffusers \
  --label "enhancement" --state open --limit 15 \
  --json number,title,body,labels,url,createdAt
```

Tell the user these are **approximations** of the board backlog — the canonical
source remains the [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1)
UI. Prefer items that also carry `roadmap` when both `feature-request` and
`roadmap` appear.

### B. Manual paste

Ask the user to open the roadmap, pick a Backlog card, and paste the issue URL
or number. Then:

```bash
gh issue view <NUMBER> --repo huggingface/diffusers \
  --json number,title,body,labels,comments,author,url,createdAt,updatedAt
```

---

## Ranking heuristics

Rank candidates (best first for a demo review):

1. **Open feature-oriented request** with a real problem statement
2. **Incomplete vs the feature-request template** — missing alternatives, examples, or success criteria (more to practice on)
3. **Single-area scope** — one pipeline family, scheduler, or loader rather than "redesign the library"
4. **Active enough to discuss** — not clearly abandoned spam; recent comments are a plus but not required
5. Avoid for this skill: pure `bug` filings with a one-line fix, `spam`, or requests that are already obviously shipped

Note [one-problem-per-pr](../onboarding-acme/conventions-overview.md#one-problem-per-pr) only as context: a feature request may still be larger than one PR — flag that in Phase 4 tradeoffs rather than rejecting the item.

---

## Label vocabulary (for Phase 2)

Refresh anytime with:

```bash
gh label list --repo huggingface/diffusers --limit 100
```

Labels that commonly matter for feature-request triage:

| Kind | Examples |
|------|----------|
| Type | `feature-request`, `enhancement`, `New pipeline/model`, `New scheduler` |
| Roadmap / priority | `roadmap`, `high-priority`, `low-priority`, `wip` |
| Area | `video`, `quantization`, `lora` / `peft`, `modular-diffusers`, `consider-for-modular-diffusers`, `performance`, `scheduler`, `training`, `documentation` |
| Needs from opener | `needs-code-example`, `question`, `help wanted` |
| Process | `duplicate`, `wontfix`, `should-move-to-discussion`, `stale` |

Only propose labels that exist on the repo. Do not apply them in this demo.

---

## After selection

1. Fetch full issue JSON (see Primary / Fallback commands above)
2. Skim comments for decisions already made — do not re-ask those in Phase 3
3. Optionally grep the local repo for named classes/pipelines to ground Phases 3–4
4. **Never** `gh issue comment`, `gh issue edit`, or project field updates
