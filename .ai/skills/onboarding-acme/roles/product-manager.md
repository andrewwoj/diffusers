# Product manager

## Tour

- **User-facing surface:** `DiffusionPipeline.from_pretrained(...)` — load pretrained checkpoints for inference
- **Model landscape:** ~90 pipeline families in `src/diffusers/pipelines/` (Flux, SD3, Wan, Hunyuan Video, etc.)
- **Roadmap tracking:** [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1) — GitHub Project board for planned features and priorities
- **Core vs community:** Core library is inference-only; training examples live in `examples/`; community pipelines in `examples/community/`
- **Feature requests:** [Feature request template](https://github.com/huggingface/diffusers/issues/new?template=feature_request.md)
- **Triage a roadmap feature request:** run the [feature-request-review](../../feature-request-review/SKILL.md) skill — demo-only analysis of labels, clarifying questions, and business value from the Diffusers Roadmap backlog
- **See a change ship end-to-end:** run the [first-contribution](../../first-contribution/SKILL.md) skill — same full contribution process as every other role, with PM spotlights on issue ranking/scoping (Phase 2), the Task Plan as a lightweight spec (Phase 3), and the anatomy of a reviewable PR (Phase 8)

## Self-serve

| I want to… | Go to… |
|------------|--------|
| Walk a real change end-to-end (full process) | [first-contribution skill](../../first-contribution/SKILL.md) — PM role spotlights included |
| See supported model families | `src/diffusers/pipelines/` (~90 directories), [API pipeline docs](https://huggingface.co/docs/diffusers/api/pipelines/overview) |
| Understand inference vs training scope | Core library = inference; training lives in `examples/` |
| Track the roadmap | [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1) |
| Practice reviewing a backlog feature request | [feature-request-review skill](../../feature-request-review/SKILL.md) (demo only — no upstream writes) |
| Request a new pipeline/model | GitHub Issues — label `New pipeline/model` |
| Map a feature to code | Pipelines = end-to-end inference; Models = building blocks; Schedulers = denoising steps |
| Check Hub model compatibility | [Hub diffusers models](https://huggingface.co/models?library=diffusers&sort=downloads) |
| Understand community vs core | Core = `src/diffusers/`; community pipelines = `examples/community/`; research examples = `examples/research_projects/` |

## Closing

Track features on the [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1).

For a PM-native demo next, run **feature-request-review** — pick a backlog item and practice labels, clarifying questions, and business value without touching upstream. When you want the full contribution process (environment through PR prep), run **first-contribution** with PM spotlights on issue ranking/scoping, the Task Plan as a lightweight spec, and the anatomy of a reviewable PR.
