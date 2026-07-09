# Product manager

## Tour

- **User-facing surface:** `DiffusionPipeline.from_pretrained(...)` — load pretrained checkpoints for inference
- **Model landscape:** ~90 pipeline families in `src/diffusers/pipelines/` (Flux, SD3, Wan, Hunyuan Video, etc.)
- **Roadmap tracking:** [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1) — GitHub Project board for planned features and priorities
- **Core vs community:** Core library is inference-only; training examples live in `examples/`; community pipelines in `examples/community/`
- **Feature requests:** [Feature request template](https://github.com/huggingface/diffusers/issues/new?template=feature_request.md)
- **See a change ship end-to-end:** the [first-contribution](../../first-contribution/SKILL.md) skill supports PMs with role spotlights — issue ranking/scoping (Phase 2), the Task Plan as a lightweight spec (Phase 3), and the anatomy of a reviewable PR (Phase 8)

## Self-serve

| I want to… | Go to… |
|------------|--------|
| See supported model families | `src/diffusers/pipelines/` (~90 directories), [API pipeline docs](https://huggingface.co/docs/diffusers/api/pipelines/overview) |
| Understand inference vs training scope | Core library = inference; training lives in `examples/` |
| Track the roadmap | [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1) |
| Request a new pipeline/model | GitHub Issues — label `New pipeline/model` |
| Map a feature to code | Pipelines = end-to-end inference; Models = building blocks; Schedulers = denoising steps |
| Check Hub model compatibility | [Hub diffusers models](https://huggingface.co/models?library=diffusers&sort=downloads) |
| Understand community vs core | Core = `src/diffusers/`; community pipelines = `examples/community/`; research examples = `examples/research_projects/` |

## Closing

Track features on the [Diffusers Roadmap](https://github.com/orgs/huggingface/projects/61/views/1). To see how an issue becomes a shipped change, run the **first-contribution** skill — it highlights the scoping, planning, and PR phases for PMs.
