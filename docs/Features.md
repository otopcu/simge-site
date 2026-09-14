# Features

SimGe brings HLA object modeling, federation architecture design, code generation, and telemetry inspection into one Windows application. Start with a model, define the federates that use it, generate their integration code, and inspect captured runs.

## Object Model Development Environment (OMDE)

Work with FOM and SOM modules in dedicated workspaces that combine a dashboard, OMT tables, document viewers, and diagrams.

### Dashboard and model analysis

- Switch between **Composed** and **Module Only** analysis to inspect a dependency closure or the selected module.
- Explore class and property counts, structural profiles, semantic indicators, and model integrity findings.
- Inspect unresolved datatype references, copy findings as Markdown, and export structural visualizations.
- Use calibration controls to explore how structural-profile settings affect the displayed interpretation.

See [FOM Dashboard](UserManual/Dashboard.md). Structural metric definitions are linked from the [published research reference](UserManual/MetricsReports.md#metric-definitions-and-published-reference); the indicators guide design review and do not guarantee runtime performance.

### OMT table editing

- Edit object and interaction classes, attributes, parameters, and datatypes through tables and dedicated dialogs.
- Manage identification, dimensions, time representations, tags, synchronization points, transportations, update rates, switches, notes, and service usage.
- Search datatype selections across local definitions and loaded dependencies, with type-kind and source-module indicators.
- Preview datatype change impact before deletion or renaming, and inspect references on demand.

See [Object Model Editor](UserManual/OME.md).

### FDD/FED inspection and exchange

- Import and export **HLA 1.3 FED**, **IEEE 1516-2010 FDD**, and **IEEE 1516-2025 FDD** models.
- Inspect model documents with syntax highlighting, line numbers, search, zoom, and copy/export actions.
- Validate XML against selectable **DIF**, **FDD**, or **OMT** schemas, using standalone-module or composed-dependency scope.
- Preserve dependency-owned datatype names while working with individual modules.

The validation scope is separate from the exported model. See [Importing & Exporting](UserManual/ImportExport.md) and [FOM Validation](UserManual/Validation.md). HLA 1.3 FED has no XML-schema validation.

### Model diagrams

- Explore object-class, interaction-class, and directed-interaction diagrams derived from the model.
- Control the display of properties, inherited members, datatypes, and base-module classes.
- Pan, zoom, navigate large diagrams, and export PNG images for documentation and reviews.

See [Diagram Editor](UserManual/Diagrams.md).

## Federation Architecture Modeling Environment (FAME)

- Define federate applications, federation execution properties, RTI settings, and FOM/SOM associations.
- Inspect federation composition through the **Federation Structure Diagram**.
- Model RTI infrastructure, network connections, and nested execution environments in a **UML Deployment Diagram**.
- Review multiplicity, module associations, and architecture diagnostics, then navigate to the related object model.

See [Federation Architecture](UserManual/FAME.md).

## Code Generator (CG)

- Generate **C# / .NET** integration code for each federate's SOM, targeting the **Fora** client for IEEE 1516-2025.
- Obtain an asynchronous federation lifecycle, federate callback scaffolding, SOM handles, object/interaction models, entities, datatypes, and codecs.
- Keep generated files and hand-editable scaffold files separate so application logic has defined extension points.
- Review per-federate diagnostics, the targeted Fora API contract, and a generated API summary in **README.generated.md**.
- Enable metric-driven object codecs, delta-tracker helpers, and supported dispatch strategies where applicable; enable telemetry instrumentation for measured runs.

Capabilities depend on the model and selected options. See [Code Generator](UserManual/CodeGenerator.md) for compatibility, regeneration guidance, and strategy boundaries, including experimental programmatic pruning.

## Report Generator (RG)

- Produce a printable report of OMT content for design reviews and archiving.
- Review, print, and export the report through the built-in report viewer.
- Read module analysis reports with metrics, integrity findings, datatype impact, and advisory generation hints.
- Refresh reports after model edits; large report datasets are prepared in the background.

See [Model Metrics & Reports](UserManual/MetricsReports.md).

## Project Explorer and workspaces

- Navigate modules, OMT elements, federate applications, and generated output from a shared project tree.
- Copy, paste, or drag supported model elements with their required datatype dependencies.
- Inspect the read-only MOM library and use **Show Impact / Usages** on datatype nodes.
- Recover missing module files and keep several editors open, including detachable workspace windows.

See [Project Explorer](UserManual/ProjectExplorer.md) and [Workspaces](UserManual/Workspaces.md).

## Telemetry Inspector Visualizer

Available under **Tools → Experimental**, the visualizer opens a captured run's manifest and telemetry logs inside SimGe.

- Check whether the recorded FOM matches the active project, or inspect the run without an open project.
- Examine warm-up behavior, steady-state latency, event sub-phases, selectivity, and operational drift.
- Review scenario context and accompanying Markdown reports.
- Export charts as PNG and copy analysis grids as Markdown.

See [Telemetry Visualizer](UserManual/TelemetryVisualizer.md). Producing runs with the command-line validation harness requires the source-based research toolchain described in [Fora Telemetry & Validation](UserManual/ForaTelemetry.md). RPR/NETN corpora and their reports are not included in the installer.

## Getting started

Read [Installation & Updates](UserManual/Installation.md) for current requirements and [Quick Start](UserManual/QuickStart.md) for the first-project workflow. SimGe's research and educational usage terms are stated in the [Disclaimer](Disclaimer.md).

---
Updated September 14, 2026
