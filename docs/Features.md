# Features

SimGe brings HLA object modeling, federation architecture design, code generation, and telemetry inspection into one Windows application. Start with a model, define the federates that use it, generate their integration code, and inspect captured runs.

## Object Model Development Environment (OMDE)

Create, explore, and refine your HLA Object Models in one connected workspace. OMDE brings together four integrated components: **Dashboard, OMT Table Editor, FDD/FED Viewers (1.3 / 2010 / 2025), and Diagram Editor**, so you can move from a model overview to detailed editing, document inspection, and visual design review.

Work across **OMT 1.3**, **IEEE 1516-2010 (HLA Evolved)**, and **IEEE 1516-2025 (HLA 4)** through supported model import, editing, and export workflows. Keep multiple Object Models open in parallel, each in its own independent OMDE workspace with dedicated toolbars and commands that adapt to the active view. Detach workspaces into separate windows to compare models side by side.

### 1. Dashboard

Understand your model at a glance and focus your next editing step. The Dashboard brings architectural complexity, model health, and analysis results together in a visual overview.

- **See the big picture:** Explore model size, class hierarchies, architectural complexity metrics, and semantic indicators through the Overview, Structure, Semantics, and Quality views.
- **Spot maintenance issues early:** Identify unused data types, unresolved type references, and empty leaf classes that may need review.
- **Move from insight to action:** Navigate from linked analysis cards directly to the related OMT tables.
- **Choose your analysis scope:** Focus on the selected module with **Module Only**, or examine it together with its dependencies using **Composed**.
- **Share your findings:** Copy summaries and diagnostic findings, export structural visualizations, and refresh the analysis as your model evolves.
- **Explore design choices:** Use interactive structural and semantic views, with calibration controls for a closer look at model profiles.

