# Conventions overview

What diffusers conventions are, why they exist, and where to read more. Linked from the [onboarding-acme](SKILL.md) and [first-contribution](../first-contribution/SKILL.md) skills.

Full domain guides: [models.md](../../models.md), [pipelines.md](../../pipelines.md), [modular.md](../../modular.md), [review-rules.md](../../review-rules.md).

---

## philosophy

**What:** The library prioritizes usability over raw performance, simplicity over convenience abstractions, and customizability over heavy encapsulation.

**Why:** Diffusers serves researchers, application developers, and integrators with different needs. A tweakable toolbox beats a rigid framework — users should be able to read and modify inference code without fighting the library.

**Read more:** [Design Philosophy](https://huggingface.co/docs/diffusers/conceptual/philosophy)

---

## ai-convention-layer

**What:** Project conventions live in `.ai/` — `AGENTS.md`, domain guides (`models.md`, `pipelines.md`, `modular.md`), task skills under `.ai/skills/`, and Cursor rules under `.ai/rules/`. Wire skills and Cursor rules locally with `make cursor`; wire skills for Claude Code or Codex with `make claude` or `make codex`.

**Why:** Conventions are written for both humans and AI agents. You can self-serve coding rules without asking maintainers — load the relevant guide or skill for your task.

**Read more:** [AGENTS.md](../../AGENTS.md), [CONTRIBUTING.md — Coding with AI agents](../../../CONTRIBUTING.md)

---

## copied-from

**What:** Identical code blocks link via a `# Copied from ...` header. Edit the source, then run `make fix-copies` to propagate. Do not edit copied blocks directly.

**Why:** Keeps duplicated snippets in sync without hidden abstractions. Reviewers can trace where shared logic lives.

**Read more:** [AGENTS.md](../../AGENTS.md) — Copied Code section

---

## no-defensive-code

**What:** No fallback paths "just in case", no unused parameters for API consistency, no deprecation shims for unreleased code. Unsupported inputs get a clear error in the docstring — not silent correction.

**Why:** Defensive code hides bugs and makes behavior unpredictable. Explicit failures are easier to debug than silent workarounds.

**Read more:** [AGENTS.md](../../AGENTS.md) — Coding style

---

## inline-helpers

**What:** Prefer inlining small helpers with one caller at the call site instead of factoring them into private methods.

**Why:** A reader should follow the full flow in one place without jumping between functions.

**Read more:** [AGENTS.md](../../AGENTS.md) — Coding style

---

## modular-preferred

**What:** New pipelines default to [Modular Diffusers](../../modular.md) — composable blocks (`encoders.py`, `before_denoise.py`, `denoise.py`, `decoders.py`) instead of monolithic `__call__` methods. Standard `DiffusionPipeline` is still supported.

**Why:** Blocks are standalone and reusable (encode once, denoise later). Easier to test, extend, and compose for evolving model families.

**Read more:** [modular.md](../../modular.md)

---

## coordinate-first

**What:** Find or open a GitHub issue, comment that you'd like to work on it, and wait for maintainer acknowledgment before opening a PR.

**Why:** Avoids duplicate work and lets maintainers confirm scope before review investment. PRs without coordination may be closed without detailed review.

**Read more:** [CONTRIBUTING.md — AI-assisted contributions](../../../CONTRIBUTING.md)

---

## one-problem-per-pr

**What:** Each pull request solves exactly one problem. No drive-by fixes in unrelated files.

**Why:** Focused PRs are reviewable. Mixed changes force reviewers to untangle unrelated diffs.

**Read more:** [CONTRIBUTING.md — How to write a good PR](../../../CONTRIBUTING.md)

---

## branch-not-main

**What:** Never commit directly to `main`. Create a feature branch for every change.

**Why:** Standard git workflow — keeps main stable and makes PRs traceable.

**Read more:** [CONTRIBUTING.md — How to open a PR](../../../CONTRIBUTING.md)

---

## make-style

**What:** Run `make style` before opening a PR (auto-fix) and `make quality` to verify (check-only). Ruff handles Python formatting; doc-builder checks docstrings.

**Why:** Style is automated so reviewers focus on correctness ([review-rules.md](../../review-rules.md)). CI runs the same checks.

**Read more:** [Makefile](../../../Makefile) — `style` and `quality` targets

---

## scoped-tests

**What:** Run tests for the area you changed: `pytest tests/<area>/test_<relevant>.py -v`. Full suite via `make test` when needed.

**Why:** The full test suite is large and slow. Scoped tests give fast feedback during development.

**Read more:** [CONTRIBUTING.md — Tests](../../../CONTRIBUTING.md)

---

## self-review

**What:** Before opening a PR, review your diff against [review-rules.md](../../review-rules.md) using the [self-review](../self-review/SKILL.md) skill — same rubric as the `@claude` CI reviewer.

**Why:** Catches convention violations and dead code before a maintainer spends review time.

**Read more:** [self-review/SKILL.md](../self-review/SKILL.md)

---

## lazy-imports

**What:** Every new public class must be registered in both the sub-package `__init__.py` and the top-level `src/diffusers/__init__.py` lazy import structure.

**Why:** Missing registration causes `ImportError` only when users call `from diffusers import YourClass`.

**Read more:** [models.md](../../models.md) — gotcha #1

---

## pipeline-no-grad

**What:** Pipeline `__call__` methods must use `@torch.no_grad()`. Do not add redundant inner `with torch.no_grad():` blocks in helpers that `__call__` already covers.

**Why:** Missing the decorator causes GPU OOM from gradient accumulation during inference.

**Read more:** [pipelines.md](../../pipelines.md) — gotcha #2
