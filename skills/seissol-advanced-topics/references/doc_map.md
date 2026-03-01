# seissol documentation map: Advanced and Specialized Topics

Use this map after reading `SKILL.md`.
All paths are repository-relative and verified in this checkout.

## Core advanced docs
- `docs/configuration.rst` | required-file layout and configuration file semantics.
- `docs/parameter-file.rst` | namelist semantics and path-resolution rules.
- `docs/initial-condition.rst` | advanced `cICType` modes and easi hooks.
- `docs/local-timestepping.rst` | LTS clustering and constraints.
- `docs/checkpointing.rst` | checkpoint write/restart behavior.
- `docs/fault-tagging.rst` | dynamic-rupture fault tag limits and usage.
- `docs/standard-rupture-format.rst` | NRF/SRF source-format details.
- `docs/point-source-older-implementation.rst` | legacy point-source behavior.

## Geometry and meshing deep dives
- `docs/meshing-with-pumgen.rst` | PUML conversion and XML-driven meshing.
- `docs/manually-fixing-an-intersection-in-gocad.rst` | GOCAD intersection repair workflow.
- `docs/remeshing-the-topography.rst` | topography remeshing workflow.

## Performance and operations
- `docs/memory-requirements.rst` | memory scaling and balancing guidance.
- `docs/frontera.rst` | Frontera-specific run/build notes.
- `docs/behind-firewall.rst` | network/proxy workarounds for restricted systems.
- `docs/heisenbug.rst` | debugging non-deterministic issues.
- `docs/tpv13.rst` | advanced benchmark setup for plasticity/nucleation checks.

## Project and reproducibility references
- `docs/reproducible-research.rst` | reproducibility practices.
- `docs/related-publications.rst` | publication context and citation anchors.
- `docs/acknowledge.rst` | acknowledgment policy.
- `docs/copyrights.rst` | licensing/copyright notes.
- `CODE_OF_CONDUCT.md` | contributor conduct policy.
