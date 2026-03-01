# seissol source map: Simulation Workflows

Use this map only after exhausting topic docs in `references/doc_map.md`.
All entry points below are verified to exist in this checkout.

## Fast source navigation
- `rg -n "main\(|readYamlParams" app/Main/Main.cpp`
- `rg -n "initSeisSol|seissolMain|closeSeisSol" src/Initializer/InitProcedure/Init.cpp`
- `rg -n "Simulator::simulate|TimeManager::advanceInTime" src/Solver`
- `rg -n "CheckpointManager::(makeWriter|loadCheckpoint)" src/IO/Instance/Checkpoint/CheckpointManager.cpp`
- `rg -n "simulationStart|syncPoint" src/ResultWriter/{WaveFieldWriter.cpp,FaultWriter.cpp,ReceiverWriter.cpp,EnergyOutput.cpp}`

## Function-level entry points
- `app/Main/Main.cpp` | symbols: `main`, `readYamlParams` | behavior check: launch CLI parsing and parameter-file ingestion.
- `src/Initializer/InitProcedure/Init.cpp` | symbols: `initSeisSol`, `seissolMain`, `closeSeisSol` | behavior check: lifecycle orchestration around the run loop.
- `src/Solver/Simulator.cpp` | symbols: `Simulator::simulate` | behavior check: high-level run loop and abort handling.
- `src/Solver/TimeStepping/TimeManager.cpp` | symbols: `addClusters`, `advanceInTime`, `setInitialTimes`, `setReceiverClusters` | behavior check: timestep progression, synchronization, and output triggers.
- `src/Solver/TimeStepping/HaloCommunication.cpp` | symbols: halo-exchange routines | behavior check: inter-rank communication behavior during timestepping.
- `src/IO/Instance/Checkpoint/CheckpointManager.cpp` | symbols: `CheckpointManager::makeWriter`, `CheckpointManager::loadCheckpoint` | behavior check: checkpoint writing and restart readback.
- `src/IO/Instance/Checkpoint/CheckpointManager.h` | symbols: `registerTree`, `registerData` | behavior check: checkpoint variable registration coverage.
- `src/ResultWriter/AsyncIO.cpp` | symbols: `AsyncIO::initDispatcher`, `AsyncIO::finalizeDispatcher` | behavior check: async output mode setup/teardown.
- `src/ResultWriter/WaveFieldWriter.cpp` | symbols: `simulationStart`, `syncPoint`, `write` | behavior check: wavefield write cadence across run/restart.
- `src/ResultWriter/FaultWriter.cpp` | symbols: `simulationStart`, `syncPoint` | behavior check: fault output cadence across run/restart.
- `src/ResultWriter/ReceiverWriter.cpp` | symbols: `simulationStart`, `syncPoint` | behavior check: receiver output continuity across run/restart.
- `src/ResultWriter/EnergyOutput.cpp` | symbols: `simulationStart`, `syncPoint`, `checkAbortCriterion` | behavior check: energy checkpoints and stop criteria.

## Workflow validation helpers
- `postprocessing/validation/compare-energies.py` | verify restart continuity in energy histories.
- `postprocessing/validation/compare-receivers.py` | verify receiver continuity across restart boundaries.
- `postprocessing/validation/compare-faults.py` | verify fault-output continuity across restart boundaries.
