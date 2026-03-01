# seissol source map: Advanced and Specialized Topics

Use this map only after exhausting topic docs in `references/doc_map.md`.
All entry points below are verified to exist in this checkout.

## Fast source navigation
- `rg -n "checkpoint|restart|checkpointinterval" src app`
- `rg -n "readInitializationParameters|readLtsParameters|readPath" src/Initializer`
- `rg -n "readNRF|transformMomentTensor|loadSources" src/SourceTerm`
- `rg -n "memcopy|allocateMemory|toPartitionerType" src`

## Function-level entry points
- `src/IO/Instance/Checkpoint/CheckpointManager.cpp` | symbols: `CheckpointManager::makeWriter`, `CheckpointManager::loadCheckpoint` | behavior check: checkpoint file naming and restart-time restoration.
- `src/IO/Instance/Checkpoint/CheckpointManager.h` | symbols: `registerTree`, `registerData`, `registerPaddedData` | behavior check: variable registration and padding transforms.
- `src/Initializer/Parameters/ParameterReader.cpp` | symbols: `readPath`, `readSubNode`, `warnUnknown` | behavior check: advanced parameter parsing and unknown-key handling.
- `src/Initializer/Parameters/InitializationParameters.cpp` | symbols: `readInitializationParameters` | behavior check: advanced `&IniCondition` modes (`easi`, travelling, scholte).
- `src/Initializer/Parameters/LtsParameters.cpp` | symbols: `readLtsParameters`, `parseAutoMergeCostBaseline` | behavior check: LTS cluster settings and auto-merge constraints.
- `src/Memory/MemoryAllocator.cpp` | symbols: `ManagedAllocator::allocateMemory`, `memcopy`, `memzero` | behavior check: allocation placement/alignment behavior.
- `src/Geometry/PartitioningLib.cpp` | symbols: `toPartitionerType`, `toStringView` | behavior check: partitioner selection fallback.
- `src/Geometry/PUMLReader.cpp` | symbols: `PUMLReader::read`, `PUMLReader::partition`, `PUMLReader::getMesh` | behavior check: mesh ingest and partition path.
- `src/SourceTerm/NRFReader.cpp` | symbols: `readNRF` | behavior check: NRF source parsing and field-size assumptions.
- `src/SourceTerm/PointSource.cpp` | symbols: `transformMomentTensor` | behavior check: strike/dip/rake tensor rotation.
- `src/SourceTerm/Manager.cpp` | symbols: `Manager::loadSources` | behavior check: source-file mapping to mesh clusters.
- `preprocessing/meshing/gmsh2gambit/src/main.cpp` | symbols: `main`, `convert` | behavior check: legacy mesh-conversion path.
- `preprocessing/science/read_ini_fault_parameter.py` | symbols: `compute_tractions` | behavior check: initial traction reconstruction.

## Validation helpers
- `postprocessing/validation/compare-faults.py` | compare fault fields across runs.
- `postprocessing/validation/compare-mesh.py` | check mesh-level consistency across outputs.
- `postprocessing/validation/compare-receivers.py` | receiver-trace comparison utility.
