# QA engineer

## Tour

- **Try it first:** run `pytest tests/schedulers/ -v` to verify your setup — schedulers are fast and don't download models
- **Test layout:** `tests/pipelines/`, `tests/models/`, `tests/schedulers/`, `tests/lora/`, `tests/modular_pipelines/`
- **Run scoped tests:** `pytest tests/pipelines/<model>/ -v` — fast feedback for one area
- **Run full suite:** `make test` — requires beefy machine; uses pytest-xdist
- **Slow tests:** gated on `@slow` / `RUN_SLOW=1` — downloads large models; runs nightly in CI
- **Bug reports:** must include minimal repro + `diffusers-cli env` output (CONTRIBUTING §2.1)
- **CI:** `.github/workflows/` — ruff, pytest, doc-builder, security scans
- **Next: walk the full contribution process:** run the [first-contribution](../../first-contribution/SKILL.md) skill — same end-to-end path as every other role, with QA spotlights on issue reproducibility (Phase 2), the test contract in the Task Plan (Phase 3), and scoped tests as PR evidence (Phase 5)

## Self-serve

| I want to… | Go to… |
|------------|--------|
| Walk a real change end-to-end (full process) | [first-contribution skill](../../first-contribution/SKILL.md) — QA role spotlights included |
| Verify my setup / get started | `pytest tests/schedulers/ -v` — fast, no model downloads |
| Run all library tests | `make test` or `python -m pytest -n auto --dist=loadfile -s -v ./tests/` |
| Run tests for one area | `pytest tests/pipelines/<model>/ -v` or `pytest tests/models/ -v` |
| Run slow / integration tests | `RUN_SLOW=yes pytest tests/<file>.py` (downloads large models) |
| Collect environment info for a bug report | `diffusers-cli env` |
| Understand test layout | `tests/pipelines/`, `tests/models/`, `tests/schedulers/`, `tests/lora/`, `tests/modular_pipelines/` |
| See CI workflows | `.github/workflows/` |
| Reproduce a pipeline bug minimally | CONTRIBUTING §2.1 — minimal copy-paste snippet, no full training workflows |
| Check example test coverage | `make test-examples` |

## Closing

Run `pytest tests/schedulers/ -v` to verify your setup, then explore `pytest tests/<area>/ -v` for the model or component you're testing. When filing bugs, include `diffusers-cli env` + a minimal repro.

When you're ready for the next step, run the **first-contribution** skill — it walks the full contribution process (environment through PR prep) with QA spotlights on reproducibility, the test contract, and scoped tests as evidence. Every role takes this path so you understand how a change ships, not only how to test it.
