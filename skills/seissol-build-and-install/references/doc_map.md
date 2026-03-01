# seissol documentation map: Build and Install

Use this map after reading `SKILL.md`.
All paths are repository-relative and verified in this checkout.

## Core docs (read first)
- `docs/build-overview.rst` | recommended build strategy and dependency flow.
- `docs/build-dependencies.rst` | compiler/library requirements.
- `docs/build-seissol.rst` | CMake configure/build commands.
- `docs/build-parameters.rst` | required/optional build parameters.
- `docs/build-archs.rst` | supported CPU/GPU architecture tags.
- `docs/build-problems.rst` | high-frequency build/runtime failures.
- `docs/build-run.rst` | smoke-test and launch guidance after build.
- `docs/gpus.rst` | GPU build and runtime specifics.

## Cluster-specific build references
- `docs/supermuc.rst` | SuperMUC module and launch notes.
- `docs/supermuc-ng-phase2.rst` | SuperMUC-NG phase-2 specifics.
- `docs/coolmuc4.rst` | CoolMUC4 build/setup notes.
- `docs/lumi.rst` | LUMI build notes.
- `docs/leonardo.rst` | Leonardo build notes.
- `docs/frontier.rst` | Frontier build notes.

## Build system source of truth
- `CMakeLists.txt` | top-level options and project wiring.
- `src/CMakeLists.txt` | core target composition.
- `codegen/CMakeLists.txt` | generated-kernel build integration.
- `app/Main/CMakeLists.txt` | main executable target definition.
