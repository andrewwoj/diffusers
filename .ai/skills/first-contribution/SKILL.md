---
name: first-contribution
description: >
  Find a good first issue on huggingface/diffusers and guide a contributor
  through planning, implementing, testing, and PR prep. Use when someone wants
  their first PR, asks to fix a good first issue, or is ready to contribute code.
---

# First contribution

Guide a contributor through fixing **one good first issue** on [huggingface/diffusers](https://github.com/huggingface/diffusers/issues). Code edits happen in the local checkout; issues and PRs target the upstream repo.

**Not familiar with the repo?** Run the [onboarding-acme](../onboarding-acme/SKILL.md) skill first.

**Other contribution types** (docs, community pipelines, training examples, new models): see [CONTRIBUTING.md](../../../CONTRIBUTING.md) §5–9 or the [model-integration](../model-integration/SKILL.md) skill.

---

## Agent rules

- **Gate each phase** — wait for user confirmation before proceeding (unless user said "proceed through all steps")
- **Never commit or open a PR** unless the user explicitly asks
- **Convention whys** — link to [conventions-overview.md](../onboarding-acme/conventions-overview.md) anchors; do not re-teach the full codebase tour
- **Handoffs:** convention questions → [onboarding-acme](../onboarding-acme/SKILL.md); pre-PR review → [self-review](../self-review/SKILL.md)

---

## Phase 1: Prerequisites

**Do:**

1. Confirm dev environment:
   ```bash
   pip install -e ".[dev]"
   pip install -e ".[test]"
   make cursor    # if using Cursor
   ```
2. Confirm on a feature branch (not `main`):
   ```bash
   git checkout -b fix-issue-<number>
   ```
3. Confirm `gh auth status` succeeds

**Convention:** [branch-not-main](../onboarding-acme/conventions-overview.md#branch-not-main)

**Gate:** proceed only when environment is ready, or user asks to set up as part of this session.

---

## Phase 2: Issue discovery

**Do:** follow [issue-discovery.md](issue-discovery.md):

```bash
gh issue list --repo huggingface/diffusers \
  --label "good first issue" --state open --limit 15 \
  --json number,title,body,labels,assignees,url
```

Cross-check each candidate for open PRs. Rank and present top 3–5 via `AskUserQuestion`.

**Convention:** [coordinate-first](../onboarding-acme/conventions-overview.md#coordinate-first) — avoid duplicate work

**Gate:** user selects an issue (or pastes one manually).

---

## Phase 3: Analyze and plan

**Do:**

1. Fetch full issue:
   ```bash
   gh issue view <NUMBER> --repo huggingface/diffusers \
     --json title,body,labels,comments,url
   ```
2. Grep the local repo for affected files
3. Skim the nearest reference implementation in the same area
4. Identify domain guide if code touches models/pipelines/modular code
5. Output a **Task Plan**:

```markdown
## Task Plan

- **Issue:** #N — <title> (<url>)
- **Hypothesis:** <root cause or approach>
- **Files to touch:** <list>
- **Domain guide:** <models.md / pipelines.md / modular.md / none>
- **Tests to run:** pytest tests/<area>/test_<relevant>.py
- **Tests to add/update:** <yes/no + what>
- **Conventions that apply:** <links to conventions-overview anchors>
```

**Gate:** wait for explicit user approval of the Task Plan before writing code.

---

## Phase 4: Implement

**Do:**

- Make the **smallest fix** that solves the issue — no unrelated refactors
- Before editing: skim reference file in the same area (e.g. sibling pipeline or model)
- At each edit category, state the convention and link its anchor:

| If you… | Convention |
|---------|-------------|
| Reuse a method from another file | [#copied-from](../onboarding-acme/conventions-overview.md#copied-from) |
| Touch pipeline `__call__` | [#pipeline-no-grad](../onboarding-acme/conventions-overview.md#pipeline-no-grad) |
| Add a new public class | [#lazy-imports](../onboarding-acme/conventions-overview.md#lazy-imports) |
| Consider a fallback path | [#no-defensive-code](../onboarding-acme/conventions-overview.md#no-defensive-code) |
| Factor out a one-off helper | [#inline-helpers](../onboarding-acme/conventions-overview.md#inline-helpers) |

**Convention:** [one-problem-per-pr](../onboarding-acme/conventions-overview.md#one-problem-per-pr)

**Gate:** show the diff summary and ask user to review before running tests.

---

## Phase 5: Tests

**Do:**

1. Run existing tests for the touched area:
   ```bash
   pytest tests/<area>/test_<relevant>.py -v
   ```
2. Add or update tests if the issue is a bug fix or adds behavior
3. Reference nearest test file for patterns (e.g. `tests/schedulers/test_scheduler_tcd.py`)

**Convention:** [scoped-tests](../onboarding-acme/conventions-overview.md#scoped-tests)

**Gate:** all relevant tests pass — paste output for the PR description later.

---

## Phase 6: Format and lint

**Do:**

```bash
make style
make quality
make fix-copies   # only if a # Copied from source was edited
```

**Convention:** [make-style](../onboarding-acme/conventions-overview.md#make-style)

**Gate:** `make quality` exits clean.

---

## Phase 7: Self-review

**Do:** run the [self-review](../self-review/SKILL.md) skill against `git diff main...HEAD`.

Fix all blocking findings. Note intentional skips for the PR description.

**Convention:** [self-review](../onboarding-acme/conventions-overview.md#self-review)

**Gate:** verdict is **READY** (or blocking items explicitly deferred with user approval).

---

## Phase 8: PR prep

**Do:** verify this checklist — do **not** open the PR unless the user asks:

```
- [ ] Issue #N linked; maintainer acknowledgment requested or received
- [ ] Branch is not main
- [ ] make style && make quality pass
- [ ] pytest output saved for PR description
- [ ] self-review skill run — blocking issues fixed
- [ ] PR description will include: issue link, test commands + output, summary of change
- [ ] No large binary files committed
```

Remind the user to comment on the issue before opening the PR if they haven't:

> I'd like to work on this issue.

**Convention:** [coordinate-first](../onboarding-acme/conventions-overview.md#coordinate-first)

---

## Quick reference

| Need | Go to |
|------|-------|
| Codebase tour | [onboarding-acme](../onboarding-acme/SKILL.md) |
| Why a convention exists | [conventions-overview.md](../onboarding-acme/conventions-overview.md) |
| Self-serve lookup | [self-serve.md](../onboarding-acme/self-serve.md) |
| Issue search commands | [issue-discovery.md](issue-discovery.md) |
| Pre-PR review | [self-review](../self-review/SKILL.md) |
| Add a new model (not a good first issue) | [model-integration](../model-integration/SKILL.md) |
