# DevOps engineer

## Tour

- **Docker:** `docker/` — PyTorch CPU/CUDA, ONNX, xformers, minimum-cuda, doc-builder variants
- **CI/CD:** `.github/workflows/` — test matrix, lint, release automation
- **Local CI parity:** `make quality` (check), `make style` (fix)
- **Packaging:** `setup.py` + `pyproject.toml` (ruff config); editable install via `pip install -e ".[dev]"`
- **Dependencies:** `src/diffusers/dependency_versions_table.py` — update with `make deps_table_update`
- **Release:** Makefile targets `pre-release`, `post-release`, `pre-patch`, `post-patch`
- **See the build environment in use:** the [first-contribution](../../first-contribution/SKILL.md) skill supports DevOps with role spotlights — the CI image as dev container with bind-mounts and editable installs (Phase 1), and local/CI parity via `make quality` (Phase 6)

## Self-serve

| I want to… | Go to… |
|------------|--------|
| Build Docker images | `docker/` — variants for PyTorch CPU/CUDA, ONNX, xformers, doc-builder |
| Understand CI checks | `.github/workflows/` — tests, ruff, doc-builder, security scans |
| Run lint/format locally (same as CI) | `make quality` (check), `make style` (fix) |
| Package for release | [setup.py](../../../../setup.py), Makefile `pre-release` / `post-release` targets |
| Manage dependencies | `setup.py`, `src/diffusers/dependency_versions_table.py` — update via `make deps_table_update` |
| Editable install for dev | `pip install -e ".[dev]"` and `pip install -e ".[test]"` |
| Repo consistency checks | `make repo-consistency` |

## Closing

Use `make quality` locally to mirror CI lint checks. To see the Docker/CI-parity setup in action, run the **first-contribution** skill — it highlights the environment and lint phases for DevOps.
