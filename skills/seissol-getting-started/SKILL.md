---
name: seissol-getting-started
description: This skill should be used when users ask about getting started in seissol; it prioritizes documentation references and then source inspection only for unresolved details.
---

# seissol: Getting Started

## High-Signal Playbook
### Route conditions
- Build/dependency failures before any run -> `seissol-build-and-install`.
- Parameter/friction/material authoring questions -> `seissol-inputs-and-modeling`.
- MPI/OpenMP/GPU placement or scheduler tuning -> `seissol-parallel-hpc`.
- Output interpretation/plotting -> `seissol-analysis-and-output`.

### Triage questions
- Do you already have a runnable binary (for example `SeisSol_Release_*`)? (`docs/build-overview.rst`, `docs/build-run.rst`)
- Are you running local single-rank or multi-rank on a cluster? (`docs/a-first-example.rst`, `docs/build-run.rst`)
- Do you have the external `SeisSol/Examples` repo and TPV33 mesh files? (`docs/a-first-example.rst`)
- Which launcher is available (`mpiexec`, `mpirun`, `srun`)? (`docs/a-first-example.rst`)
- Do you want cookbook-style benchmark progression after first run? (`docs/cookbook-overview.rst`, `docs/table-cookbook.rst`)

### Canonical workflow
1. Confirm build path and choose a compiled configuration (`docs/build-overview.rst`, `docs/build-run.rst`).
2. Clone `SeisSol/Examples`, enter `Examples/tpv33` (`docs/a-first-example.rst`).
3. Download mesh assets from Zenodo and create `output/` (`docs/a-first-example.rst`).
4. Set OpenMP threads/places; leave a free core in multi-rank mode (`docs/a-first-example.rst`).
5. Run SeisSol with your launcher and `parameters.par` (`docs/a-first-example.rst`).
6. Verify expected outputs (`*.xdmf`, `*-receiver-*.dat`, `*-energy.csv`) (`docs/a-first-example.rst`).
7. Visualize with ParaView or `viewrec` (`docs/a-first-example.rst`).
8. Move to cookbook benchmarks once TPV33 is stable (`docs/cookbook-overview.rst`, `docs/table-cookbook.rst`).

### Minimal working example
```bash
# first-run path from the docs
git clone https://github.com/SeisSol/Examples.git
cd Examples/tpv33
mkdir -p output

export OMP_NUM_THREADS=7
export OMP_PLACES="cores(7)"
mpiexec -np 1 ./SeisSol_Release_dhsw_4_elastic parameters.par
```

### Pitfalls and fixes
- Missing TPV33 mesh files from Zenodo -> download before launch (`docs/a-first-example.rst`).
- Multi-rank run with all cores occupied by compute threads -> reserve one core for comm/IO (`docs/a-first-example.rst`, `docs/build-run.rst`).
- Wrong MPI launcher assumption -> switch to cluster launcher (`mpirun`/`srun`) (`docs/a-first-example.rst`).
- Output path missing -> create `output/` first (`docs/a-first-example.rst`).
- Expecting full benchmark bundles in this repo -> use external `SeisSol/Examples` (`docs/a-first-example.rst`, `docs/cookbook-overview.rst`).

### Convergence and validation checks
- Confirm creation of wavefield, surface, fault, receiver, and energy outputs (`docs/a-first-example.rst`).
- Run quick file checks after launch:
  - `ls output/*-fault.xdmf output/*-surface.xdmf`
  - `ls output/*-receiver-*.dat output/*-energy.csv`
- Compare against `precomputed-seissol` or SCEC CVWS references for sanity (`docs/a-first-example.rst`).
- Run proxy smoke test if uncertain about installation integrity (`docs/build-run.rst`).

## Scope
- Handle questions about initial setup, quickstarts, and core concepts.
- Keep responses workflow-oriented and minimal-first.

## Primary documentation references
- `docs/introduction.rst`
- `docs/build-overview.rst`
- `docs/build-run.rst`
- `docs/a-first-example.rst`
- `docs/cookbook-overview.rst`
- `docs/table-cookbook.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `docs`
- `preprocessing/meshing/gmsh_example`

## Test references
- `tests`

## Optional deeper inspection
- `app`
- `codegen`
- `postprocessing`
- `preprocessing`
- `src`

## Source entry points for unresolved issues
- `app/Main/Main.cpp`
- `src/Initializer/InitProcedure/Init.cpp`
- `src/Initializer/Parameters/ParameterReader.cpp`
- `src/Solver/Simulator.cpp`
- `src/Solver/TimeStepping/TimeManager.cpp`
- `src/ResultWriter/WaveFieldWriter.cpp`
- `src/ResultWriter/FaultWriter.cpp`
- `src/ResultWriter/ReceiverWriter.cpp`
- `postprocessing/visualization/receiver/src/viewrec.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app codegen postprocessing preprocessing src`).
