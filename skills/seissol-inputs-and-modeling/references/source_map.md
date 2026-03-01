# seissol source map: Inputs and Modeling

Use this map only after exhausting topic docs in `references/doc_map.md`.
All entry points below are verified to exist in this checkout.

## Fast source navigation
- `rg -n "read(SeisSol|Model|DR|Initialization|Mesh|Source|Output)Parameters" src/Initializer/Parameters`
- `rg -n "readPath|readPathOrFail|readSubNode|warnUnknown" src/Initializer/Parameters/ParameterReader.cpp`
- `rg -n "PUMLReader::|toPartitionerType" src/Geometry`
- `rg -n "loadSources|readNRF|transformMomentTensor" src/SourceTerm`
- `rg -n "computeDeltaT|copyStorageToLocal" src/DynamicRupture/FrictionLaws/FrictionSolver.cpp`

## Function-level entry points
- `src/Initializer/Parameters/ParameterReader.cpp` | symbols: `readPath`, `readPathOrFail`, `readSubNode`, `warnUnknown` | behavior check: namelist path resolution and unknown-field diagnostics.
- `src/Initializer/Parameters/SeisSolParameters.cpp` | symbols: `readSeisSolParameters` | behavior check: top-level parameter aggregation.
- `src/Initializer/Parameters/ModelParameters.cpp` | symbols: `readModelParameters` | behavior check: equation/material model selection.
- `src/Initializer/Parameters/DRParameters.cpp` | symbols: `readDRParameters` | behavior check: dynamic-rupture friction parameter decoding.
- `src/Initializer/Parameters/InitializationParameters.cpp` | symbols: `readInitializationParameters` | behavior check: `&IniCondition` mode and file settings.
- `src/Initializer/Parameters/MeshParameters.cpp` | symbols: `readMeshParameters` | behavior check: mesh file/partitioning options.
- `src/Initializer/Parameters/SourceParameters.cpp` | symbols: `readSourceParameters` | behavior check: source type and source-file path requirements.
- `src/Initializer/Parameters/OutputParameters.cpp` | symbols: `readOutputParameters`, `readWaveFieldParameters`, `readReceiverParameters` | behavior check: output-mask and cadence coupling.
- `src/Geometry/PUMLReader.cpp` | symbols: `PUMLReader::read`, `PUMLReader::partition`, `PUMLReader::getMesh` | behavior check: mesh ingest and topology mapping.
- `src/Geometry/PartitioningLib.cpp` | symbols: `toPartitionerType` | behavior check: partitioning library selection.
- `src/SourceTerm/Manager.cpp` | symbols: `Manager::loadSources` | behavior check: source mapping to elements/clusters.
- `src/SourceTerm/NRFReader.cpp` | symbols: `readNRF` | behavior check: NRF field parsing.
- `src/SourceTerm/PointSource.cpp` | symbols: `transformMomentTensor` | behavior check: strike/dip/rake conversion.
- `src/DynamicRupture/FrictionLaws/FrictionSolver.cpp` | symbols: `computeDeltaT`, `copyStorageToLocal` | behavior check: friction-law timestep/data-flow behavior.

## Validation helpers
- `preprocessing/science/read_ini_fault_parameter.py` | inspect initialized fault tractions and stress state.
- `postprocessing/validation/compare-faults.py` | validate friction/source effects in fault outputs.
- `postprocessing/validation/compare-mesh.py` | validate mesh-level assumptions after preprocessing.
