# Repository Guidelines

## Project Structure & Modules
- Core training loop lives in `train.py`; GPT architecture helpers are in `model.py`.
- Configuration presets reside under `config/`; dataset prep and tokenization scripts in `data/`.
- Assets and docs live in `assets/`; exploratory notebooks sit at the repository root.
- Experiment outputs default to `out-*` directories; keep large checkpoints untracked and document storage paths.
- Supporting utilities include `sample.py` (inference), `bench.py` (throughput checks), and `configurator.py` (config inspection).

## Build, Test & Run
- `python train.py config/train_shakespeare_char.py` reproduces the reference character-level run; update configs for new experiments.
- `python sample.py --out_dir=<run_dir>` generates text from trained checkpoints or GPT-2 baselines.
- `python bench.py` profiles forward/backward passes to detect hardware regressions.
- `python configurator.py --config <path>` emits resolved hyperparameters before long training jobs.

## Coding Style & Naming
- Follow PEP 8 with 4-space indentation; keep files Black-compatible.
- Use `snake_case` for functions/variables, `CamelCase` for classes, and `UPPER_SNAKE` for constants.
- Prefer explicit type hints on public APIs and clarify tensor shapes when non-obvious.
- Reuse existing logging hooks (Weights & Biases or stderr); discuss before introducing new logging frameworks.

## Testing Guidelines
- Place automated tests in `tests/` mirroring source layout; name files `test_*.py`.
- Use lightweight fixtures (e.g., Shakespeare dataset) for smoke coverage before GPU runs.
- Run `pytest -q` and a CPU-only dry run (`python train.py ... --device=cpu --compile=False --max_iters=20`) prior to submitting.

## Commit & PR Process
- Write imperative, scoped commits, e.g., `fix(train): guard warmup step`.
- Squash noisy history and reference related issues, experiments, or configs.
- PRs must include a summary, validation commands with results, notable config diffs, and artifacts (loss curves, sample outputs) when relevant.

## Security & Configuration
- Keep secrets (Weights & Biases keys, dataset credentials) out of git; rely on ignored `.env` files and environment variables.
- Document new config flags inline and note resource requirements in PR descriptions.
- Confirm dataset scripts respect licensing and list any manual preprocessing steps contributors need to follow.
