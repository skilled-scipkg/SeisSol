# seissol source map: Build and Install

Use this map only after exhausting topic docs in `references/doc_map.md`.
All entry points below are verified to exist in this checkout.

## Fast source navigation
- `rg -n "option\(|set\((HOST_ARCH|ORDER|EQUATIONS|PRECISION|DEVICE_BACKEND)" CMakeLists.txt src/CMakeLists.txt`
- `rg -n "add_subdirectory|add_executable|target_link_libraries" CMakeLists.txt app src codegen`
- `rg -n "readSeisSolParameters|readModelParameters" src/Initializer/Parameters`
- `rg -n "setDataTransferModeFromEnv|bindNativeDevice" src/Parallel`

## Build-system entry points
- `CMakeLists.txt` | behavior check: top-level options, feature toggles, and subdirectory wiring.
- `src/CMakeLists.txt` | behavior check: core solver library/targets.
- `app/Main/CMakeLists.txt` | behavior check: main executable tuple generation and linkage.
- `app/Proxy/CMakeLists.txt` | behavior check: proxy target for smoke/performance checks.
- `codegen/CMakeLists.txt` | behavior check: generated-kernel integration into build graph.
- `src/configprint.cmake` | behavior check: runtime-visible build tuple and option printout.
- `src/sources.cmake` | behavior check: translation-unit inclusion/exclusion.

## Runtime confirmation entry points
- `app/Main/Main.cpp` | symbols: `main`, `readYamlParams` | behavior check: startup path from binary to parameter parsing.
- `src/Initializer/Parameters/SeisSolParameters.cpp` | symbols: `readSeisSolParameters` | behavior check: top-level run/build parameter decode.
- `src/Initializer/Parameters/ModelParameters.cpp` | symbols: `readModelParameters` | behavior check: equation/material compatibility at runtime.
- `src/Parallel/MPI.cpp` | symbols: `Mpi::setDataTransferModeFromEnv` | behavior check: MPI transfer-mode env handling.
- `src/Parallel/AcceleratorDevice.cpp` | symbols: `bindNativeDevice`, `printInfo` | behavior check: accelerator binding and visibility.

## Practical validation commands
- `cmake -LAH <build_dir> | rg "HOST_ARCH|ORDER|EQUATIONS|PRECISION|DEVICE_BACKEND"`
- `./SeisSol_proxy_<configuration> 1000 10 all`
