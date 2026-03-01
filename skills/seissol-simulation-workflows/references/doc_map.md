# seissol documentation map: Simulation Workflows

Use this map after reading `SKILL.md`.
All paths are repository-relative and verified in this checkout.

## Core docs (read first)
- `docs/build-run.rst` | launch commands, rank/thread placement, proxy smoke runs.
- `docs/checkpointing.rst` | checkpoint write/restart semantics.
- `docs/parameter-file.rst` | run-directory relative paths and output/checkpoint namelist fields.
- `docs/environment-variables.rst` | runtime behavior controls.
- `docs/performance-measurement.rst` | runtime timing and FLOP/s instrumentation.
- `docs/a-first-example.rst` | practical baseline run for expected artifacts.

## Runtime troubleshooting references
- `docs/postprocessing-and-visualization.rst` | quick output inspection after run/restart.
- `docs/known-issues.rst` | known async/output caveats relevant to workflows.
- `docs/gpus.rst` | GPU-specific launch/restart caveats.
