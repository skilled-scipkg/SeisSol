# seissol source map: Analysis and Output

Use this map only after exhausting topic docs in `references/doc_map.md`.
All entry points below are verified to exist in this checkout.

## Fast source navigation
- `rg -n "FaultWriter::|WaveFieldWriter::|FreeSurfaceWriter::|ReceiverWriter::" src/ResultWriter`
- `rg -n "EnergyOutput::(init|syncPoint|compute|write|checkAbortCriterion)" src/ResultWriter/EnergyOutput.cpp`
- `rg -n "VtkHdfWriter::|TableWriter::|PostProcessor::" src/IO src/ResultWriter`
- `rg -n "compare-(faults|receivers|energies)" postprocessing/validation`

## Function-level entry points
- `src/ResultWriter/FaultWriter.cpp` | symbols: `FaultWriter::init`, `FaultWriter::simulationStart`, `FaultWriter::syncPoint` | behavior check: fault output cadence and startup flush.
- `src/ResultWriter/WaveFieldWriter.cpp` | symbols: `WaveFieldWriter::init`, `WaveFieldWriter::write`, `WaveFieldWriter::syncPoint` | behavior check: wavefield masks/refinement/write cadence.
- `src/ResultWriter/FreeSurfaceWriter.cpp` | symbols: `FreeSurfaceWriter::constructSurfaceMesh`, `FreeSurfaceWriter::write`, `FreeSurfaceWriter::syncPoint` | behavior check: free-surface mesh build and write schedule.
- `src/ResultWriter/ReceiverWriter.cpp` | symbols: `parseReceiverFile`, `ReceiverWriter::init`, `ReceiverWriter::syncPoint` | behavior check: receiver-file parsing and sampling output.
- `src/ResultWriter/EnergyOutput.cpp` | symbols: `EnergyOutput::init`, `EnergyOutput::computeEnergies`, `EnergyOutput::writeEnergies`, `EnergyOutput::checkAbortCriterion` | behavior check: energy accumulation/reduction and abort logic.
- `src/IO/Instance/Mesh/VtkHdf.cpp` | symbols: `VtkHdfWriter::addHook`, `VtkHdfWriter::makeWriter` | behavior check: VTK-HDF metadata and timestep/counter writes.
- `src/IO/Instance/Point/TableWriter.cpp` | symbols: `addQuantity`, `getRowDatatype`, `addCellRaw` | behavior check: tabular receiver/output record layout.
- `src/ResultWriter/PostProcessor.cpp` | symbols: `integrateQuantities`, `setIntegrationMask` | behavior check: element-wise integration masks and integral storage.

## Postprocessing and regression checks
- `postprocessing/visualization/tools/extractDataFromUnstructuredOutput.py` | extract wavefield/fault fields for checks.
- `postprocessing/visualization/receiver/src/viewrec.py` | quick receiver trace inspection.
- `postprocessing/validation/compare-faults.py` | fault regression check.
- `postprocessing/validation/compare-receivers.py` | receiver regression check.
- `postprocessing/validation/compare-energies.py` | energy time-series regression check.
