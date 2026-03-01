# seissol source map: Getting Started

Use this map only after exhausting topic docs in `references/doc_map.md`.
All entry points below are verified to exist in this checkout.

## Fast source navigation
- `rg -n "readYamlParams|main\(" app/Main/Main.cpp`
- `rg -n "initSeisSol|seissolMain|closeSeisSol" src/Initializer/InitProcedure/Init.cpp`
- `rg -n "readPath|readSubNode|readSeisSolParameters" src/Initializer/Parameters`
- `rg -n "simulate|advanceInTime|syncPoint" src/Solver src/ResultWriter`

## Function-level entry points
- `app/Main/Main.cpp` | symbols: `readYamlParams`, `main` | behavior check: command-line startup and parameter loading.
- `src/Initializer/InitProcedure/Init.cpp` | symbols: `initSeisSol`, `seissolMain`, `closeSeisSol` | behavior check: startup/teardown sequence.
- `src/Initializer/Parameters/ParameterReader.cpp` | symbols: `readPath`, `readSubNode`, `warnUnknown` | behavior check: launch-directory relative path handling.
- `src/Initializer/Parameters/SeisSolParameters.cpp` | symbols: `readSeisSolParameters` | behavior check: top-level parameter decoding.
- `src/Solver/Simulator.cpp` | symbols: `Simulator::simulate` | behavior check: main simulation execution loop.
- `src/Solver/TimeStepping/TimeManager.cpp` | symbols: `addClusters`, `advanceInTime`, `setInitialTimes` | behavior check: timestep progression and synchronization.
- `src/ResultWriter/WaveFieldWriter.cpp` | symbols: `init`, `write`, `syncPoint` | behavior check: wavefield artifact creation.
- `src/ResultWriter/FaultWriter.cpp` | symbols: `init`, `syncPoint` | behavior check: fault artifact creation.
- `src/ResultWriter/ReceiverWriter.cpp` | symbols: `parseReceiverFile`, `init`, `syncPoint` | behavior check: receiver artifact creation.

## Validation helpers
- `postprocessing/visualization/receiver/src/viewrec.py` | quick receiver trace inspection.
- `postprocessing/validation/compare-faults.py` | fault-output sanity checks.
- `postprocessing/validation/compare-receivers.py` | receiver sanity checks.
- `postprocessing/validation/compare-energies.py` | energy sanity checks.
