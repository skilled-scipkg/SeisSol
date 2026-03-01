---
name: seissol-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in seissol; it prioritizes documentation references and then source inspection only for unresolved details.
---

# seissol: Inputs and Modeling

## High-Signal Playbook
### Route conditions
- Build/dependency questions -> `seissol-build-and-install`.
- Launch/checkpoint/runtime control questions -> `seissol-simulation-workflows`.
- Output interpretation and plotting -> `seissol-analysis-and-output`.

### Triage questions
- Which equation set/material class (elastic, anisotropic, viscoelastic, poroelastic)? (`docs/physical-models.rst`)
- Which mesh format and path (`PUML`/`Netcdf`) and are file paths launch-dir relative? (`docs/parameter-file.rst`, `docs/parameters.par`)
- Is dynamic rupture enabled, and which friction law (`FL`)? (`docs/dynamic-rupture.rst`, `docs/parameters.par`)
- Are material and fault fields provided by YAML/easi (and where)? (`docs/parameter-file.rst`, `docs/dynamic-rupture.rst`, `docs/initial-condition.rst`)
- Is initial condition hard-coded mode or `cICType='easi'`? (`docs/initial-condition.rst`)

### Canonical workflow
1. Start from `docs/parameter-file.rst` and the commented template `docs/parameters.par`.
2. Set `&equations` material controls (`MaterialFileName`, `UseCellHomogenizedMaterial`) (`docs/physical-models.rst`, `docs/parameters.par`).
3. Configure mesh reader and partitioning in `&MeshNml` (`docs/parameters.par`).
4. Configure `&IniCondition` (`Zero`/`Planarwave`/`easi`) (`docs/initial-condition.rst`).
5. If dynamic rupture is used: set `FL`, `ModelFileName`, `XRef/YRef/ZRef`, `refPointMethod` (`docs/dynamic-rupture.rst`, `docs/parameter-file.rst`).
6. Set source model (`&SourceType`) for point/finite sources if needed (`docs/parameters.par`, `docs/pointsource.rst`).
7. Add outputs and termination criteria (`&Output`, `&AbortCriteria`) (`docs/parameters.par`).
8. Preflight with a benchmark-like case (TPV* or first example) before production (`docs/table-cookbook.rst`, `docs/a-first-example.rst`).

### Minimal working example
```fortran
&equations
MaterialFileName = '33_layered_constant.yaml'
UseCellHomogenizedMaterial = 1
/

&IniCondition
cICType = 'Zero'
/

&DynamicRupture
enabled = 1
FL = 16
ModelFileName = '33_fault0.yaml'
XRef = -0.1
YRef = 0.0
ZRef = -1.0
refPointMethod = 1
/

&MeshNml
MeshFile = 'tpv33_gmsh'
meshgenerator = 'PUML'
/
```

### Pitfalls and fixes
- Relative paths interpreted from launch directory, not repo root -> normalize run directory/path prefixes (`docs/parameter-file.rst`).
- Missing output directory for output path prefixes -> create directory explicitly (`docs/parameter-file.rst`, `docs/a-first-example.rst`).
- Wrong reference-point orientation flips fault sign conventions -> recheck `XRef/YRef/ZRef` and `refPointMethod` (`docs/parameter-file.rst`, `docs/left-lateral-right-lateral-normal-reverse.rst`).
- Friction-law parameter mismatch (for example FL 16/1058/3/4/103 fields) -> align YAML + parameter file to selected law (`docs/dynamic-rupture.rst`).
- Unstable/artefactual slip-rate checkerboards near rupture front -> increase local h-refinement on fault (`docs/dynamic-rupture.rst`).
- `cICType='easi'` used without valid easi file support -> ensure file exists and version supports `hastime` if used (`docs/initial-condition.rst`).

### Convergence and validation checks
- Validate t=0 fault tractions/parameters using `read_ini_fault_parameter.py` when needed (`docs/fault-output.rst`).
- For new physical setups, compare against a close SCEC benchmark family from cookbook (`docs/table-cookbook.rst`).
- Confirm expected output variables are enabled for target physics (for example thermal pressurization adds `P_f` and `Tmp`) (`docs/fault-output.rst`, `docs/dynamic-rupture.rst`).
- Recheck `ORDER`/mesh resolution interplay before scaling up (`docs/build-parameters.rst`, `docs/a-first-example.rst`).

## Scope
- Handle parameterization, model configuration, and source/initial-condition setup.
- Focus on correctness-critical namelists and YAML coupling.

## Primary documentation references
- `docs/parameter-file.rst`
- `docs/parameters.par`
- `docs/physical-models.rst`
- `docs/dynamic-rupture.rst`
- `docs/initial-condition.rst`
- `docs/configuration.rst`
- `docs/fault-tagging.rst`
- `docs/standard-rupture-format.rst`

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
- `src/Initializer/Parameters/ParameterReader.cpp`
- `src/Initializer/Parameters/ModelParameters.cpp`
- `src/Initializer/Parameters/DRParameters.cpp`
- `src/Initializer/Parameters/InitializationParameters.cpp`
- `src/Geometry/PUMLReader.cpp`
- `src/Model/CommonDatastructures.h`
- `src/DynamicRupture/Initializer/Initializers.h`
- `src/DynamicRupture/FrictionLaws/FrictionSolver.cpp`
- `src/SourceTerm/Manager.cpp`
- `preprocessing/science/read_ini_fault_parameter.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app codegen postprocessing preprocessing src`).
