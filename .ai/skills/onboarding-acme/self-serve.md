# Self-serve index

Lookup table: **I want to…** → where to go without asking a maintainer.

---

## Everyone

| I want to… | Go to… |
|------------|--------|
| Understand what diffusers is | [README.md](../../../README.md), [official docs](https://huggingface.co/docs/diffusers/index) |
| Understand design philosophy | [docs/source/en/conceptual/philosophy.md](https://huggingface.co/docs/diffusers/conceptual/philosophy) |
| See open work / bugs | [GitHub Issues](https://github.com/huggingface/diffusers/issues) |
| Track roadmap themes | [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1) |
| Ask a general question | [Discussion forum](https://discuss.huggingface.co/c/discussion-related-to-httpsgithubcomhuggingfacediffusers/) or [Discord](https://discord.gg/G7tWnz98XR) |
| Report a library bug | [Bug report template](https://github.com/huggingface/diffusers/issues/new?template=bug-report.yml) — include `diffusers-cli env` output |
| Learn contribution etiquette | [CONTRIBUTING.md](../../../CONTRIBUTING.md) |
| Wire AI agent config | `make cursor` / `make claude` / `make codex` — see [AGENTS.md](../../AGENTS.md) (`make cursor` wires skills and Cursor rules) |

---

## Product managers

| I want to… | Go to… |
|------------|--------|
| See supported model families | `src/diffusers/pipelines/` (~90 directories), [API pipeline docs](https://huggingface.co/docs/diffusers/api/pipelines/overview) |
| Understand inference vs training scope | Core library = inference; training lives in `examples/` |
| Track the roadmap | [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1) |
| Request a new pipeline/model | GitHub Issues — label `New pipeline/model` |
| Map a feature to code | Pipelines = end-to-end inference; Models = building blocks; Schedulers = denoising steps |
| Check Hub model compatibility | [Hub diffusers models](https://huggingface.co/models?library=diffusers&sort=downloads) |
| Understand community vs core | Core = `src/diffusers/`; community pipelines = `examples/community/`; research examples = `examples/research_projects/` |

---

## QA engineers

| I want to… | Go to… |
|------------|--------|
| Verify my setup / get started | `pytest tests/schedulers/ -v` — fast, no model downloads |
| Run all library tests | `make test` or `python -m pytest -n auto --dist=loadfile -s -v ./tests/` |
| Run tests for one area | `pytest tests/pipelines/<model>/ -v` or `pytest tests/models/ -v` |
| Run slow / integration tests | `RUN_SLOW=yes pytest tests/<file>.py` (downloads large models) |
| Collect environment info for a bug report | `diffusers-cli env` |
| Understand test layout | `tests/pipelines/`, `tests/models/`, `tests/schedulers/`, `tests/lora/`, `tests/modular_pipelines/` |
| See CI workflows | `.github/workflows/` |
| Reproduce a pipeline bug minimally | CONTRIBUTING §2.1 — minimal copy-paste snippet, no full training workflows |
| Check example test coverage | `make test-examples` |

---

## DevOps engineers

| I want to… | Go to… |
|------------|--------|
| Build Docker images | `docker/` — variants for PyTorch CPU/CUDA, ONNX, xformers, doc-builder |
| Understand CI checks | `.github/workflows/` — tests, ruff, doc-builder, security scans |
| Run lint/format locally (same as CI) | `make quality` (check), `make style` (fix) |
| Package for release | [setup.py](../../../setup.py), Makefile `pre-release` / `post-release` targets |
| Manage dependencies | `setup.py`, `src/diffusers/dependency_versions_table.py` — update via `make deps_table_update` |
| Editable install for dev | `pip install -e ".[dev]"` and `pip install -e ".[test]"` |
| Repo consistency checks | `make repo-consistency` |

---

## Developers (contributors)

| I want to… | Go to… |
|------------|--------|
| Get oriented in the codebase | [onboarding-acme skill](SKILL.md) (this skill) |
| Fix my first issue | [first-contribution skill](../first-contribution/SKILL.md) |
| Add a new model or pipeline | [model-integration skill](../model-integration/SKILL.md) |
| Review my diff before a PR | [self-review skill](../self-review/SKILL.md) |
| Know model coding rules | [.ai/models.md](../../models.md) |
| Know pipeline coding rules | [.ai/pipelines.md](../../pipelines.md) |
| Know modular pipeline rules | [.ai/modular.md](../../modular.md) |
| Find good first issues | [GitHub search](https://github.com/huggingface/diffusers/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22) or first-contribution skill |
| Understand why a convention exists | [conventions-overview.md](conventions-overview.md) |
| Propagate copied code changes | `make fix-copies` |
| Generate model tests | `python utils/generate_model_tests.py src/diffusers/models/...` |
| Reference a pipeline implementation | `pipeline_flux.py`, `pipeline_wan.py`, `pipeline_qwenimage.py` |
| Reference a model implementation | `transformer_flux.py`, `transformer_wan.py`, `transformer_qwenimage.py` |
| Reference modular pipeline layout | `modular_pipelines/qwenimage/`, `modular_pipelines/wan/` |
| Other contribution types (docs, examples, schedulers) | [CONTRIBUTING.md](../../../CONTRIBUTING.md) §5–9 |
