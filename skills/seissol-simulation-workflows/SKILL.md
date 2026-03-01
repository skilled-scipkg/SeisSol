---
name: seissol-simulation-workflows
description: This skill should be used when users ask about simulation workflows in seissol; it prioritizes documentation references and then source inspection only for unresolved details.
---

# seissol: Simulation Workflows

## High-Signal Playbook
### Route conditions
- Build/dependency/toolchain failures before launch -> `seissol-build-and-install`.
- Parameterization or physics-model setup issues -> `seissol-inputs-and-modeling`.
- MPI/OpenMP/GPU placement and scheduler scripts -> `seissol-parallel-hpc`.
- Output interpretation/postprocessing -> `seissol-analysis-and-output`.

### Required run inputs
- Executable: `./SeisSol_<configuration>`.
- Parameter file: `parameters.par` (or YAML equivalent accepted by your branch).
- All mesh/material/source files referenced by that parameter file.
- Writable output directory matching `OutputFile` prefix.

### Canonical workflow
1. Confirm binary/parameter compatibility and launch directory (`docs/build-run.rst`, `docs/parameter-file.rst`).
2. Set runtime affinity/threading environment (`docs/build-run.rst`, `docs/environment-variables.rst`).
3. Launch with cluster-native MPI command (`mpiexec`, `mpirun`, or `srun`) (`docs/build-run.rst`).
4. Enable checkpoint writing in `&Output` (`checkpoint`, `checkpointinterval`) (`docs/checkpointing.rst`).
5. Restart with `-c <checkpoint_file>` when needed (`docs/checkpointing.rst`).
6. Validate output cadence, checkpoint cadence, and end-time behavior (`docs/build-run.rst`, `docs/checkpointing.rst`).

### Minimal working example
```bash
# launch
export OMP_NUM_THREADS=<threads_per_rank>
export OMP_PLACES="cores(<threads_per_rank>)"
export MP_SINGLE_THREAD=no
unset KMP_AFFINITY

mpiexec -np <mpi_ranks> ./SeisSol_<configuration> parameters.par

# restart from a produced checkpoint
mpiexec -np <mpi_ranks> ./SeisSol_<configuration> parameters.par \
  -c output/<prefix>-checkpoint-0000.h5
```

### Common run-time traps
- Wrong CPU pinning leaves no free core for communication thread -> reduce compute cores by one (`docs/build-run.rst`).
- IO stalls in async mode -> test `ASYNC_MODE=SYNC` (`docs/build-run.rst`, `docs/environment-variables.rst`).
- Restart mismatch across architecture/order/padding -> use compatible restart configuration (`docs/checkpointing.rst`).
- Restart overrides initial condition -> expected checkpoint behavior (`docs/checkpointing.rst`).

### Validation checkpoints
- Confirm run starts and advances time (check logs for timestep progression) (`docs/build-run.rst`).
- Confirm checkpoint files appear as expected:
  - `ls output/*-checkpoint-*.h5`
- Confirm output families exist for configured masks:
  - `ls output/*-fault.xdmf output/*-surface.xdmf output/*-receiver-*.dat`
- For restart, verify resumed time is read from checkpoint metadata (`docs/checkpointing.rst`).

## Scope
- Handle simulation launch, runtime controls, checkpoint/restart, and execution-time output expectations.
- Keep guidance execution-oriented from first launch to validated restart.

## Primary documentation references
- `docs/build-run.rst`
- `docs/checkpointing.rst`
- `docs/environment-variables.rst`
- `docs/performance-measurement.rst`

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
- `src/Solver/Simulator.cpp`
- `src/Solver/TimeStepping/TimeManager.cpp`
- `src/Solver/TimeStepping/HaloCommunication.cpp`
- `src/IO/Instance/Checkpoint/CheckpointManager.cpp`
- `src/IO/Instance/Checkpoint/CheckpointManager.h`
- `src/ResultWriter/AsyncIO.cpp`
- `src/ResultWriter/WaveFieldWriter.cpp`
- `src/ResultWriter/FaultWriter.cpp`
- `src/ResultWriter/ReceiverWriter.cpp`
- `src/ResultWriter/EnergyOutput.cpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app codegen postprocessing preprocessing src`).
