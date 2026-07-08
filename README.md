<!---
Copyright 2022 - The HuggingFace Team. All rights reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

# Diffusers — ACME shared library

Diffusers is ACME's shared library for pretrained diffusion models — text-to-image, image-to-video, audio, and more. It provides pipelines, models, and schedulers as interchangeable components for inference and integration.

This repo is a maintained fork of the upstream [Hugging Face diffusers](https://github.com/huggingface/diffusers) codebase, extended with ACME contributor tooling under `.ai/`.

## What is Diffusers

Three core components:

| Component | Location | Role |
|-----------|----------|------|
| **Pipelines** | `src/diffusers/pipelines/` | End-to-end inference (prompt to output) |
| **Models** | `src/diffusers/models/` | Building blocks: UNets, transformers, VAEs, controlnets |
| **Schedulers** | `src/diffusers/schedulers/` | Noise schedules and denoising steps |

### Repository layout

```
diffusers/
├── src/diffusers/       # Library source
├── tests/               # pytest suite
├── examples/            # Training scripts + community pipelines
├── docs/source/         # Multi-language documentation
├── scripts/             # Model conversion scripts
├── docker/              # Docker images for CI and dev
├── .github/workflows/   # CI/CD
└── .ai/                 # Agent conventions, domain guides, skills
```

## Using the library

### Installation

For work in this repo, use an editable install:

```bash
pip install -e ".[dev,test]"
```

When consuming diffusers as a dependency in another project:

```bash
pip install --upgrade diffusers[torch]
```

For PyTorch setup details, see the [official PyTorch documentation](https://pytorch.org/get-started/locally/). Apple Silicon (M1/M2) guidance is in the [MPS optimization guide](https://huggingface.co/docs/diffusers/optimization/mps).

### Quickstart

```python
from diffusers import DiffusionPipeline
import torch

pipeline = DiffusionPipeline.from_pretrained("stable-diffusion-v1-5/stable-diffusion-v1-5", torch_dtype=torch.float16)
pipeline.to("cuda")
pipeline("An image of a squirrel in Picasso style").images[0]
```

Browse checkpoints on the [Hugging Face Hub](https://huggingface.co/models?library=diffusers&sort=downloads). See the [quick tour](https://huggingface.co/docs/diffusers/quicktour) for more.

### Reference documentation

External API reference for library consumers:

| Documentation | What you can learn |
|---------------|-------------------|
| [Tutorial](https://huggingface.co/docs/diffusers/tutorials/tutorial_overview) | Core features: models, schedulers, and building your own diffusion system |
| [Loading](https://huggingface.co/docs/diffusers/using-diffusers/loading) | Loading and configuring pipelines, models, and schedulers |
| [Pipelines for inference](https://huggingface.co/docs/diffusers/using-diffusers/overview_techniques) | Inference tasks, batched generation, output control |
| [Optimization](https://huggingface.co/docs/diffusers/optimization/fp16) | Speed and memory optimization |
| [Training](https://huggingface.co/docs/diffusers/training/overview) | Training diffusion models for different tasks |

## Getting started in this repo

This section is for ACME engineers contributing to or working inside the codebase. It applies to developers, QA engineers, product managers, and DevOps engineers.

### 1. Wire agent tooling

After cloning, link the convention layer into your AI agent:

```bash
make cursor   # Cursor: skills + rules
# make claude / make codex for other agents
```

See [`.ai/AGENTS.md`](.ai/AGENTS.md) for coding philosophy, formatting rules, and the skills index.

### 2. Run the onboarding-acme skill

**Purpose:** Informational codebase tour — no code changes.

The skill orients you by role (developer, QA, PM, or DevOps), walks through the repo layout and conventions, and points to self-serve resources. Run it before your first contribution if you are unfamiliar with the repo.

- Skill: [`.ai/skills/onboarding-acme/SKILL.md`](.ai/skills/onboarding-acme/SKILL.md)
- Conventions catalog: [`.ai/skills/onboarding-acme/conventions-overview.md`](.ai/skills/onboarding-acme/conventions-overview.md)
- Self-serve lookup: [`.ai/skills/onboarding-acme/self-serve.md`](.ai/skills/onboarding-acme/self-serve.md)

### 3. Run the first-contribution skill

**Purpose:** Guided end-to-end first PR on the demo fork (`andrewwoj/diffusers`).

Prerequisite: run **onboarding-acme** if you are new to the repo. The skill walks through environment setup, issue selection, implementation, testing, and PR prep. Each phase highlights conventions relevant to your role.

| Phase | What happens | Why it exists |
|-------|--------------|---------------|
| 1 — Environment | Docker dev container, feature branch, fork CI | Tests must match CI; PRs must come from a named branch |
| 2 — Issue discovery | Pick a scoped good-first issue | A first PR should be one well-scoped problem |
| 3 — Analyze and plan | Investigate the issue and write a Task Plan | Catches wrong hypotheses before code is written |
| 4 — Implement | Make the smallest fix | Minimal diffs get reviewed and merged |
| 5 — Tests | Run scoped tests in the container | Tests are proof a reviewer needs |
| 6 — Format and lint | `make style` / `make quality` | CI runs the same checks locally |
| 7 — Self-review | Review the diff against project rules | Catches convention violations before maintainer review |
| 8 — PR prep | Verify the pre-PR checklist; open PR when ready | A complete PR with test evidence gets merged |

- Skill: [`.ai/skills/first-contribution/SKILL.md`](.ai/skills/first-contribution/SKILL.md)
- Role spotlights: QA (tests and repro), PM (scoping and Task Plan), DevOps (Docker and CI parity)

### 4. Verify your setup (optional)

Quick smoke test (fast, no model downloads):

```bash
pytest tests/schedulers/ -v
```

For CI-parity development, build the dev container per first-contribution Phase 1:

```bash
docker build -t diffusers-dev docker/diffusers-pytorch-cpu
```

## Ongoing contribution

For engineers past their first PR.

### Skill progression

```
onboarding-acme → first-contribution → model-integration / self-review
```

| Task | Skill / doc |
|------|-------------|
| Add or convert a model/pipeline | [model-integration](.ai/skills/model-integration/SKILL.md) + [models.md](.ai/models.md) / [pipelines.md](.ai/pipelines.md) / [modular.md](.ai/modular.md) |
| Review your diff before opening a PR | [self-review](.ai/skills/self-review/SKILL.md) against [review-rules.md](.ai/review-rules.md) |
| Look up "I want to…" without asking a maintainer | [self-serve.md](.ai/skills/onboarding-acme/self-serve.md) |
| Understand why a convention exists | [conventions-overview.md](.ai/skills/onboarding-acme/conventions-overview.md) |

### Conventions to internalize

- **Coordinate first** — get maintainer acknowledgment on an issue before investing in a large PR
- **One problem per PR** — keep changes scoped to a single fix or feature
- **Format before PR** — run `make style && make quality` (and `make fix-copies` if `# Copied from` blocks were touched)
- **Scoped tests** — `pytest tests/<area>/ -v` for fast, relevant feedback
- **Modular preferred** — new pipeline work should follow [modular.md](.ai/modular.md) conventions

### PR quality gates

**Local checks:**

- `make quality` — lint and format (same as CI)
- Scoped `pytest` for the touched area
- [self-review](.ai/skills/self-review/SKILL.md) skill on `git diff main...HEAD`

**CI on the demo fork:**

[Fork-friendly CI](.github/workflows/fork_friendly_ci.yml) runs three jobs: `check_code_quality`, `check_repository_consistency`, `run_smoke_tests`. The full upstream workflow matrix applies on `huggingface/diffusers`; ignore failed upstream-only workflows (GPU runners, org secrets) on the demo fork.

### Finding work

- **Good first issues** (reference): [GitHub Issues](https://github.com/huggingface/diffusers/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22) on the upstream tracker
- **Roadmap themes:** [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1)
- **Other contribution types** (docs, community pipelines, schedulers): [CONTRIBUTING.md](CONTRIBUTING.md) sections 5–9

### Role quick links

- **PM** — roadmap tracking, issue scoping, feature-to-code mapping → [self-serve.md — Product managers](.ai/skills/onboarding-acme/self-serve.md#product-managers)
- **QA** — test layout, `diffusers-cli env`, bug repro standards → [self-serve.md — QA engineers](.ai/skills/onboarding-acme/self-serve.md#qa-engineers)
- **DevOps** — `docker/`, CI workflows, `make quality` parity → [self-serve.md — DevOps engineers](.ai/skills/onboarding-acme/self-serve.md#devops-engineers)
