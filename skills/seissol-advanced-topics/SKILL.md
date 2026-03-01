---
name: seissol-advanced-topics
description: This skill should be used when users ask about advanced and specialized topics in seissol; it prioritizes documentation references and then source inspection only for unresolved details.
---

# seissol: Advanced and Specialized Topics

## Scope
- Handle specialized, lower-frequency topics consolidated from many single-doc skills.
- Route back to core skills when a request is actually build/run/modeling/HPC/output critical.

## Collapsed topic routing
- `configuration`, `parameter-file`, `initial-condition`, `local-timestepping`, `checkpointing`, `fault-tagging`.
- `meshing-with-pumgen`, `manually-fixing-an-intersection-in-gocad`, `remeshing-the-topography`.
- `standard-rupture-format`, `point-source-older-implementation`, `tpv13`.
- `memory-requirements`, `behind-firewall`, `heisenbug`, `frontera`.
- `code-of-conduct`, `related-publications`, `reproducible-research`, `requirements`, plus previously grouped advanced docs.

## Primary documentation references
- `docs/configuration.rst`
- `docs/parameter-file.rst`
- `docs/initial-condition.rst`
- `docs/local-timestepping.rst`
- `docs/checkpointing.rst`
- `docs/fault-tagging.rst`
- `docs/meshing-with-pumgen.rst`
- `docs/manually-fixing-an-intersection-in-gocad.rst`
- `docs/remeshing-the-topography.rst`
- `docs/standard-rupture-format.rst`
- `docs/point-source-older-implementation.rst`
- `docs/tpv13.rst`
- `docs/memory-requirements.rst`
- `docs/behind-firewall.rst`
- `docs/heisenbug.rst`
- `docs/frontera.rst`
- `CODE_OF_CONDUCT.md`
- `docs/related-publications.rst`
- `docs/reproducible-research.rst`
- `docs/requirements.txt`
- `docs/acknowledge.rst`
- `docs/copyrights.rst`
- `docs/computing-time-vs-order-of-accuracy.rst`

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
- `src/IO/Instance/Checkpoint/CheckpointManager.cpp`
- `src/Initializer/Parameters/ParameterReader.cpp`
- `src/Initializer/Parameters/InitializationParameters.cpp`
- `src/Initializer/Parameters/LtsParameters.cpp`
- `src/Memory/MemoryAllocator.cpp`
- `src/Geometry/PartitioningLib.cpp`
- `src/SourceTerm/NRFReader.cpp`
- `src/SourceTerm/PointSource.cpp`
- `preprocessing/meshing/gmsh2gambit/src/main.cpp`
- `preprocessing/science/read_ini_fault_parameter.py`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app codegen postprocessing preprocessing src`).
