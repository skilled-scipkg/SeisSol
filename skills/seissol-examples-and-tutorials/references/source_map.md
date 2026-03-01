# seissol source map: Examples and Tutorials

Use this map only after exhausting topic docs in `references/doc_map.md`.
All entry points below are verified to exist in this checkout.

## Fast source navigation
- `rg -n "readYamlParams|main\(" app/Main/Main.cpp`
- `rg -n "readPath|readSubNode|warnUnknown" src/Initializer/Parameters/ParameterReader.cpp`
- `rg -n "loadSources|readNRF|transformMomentTensor" src/SourceTerm`
- `rg -n "ReceiverWriter::|FaultWriter::|WaveFieldWriter::" src/ResultWriter`

## Function-level entry points
- `app/Main/Main.cpp` | symbols: `readYamlParams`, `main` | behavior check: example launch CLI flow.
- `src/Initializer/Parameters/ParameterReader.cpp` | symbols: `readPath`, `readPathOrFail`, `warnUnknown` | behavior check: tutorial parameter-file path and key parsing.
- `src/SourceTerm/Manager.cpp` | symbols: `Manager::loadSources` | behavior check: source-file ingestion for benchmark cases.
- `src/SourceTerm/NRFReader.cpp` | symbols: `readNRF` | behavior check: NRF benchmark source parsing.
- `src/SourceTerm/PointSource.cpp` | symbols: `transformMomentTensor` | behavior check: point-source orientation mapping.
- `src/ResultWriter/ReceiverWriter.cpp` | symbols: `parseReceiverFile`, `ReceiverWriter::init`, `ReceiverWriter::syncPoint` | behavior check: receiver output from tutorial runs.
- `src/ResultWriter/FaultWriter.cpp` | symbols: `FaultWriter::init`, `FaultWriter::syncPoint` | behavior check: fault-output generation in examples.
- `src/ResultWriter/WaveFieldWriter.cpp` | symbols: `WaveFieldWriter::init`, `WaveFieldWriter::write` | behavior check: wavefield artifact generation.

## Tutorial validation helpers
- `preprocessing/science/read_ini_fault_parameter.py` | inspect initialized fault parameters.
- `postprocessing/validation/compare-faults.py` | compare example fault outputs against references.
- `postprocessing/validation/compare-receivers.py` | compare receiver traces across runs.
- `postprocessing/validation/compare-energies.py` | compare energy histories across runs.