See [FOM Dashboard](UserManual/Dashboard.md) for a guided tour. Structural metric definitions are available through the [published research reference](UserManual/MetricsReports.md#metric-definitions-and-published-reference).

### 2. OMT Table Editor

Build and maintain Object Models with structured tables and dedicated editing dialogs. A consistent editing experience helps you work efficiently across model elements while keeping their relationships in view.

- **Edit the complete model:** Manage Object Classes, Interaction Classes, Attributes, Parameters, Data Types, Dimensions, and Directed Interactions.
- **Define simulation time:** Edit Time Representations, including **LogicalTime** and **LogicalTimeInterval** definitions for IEEE 1516-2025 models.
- **Keep model information organized:** Maintain identification metadata, Tags, Transportations, Switches, Update Rates, Synchronization Settings, Notes, and service usage.
- **Find the right data type quickly:** Search local and dependency-provided types with highlighted matches, category indicators, and source-module labels.
- **Review changes with context:** Inspect datatype usages and preview the impact of renaming or deleting a type before applying the change.
- **Work consistently across tables:** Use context-aware editing controls and dedicated dialogs to review related properties and maintain standards-aligned model data.

See [Object Model Editor](UserManual/OME.md).

### 3. FDD/FED Viewers (HLA 1.3, 1516-2010, 1516-2025)

Bring existing models into SimGe and prepare documents for the HLA environments you work with. Integrated FED/FDD viewers make large model documents easier to inspect, validate, and share.

- **HLA 1.3:** Import and export FED files for legacy model exchange.
- **IEEE 1516-2010 (HLA Evolved):** Import, export, and validate FDD files.
- **IEEE 1516-2025 (HLA 4):** Import, export, and validate FDD files, with selectable DIF, FDD, and OMT schema options where applicable.
- **Cross-standard workflows:** Preserve model-identification text and dependency-owned datatype references through supported conversions.

Document inspection tools keep essential information within reach:

- **Integrated Status Bar:** Track cursor line and column, selection details, and document or validation status while you review.
- **Inline Notifications:** Receive feedback on actions such as copying without interrupting your workflow.
- **Quick Zoom Controls:** Adjust text size for comfortable reading.
- **Syntax Highlighting:** Read FED and FDD structure with clear color differentiation; the shared viewer also supports generated code files.
- **Line Numbering:** Locate and discuss specific parts of a document precisely.
- **Search Panel (Ctrl+F):** Find content quickly with highlighted search results.
- **One-Click Copy All:** Copy the complete document for sharing or further review, or export it directly to a file.

For XML validation, choose a standalone module or its composed dependency closure. See [Importing & Exporting](UserManual/ImportExport.md) and [FOM Validation](UserManual/Validation.md) for format and scope guidance.

### 4. Diagram Editor

Turn model structure into a visual story for design reviews, documentation, and team discussions. Diagrams are generated directly from the underlying Object Model, helping you explore relationships without drawing them by hand.

- **Object Class Diagram:** Explore class hierarchies and their attributes in a familiar visual layout.
- **Interaction Class Diagram:** Review interaction hierarchies and their parameters.
- **Directed Interactions Diagram:** Make directed relationships between interaction classes and object classes visible for a clearer view of model coupling.
- **Flexible detail:** Show or hide properties, inherited members, data types, and base-module classes to match the purpose of your review.
- **Easy navigation:** Pan, zoom, adjust layouts, and use the mini-map to move through large models.
- **High-resolution PNG export:** Share clear diagrams in reports, presentations, and architecture reviews.

See [Diagram Editor](UserManual/Diagrams.md).

## Federation Architecture Modeling Environment (FAME)

Design your federation before you deploy it. FAME brings the definition and management of **Federation Architecture Models (FAM)** into a visual workspace, supporting early design, model validation, and prototyping. Connect federate applications with their Object Models, review the federation's composition, and explore its deployment topology.

### Federation Architecture Model (FAM)

Keep the information that defines your federation together and move easily between architecture design and object modeling.

- **Centralized configuration:** Manage federation execution names, federate names and types, RTI connection settings, and modular FOM/SOM associations within the model.
- **Jump to OMT:** Open the associated Object Model Editor directly from FOM/SOM selectors or module icons in the diagram.
- **MOM Explorer:** Inspect the IEEE 1516-2025 Management Object Model through the read-only system library in Project Explorer, with access to its standard objects, interactions, and data types.

### Federate Applications

Build up your federation one application at a time, with immediate visual feedback as the design takes shape.

- **Add and manage applications:** Create or remove federate applications and configure their model associations, host assignments, connection settings, and notes.
- **Make multiplicity visible:** Define the expected number of federate instances and recognize multi-instance applications through stacked boxes and multiplicity labels.
- **Catch configuration issues early:** Architectural health badges highlight invalid application names, missing SOM associations, and empty RTI connection settings directly on the diagram.
- **Move toward a prototype:** Launch code generation for the selected federate application from the same workspace.

### Federation Structure Diagram (FSD)

Explore how federate applications, Object Models, and the RTI fit together through an interactive view of federation composition.

- **Trace connections visually:** Hover over a federate or its connection icon to highlight its RTI link with a glow and a thicker line.
- **Inspect rich metadata:** Read connection details, multiplicity, and linked modules in contextual tooltips.
- **Keep selection in sync:** Select a federate in the diagram to focus its settings in the properties pane.
- **Read the architecture at a glance:** Use the diagram legend to distinguish applications, federates, OMT modules, and multi-instance groups.

### UML Deployment Diagram

Show how your federation maps onto hosts and the network. Switch to the deployment view to visualize **RTI infrastructure nodes, network buses, and nested execution environments**, with federate artifacts and Fora client components shown in context.

- **Describe the physical topology:** Use Host / Node assignments and connection settings to document where federate applications run and how they connect to the RTI.
- **Review deployment relationships:** Inspect hosts, execution environments, components, and protocol labels in a shared architectural view.
- **Share the design:** Use zoom and fit-to-window controls during reviews, then export the diagram for documentation and team discussions.

See [Federation Architecture](UserManual/FAME.md) for the workflow and [MOM Explorer](UserManual/ProjectExplorer.md#mom-explorer-system-library) for the standard model library.

## Code Generator (CG)

Turn your federation design into modern **C# / .NET federate code** for [**Fora.Client**](https://sites.google.com/view/okantopcu/fora), targeting **IEEE 1516-2025 (HLA 4)**. SimGe generates code separately for each federate application from its associated SOM, giving you a structured starting point for application development.

- **Layered architecture:** Keep federation integration, model representation, serialization, and application logic organized around clear responsibilities.
- **Asynchronous lifecycle:** Use the generated **Simulation Manager** to coordinate connect, create, join, initialize, resign, and dispose operations.
- **Extendable federate scaffold:** Add application behavior through a Fora-based partial class and defined callback extension points.

### Generated Components

- **Federate class:** A Fora-compatible callback scaffold with extension points for simulation behavior.
- **Object and interaction models:** C# representations of the SOM's HLA classes, with inheritance support, entity wrappers, and access to attributes and parameters.
- **Federate SOM class:** Model handles and initialization support for the federate's simulation object model.
- **Simulation Manager:** Lifecycle orchestration and helpers for working with the federation.
- **Data types and codecs:** Generated encoders, decoders, and supporting types for model data exchange.
- **Static Tags class:** Type-safe factory methods for encoding user-defined tags as byte arrays, including typed values and text tags.

### Additional Capabilities

- **Organized output:** Dedicated folders separate core integration, SOM definitions, model and codec files, and entity classes.
- **Project-level configuration:** Set naming, output location, metric-driven generation options, and telemetry preferences from Code Generator settings.
- **Model-driven options:** Generate specialized object codecs, delta-tracker helpers, and supported dispatch strategies where the model and selected settings make them applicable.
- **Accompanying model documents:** Export FED and FDD files alongside the generated source from the federation's FOM.
- **Generated API guide:** Read **README.generated.md** for the targeted Fora version, generated components, strategy decisions, and recommended extension points.

### Fora Telemetry Instrumentation

Enable **Fora Telemetry** in project settings to prepare generated federates for measured runs. SimGe instruments serialization in generated encoders, decoders, and applicable codecs, and emits **FomMetricsMetadata** with design-time semantic weights, volatility flags, and declared counts. This carries model context into the Fora validation workflow so you can compare captured behavior with the design that produced it.

See [Fora Telemetry & Validation](UserManual/ForaTelemetry.md) for capture requirements and [Telemetry Visualizer](UserManual/TelemetryVisualizer.md) for inspecting recorded runs.

### Reliable, Reportable Generation

- **Clear results per application:** Review **SUCCESS**, **WARNINGS**, **FAILED**, or **SKIPPED** outcomes, file counts, and diagnostics after generation.
- **Useful progress despite individual failures:** File-level errors are collected in the report while remaining generation steps continue where possible.
- **Early input checks:** Catch invalid C# namespaces and project or file names before generation, with suggested namespace corrections where available.
- **Protected file output:** Generated source files are written atomically within the configured output directory, reducing the risk of incomplete files.
- **Fora compatibility feedback:** Review contract-validation results against SimGe's bundled Fora API profile before integrating the output into your application.

### Generated-File Convention

Files under **Generated/** are managed by SimGe and overwritten during regeneration. They carry an **auto-generated** marker recognized by development tools. The federate class, Simulation Manager, and entity scaffolds remain hand-editable and analyzable, providing clear places for your application logic.

See [Code Generator](UserManual/CodeGenerator.md) for compatible Fora dependencies, regeneration guidance, and the scope of optional generation strategies.

## Report Generator (RG)

- Produce a printable report of OMT content for design reviews and archiving.
- Review, print, and export the report through the built-in report viewer.
- Read module analysis reports with metrics, integrity findings, datatype impact, and advisory generation hints.
- Refresh reports after model edits; large report datasets are prepared in the background.

See [Model Metrics & Reports](UserManual/MetricsReports.md).

## Project Explorer

Keep your simulation project within reach. The **Project Explorer** brings Object Models, federate applications, and generated outputs into one searchable hierarchy, helping you move from model design to configuration and code without losing context.

### Object Models

Inspect, navigate, and manage **Federation Object Models (FOM)**, **Simulation Object Models (SOM)**, and their structural elements in a single tree. Open the relevant editor directly, manage module dependencies, and browse the read-only MOM system library.

### Federate Applications

Access the applications participating in the federation and work with their configuration in context. Use item-specific commands to manage applications, open related views, and generate code for one federate or the whole federation.

### Generated Code & Artifacts

Browse source files and related artifacts under their owning federate application. Generation runs are grouped by time with the newest first, making it easy to find the latest output or revisit an earlier run. Search the hierarchy, inspect generated code, or open its folder directly.

Context-aware menus and live tree updates keep everyday work consistent. Copy, paste, or drag supported model elements with their datatype dependencies, inspect **Show Impact / Usages**, and recover missing module files from the same workspace.

See [Project Explorer](UserManual/ProjectExplorer.md) and [Workspaces](UserManual/Workspaces.md).

## Telemetry Inspector Visualizer

Understand what happened during a simulation run without leaving SimGe. Available under **Tools → Experimental**, the **Telemetry Inspector Visualizer** opens a run's **manifest.json** and associated **.fort** telemetry logs in an integrated analysis workspace. Once you have the captured files, inspection requires no external tools.

### Performance and Workload Views

Explore native charts and grids for **JIT warm-up**, **steady-state latency**, **event sub-phase timing**, and **attribute/parameter selectivity**. The concentric **Operational Drift** radar compares the design-time model profile with the captured runtime workload, helping you identify classes that deserve closer investigation.

### FOM-Match Verification

Know which model a run represents. SimGe compares the recorded FOM's **SHA-256 checksum** with the active merged FOM and displays **FOM Matched**, **FOM Mismatch**, or **Standalone — no project**.

Standalone inspection remains available when no project is open or the models differ. Analysis then uses the manifest's captured design snapshot, so you can investigate a run independently of your current project.

### Scenario and Reports

- **Scenario:** See a plain-language summary of the scenario, run time and duration, federate roster, and object/interaction class inventory. Optional **scenario.md** or **README.md** notes appear alongside the run context.
- **Reports:** Discover accompanying Markdown reports automatically and read them with formatted headings and tables. Switch between **Rendered / Raw**, open a report in its default application, or jump to its folder.

### Export and Share

Save charts as **high-resolution PNG** for reports and presentations. Copy analysis grids as **GitHub-flavored Markdown** with one click to share findings in documentation and review discussions.

See [Telemetry Visualizer](UserManual/TelemetryVisualizer.md). Producing runs with the command-line validation harness requires the source-based research toolchain described in [Fora Telemetry & Validation](UserManual/ForaTelemetry.md). RPR/NETN corpora and their reports are not included in the installer.

## Getting started

Read [Installation & Updates](UserManual/Installation.md) for current requirements and [Quick Start](UserManual/QuickStart.md) for the first-project workflow. SimGe's research and educational usage terms are stated in the [Disclaimer](Disclaimer.md).

---
Updated September 14, 2026
