---
name: seissol-parallel-hpc
description: This skill should be used when users ask about parallel and hpc in seissol; it prioritizes documentation references and then source inspection only for unresolved details.
---

# seissol: Parallel and HPC

## High-Signal Playbook
### Route conditions
- Build-time architecture/dependency failures -> `seissol-build-and-install`.
- Parameter-file physics setup -> `seissol-inputs-and-modeling`.
- Output content/analysis semantics -> `seissol-analysis-and-output`.

### Triage questions
- CPU-only or GPU run, and how many GPUs per node? (`docs/gpus.rst`, `docs/build-run.rst`)
- Scheduler and launcher (`srun`, `mpirun`, `mpiexec`)? (`docs/supermuc.rst`, `docs/build-run.rst`)
- How many MPI ranks per node and OpenMP threads per rank? (`docs/build-run.rst`)
- Is MPI GPU-aware, or is host-staged transfer required? (`docs/gpus.rst`)
- Are affinity variables inherited from modules (for example `KMP_AFFINITY`)? (`docs/supermuc.rst`, `docs/environment-variables.rst`)

### Canonical workflow
1. Choose rank layout (`M x N` ranks for `M` nodes and `N` GPUs/node on GPU runs) (`docs/gpus.rst`).
2. Set OpenMP placement and free-thread policy for comm/IO (`docs/build-run.rst`, `docs/environment-variables.rst`).
3. Set async-IO and checkpoint alignment variables for cluster runs (`docs/environment-variables.rst`, `docs/supermuc.rst`).
4. Ensure per-rank GPU affinity (scheduler gpu-bind/rank-local mapping) (`docs/gpus.rst`, `docs/build-run.rst`).
5. Launch with scheduler-native command (`srun` on Slurm clusters) (`docs/supermuc.rst`).
6. If MPI is not GPU-aware, force host transfer mode (`docs/gpus.rst`).
7. Validate scaling/runtime behavior before full-size campaigns (`docs/scaling.rst`, `docs/performance-measurement.rst`).

### Minimal working example
```bash
# CPU/GPU runtime placement baseline
export MP_SINGLE_THREAD=no
unset KMP_AFFINITY
export OMP_NUM_THREADS=94
export OMP_PLACES="cores(47)"

export XDMFWRITER_ALIGNMENT=8388608
export XDMFWRITER_BLOCK_SIZE=8388608
export ASYNC_MODE=THREAD

# one rank per GPU on GPU runs
mpirun -n <M x N> ./SeisSol_dsm70_cuda_* parameters.par
```

### Pitfalls and fixes
- All cores consumed by OpenMP workers -> leave one free core per rank for communication/IO (`docs/build-run.rst`, `docs/environment-variables.rst`).
- `KMP_AFFINITY` conflicts with chosen pinning -> `unset KMP_AFFINITY` after module load (`docs/supermuc.rst`).
- Multiple ranks bind same GPU -> enforce scheduler/rank-local GPU binding (`docs/gpus.rst`, `docs/build-run.rst`).
- Non-GPU-aware MPI with direct device transfers -> use `SEISSOL_PREFERRED_MPI_DATA_TRANSFER_MODE=host` (`docs/gpus.rst`).
- Async IO instability on a system -> temporarily switch `ASYNC_MODE=SYNC` for diagnosis (`docs/build-run.rst`).
- Confusing physical-unit scaling docs with strong/weak scaling -> treat `docs/scaling.rst` as equation-unit scaling guidance.

### Convergence and validation checks
- Confirm rank/thread/GPU mapping from job logs and performance counters (`docs/performance-measurement.rst`).
- Run a proxy benchmark before production scaling (`docs/build-run.rst`).
- Verify expected throughput change when toggling key knobs (`OMP_NUM_THREADS`, comm thread, async mode) (`docs/environment-variables.rst`).
- For GPU runs, verify one-rank-per-GPU behavior and stable communication mode (`docs/gpus.rst`).

## Scope
- Handle MPI/OpenMP/GPU execution, scaling-related runtime setup, and scheduler patterns.
- Prioritize robust placement and reproducible launch behavior.

## Primary documentation references
- `docs/gpus.rst`
- `docs/environment-variables.rst`
- `docs/build-run.rst`
- `docs/supermuc.rst`
- `docs/scaling.rst`
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
- `src/Parallel/MPI.cpp`
- `src/Parallel/OpenMP.cpp`
- `src/Parallel/Pin.cpp`
- `src/Parallel/HelperThread.cpp`
- `src/Parallel/AcceleratorDevice.cpp`
- `src/Parallel/Runtime/Stream.cpp`
- `src/IO/Datatype/MPIType.cpp`
- `app/Main/Main.cpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app codegen postprocessing preprocessing src`).
