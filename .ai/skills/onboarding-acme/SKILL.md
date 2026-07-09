---
name: onboarding-acme
description: >
  Introduce the diffusers codebase, conventions, and self-serve resources.
  Use when someone is new to the project, asks how the repo is organized,
  wants to understand conventions, or is a PM, QA engineer, or DevOps engineer
  exploring the codebase. Informational only — does not make code changes.
  After the tour, guide every role (including QA and DevOps) to the
  first-contribution skill so they learn the full contribution process.
  Named onboarding-acme to avoid clashing with Cursor's built-in onboarding skill.
---

# Onboarding (ACME)

**Mode: informational only.** Explain and orient — do not edit files, run tests, pick issues, open PRs, or make commits.

After this tour, guide **every role** — Developer, QA, PM, and DevOps — toward the [first-contribution](../first-contribution/SKILL.md) skill. That walkthrough is how they learn the full contribution process (environment → issue → plan → implement → test → PR), not only how to write code. It has role spotlights for QA, PM, and DevOps so each participant sees the same end-to-end path with their lens highlighted.

---

## Step 1 — Ask the user's role

Use `AskUserQuestion` to tailor the tour. Map the answer to a role file:

| Option | Role file | Audience |
|--------|-----------|----------|
| Product manager | [roles/product-manager.md](roles/product-manager.md) | Feature landscape, roadmap, user-facing capabilities |
| QA engineer | [roles/qa-engineer.md](roles/qa-engineer.md) | Tests, CI, reproducing bugs, issue reporting |
| DevOps engineer | [roles/devops-engineer.md](roles/devops-engineer.md) | Docker, CI workflows, packaging, dependencies |
| Developer (contributor) | [roles/developer.md](roles/developer.md) | Architecture, conventions, skills, path to first PR |

If the user already stated their role, skip the question and load the matching role file.

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

Read the role file from Step 1. Deliver the bullets under **Tour**. Do not invent content for other roles.

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

## Step 5 — Self-serve

Present the **Self-serve** table from the role file. Also point to [self-serve.md](self-serve.md) for the shared **Everyone** lookup table.

Emphasize: **most answers live in `.ai/`, CONTRIBUTING.md, or the docs** — you rarely need to ask a maintainer for process questions.

---

## Step 6 — Close

Deliver the **Closing** section from the role file.

Then explicitly invite them to run **first-contribution** next so they see the full process end-to-end — not only the role-specific highlights. Frame it as the natural next step for every role (including QA and DevOps), not an optional deep-dive for developers only. Ask if they want to start that skill now, or dive deeper into any onboarding area first.

---

## Adding a role

1. Copy [roles/_template.md](roles/_template.md) to `roles/<slug>.md`
2. Fill **Tour**, **Self-serve**, and **Closing**
3. Add one row to the Step 1 options table above
