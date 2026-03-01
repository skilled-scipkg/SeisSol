---
name: seissol-analysis-and-output
description: This skill should be used when users ask about analysis and output in seissol; it prioritizes documentation references and then source inspection only for unresolved details.
---

# seissol: Analysis and Output

## High-Signal Playbook
### Route conditions
- Output is incorrect because model/input setup is wrong -> `seissol-inputs-and-modeling`.
- Runtime/checkpoint scheduling issues -> `seissol-simulation-workflows`.
- Cluster visualization server setup/performance -> `seissol-parallel-hpc`.

### Triage questions
- Which outputs are needed: fault, wavefield, free-surface, off-fault receivers, energy? (`docs/fault-output.rst`, `docs/wave-field-output.rst`, `docs/free-surface-output.rst`, `docs/off-fault-receivers.rst`, `docs/energy-output.rst`)
- Desired cadence: wall-clock time, physical time, or timesteps? (`docs/fault-output.rst`, `docs/off-fault-receivers.rst`, `docs/wave-field-output.rst`)
- Need high-order VTKHDF outputs (`vtkorder`, `wavefieldvtkorder`, `surfacevtkorder`)? (`docs/fault-output.rst`, `docs/wave-field-output.rst`, `docs/free-surface-output.rst`)
- Are receivers on rough topography requiring preprocessing placement tools? (`docs/off-fault-receivers.rst`)
- Is postprocessing local (Python/MATLAB) or remote ParaView server mode? (`docs/postprocessing-and-visualization.rst`)

### Canonical workflow
1. Decide mandatory output families and cadence before run (`docs/fault-output.rst`, `docs/wave-field-output.rst`).
2. Configure `&Output`, `&Elementwise`, and `&Pickpoint` masks/refinement/order knobs (`docs/fault-output.rst`, `docs/wave-field-output.rst`, `docs/free-surface-output.rst`).
3. If receiver outputs are needed, set `RFileName` and sampling mode (`docs/off-fault-receivers.rst`).
4. Enable energy diagnostics for run-quality checks (`docs/energy-output.rst`).
5. Run and verify expected file families (`-fault.xdmf`, `-surface.xdmf`, `-receiver-*.dat`, `-energy.csv`) (`docs/a-first-example.rst`).
6. Use `seissolxdmf`, `viewrec`, and visualization scripts for handoff (`docs/fault-output.rst`, `docs/postprocessing-and-visualization.rst`).
7. Cross-check against benchmark references and validation utilities (`docs/a-first-example.rst`, `postprocessing/validation`).

### Minimal working example
```fortran
&Elementwise
printtimeinterval_sec = 1.0
OutputMask = 1 1 1 1 1 1 1 1 1 1 1 1
refinement = 1
vtkorder = -1
/

&Output
OutputFile = 'output/run'
Format = 6
iOutputMask = 0 0 0 0 0 0 1 1 1
SurfaceOutput = 1
ReceiverOutput = 1
EnergyOutput = 1
/
```

### Pitfalls and fixes
- Fault `Ts0/Td0/Pn0` at `t=0` differ from raw YAML tractions -> expected post-friction initialization behavior (`docs/fault-output.rst`).
- Receiver files sampled inconsistently with local timestepping -> account for `printtimeinterval` semantics (`docs/fault-output.rst`).
- `IntegrationMask` assumptions for wavefield integration output -> currently documented as broken (`docs/wave-field-output.rst`).
- Free-surface mixed interfaces interpreted incorrectly -> filter by `locationFlag` (`docs/free-surface-output.rst`).
- Remote ParaView server unusable on cluster -> require MPI+MESA ParaView build (`docs/postprocessing-and-visualization.rst`).

### Convergence and validation checks
- Validate output-file completeness and sampling intervals against namelists (`docs/fault-output.rst`, `docs/wave-field-output.rst`).
- Track energy series and derived rates for physical sanity (`docs/energy-output.rst`).
- Compare with SCEC/precomputed references when available (`docs/a-first-example.rst`, `docs/table-cookbook.rst`).
- Use `postprocessing/validation` scripts for mesh/fault/receiver comparisons where applicable.

## Scope
- Handle output configuration, interpretation, and postprocessing handoff.
- Emphasize correctness checks, not only visualization.

## Primary documentation references
- `docs/postprocessing-and-visualization.rst`
- `docs/fault-output.rst`
- `docs/wave-field-output.rst`
- `docs/free-surface-output.rst`
- `docs/off-fault-receivers.rst`
- `docs/energy-output.rst`
- `docs/checkpointing.rst`

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
- `src/ResultWriter/FaultWriter.cpp`
- `src/ResultWriter/WaveFieldWriter.cpp`
- `src/ResultWriter/FreeSurfaceWriter.cpp`
- `src/ResultWriter/ReceiverWriter.cpp`
- `src/ResultWriter/EnergyOutput.cpp`
- `src/IO/Instance/Mesh/VtkHdf.cpp`
- `postprocessing/visualization/tools/extractDataFromUnstructuredOutput.py`
- `postprocessing/visualization/receiver/bin/viewrec`
- `postprocessing/validation/compare-faults.py`
- `postprocessing/validation/compare-receivers.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app codegen postprocessing preprocessing src`).
