---
name: seissol-build-and-install
description: This skill should be used when users ask about build and install in seissol; it prioritizes documentation references and then source inspection only for unresolved details.
---

# seissol: Build and Install

## High-Signal Playbook
### Route conditions
- First simulation launch/restart behavior -> `seissol-simulation-workflows`.
- Parameter-file/modeling correctness -> `seissol-inputs-and-modeling`.
- MPI/OpenMP/GPU placement and scheduler tuning -> `seissol-parallel-hpc`.

### Triage questions
- CPU-only or GPU build target? (`docs/build-seissol.rst`, `docs/gpus.rst`)
- Host CPU architecture and accelerator architecture? (`docs/build-seissol.rst`, `docs/build-parameters.rst`)
- Spack module flow or manual dependency installation? (`docs/build-overview.rst`, `docs/build-dependencies.rst`)
- Is the failure in configure/build, codegen, or runtime (`SIGILL`)? (`docs/build-problems.rst`)
- Were submodules initialized after cloning/updating? (`docs/build-problems.rst`)

### Canonical workflow
1. Choose dependency strategy (`spack` preferred, manual fallback) (`docs/build-overview.rst`, `docs/build-dependencies.rst`).
2. Install required toolchain/libs (CMake, Python, MPI, Eigen, HDF5, easi; optional generators/partitioners) (`docs/build-dependencies.rst`).
3. Ensure repository and submodules are complete (`docs/build-problems.rst`).
4. Configure CMake with explicit `HOST_ARCH`, `ORDER`, `EQUATIONS`, `PRECISION` (`docs/build-seissol.rst`, `docs/build-parameters.rst`).
5. Build with `make` or `ninja`; optionally build proxy-only for smoke testing (`docs/build-seissol.rst`).
6. Run proxy first, then a real example (`docs/build-run.rst`, `docs/a-first-example.rst`).

### Minimal working example
```bash
# CPU build baseline
mkdir -p build && cd build
cmake -DNUMA_AWARE_PINNING=ON -DASAGI=ON -DPRECISION=double -DORDER=4 -DEQUATIONS=elastic ..
make -j 4

# smoke test
./SeisSol_proxy_Release_hsw_4_elastic 1000 10 all
```

### Pitfalls and fixes
- Build fails due to missing files -> `git submodule update --init --recursive` (`docs/build-problems.rst`).
- Runtime `SIGILL` -> lower `HOST_ARCH` (`skx` -> `hsw`, fallback `noarch`) (`docs/build-problems.rst`, `docs/build-seissol.rst`).
- Codegen/toolchain instability -> use `GEMM_TOOLS_LIST=PSpaMM`, fallback `Eigen` for minimal build (`docs/build-problems.rst`).
- Manual dependency install not discovered -> export `CMAKE_PREFIX_PATH`/`PKG_CONFIG_PATH` (`docs/build-seissol.rst`).
- `Inf/NaN` in single precision -> rebuild with `PRECISION=double` (`docs/build-seissol.rst`, `docs/build-parameters.rst`).
- Cluster login and compute nodes differ in ISA -> build/test on target compute architecture (`docs/build-problems.rst`).

### Convergence and validation checks
- Executable name matches intended configuration tuple (`build type`, `host arch`, `order`, `equations`) (`docs/build-parameters.rst`).
- Proxy full-functionality run completes (`docs/build-run.rst`).
- First real run produces expected output artifacts (`docs/a-first-example.rst`).
- For GPU builds, validate one-rank-per-GPU launch behavior (`docs/gpus.rst`, `docs/build-run.rst`).

## Scope
- Handle build, installation, compilation, and environment setup.
- Keep guidance architecture-aware and failure-driven.

## Primary documentation references
- `docs/build-overview.rst`
- `docs/build-dependencies.rst`
- `docs/build-seissol.rst`
- `docs/build-parameters.rst`
- `docs/build-archs.rst`
- `docs/build-problems.rst`
- `docs/build-run.rst`
- `docs/gpus.rst`
- `docs/supermuc.rst`
- `CMakeLists.txt`

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
- `CMakeLists.txt`
- `src/CMakeLists.txt`
- `app/Main/CMakeLists.txt`
- `codegen/CMakeLists.txt`
- `src/configprint.cmake`
- `src/Initializer/Parameters/SeisSolParameters.cpp`
- `src/Initializer/Parameters/ParameterReader.cpp`
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app codegen postprocessing preprocessing src`).
