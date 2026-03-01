---
name: seissol-examples-and-tutorials
description: This skill should be used when users ask about examples and tutorials in seissol; it prioritizes documentation references and then source inspection only for unresolved details.
---

# seissol: Examples and Tutorials

## High-Signal Playbook
### Route conditions
- Cannot build/run baseline binary -> `seissol-build-and-install` or `seissol-simulation-workflows`.
- Need to author/customize parameter files beyond example defaults -> `seissol-inputs-and-modeling`.
- Need output interpretation scripts/diagnostics -> `seissol-analysis-and-output`.

### Triage questions
- Is the goal first-install smoke test (`tpv33`) or benchmark progression (TPV5/6/12/13/16/24/29/34/104)? (`docs/a-first-example.rst`, `docs/table-cookbook.rst`)
- Are external assets available (`SeisSol/Examples`, mesh downloads)? (`docs/a-first-example.rst`, `docs/cookbook-overview.rst`)
- Which faulting/friction family is required (LSW vs rate-and-state)? (`docs/table-cookbook.rst`)
- Is run local desktop or HPC cluster? (`docs/a-first-example.rst`)

### Canonical workflow
1. Start with `A first example` (TPV33) to validate executable + runtime environment (`docs/a-first-example.rst`).
2. Move to cookbook benchmark table and choose a case by mechanism/friction complexity (`docs/table-cookbook.rst`).
3. Pull inputs from `SeisSol/Examples` and required mesh artifacts (`docs/cookbook-overview.rst`, `docs/a-first-example.rst`).
4. Run with documented launcher/thread settings (`docs/a-first-example.rst`).
5. Validate outputs against reference solutions/CVWS where provided (`docs/a-first-example.rst`, `docs/cookbook-overview.rst`).
6. Escalate to topic skills for customization (build/modeling/output/HPC).

### Minimal working example
```bash
# docs-first first example (tpv33)
git clone https://github.com/SeisSol/Examples.git
cd Examples/tpv33
mkdir -p output

export OMP_NUM_THREADS=<threads>
export OMP_PLACES="cores(<cores>)"
mpiexec -np <n> ./SeisSol_<configuration> parameters.par
```

### Pitfalls and fixes
- Expecting full benchmark assets inside this repository -> use external `SeisSol/Examples` (`docs/a-first-example.rst`, `docs/cookbook-overview.rst`).
- Missing TPV33 mesh files -> fetch the Zenodo dataset first (`docs/a-first-example.rst`).
- Wrong binary/configuration tuple for the selected example -> verify executable naming and build flags (`docs/a-first-example.rst`, `docs/build-parameters.rst`).
- Misread faulting/rake sign conventions while interpreting cookbook setups -> verify conventions (`docs/left-lateral-right-lateral-normal-reverse.rst`).

### Convergence and validation checks
- Confirm all expected output families are generated for tutorial runs (`docs/a-first-example.rst`).
- Compare against `precomputed-seissol` and SCEC CVWS references when available (`docs/a-first-example.rst`).
- Promote to heavier cookbook cases only after TPV33 baseline is stable (`docs/cookbook-overview.rst`, `docs/table-cookbook.rst`).

## Scope
- Handle worked examples, benchmark selection, and tutorial progression.
- Keep guidance reproducible and example-driven.

## Primary documentation references
- `docs/a-first-example.rst`
- `docs/cookbook-overview.rst`
- `docs/table-cookbook.rst`
- `docs/left-lateral-right-lateral-normal-reverse.rst`

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
- `src/Initializer/Parameters/ParameterReader.cpp`
- `src/Initializer/BatchRecorders/DataTypes/Table.h`
- `src/IO/Instance/Point/TableWriter.cpp`
- `postprocessing/validation/compare-faults.py`
- `postprocessing/validation/compare-receivers.py`
- `preprocessing/science/place_quickly_faultreceivers_netcdf_any_normal.m`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app codegen postprocessing preprocessing src`).
