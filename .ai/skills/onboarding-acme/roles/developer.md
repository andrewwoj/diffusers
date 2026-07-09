# Developer (contributor)

## Tour

- **Architecture:** pipelines compose models + schedulers; modular pipelines split into reusable blocks
- **Learn by comparison:** skim reference files before writing — `pipeline_flux.py`, `transformer_wan.py`, `modular_pipelines/qwenimage/`
- **Convention guides:** [models.md](../../../models.md), [pipelines.md](../../../pipelines.md), [modular.md](../../../modular.md)
- **Skills for tasks:** onboarding-acme (you are here) → first-contribution → model-integration → self-review
- **Formatting before PR:** `make style && make quality`
- **Coordination rule:** get maintainer acknowledgment on an issue before opening a PR

## Self-serve

| I want to… | Go to… |
|------------|--------|
| Get oriented in the codebase | [onboarding-acme skill](../SKILL.md) (this skill) |
| Fix my first issue | [first-contribution skill](../../first-contribution/SKILL.md) |
| Add a new model or pipeline | [model-integration skill](../../model-integration/SKILL.md) |
| Review my diff before a PR | [self-review skill](../../self-review/SKILL.md) |
| Know model coding rules | [.ai/models.md](../../../models.md) |
| Know pipeline coding rules | [.ai/pipelines.md](../../../pipelines.md) |
| Know modular pipeline rules | [.ai/modular.md](../../../modular.md) |
| Find good first issues | [GitHub search](https://github.com/huggingface/diffusers/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22) or first-contribution skill |
| Understand why a convention exists | [conventions-overview.md](../conventions-overview.md) |
| Propagate copied code changes | `make fix-copies` |
| Generate model tests | `python utils/generate_model_tests.py src/diffusers/models/...` |
| Reference a pipeline implementation | `pipeline_flux.py`, `pipeline_wan.py`, `pipeline_qwenimage.py` |
| Reference a model implementation | `transformer_flux.py`, `transformer_wan.py`, `transformer_qwenimage.py` |
| Reference modular pipeline layout | `modular_pipelines/qwenimage/`, `modular_pipelines/wan/` |
| Other contribution types (docs, examples, schedulers) | [CONTRIBUTING.md](../../../../CONTRIBUTING.md) §5–9 |

## Closing

When you're ready to contribute, run the **first-contribution** skill — it will find a good first issue and walk you through the fix.
