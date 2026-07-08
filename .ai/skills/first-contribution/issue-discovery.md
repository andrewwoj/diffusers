# Issue discovery

How to find and rank good first issues on **huggingface/diffusers** for use as a **demo exercise**. Issues are read-only reference material — code edits and PRs go to **andrewwoj/diffusers**. Do not comment on huggingface issues.

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

## Ranking heuristics

Rank candidates (best first):

1. **Proposed fix or clear repro** in the issue body — easiest to implement
2. **Unassigned** — simpler to reason about scope
3. **`bug` label** over broad `feature-request` — smaller, clearer scope
4. **Single-file scope** — issue mentions one file or one component

Do **not** skip issues that already have open PRs on huggingface/diffusers — this is a demo exercise on andrewwoj/diffusers.

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
- Rename the feature branch to `fix-issue-<NUMBER>-<short-kebab-description>` (see [SKILL.md](SKILL.md) Phase 1c)

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
