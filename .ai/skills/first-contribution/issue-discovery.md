# Issue discovery

How to find and rank good first issues on **huggingface/diffusers**. Issues live on GitHub; code edits happen in the **local checkout**.

---

## Prerequisites

```bash
gh auth status   # must be authenticated
```

If not authenticated: `gh auth login`

---

## Primary query

```bash
gh issue list --repo huggingface/diffusers \
  --label "good first issue" \
  --state open \
  --limit 15 \
  --json number,title,body,labels,assignees,url
```

---

## Cross-check for duplicate work

For each candidate issue `#N`:

```bash
gh pr list --repo huggingface/diffusers \
  --search "fixes #N" \
  --state open \
  --json number,title,url
```

Skip issues that already have an active open PR.

---

## Ranking heuristics

Rank candidates (best first):

1. **Proposed fix or clear repro** in the issue body — easiest to implement
2. **No open linked PR** — no duplicate work in flight
3. **Unassigned** — less likely someone else is already working on it
4. **`bug` label** over broad `feature-request` — smaller, clearer scope
5. **Single-file scope** — issue mentions one file or one component

Present the top 3–5 to the user via `AskUserQuestion`.

---

## Fetch full issue context

Once selected:

```bash
gh issue view <NUMBER> --repo huggingface/diffusers \
  --json title,body,labels,comments,url
```

Then in the local repo:

- Grep for files/classes mentioned in the issue
- Read the affected code and nearest reference implementation
- Identify the domain guide: [models.md](../../models.md), [pipelines.md](../../pipelines.md), or [modular.md](../../modular.md)

---

## Empty results fallback

If zero issues have the `good first issue` label:

1. Widen search:
   ```bash
   gh issue list --repo huggingface/diffusers \
     --label "bug" \
     --state open \
     --limit 15 \
     --json number,title,body,labels,url
   ```
2. Ask the user to paste an issue URL or number they found manually
3. Search [good first issues link](https://github.com/huggingface/diffusers/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22) in browser and paste a candidate

---

## Coordination reminder

Before implementing, the contributor should comment on the selected issue:

> I'd like to work on this issue.

Wait for maintainer acknowledgment before opening a PR. See [coordinate-first](../onboarding-acme/conventions-overview.md#coordinate-first).
