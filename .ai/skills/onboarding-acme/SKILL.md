---
name: onboarding-acme
description: >
  Introduce the diffusers codebase, conventions, and self-serve resources.
  Use when someone is new to the project, asks how the repo is organized,
  wants to understand conventions, or is a PM, QA engineer, or DevOps engineer
  exploring the codebase. Informational only — does not make code changes.
  Named onboarding-acme to avoid clashing with Cursor's built-in onboarding skill.
---

# Onboarding (ACME)

**Mode: informational only.** Explain and orient — do not edit files, run tests, pick issues, open PRs, or make commits.

If the user wants to contribute code, hand off to the [first-contribution](../first-contribution/SKILL.md) skill after this tour. That skill is not just for developers — it has role spotlights for QA, PM, and DevOps, so any role can walk a real change from environment setup to PR prep with their own lens highlighted.

---

## Step 1 — Ask the user's role

Use `AskUserQuestion` to tailor the tour:

| Option | Audience |
|--------|----------|
| Product manager | Feature landscape, roadmap, user-facing capabilities |
| QA engineer | Tests, CI, reproducing bugs, issue reporting |
| DevOps engineer | Docker, CI workflows, packaging, dependencies |
| Developer (contributor) | Architecture, conventions, skills, path to first PR |

If the user already stated their role, skip the question.

---

## Step 2 — Codebase map

Present this overview for every role (adjust depth to the audience):

### What diffusers is

ACME's shared library for **pretrained diffusion models** — text-to-image, image-to-video, audio, and more. Three interchangeable core components:

| Component | Location | Role |
|-----------|----------|------|
| **Pipelines** | `src/diffusers/pipelines/` | End-to-end inference (prompt → output) |
| **Models** | `src/diffusers/models/` | Building blocks: UNets, transformers, VAEs, controlnets |
| **Schedulers** | `src/diffusers/schedulers/` | Noise schedules and denoising steps |

### Repository layout

```
diffusers/
├── src/diffusers/       # Library source (~975 Python files)
├── tests/               # pytest suite (~579 test files)
├── examples/            # Training scripts + community pipelines
├── docs/source/         # Multi-language documentation
├── scripts/             # Model conversion scripts
├── docker/              # Docker images for CI and dev
├── .github/workflows/   # CI/CD
└── .ai/                 # Agent conventions, domain guides, skills
```

### Convention system (self-serve layer)

Maintainers document rules in `.ai/` so contributors and agents can find answers without tribal knowledge:

| Path | Purpose |
|------|---------|
| [AGENTS.md](../../AGENTS.md) | Coding philosophy, formatting, skills index |
| [models.md](../../models.md) | Model implementation rules |
| [pipelines.md](../../pipelines.md) | Standard pipeline conventions |
| [modular.md](../../modular.md) | Modular pipeline conventions (preferred for new work) |
| [review-rules.md](../../review-rules.md) | What PR reviewers check |
| `.ai/skills/` | Task-specific workflows loaded on demand |

Wire agent config locally: `make cursor` (Cursor — skills + rules), `make claude` (Claude Code), `make codex` (Codex).

---

## Step 3 — Role-specific tour

Deliver 5–10 bullets for the selected role. Link to [self-serve.md](self-serve.md) for the full lookup table.

### Product manager

