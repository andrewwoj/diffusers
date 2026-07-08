---
name: first-contribution
description: >
  Demo workflow: find a good first issue on huggingface/diffusers (reference only),
  guide a contributor through Docker setup, planning, implementing, testing, and
  PR prep on andrewwoj/diffusers. Starts with a welcome and phase roadmap; supports
  Developer, QA, PM, and DevOps participants with role-specific attention callouts.
  Use when someone wants their first PR, asks to fix a good first issue, or is
  ready to contribute code in a demo session.
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

- **Start with Phase 0** — deliver the welcome and phase roadmap (and ask the user's role) before touching the environment or listing issues.
- **Demo only** — **never** open a PR on `huggingface/diffusers`. **Never** comment on issues there. All PRs target `andrewwoj/diffusers`.
- **Gate each phase** — wait for user confirmation before proceeding (unless user said "proceed through all steps")
- **Explain what and why at every phase** — open each phase by saying what is about to happen and why the project does it this way, not just the commands. Phase intros below provide the material.
- **Voice the role spotlight** — at each phase, surface the callout for the user's role (from Phase 0) using the [Role spotlights](#role-spotlights) table. Other roles' callouts stay in the table for reference.
- **Never commit or open a PR** unless the user explicitly asks
- **Teach conventions in context** — this is a first contribution to a large codebase. At each phase (and before each edit category in Phase 4), explain conventions using **where → why → link**:
  - **Where** — the file, phase step, or decision point where the convention applies
  - **Why** — one sentence on what goes wrong if ignored (review rejection, silent bugs, CI failure)
  - **Link** — anchor in [conventions-overview.md](../onboarding-acme/conventions-overview.md) or the domain guide ([models.md](../../models.md), [pipelines.md](../../pipelines.md), [modular.md](../../modular.md))
- Do not dump the full conventions doc — point to the 1–3 conventions relevant to the current step. Deeper questions → [onboarding-acme](../onboarding-acme/SKILL.md).
- **Handoffs:** convention questions → [onboarding-acme](../onboarding-acme/SKILL.md); pre-PR review → [self-review](../self-review/SKILL.md)

---

## Conventions by phase (quick map)

Use this map to decide what to explain at each step. Full definitions: [conventions-overview.md](../onboarding-acme/conventions-overview.md).

| Phase | Convention | Where it applies | Why it matters on a first PR |
|-------|------------|------------------|------------------------------|
| 1c | [branch-not-main](../onboarding-acme/conventions-overview.md#branch-not-main) | `git checkout -b fix-issue-…` | PRs must come from a feature branch — commits on `main` are not mergeable |
| 1b | [scoped-tests](../onboarding-acme/conventions-overview.md#scoped-tests) (setup) | Docker + `pip install -e ".[dev,test]"` | Tests you run in Phase 5 must match CI's Python/deps — host envs vary |
| 1 | [ai-convention-layer](../onboarding-acme/conventions-overview.md#ai-convention-layer) | `make cursor` on host | Wires `.ai/skills/` and `.ai/rules/` so agents (and you) can self-serve conventions without asking maintainers |
| 2 | [one-problem-per-pr](../onboarding-acme/conventions-overview.md#one-problem-per-pr) | Issue ranking / selection | Good first issues are single-file or single-component — broad features stall in review |
| 2 | [coordinate-first](../onboarding-acme/conventions-overview.md#coordinate-first) | Issue choice (reference only in demo) | On the real repo, maintainers confirm scope before you invest days of work |
| 3 | Domain guides | Task Plan → **Domain guide** row | Models, pipelines, and modular code each have different file layout and gotchas |
| 3 | [philosophy](../onboarding-acme/conventions-overview.md#philosophy) | Hypothesis / approach | Smallest readable fix beats a clever refactor — reviewers expect minimal diffs |
| 4 | [one-problem-per-pr](../onboarding-acme/conventions-overview.md#one-problem-per-pr) | Every edit | Drive-by fixes in unrelated files get rejected even if correct |
| 4 | [no-defensive-code](../onboarding-acme/conventions-overview.md#no-defensive-code) | Error handling / fallbacks | Silent workarounds hide bugs; explicit errors match project style |
| 4 | [inline-helpers](../onboarding-acme/conventions-overview.md#inline-helpers) | Temptation to extract helpers | Reviewers read the diff top-to-bottom — jumping between one-off helpers slows review |
| 4 | [copied-from](../onboarding-acme/conventions-overview.md#copied-from) | Copying `__init__` / `encode_prompt` from a sibling | Near-duplicate code must stay linked or it drifts silently |
| 4 | [pipeline-no-grad](../onboarding-acme/conventions-overview.md#pipeline-no-grad) | Pipeline `__call__` | Missing `@torch.no_grad()` causes GPU OOM during inference |
| 4 | [lazy-imports](../onboarding-acme/conventions-overview.md#lazy-imports) | New public class in `src/diffusers/` | Unregistered classes fail at import time for users, not in your local test |
| 5 | [scoped-tests](../onboarding-acme/conventions-overview.md#scoped-tests) | `pytest tests/<area>/…` | Full suite is slow; area tests give fast feedback and are what you cite in the PR |
| 6 | [make-style](../onboarding-acme/conventions-overview.md#make-style) | `make style` / `make quality` | CI runs the same checks — failing lint blocks merge regardless of correctness |
| 6 | [copied-from](../onboarding-acme/conventions-overview.md#copied-from) | After editing a `# Copied from` block | Run `make fix-copies` or CI will report out-of-sync duplicates |
| 7 | [self-review](../onboarding-acme/conventions-overview.md#self-review) | Pre-PR diff review | Same rubric as `@claude` CI — catches dead code and convention violations early |
| 8 | [one-problem-per-pr](../onboarding-acme/conventions-overview.md#one-problem-per-pr) | PR title and description | Title should describe one change; body links the reference issue and test proof |

**Domain guide routing (Phase 3):**

| If the issue touches… | Read first | Common gotcha |
|-----------------------|------------|---------------|
| `src/diffusers/models/` | [models.md](../../models.md) | Attention blocks, config mixins, lazy import registration |
| `src/diffusers/pipelines/` or `examples/community/` | [pipelines.md](../../pipelines.md) | `register_modules`, `@torch.no_grad()`, `# Copied from` |
| `src/diffusers/modular_pipelines/` | [modular.md](../../modular.md) | Block composition — not monolithic `__call__` |
| Schedulers, loaders, utils only | [conventions-overview.md](../onboarding-acme/conventions-overview.md) | General style rules; grep nearest sibling file |

---

## Role spotlights

The workflow is written for contributors, but QA engineers, product managers, and DevOps engineers get real value from walking through it — each phase demonstrates something their role owns in a normal project. Voice the relevant callout when entering each phase.

| Phase | QA engineer | Product manager | DevOps engineer |
|-------|-------------|-----------------|-----------------|
| 1 — Environment | Docker container is where all tests run — same env as CI, so results are reproducible | Setup cost is a one-time investment; everything after runs against a known-good baseline | **Core section:** why the CI image doubles as the dev environment, bind-mounts, editable installs, image variants in `docker/` |
| 2 — Issue discovery | What makes an issue *testable*: clear repro steps, expected vs actual behavior, single component | **Core section:** how issues are ranked and scoped — one-problem-per-pr is a scoping tool, not just a review rule | Issues touching CI, packaging, or Docker land in `.github/workflows/`, `setup.py`, `docker/` |
| 3 — Analyze and plan | The Task Plan's "Tests to run / Tests to add" rows are the QA contract for the change | **Core section:** the Task Plan is a lightweight spec — hypothesis, scope, and acceptance criteria in one block | The Domain guide row shows how the repo routes work by area instead of tribal knowledge |
| 4 — Implement | Watch for edits that change behavior without a test to catch regressions | Smallest-fix discipline keeps the change shippable and reviewable in one pass | Edits happen on the host; the bind-mount makes them live inside the container instantly |
| 5 — Tests | **Core section:** scoped tests vs full suite, `@slow`/`RUN_SLOW` gating, saved output as PR evidence | Passing scoped tests are the "definition of done" evidence attached to the PR | Test commands here are the same ones CI runs — the container guarantees parity |
| 6 — Format and lint | Lint failures block merge regardless of test results — this is a quality gate, not cosmetics | Automated style means review time goes to correctness, not formatting debates | **Core section:** `make quality` is the exact CI check run locally — a failed CI round-trip becomes a 30-second local fix |
| 7 — Self-review | Same rubric as the `@claude` CI reviewer — an automated pre-merge quality pass | Shows how the project front-loads review so maintainer time is spent once | The review rubric is versioned in-repo (`.ai/review-rules.md`) — CI and local runs can't drift |
| 8 — PR prep | Test evidence (commands + output) is a required part of the PR body | **Core section:** anatomy of a reviewable PR — one problem, linked issue, test proof | The checklist mirrors what CI will verify on the PR: lint, tests, branch hygiene |

---

## Phase 0: Welcome and orientation

Before any commands or issue lists, deliver a welcome message covering the points below, then ask the user's role.

**1. What this is.** A guided demo of a complete first contribution to diffusers — from empty environment to PR-ready branch. Issues are picked from the real **huggingface/diffusers** tracker for realism, but all code and PRs go to the **andrewwoj/diffusers** demo fork, so nothing here touches the upstream project (see the repo table at the top of this skill).

**2. The roadmap.** Present all eight phases with one line each of *what* happens and *why* it exists:

| Phase | What happens | Why it exists |
|-------|--------------|---------------|
| 1 — Environment setup | Verify auth/remote, build a Docker dev container, create a feature branch | Tests must match CI's environment, and PRs must come from a named branch — fixing this later is painful |
| 2 — Issue discovery | List good-first-issues from huggingface/diffusers, rank them, user picks one | A first PR should be one well-scoped problem; picking well is half the work |
| 3 — Analyze and plan | Investigate the issue in the local code and write a Task Plan | A shared plan catches wrong hypotheses before code is written, when changes are cheap |
| 4 — Implement | Make the smallest fix, teaching conventions before each edit | Minimal diffs get reviewed and merged; sprawling ones stall |
| 5 — Tests | Run scoped tests in the container, add tests if behavior changed | Tests are the proof a reviewer needs that the fix works and stays fixed |
| 6 — Format and lint | `make style` / `make quality` (and `make fix-copies` if needed) | CI runs the same checks — passing them locally avoids a failed CI round-trip |
| 7 — Self-review | Review the diff against the project's review rules | Catches convention violations and dead code before a maintainer spends time on them |
| 8 — PR prep | Verify the pre-PR checklist; open the PR only if asked | A complete, single-problem PR with test evidence is what gets merged |

**3. How teaching works.** Explain that at each phase the workflow calls out the 1–3 project conventions that apply — where they apply, why they exist, and a link to read more. Deeper convention or codebase questions can branch to the [onboarding-acme](../onboarding-acme/SKILL.md) skill at any time.

**4. Ask the user's role** via `AskUserQuestion` (skip if already stated): Developer (contributor), QA engineer, Product manager, or DevOps engineer. All roles walk the same phases — the role determines which [Role spotlights](#role-spotlights) get emphasized. Non-developer roles are welcome: the flow doubles as a tour of how code, tests, CI, and review connect in this project.

**Gate:** welcome delivered and role known → proceed to Phase 1.

---

## Phase 1: Environment setup

**Why this phase exists:** everything later depends on a trustworthy environment. Tests (Phase 5) only mean something if they run against the same Python and dependency versions as CI; the PR (Phase 8) is only safe if `origin` points at the demo fork and the work sits on a feature branch. Doing these checks first turns end-of-workflow surprises into two-minute fixes now.

**Role spotlight:** core phase for **DevOps** — the CI image doubling as the dev environment, bind-mounts, editable installs, and the image variants in `docker/` are all here. See [Role spotlights](#role-spotlights).

Complete all three steps before **Phase 3**. Phase 2 (issue discovery) can run in parallel with Phase 1b/1c, but do not analyze, plan, or write code until the Phase 2 exit gate passes (see below).

### 1a. Repo and auth

**What and why:** confirm GitHub auth works and `origin` points at the demo fork before anything else. The remote check is a safety guarantee — with `origin` on `andrewwoj/diffusers`, an accidental `git push` or `gh pr create` cannot touch the upstream project. Auth is checked now, rather than discovered broken in Phase 8, because a failed push after hours of work is the most frustrating place to hit it.

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

**Conventions here:**

- **[branch-not-main](../onboarding-acme/conventions-overview.md#branch-not-main)** — *where:* before your first commit. *why:* all changes reach maintainers through PRs from a named branch; working on `main` mixes local experiments with the shared baseline.

### 1b. Docker container

**What and why:** build a dev container from the same image family CI uses (`docker/diffusers-pytorch-cpu`) instead of installing into a host venv. Diffusers has heavy, version-sensitive dependencies (PyTorch, transformers) — host environments drift, but the container pins the same Python and deps as CI, so "passes locally, fails in CI" mostly disappears. The repo is bind-mounted into the container, so edits made in Cursor on the host apply inside it instantly. The install is *editable* (`pip install -e`), meaning Python imports the mounted source directly — code changes take effect without reinstalling.

**Do:** build and enter the dev container before any code changes.

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

**Verify** (agent can run non-interactively; user runs `docker run -it ... bash` when they need a shell):

```bash
docker info   # daemon reachable — start Colima/Docker Desktop if this fails
docker images diffusers-dev --format '{{.Repository}}'   # image exists
docker run --rm -v "$(pwd)":/workspace -w /workspace diffusers-dev \
  bash -c 'pip install -e ".[dev,test]" && pytest --version'
```

**Gate:** Docker daemon is running, `diffusers-dev` image is built, `pip install -e` succeeded, and `pytest --version` works inside the container.

**Conventions here:**

- **[scoped-tests](../onboarding-acme/conventions-overview.md#scoped-tests)** — *where:* inside this container in Phases 5–6. *why:* diffusers has heavy deps (PyTorch, transformers); Docker matches CI so "passes locally, fails in CI" is less likely.
- **[ai-convention-layer](../onboarding-acme/conventions-overview.md#ai-convention-layer)** — *where:* `make cursor` on the **host**. *why:* links `.ai/skills/` and `.ai/rules/` into Cursor so convention docs stay one command away during the rest of the workflow.

### 1c. Feature branch

**What and why:** create a feature branch before picking an issue, so `main` stays a clean mirror of upstream from the very first edit. PRs are diffed against `main` — commits made on `main` itself are not mergeable and are painful to untangle later. The naming pattern ties the branch (and eventually the PR) to one specific issue, which is how reviewers and CI trace what a change is for.

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

**Convention:** [branch-not-main](../onboarding-acme/conventions-overview.md#branch-not-main) — *where:* branch name `fix-issue-<N>-<description>`. *why:* reviewers and CI tie a PR to one issue; predictable names make your demo PR easy to follow.

**Gate:** on a correctly named feature branch (not `main`).

---

## Phase 2: Issue discovery

**Why this phase exists:** a first PR lives or dies on issue selection. The goal is one well-scoped, reproducible problem that fits in a single review session — ranking issues against that bar *is* the work here, not just browsing the tracker.

**Role spotlight:** core phase for **PM** (issue ranking and scoping is product triage) and highly relevant for **QA** (a good issue has clear repro steps and expected-vs-actual behavior). See [Role spotlights](#role-spotlights).

**Do:** follow [issue-discovery.md](issue-discovery.md). Issues come from **huggingface/diffusers** for reference only — do not comment on them and do not check for existing PRs (demo exercise).

```bash
gh issue list --repo huggingface/diffusers \
  --label "good first issue" --state open --limit 15 \
  --json number,title,body,labels,assignees,url
```

Rank and present top 3–5 via `AskUserQuestion`. After selection, rename the branch if needed (see Phase 1c).

**Conventions here:**

- **[one-problem-per-pr](../onboarding-acme/conventions-overview.md#one-problem-per-pr)** — *where:* issue ranking. *why:* prefer issues with a clear repro, single file, or `bug` label — a first PR should fit in one review session, not sprawl across the tree.
- **[coordinate-first](../onboarding-acme/conventions-overview.md#coordinate-first)** — *where:* real contributions on huggingface/diffusers (demo skips this). *why:* maintainers may already be working on the issue or want a different approach — worth knowing before you treat an issue as "yours."

**Gate:** user selects an issue (or pastes one manually).

### Phase 2 exit gate — environment ready (required before Phase 3)

Do **not** start Phase 3 until **all** Phase 1 checks pass. Re-run verification even if Phase 1 was discussed earlier:

```bash
# 1a — auth and remote
gh auth status
git remote -v   # origin → andrewwoj/diffusers

# 1b — Docker daemon, image, and editable install
docker info
docker run --rm -v "$(pwd)":/workspace -w /workspace diffusers-dev \
  bash -c 'pip install -e ".[dev,test]" && pytest --version'

# 1c — feature branch (rename after issue selection if still fix-issue-TBD)
git branch --show-current   # not main; ideally fix-issue-<N>-<short-kebab-description>
```

If any check fails, stop and complete the missing Phase 1 step. Common fixes:

| Failure | Fix |
|---------|-----|
| `Cannot connect to the Docker daemon` | `colima start` or start Docker Desktop / OrbStack |
| `diffusers-dev` image missing | `docker build -t diffusers-dev docker/diffusers-pytorch-cpu` |
| Branch still `fix-issue-TBD` | `git branch -m fix-issue-<NUMBER>-<short-kebab-description>` |

**Gate:** Phase 1a, 1b, and 1c all pass → proceed to Phase 3.

---

## Phase 3: Analyze and plan

**Why this phase exists:** writing the Task Plan before touching code catches wrong hypotheses when they cost nothing to fix. The plan names the files, the reference implementation to imitate, the tests that define "done", and the conventions in play — so implementation becomes execution rather than exploration.

**Role spotlight:** core phase for **PM** (the Task Plan is a lightweight spec: hypothesis, scope, acceptance criteria) and for **QA** (the "Tests to run / Tests to add" rows are the QA contract for the change). See [Role spotlights](#role-spotlights).

**Prerequisite:** Phase 2 exit gate (Phase 1 complete) must pass. If Docker or the feature branch is not ready, finish Phase 1 before producing the Task Plan.

**Do:**

1. Fetch full issue (read-only reference):
   ```bash
   gh issue view <NUMBER> --repo huggingface/diffusers \
     --json title,body,labels,comments,url
   ```
2. Grep the local repo for affected files
3. Skim the nearest reference implementation in the same area
4. Identify domain guide if code touches models/pipelines/modular code (see [Domain guide routing](#conventions-by-phase-quick-map) above)
5. Output a **Task Plan** — include a **Conventions that apply** section with *where* and *why* for each (not just anchor links):

```markdown
## Task Plan

- **Issue (reference):** #N — <title> (<huggingface issue url>)
- **Target repo:** andrewwoj/diffusers
- **Branch:** fix-issue-<N>-<short-kebab-description>
- **Hypothesis:** <root cause or approach>
- **Files to touch:** <list>
- **Domain guide:** <models.md / pipelines.md / modular.md / none> — <one line: why this guide for these files>
- **Reference implementation:** <nearest sibling file to skim before editing>
- **Tests to run:** pytest tests/<area>/test_<relevant>.py
- **Tests to add/update:** <yes/no + what>
- **Conventions that apply:**
  - [<name>](../onboarding-acme/conventions-overview.md#<anchor>) — **where:** <file or step>; **why:** <one sentence for this issue>
  - …
```

**Conventions here:**

- **[philosophy](../onboarding-acme/conventions-overview.md#philosophy)** — *where:* hypothesis and file list. *why:* the fix should be the smallest change a reader can follow — not a refactor "while we're here."
- **Domain guides** — *where:* before grep/edit. *why:* pipelines, models, and modular blocks look similar but have different inheritance, registration, and test layout; copying the wrong sibling causes review churn.

**Gate:** wait for explicit user approval of the Task Plan before writing code.

---

## Phase 4: Implement

**Why this phase exists:** the code change itself — kept deliberately small. Reviewers merge minimal diffs they can follow top-to-bottom; every convention taught here (no drive-by fixes, no defensive fallbacks, no one-off helpers) exists to keep the diff readable in one pass.

**Role spotlight:** **DevOps** — edits happen on the host, and the bind-mount from Phase 1b makes them live inside the container instantly. **QA** — watch for behavior changes that lack a test to catch regressions. See [Role spotlights](#role-spotlights).

**Do:**

- Make the **smallest fix** that solves the issue — no unrelated refactors ([one-problem-per-pr](../onboarding-acme/conventions-overview.md#one-problem-per-pr))
- Before editing: skim the **reference implementation** named in the Task Plan (e.g. sibling pipeline in the same folder)
- **Before each edit**, tell the user which convention applies using *where → why → link* (see table below)

| If you… | Convention | Where | Why |
|---------|------------|-------|-----|
| Copy `__init__`, `encode_prompt`, or similar from another file | [#copied-from](../onboarding-acme/conventions-overview.md#copied-from) | Top of copied block | Without the header, your copy drifts when the source changes — Phase 6 needs `make fix-copies` |
| Touch pipeline `__call__` | [#pipeline-no-grad](../onboarding-acme/conventions-overview.md#pipeline-no-grad) | `@torch.no_grad()` on `__call__` | Inference must not build autograd graphs — missing decorator → OOM |
| Add a new public class under `src/diffusers/` | [#lazy-imports](../onboarding-acme/conventions-overview.md#lazy-imports) | Subpackage + top-level `__init__.py` | Users import from `diffusers` — unregistered classes raise `ImportError` in the wild |
| Consider a fallback or "just in case" branch | [#no-defensive-code](../onboarding-acme/conventions-overview.md#no-defensive-code) | `if/else` around bad inputs | Reviewers prefer a clear error in the docstring over silent correction |
| Factor out a helper used once | [#inline-helpers](../onboarding-acme/conventions-overview.md#inline-helpers) | New `_foo()` or module function | One-off helpers force reviewers to jump around the diff |
| Edit `examples/community/` pipeline `__init__` | [pipelines.md](../../pipelines.md) — `register_modules` | `super().__init__()` + keyword `register_modules(...)` | Positional `super().__init__(vae, …, requires_safety_checker)` breaks when the parent signature adds parameters |
| Change only community examples, not `src/` | [#one-problem-per-pr](../onboarding-acme/conventions-overview.md#one-problem-per-pr) | Scope of diff | Stay within files listed in the Task Plan |

**Gate:** show the diff summary, cite which conventions guided the edits, and ask user to review before running tests.

---

## Phase 5: Tests

**Why this phase exists:** tests are the proof a reviewer needs that the fix works and stays fixed. They run inside the Phase 1b container so the results match what CI will see, and the saved output becomes evidence in the PR body.

**Role spotlight:** core phase for **QA** — scoped tests vs the full suite, `@slow`/`RUN_SLOW=1` gating for tests that download large models, and test output as PR evidence. See [Role spotlights](#role-spotlights).

**Do:** run tests **inside the Docker container** from Phase 1b.

1. Run existing tests for the touched area:
   ```bash
   pytest tests/<area>/test_<relevant>.py -v
   ```
2. Add or update tests if the issue is a bug fix or adds behavior
3. Reference nearest test file for patterns (e.g. `tests/schedulers/test_scheduler_tcd.py`)

**Convention:** [scoped-tests](../onboarding-acme/conventions-overview.md#scoped-tests) — *where:* `pytest tests/<area>/test_<relevant>.py`. *why:* the full suite can take hours; area tests prove your change without blocking on unrelated failures. Save the command + output for the PR body (Phase 8).

**Gate:** all relevant tests pass — paste output for the PR description later.

---

## Phase 6: Format and lint

**Why this phase exists:** CI runs the exact same `make quality` check on every PR — running it locally turns a failed CI round-trip (push, wait, read logs, fix, push again) into a 30-second local fix. Style is automated precisely so human review time goes to correctness.

**Role spotlight:** core phase for **DevOps** — this is local/CI parity in action: the Makefile targets are the CI check, versioned in the repo. See [Role spotlights](#role-spotlights).

**Do:** inside the Docker container (or host if tooling is only on host):

```bash
make style
make quality
make fix-copies   # only if a # Copied from source was edited
```

**Conventions here:**

- **[make-style](../onboarding-acme/conventions-overview.md#make-style)** — *where:* before every PR. *why:* `@claude` and CI run `make quality` — style nits block merge even when logic is correct.
- **[copied-from](../onboarding-acme/conventions-overview.md#copied-from)** — *where:* only if you edited a `# Copied from` block. *why:* `make fix-copies` propagates your change to linked copies; skipping it leaves the repo inconsistent.

**Gate:** `make quality` exits clean.

---

## Phase 7: Self-review

**Why this phase exists:** after hours in a diff, the author is blind to its problems. This pass applies the same rubric the `@claude` CI reviewer uses, so convention violations and dead code get fixed before a maintainer spends time finding them.

**Role spotlight:** **QA** — an automated pre-merge quality gate. **DevOps** — the rubric lives in-repo (`.ai/review-rules.md`), so local and CI review can't drift. See [Role spotlights](#role-spotlights).

**Do:** run the [self-review](../self-review/SKILL.md) skill against `git diff main...HEAD`.

Fix all blocking findings. Note intentional skips for the PR description.

**Convention:** [self-review](../onboarding-acme/conventions-overview.md#self-review) — *where:* `git diff main...HEAD`. *why:* catches dead code paths and convention violations the author is blind to after hours in the diff — same checklist CI uses.

**Gate:** verdict is **READY** (or blocking items explicitly deferred with user approval).

---

## Phase 8: PR prep

**Why this phase exists:** a PR is an argument that the change is safe to merge — one problem, a linked issue, and test evidence. The checklist verifies everything CI and a reviewer will look for, so the PR is complete on the first submission.

**Role spotlight:** core phase for **PM** — the anatomy of a reviewable PR. **QA** — test commands and output are a required part of the body. **DevOps** — the checklist mirrors what CI verifies on the PR. See [Role spotlights](#role-spotlights).

**Do:** verify this checklist — do **not** open the PR unless the user asks:

```
- [ ] Branch is not main and follows fix-issue-<N>-<description> naming  → branch-not-main
- [ ] origin points to andrewwoj/diffusers
- [ ] make style && make quality pass  → make-style
- [ ] pytest output saved for PR description  → scoped-tests
- [ ] self-review skill run — blocking issues fixed  → self-review
- [ ] PR description: reference issue link (huggingface), test commands + output, summary of change, conventions considered
- [ ] No large binary files committed
- [ ] PR will be opened on andrewwoj/diffusers — NOT huggingface/diffusers
- [ ] Single problem in title/body  → one-problem-per-pr
```

**Conventions here:**

- **[one-problem-per-pr](../onboarding-acme/conventions-overview.md#one-problem-per-pr)** — *where:* PR title and Summary section. *why:* reviewers decide merge in one pass; mixed concerns get "please split this PR" feedback.
- **[coordinate-first](../onboarding-acme/conventions-overview.md#coordinate-first)** — *where:* PR body links `huggingface/diffusers#N` as reference. *why:* shows you understand the upstream issue without claiming merge to the main repo in this demo.

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
| Why a convention exists (full list) | [conventions-overview.md](../onboarding-acme/conventions-overview.md) |
| Which convention at which phase | [Conventions by phase](#conventions-by-phase-quick-map) (this skill) |
| Pipeline / model / modular rules | [pipelines.md](../../pipelines.md), [models.md](../../models.md), [modular.md](../../modular.md) |
| Self-serve lookup | [self-serve.md](../onboarding-acme/self-serve.md) |
| Issue search commands | [issue-discovery.md](issue-discovery.md) |
| Pre-PR review | [self-review](../self-review/SKILL.md) |
| Add a new model (not a good first issue) | [model-integration](../model-integration/SKILL.md) |
