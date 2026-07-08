---
name: first-contribution
description: >
  Demo workflow: find a good first issue on huggingface/diffusers (reference only),
  guide a contributor through Docker setup, planning, implementing, testing, and
  PR prep on andrewwoj/diffusers. Use when someone wants their first PR, asks to
  fix a good first issue, or is ready to contribute code in a demo session.
---

# First contribution (demo)

Guide a contributor through fixing **one good first issue** as a **demonstration exercise**.

| Resource | Repo | Purpose |
|----------|------|---------|
| Issues | [huggingface/diffusers](https://github.com/huggingface/diffusers/issues) | **Reference only** — pick an issue to practice on |
| Code + PRs | [andrewwoj/diffusers](https://github.com/andrewwoj/diffusers) | **All edits and PRs go here** |

**Not familiar with the repo?** Run the [onboarding-acme](../onboarding-acme/SKILL.md) skill first.

**Other contribution types** (docs, community pipelines, training examples, new models): see [CONTRIBUTING.md](../../../CONTRIBUTING.md) §5–9 or the [model-integration](../model-integration/SKILL.md) skill.

---

## Agent rules

- **Demo only** — **never** open a PR on `huggingface/diffusers`. **Never** comment on issues there. All PRs target `andrewwoj/diffusers`.
- **Gate each phase** — wait for user confirmation before proceeding (unless user said "proceed through all steps")
- **Never commit or open a PR** unless the user explicitly asks
- **Convention whys** — link to [conventions-overview.md](../onboarding-acme/conventions-overview.md) anchors; do not re-teach the full codebase tour
- **Handoffs:** convention questions → [onboarding-acme](../onboarding-acme/SKILL.md); pre-PR review → [self-review](../self-review/SKILL.md)

---

## Phase 1: Environment setup

Complete all three steps before issue discovery. Do not write code until Docker and the feature branch are ready.

### 1a. Repo and auth

**Do:**

1. Confirm `gh auth status` succeeds
2. Confirm `origin` points to **andrewwoj/diffusers** (not huggingface):
   ```bash
   git remote -v
   ```
   If `origin` is wrong, fix it:
   ```bash
   git remote set-url origin git@github.com:andrewwoj/diffusers.git
   ```
3. Confirm not on `main`:
   ```bash
   git branch --show-current
   ```

**Gate:** `gh` is authenticated and `origin` is `andrewwoj/diffusers`.

### 1b. Docker container

**Do:** build and enter a dev container before any code changes. The repo is bind-mounted so edits in Cursor apply inside the container.

```bash
docker build -t diffusers-dev docker/diffusers-pytorch-cpu
docker run -it --rm -v "$(pwd)":/workspace -w /workspace diffusers-dev bash
```

Inside the container, install the mounted checkout:

```bash
pip install -e ".[dev,test]"
pytest --version
make cursor    # if using Cursor — run on host if make is unavailable in container
```

**Gate:** container is running, `pip install -e` succeeded, and `pytest --version` works.

### 1c. Feature branch

**Do:** create a branch **before** selecting an issue. Use this naming pattern:

```
fix-issue-<NUMBER>-<short-kebab-description>
```

Examples: `fix-issue-1234-wrong-dtype`, `fix-issue-567-missing-docstring`

**Rules:**

| Rule | Good | Bad |
|------|------|-----|
| Lowercase kebab-case | `fix-issue-42-scheduler-step` | `Fix_Issue_42` |
| Starts with `fix-issue-` | `fix-issue-99-typo` | `my-branch`, `issue-99` |
| Includes issue number once chosen | `fix-issue-1234-...` | `fix-scheduler` (no number) |
| No spaces or underscores | `fix-issue-1-null-check` | `fix issue 1` |
| Not `main` | any feature branch | `main` |

If the issue is not selected yet, create a temporary branch and rename after Phase 2:

```bash
git checkout -b fix-issue-TBD
# after issue selection:
git branch -m fix-issue-<NUMBER>-<short-kebab-description>
```

Verify:

```bash
git branch --show-current
```

**Convention:** [branch-not-main](../onboarding-acme/conventions-overview.md#branch-not-main)

**Gate:** on a correctly named feature branch (not `main`).

---

## Phase 2: Issue discovery

**Do:** follow [issue-discovery.md](issue-discovery.md). Issues come from **huggingface/diffusers** for reference only — do not comment on them and do not check for existing PRs (demo exercise).

```bash
gh issue list --repo huggingface/diffusers \
  --label "good first issue" --state open --limit 15 \
  --json number,title,body,labels,assignees,url
```

Rank and present top 3–5 via `AskUserQuestion`. After selection, rename the branch if needed (see Phase 1c).

**Gate:** user selects an issue (or pastes one manually).

---

## Phase 3: Analyze and plan

**Do:**

1. Fetch full issue (read-only reference):
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

- **Issue (reference):** #N — <title> (<huggingface issue url>)
- **Target repo:** andrewwoj/diffusers
- **Branch:** fix-issue-<N>-<short-kebab-description>
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

**Do:** run tests **inside the Docker container** from Phase 1b.

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

**Do:** inside the Docker container (or host if tooling is only on host):

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
- [ ] Branch is not main and follows fix-issue-<N>-<description> naming
- [ ] origin points to andrewwoj/diffusers
- [ ] make style && make quality pass
- [ ] pytest output saved for PR description
- [ ] self-review skill run — blocking issues fixed
- [ ] PR description will include: reference issue link (huggingface), test commands + output, summary of change
- [ ] No large binary files committed
- [ ] PR will be opened on andrewwoj/diffusers — NOT huggingface/diffusers
```

When the user asks to open the PR:

```bash
git push -u origin HEAD
gh pr create --repo andrewwoj/diffusers \
  --title "<summary>" \
  --body "<description with reference to huggingface/diffusers#N>"
```

**Never** run `gh pr create --repo huggingface/diffusers` or comment on huggingface issues.

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
