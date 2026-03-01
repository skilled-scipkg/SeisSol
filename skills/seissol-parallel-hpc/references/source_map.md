# seissol source map: Parallel and HPC

Use this map only after exhausting topic docs in `references/doc_map.md`.
All entry points below are verified to exist in this checkout.

## Fast source navigation
- `rg -n "Mpi::(init|bindAcceleratorDevice|setDataTransferModeFromEnv)" src/Parallel/MPI.cpp`
- `rg -n "Pinning::(checkEnvVariables|getFreeCPUsMask|pinToFreeCPUs)" src/Parallel/Pin.cpp`
- `rg -n "OpenMP::(enabled|threadCount|threadId)" src/Parallel/OpenMP.cpp`
- `rg -n "HelperThread::(start|stop|restart)" src/Parallel/HelperThread.cpp`
- `rg -n "initDispatcher|finalizeDispatcher" src/ResultWriter/AsyncIO.cpp`

## Function-level entry points
- `src/Parallel/MPI.cpp` | symbols: `Mpi::init`, `Mpi::bindAcceleratorDevice`, `Mpi::setDataTransferModeFromEnv` | behavior check: MPI init and GPU transfer-mode selection.
- `src/Parallel/OpenMP.cpp` | symbols: `OpenMP::enabled`, `OpenMP::threadCount` | behavior check: OpenMP runtime availability and thread counts.
- `src/Parallel/Pin.cpp` | symbols: `Pinning::checkEnvVariables`, `Pinning::getFreeCPUsMask`, `Pinning::pinToFreeCPUs` | behavior check: affinity/env-var interpretation and free-core handling.
- `src/Parallel/HelperThread.cpp` | symbols: `HelperThread::start`, `HelperThread::stop` | behavior check: communication/helper-thread lifecycle.
- `src/Parallel/AcceleratorDevice.cpp` | symbols: `bindNativeDevice`, `printInfo` | behavior check: GPU device binding and diagnostics.
- `src/Parallel/Runtime/Stream.cpp` | symbols: `ManagedStream`, `ManagedEvent` | behavior check: accelerator stream/event lifecycle.
- `src/IO/Datatype/MPIType.cpp` | symbols: MPI datatype conversion helpers | behavior check: IO datatype MPI compatibility.
- `src/ResultWriter/AsyncIO.cpp` | symbols: `AsyncIO::initDispatcher`, `AsyncIO::finalizeDispatcher` | behavior check: async writer thread lifecycle.
- `src/ResultWriter/FaultWriter.cpp` | symbols: `FaultWriter::setUp` | behavior check: writer-thread affinity on comm-thread runs.

## Runtime validation hooks
- `app/Main/Main.cpp` | confirm startup path applies env-driven runtime settings.
- `postprocessing/validation/compare-energies.py` | detect unintended changes after placement/env tuning.