- **User-facing surface:** `DiffusionPipeline.from_pretrained(...)` — load pretrained checkpoints for inference
- **Model landscape:** ~90 pipeline families in `src/diffusers/pipelines/` (Flux, SD3, Wan, Hunyuan Video, etc.)
- **Roadmap tracking:** [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1) — GitHub Project board for planned features and priorities
- **Core vs community:** Core library is inference-only; training examples live in `examples/`; community pipelines in `examples/community/`
- **Feature requests:** [Feature request template](https://github.com/huggingface/diffusers/issues/new?template=feature_request.md)
- **See a change ship end-to-end:** the [first-contribution](../first-contribution/SKILL.md) skill supports PMs with role spotlights — issue ranking/scoping (Phase 2), the Task Plan as a lightweight spec (Phase 3), and the anatomy of a reviewable PR (Phase 8)
- **Self-serve next:** [self-serve.md — Product managers](self-serve.md#product-managers)

### QA engineer

- **Try it first:** run `pytest tests/schedulers/ -v` to verify your setup — schedulers are fast and don't download models
- **Test layout:** `tests/pipelines/`, `tests/models/`, `tests/schedulers/`, `tests/lora/`, `tests/modular_pipelines/`
- **Run scoped tests:** `pytest tests/pipelines/<model>/ -v` — fast feedback for one area
- **Run full suite:** `make test` — requires beefy machine; uses pytest-xdist
- **Slow tests:** gated on `@slow` / `RUN_SLOW=1` — downloads large models; runs nightly in CI
- **Bug reports:** must include minimal repro + `diffusers-cli env` output (CONTRIBUTING §2.1)
- **CI:** `.github/workflows/` — ruff, pytest, doc-builder, security scans
- **See how tests gate a real change:** the [first-contribution](../first-contribution/SKILL.md) skill supports QA with role spotlights — issue reproducibility (Phase 2), the test contract in the Task Plan (Phase 3), and scoped tests as PR evidence (Phase 5)
- **Self-serve next:** [self-serve.md — QA engineers](self-serve.md#qa-engineers)

### DevOps engineer

- **Docker:** `docker/` — PyTorch CPU/CUDA, ONNX, xformers, minimum-cuda, doc-builder variants
- **CI/CD:** `.github/workflows/` — test matrix, lint, release automation
- **Local CI parity:** `make quality` (check), `make style` (fix)
- **Packaging:** `setup.py` + `pyproject.toml` (ruff config); editable install via `pip install -e ".[dev]"`
- **Dependencies:** `src/diffusers/dependency_versions_table.py` — update with `make deps_table_update`
- **Release:** Makefile targets `pre-release`, `post-release`, `pre-patch`, `post-patch`
- **See the build environment in use:** the [first-contribution](../first-contribution/SKILL.md) skill supports DevOps with role spotlights — the CI image as dev container with bind-mounts and editable installs (Phase 1), and local/CI parity via `make quality` (Phase 6)
- **Self-serve next:** [self-serve.md — DevOps engineers](self-serve.md#devops-engineers)

### Developer (contributor)

- **Architecture:** pipelines compose models + schedulers; modular pipelines split into reusable blocks
- **Learn by comparison:** skim reference files before writing — `pipeline_flux.py`, `transformer_wan.py`, `modular_pipelines/qwenimage/`
- **Convention guides:** [models.md](../../models.md), [pipelines.md](../../pipelines.md), [modular.md](../../modular.md)
- **Skills for tasks:** onboarding-acme (you are here) → first-contribution → model-integration → self-review
- **Formatting before PR:** `make style && make quality`
- **Coordination rule:** get maintainer acknowledgment on an issue before opening a PR
- **Self-serve next:** [self-serve.md — Developers](self-serve.md#developers-contributors)

---

## Step 4 — Conventions primer

Explain the top principles with **why** — full catalog in [conventions-overview.md](conventions-overview.md):

| Principle | Why it matters |
|-----------|----------------|
| [Philosophy](conventions-overview.md#philosophy) — usability, simplicity, customizability | Shapes every API and review decision |
| [AI convention layer](conventions-overview.md#ai-convention-layer) — `.ai/` guides + skills | Self-serve rules without asking maintainers |
| [No defensive code](conventions-overview.md#no-defensive-code) — fail loudly | Predictable behavior; easier debugging |
| [Modular preferred](conventions-overview.md#modular-preferred) — composable pipeline blocks | Reusable encode/denoise/decode steps |
| [Coordinate first](conventions-overview.md#coordinate-first) — issue acknowledgment before PR | Avoids duplicate work and wasted review |
| [Make style](conventions-overview.md#make-style) — automated formatting | Reviewers focus on correctness, not style |
| [Scoped tests](conventions-overview.md#scoped-tests) — `pytest tests/<area>/` | Fast local feedback loop |

As you work through issues, reference area-specific conventions when you need them — don't memorize them upfront:

| Area | Guide |
|------|-------|
| Models | [models.md](../../models.md) |
| Pipelines | [pipelines.md](../../pipelines.md) |
| Modular pipelines | [modular.md](../../modular.md) |

---

## Step 5 — Self-serve highlights

Point the user to [self-serve.md](self-serve.md) as their ongoing reference. Highlight these entries (prioritize **Run tests** for QA engineers):

- **Run tests** → `pytest tests/<area>/`, `make test` — QA: start with `pytest tests/schedulers/ -v`
- **Find work to do** → [GitHub Issues](https://github.com/huggingface/diffusers/issues), [good first issues](https://github.com/huggingface/diffusers/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)
- **Understand a convention** → [conventions-overview.md](conventions-overview.md)
- **Format code** → `make style`, `make quality`
- **Review before PR** → [self-review](../self-review/SKILL.md) skill
- **Add a model** → [model-integration](../model-integration/SKILL.md) skill
- **Fix a first issue** → [first-contribution](../first-contribution/SKILL.md) skill

Emphasize: **most answers live in `.ai/`, CONTRIBUTING.md, or the docs** — you rarely need to ask a maintainer for process questions.

---

## Step 6 — Close

Tailor the closing message to role:

- **Developer:** "When you're ready to contribute, run the **first-contribution** skill — it will find a good first issue and walk you through the fix."
- **PM:** "Track features on the [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1). To see how an issue becomes a shipped change, run the **first-contribution** skill — it highlights the scoping, planning, and PR phases for PMs."
- **QA:** "Run `pytest tests/schedulers/ -v` to verify your setup, then explore `pytest tests/<area>/ -v` for the model or component you're testing. When filing bugs, include `diffusers-cli env` + a minimal repro. To see how tests gate a real change, run the **first-contribution** skill — it highlights the test phases for QA."
- **DevOps:** "Use `make quality` locally to mirror CI lint checks. To see the Docker/CI-parity setup in action, run the **first-contribution** skill — it highlights the environment and lint phases for DevOps."

Ask if they want to dive deeper into any specific area before ending.
