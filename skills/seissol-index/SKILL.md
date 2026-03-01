---
name: seissol-index
description: This skill should be used when users ask how to use seissol and the correct generated documentation skill must be selected before going deeper into source code.
---

# seissol Skills Index

## Route the request
- `seissol-getting-started`: first run, minimal setup assumptions, first validation outputs.
- `seissol-build-and-install`: dependencies, CMake/build flags, architecture selection, build failures.
- `seissol-inputs-and-modeling`: parameter files, material/source/initial-condition setup, dynamic-rupture friction/input semantics.
- `seissol-simulation-workflows`: launch commands, runtime controls, checkpoint/restart flow, execution-time expectations.
- `seissol-parallel-hpc`: MPI/OpenMP/GPU placement, scheduler scripts, GPU runtime variables.
- `seissol-analysis-and-output`: fault/free-surface/wavefield/receiver output semantics and postprocessing handoff.
- `seissol-examples-and-tutorials`: docs-driven benchmark selection and reproducible starter cases.
- `seissol-advanced-topics`: collapsed low-frequency one-doc topics and specialized workflows.

## Quick simulation bootstrap
1. Build or pick a verified executable (`seissol-build-and-install`).
2. Run TPV33 from `SeisSol/Examples` with `parameters.par` (`seissol-getting-started`).
3. Validate core outputs (`*-fault.xdmf`, `*-surface.xdmf`, `*-receiver-*.dat`, `*-energy.csv`) and only then scale (`seissol-simulation-workflows` + `seissol-parallel-hpc`).

## Docs-first escalation policy
- Start in the chosen skill `SKILL.md` and its **Primary documentation references**.
- If still ambiguous, open that skill's `references/doc_map.md` for the complete doc inventory.
- Escalate to `references/source_map.md` only after docs are exhausted.
- Use targeted source lookup, not broad spelunking: `rg -n "<symbol_or_keyword>" app codegen postprocessing preprocessing src`.

## Documentation-first inputs
- `docs`

## Tutorials and examples roots
- `docs`
- `preprocessing/meshing/gmsh_example`

## Test roots for behavior checks
- `tests`

## Source directories for deeper inspection
- `app`
- `codegen`
- `postprocessing`
- `preprocessing`
- `src`
