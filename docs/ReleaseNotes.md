# SimGe Release Notes

## [0.4.7] - Unreleased

### Object Model
- **Dependency Persistence Fix**: Saving a project no longer turns valid modular FOM dependencies into unresolved links in the Start Page dependency graph. Save, import, and load dependency matching is now aligned for RPR and NETN naming variants, including short names, versioned file names, and long SISO model-identification names.
- **Bulk Remove All Modules Refresh**: Project Explorer `Remove All Modules` now uses a batch-clear path instead of repeated single-module dependency teardown. This removes unnecessary dependency/orphan processing, reduces UI freeze risk on large module sets, and reports progress through the shared shell busy/status surface.

### Workspace
- **Installer Auto-Harvest from Publish Output**: `SimGe.Setup` no longer hand-lists ~44 dependency DLLs sourced from the shared `../Bin` build folder (which risked stale/missing files and Debug/Release leftovers, and required manually adding every new NuGet dependency). The wixproj now runs `dotnet publish` (framework-dependent) of `SimGe.UI` into a `publish/` folder, and `Product.wxs` auto-harvests that folder with the WiX v6 `<Files>` element (`ComponentGroup AppPublishedFiles`). The publish output is the single source of truth — the exact, deps.json-validated dependency closure — so adding/removing packages now flows into the MSI automatically. `SimGe.exe` stays an explicit component (Start-menu shortcut + `.fap` association); installer resources and sample projects are unchanged. The harvest even picks up files the manual list missed (e.g. localization satellite assemblies). Debug symbols (`*.pdb`) and library XML doc files are excluded from the MSI. MSI output location is unchanged (`SimGe.Setup/bin/<Config>/SimGe.msi`).
- **Code Comment Localization (English)**: Began translating remaining Turkish source-code comments to English per the project's English-only comment guideline, starting with `SimGe.Application/Project/Project.cs` and `SimGe.UI/WPF/omde/OmeVM.cs` (comments only; no behavioral change).
- **Main Status Bar Refresh**: The main window status bar was reorganized into clearer shell zones for primary feedback, persistent save state, busy activity, and workspace/project meta information.
- **Save Feedback**: Successful project saves now show a transient `<ProjectName> saved.` message, while persistent shell state indicates `Unsaved changes`, `All changes saved`, `Read-only sample`, or `No project` as appropriate.
- **Save-State Reliability**: Project load now starts from a clean shell state, and subsequent OME and FAME model edits consistently return the shell to `Unsaved changes` until the next successful save.
- **Workspace Hints**: Active workspace guidance is now owned by each workspace through a shared shell hint model, improving consistency across Start Page, OME, FAME, Code Generator, Reports, and Preferences.
- **Unified Explorer Launcher**: Replaced duplicated explorer process launching logic with validated static helpers. It guarantees that only absolute local filesystem paths are allowed, checks file/directory existence with warning logs, and provides parent folder view fallbacks if target is missing.
- **Robust Preferences Persistence**: Solves silent preferences reset on JSON corruption. The persistence service now backs up corrupted settings files as `.corrupt-{timestamp}`, alerts the user, and employs atomic write operations (temp file write + replacement move) to prevent file write failures.
- **Resource Key Typo Fix**: Renamed the misspelled `DeafultOptionsFile` string-resource key to `DefaultOptionsFile` across `Strings.resx`, the generated `Strings.Designer.cs` property, and its single consumer (`Options.cs`). The key surfaces as a public member of `SimGe.Properties.Strings`; with no external consumers it was corrected in place rather than aliased.

### OME (Object Model Environment)
- **Dirty/Save Tracking Alignment**: OME `TableView` direct-binding edits and editor commit operations now share the same change-tracking path, so module dirty state and project save-required state stay aligned across both editing surfaces.
- **OC/IC Property Editor Guardrails**: Nested `Attribute` and `Parameter` editors opened from `Object Class` / `Interaction Class` editors now preserve outer `OK / Cancel` semantics by showing the current parent without allowing reparenting from that embedded context.
- **Property Editor Resolution Fixes**: `Attribute` and `Parameter` editors now resolve parent-class and datatype selections correctly when opened from both table views and nested class editors.
- **Toolbar Add-State Fixes**: The OME toolbar `Add` button now follows active-table capabilities more accurately and is visually disabled in unsupported tables including `Time Representations`, `Tags`, `Switches`, `Services`, and non-POC `Identification` views.
- **Tag/Time NA Selection Fixes**: `Tag` and `Time Representation` editors now correctly resolve `NA` datatype values on open instead of showing an empty datatype selection.
- **Tag Validation Fix**: Tag editors now accept reference data types consistently with IEEE 1516-2025 tag rules and with the editor lookup list.
- **Culture-Invariant Update Rate Handling**: Normalized all UI editing, XML import (FDD 2010), and copy-table clipboard export paths for the update rate to use invariant culture parsing and formatting. This ensures consistent decimal value handling (always using a dot as a decimal separator) regardless of regional Windows settings, such as Turkish locale.


### FAME (Federation Architecture Modeling Environment)
- **Project Save-State Tracking**: Federation, federate application, and federate property edits now consistently mark the project as requiring save, matching the shell save-state feedback used elsewhere in the workspace system.
- **Diagram Render Optimization**: Replaced dynamic Expression compilation in `AddEditableText` with a direct `(object source, string propertyName)` signature and a thread-safe property read-only status cache, eliminating high runtime IL compilation overhead and GC pressure during canvas redraws.
- **Soft Renaming in Diagram and Properties**: Changed the inline editing of the Federate Application name in both the diagram view canvas and the properties pane tab to a soft renaming flow. Double-clicking the name or clicking the new edit button triggers the validation-guarded `RenameAppInteractive` dialog, protecting the model from invalid class identifiers (e.g. names with hyphens).
- **Uniqueness Check on Federate Application Renaming and Creation**: Fixed a bug where creating a new Federate Application generated a duplicate name (e.g. `NewFdApp_0` if it already existed in the project). The creation action now dynamically computes a unique name (such as `NewFdApp_1`), and the `RenameAppInteractive` service now queries the active project repository to block renaming an application to a name already in use.

### Reporting
- **Report Dataset Security Hardening**: Secured the legacy typed dataset `OmtDataSet.Designer.cs` against potential XML External Entity (XXE) and obsolete binary serialization security risks. The 14 legacy `SerializationInfo` constructors now immediately throw a `PlatformNotSupportedException` at runtime to block binary deserialization attempts, and the associated legacy `SYSLIB0051` compiler warnings are suppressed. Additionally, insecure `XmlTextReader` instances used to read schema metadata have been replaced with securely configured `XmlReader` instances via `XmlReader.Create` that explicitly prohibit DTD parsing and disable external XML resolvers.

### Code Generator
- **Misleading ProjectName Refactor**: Disentangled misleading uses of `ProjectName` inside `CodeGeneratorSettings` when referencing the active federate class being generated. Introduced a separate `FdAppName` property that dynamically binds to the federate application name, while keeping the `ProjectName` settings property representing the actual project name.
- **Secure Code Generator File Output Containment**: Centralized all file-writing tasks in the code generators to use `GetValidatedPath(...)`, which validates and normalizes target file paths against the base directory to block any path traversal attempts.
- **Atomic Code-File Writes**: The shared generator `Save(...)` now writes each file atomically — to a sibling temp file flushed to disk, then replaced via `File.Move(overwrite)` — so a crash, full disk, or AV lock can no longer leave a half-written or empty generated file. The temp file is cleaned up on failure.
- **Project Name Validation & Save Guards**: Implemented design-time validation on `ProjectSettings.ProjectName` using `Path.GetInvalidFileNameChars` to prevent naming folders and files with invalid path characters. Implemented save commands check that abort saving if validation errors are found on the project settings or code generator settings.
- **Interactive Namespace Correction Dialog**: Embedded an interactive validation guard when initiating code generation. If the current namespace is invalid, the system displays a warning and automatically cleans it to suggest a valid C# identifier alternative, prompting the user for approval to apply and proceed.
- **Fora Telemetry Settings & Instrumentation**: Added a new `EnableForaTelemetry` project setting to toggle Fora telemetry instrumentation code generation. Reorganized the general code generator settings view (`CodeGen_GeneralView.xaml`) into distinct sub-sections: "Writer Settings", "Metric-Driven Optimization Settings", and "Fora Telemetry Settings". When enabled, interaction/object encoders, decoders, and specialized codecs wrap their serialization/deserialization logic in ambient `EncodingTelemetryScope` blocks.
- **OC Sub-Phase Validation Surface**: The validation pipeline now separates the object path into `ENC_OC.materialization`, `ENC_OC.serialization`, `DEC_OC.decode`, and `DEC_OC.entity_apply`. Generated federate helpers use encoded send/update delegates so object-side telemetry stays inside the active event scope, and validation reports can interpret `WHL_OC`, `CV_p`, and `SSI_n` against a more precise runtime breakdown.
- **Per-Class Design Metadata Emission** (`CFomMetricsMetadataGenerator_Fora`): Extended to emit `OcClassDesignMetadata` and `IcClassDesignMetadata` record types and static `OcDesign` / `IcDesign` dictionaries into the generated `FomMetricsMetadata` class. Each entry carries `ClassName`, `DeclaredCount`, semantic weight `W_i`, and volatility flags `IsPeriodic` / `IsConditional` / `IsStatic`. Only classes with at least one declared attribute or parameter are emitted; entries are sorted alphabetically.
- **Code Generation Diagnostics & Report**: Code generation no longer collapses to a bare success/fail boolean. A lightweight, structured diagnostics layer (`CodeGenDiagnostic` with severity/code/generator/class/file/exception, collected per run) isolates each generator step so one failing file no longer silently aborts the rest or returns `null` into the file list. At the end of a run, a validation-style report dialog (the same `CValidationResultsVM` surface used by FOM composition) presents the outcome as one plain-text block per federate application (`Federate Application: …` / `Status: SUCCESS | WARNINGS | FAILED | SKIPPED`, with that federate's file count, a detailed list of generated file names, and diagnostics) under a run-level header (output path, timestamp, and a one-line summary), and the dialog is colored Success / Warning / Error accordingly. The previous separate warning message box shown before the report was removed as redundant, and the dialog titles were shortened to concise visual status labels (e.g., `Code Generation Success`, `Code Generation Warnings`, `Code Generation Errors`).
- **Generated-File Convention & `<auto-generated>` Marker**: Standardized the machine/scaffold boundary. Every machine-layer file (everything under `Generated/`, overwritten each run) now emits a Roslyn-recognized `// <auto-generated>` banner as its first lines, so analyzers/StyleCop/IDEs treat it as generated regardless of extension; user scaffold files (`SimulationManager.cs`, the federate class, `Entities/*.cs`) are intentionally excluded so they remain analyzable, hand-editable code. Fixed the lone extension anomaly: `FomMetricsMetadata` had no scaffold pair yet used `.simge.g.cs`, so it now uses `.simge.cs` like the other standalone generated artifacts (`.simge.g.cs` is reserved for scaffold-paired partials such as entities). The convention is now documented explicitly in `Architecture/11_Code_Generator.md` (§11.4a).

### Telemetry Visualizer
- **Telemetry Visualizer Workspace**: Integrated a global, tab-based Telemetry Inspector Visualizer in SimGe Editor. It enables users to browse, load, and inspect simulation runs (`manifest.json` and `.fort` logs) directly. High-fidelity native WPF canvas charts visualize JIT compiler warmup curves, steady-state latency histograms, event sub-phase breakdowns, attribute/parameter selectivity grids decoded using FNV-1a search combinations, and a concentric radar chart showing design-to-runtime workload shifts (*Operational Drift*).
- **Export & Clipboard Copies**: Added "Save as PNG..." options for all canvas charts (Operational Drift radar, JIT warmup step-line chart, Latency frequency histogram, and Stacked Sub-phase breakdown) to easily export visualizations, and "Copy Table (MD)" clipboard copy actions for the hotspot burden grid and the attribute selectivity grid to easily copy tables in GitHub-flavored Markdown.
- **Real FOM Match Verification**: The verification badge now reflects an actual checksum comparison instead of always reporting "FOM Matched" whenever any project was open. The loaded manifest's `resolved_fom_sha256` is compared against the canonical SHA-256 of the active merged FOM (shared algorithm extracted to `FomChecksumService`, the same one used by `CFomMetricsMetadataGenerator_Fora`), and the badge shows **FOM Matched**, **FOM Mismatch**, or **Standalone — no project**. Standalone inspection is unchanged: a mismatch or absent project never disables analysis — it only suppresses live project-correlated design metrics in favor of the manifest's own `metric_snapshot`.
- **Run Provenance in Metadata Card**: The summary card now shows when the experiment was executed (**Run Time**, from `start_time_utc`, localized), how long it took (**Duration**, `end_time_utc − start_time_utc`), and the number of **Federate Streams** (`.fort` logs) captured. The card notes that each manifest is a single replication — pooling across independent runs remains a `ValidationHarness` (Fora Ch.5 §2.4) responsibility, not something inferred from one manifest.
- **"Scenario & Reports" Tab**: A new first tab answers "what am I looking at?" before any chart. The left panel auto-derives a scenario picture from the manifest — a plain-language summary (scenario name/tier, run time/duration, stream and class counts), the **federate roster** (from `.fort` file names), and the **object/interaction class inventory** (from the manifest handle maps). An optional `scenario.md` / `README.md` sidecar placed next to the manifest is shown as free-text scenario notes when present. The right panel auto-discovers the generated Markdown reports sitting next to the manifest (`ValidationReport.md` and any `*.md` siblings), selectable from a dropdown and shown in a read-only viewer (see *Rendered Markdown Reports* below). **Open** launches a report in the default application and **Folder** reveals it in Explorer.
- **Separate Scenario / Reports Tabs**: The combined panel was split into dedicated full-width **Scenario** and **Reports** tabs.
- **Rendered Markdown Reports**: Generated reports now render as formatted Markdown (MdXaml, GitHub-like style with real tables/headings) with a **Rendered / Raw** toggle (Raw = AvalonEdit with highlighting and search). The optional scenario sidecar renders the same way inside a collapsible panel. "Copy" still yields raw Markdown.
- **Trustworthy Run Metadata**: Scenario and variant are now derived from the run's folder layout (e.g. `Restaurant` / `Base` / `VariantA`) rather than the manifest's federation-name placeholder (`rti`); the metadata card is collapsible; the pooled-replication count is read from the sibling `ValidationReport.md`; and instrumentation overhead is surfaced from `TelemetryReliabilityReport.md`, showing **"not measured"** when the overhead experiment (`--measure-overhead`) was not run.
- **Modern Toolbar UI & Discoverability**: Action buttons (Copy / Open / Folder / Save) were restyled as grouped flat toolbar buttons with icons and tooltips. The visualizer moved under **Tools → Experimental**, and the landing screen now explains its purpose and the prerequisite of generating run artifacts via `SimGe.ValidationHarness` first.

### Fora Telemetry Validation

#### Stage 1 — Identity and Projection (Complete, Fora v1.1)
- **ValidationHarness — Handle Map Consumption**: `ValidationHarness` reads `class_handle_map` and `interaction_handle_map` from `manifest.json` (v1.1), resolves `.fort` `ClassHandle` integers to semantic class names, and builds per-class `RuntimeClassStat` aggregations.
- **Per-Class Breakdown Tables**: `ValidationReport` now includes OC and IC breakdown tables with per-class event count, total payload, average payload, average duration, and role label (Predicted Hotspot / Runtime Dominant).
- **Hotspot Alignment**: OC hotspot uses average payload comparison; IC hotspot uses event count comparison against the top runtime class. Fixed an earlier bug where IC was incorrectly using average payload, causing `CastVote` to be marked `Contradicted` despite being the runtime frequency leader.

#### Stage 2a — Selectivity Analysis (Complete)
- **Update Selectivity Section**: `ValidationReport` section 3 now includes a per-class selectivity table: `FOM Declared Attrs`, `Avg Sent`, `Selectivity %`, `Design Volatility`, and `Verdict`. Selectivity = `AvgEncAttributeCount / DeclaredCount` from `ENC_OC` / `ENC_IC` records; no `.fort` schema change required.
- **Volatility Verdict Logic**: `Periodic` classes at ≥ 85% selectivity are Consistent; `Conditional` classes at < 70% are Consistent; mismatches are flagged Inconsistent. Static classes always emit "Static (sends on change only)".
- **λ_uv Row in Validation Matrix**: Section 4 now includes a `λ_uv (selectivity)` row summarizing how many classes match or contradict their design volatility selectivity pattern.
- **`DeclaredCount` Fix**: Selectivity computation now uses `DeclaredCount` (own-class declared attributes only) rather than `AllPropertyCount` (which included inherited HLA root attributes), preventing false low-selectivity readings.

#### Stage 2b — Handle Attribution (Complete, Fora v1.2)
- **Design Metadata in Generated Binary**: `CFomMetricsMetadataGenerator_Fora` emits `OcDesign` and `IcDesign` static dictionaries so the running federate binary carries design-time `W_i`, volatility flags, and declared counts without a separate FOM file at validation time.
- **`metric_snapshot` Injection Path**: `TelemetryConfiguration.MetricSnapshot` (new Fora field) can now carry domain-level baseline metrics from SimGe's `FomMetricsMetadata` into `manifest.json fom.metric_snapshot` at session start. The manifest v1.2 contract defines this as a SimGe-origin, Fora-carried field.

#### Stage 3 — Event Attribution (Complete, Fora v1.3)
- **`PropertySetId` in `.fort` records**: `ValidationHarness` reads `ExtensionField1` from `ENC_OC` / `ENC_IC` records (written by Fora v1.3 as a FNV-1a hash of the sorted attribute/parameter handle set). Per-class `PropertySets` distribution is tracked in `RuntimeClassStat`.
- **"Property Set Distribution" section in `ValidationReport`**: Section 3 now includes per-class distinct property-set count, dominant set ID (hex), dominant set event frequency, and dominant set average payload. OC pattern classification (`Full-update` / `Partial-update`) and IC classification (`Full-send` / `Partial-send`) are shown.
- **`UpdateTypeHint` tracking**: `RuntimeClassStat` accumulates `UpdateTypeHint` breakdown counters (`Periodic`, `Conditional`, `OnChange`, `Unspecified`) from `ExtensionField2`. A distribution table is included in the Property Set Distribution section.
- **Manifest attribute/parameter map consumption**: `ManifestHandleMaps` and `TelemetryManifestFom` now load `attribute_handle_map` and `parameter_handle_map` from manifest v1.2/v1.3 for future reverse-map use.
- **`PropertySetId` Client/Ambassador-Side Hashing & Validation**: Implemented client-side calculation of the FNV-1a hash of sorted attribute/parameter handles, passing it as `ExtensionField1` (`PropertySetId`) for all `ENC_OC`, `DEC_OC`, `ENC_IC`, and `DEC_IC` events. Verified name resolution matches correctly in generated validation reports.
- **`InheritanceDepth` Propagation**: Updated `TelemetryToken` and `TelemetryEmitter` on the client/ambassador side to pass and log the class/interaction hierarchy depth from `BeginEvent` to `EndEvent` instead of logging `0` by default.

#### Stage 3b — UpdateTypeHint at Generated Send Sites (Complete)
- **`UpdateTypeHint` parameter added to public `IForaClient` / `ForaClient` API**: `UpdateAttributeValuesAsync`, `SendInteractionAsync`, and `SendDirectedInteractionAsync` now accept optional `byte updateTypeHint = 0`. `InstrumentedForaClient` updated to forward the hint and pass it to `EndEvent` as `extensionField2`.
- **`ClassCodeGen_SimMngr_Fora` emits hint per class**: Added `GetUpdateTypeHintLiteral` helper that maps `ClassGenStrategy.IsPeriodic → Periodic`, `IsConditional → Conditional`, `IsStatic → OnChange`, default → `Unspecified`. All three send-site generators (`generateObjectHelpers`, `generateInteractionHelpers`, directed interaction loop) now emit `updateTypeHint: (byte)UpdateTypeHint.{hint}` at each `UpdateAttributeValuesAsync` / `SendInteractionAsync` / `SendDirectedInteractionAsync` call.
- **`using Fora.Telemetry;` added to generated `SimulationManagerBase`**: The generated file imports `Fora.Telemetry` so `UpdateTypeHint` is resolvable without fully-qualified names.
- **`UpdateTypeHint Distribution` table now populated**: Chat-Headless sample shows `User=OnChange`, `Poll=Conditional`, `ChatGroup=Conditional`, all ICs=`Unspecified` (no IC volatility analysis yet).

#### Stage 4 — ScenarioStepId / Workload Phase Annotation (Complete)

- **`SetScenarioStep(byte)` API on `IForaClient` / `ForaClient` / `InstrumentedForaClient`**: Callers set the active workload phase; stored as `volatile byte _scenarioStep` in `TelemetryEmitter` and written into `ExtensionField3` of every subsequent ENC event. `NullTelemetryEmitter` no-ops. Default interface method on `ITelemetryEmitter` maintains backward compatibility.
- **Generated `SimulationManagerBase` emits three phase markers**: `SetScenarioStep(1)` immediately after `ConnectAsync`/`CreateFederationExecutionAsync` (Setup), `SetScenarioStep(2)` before the main simulation loop (Steady-State), `SetScenarioStep(3)` at the start of `CleanupAsync` (Teardown).
- **`telemetry_version` bumped to `1.4`** in `ManifestWriter`.
- **`ValidationHarness` consumes `ExtensionField3`**: `AccumulateClassStat` tracks `Dictionary<byte, StepStat>` per class; `AppendWorkloadPhaseBreakdown` emits a per-class, per-phase event/payload table in `ValidationReport.md`.
- **Fix: `SetScenarioStep(1)` timing corrected** — moved from before `ConnectAsync` (where it hits `NullTelemetryEmitter`) to after the connect/create-federation block so the real emitter is live.

#### Stage 5 — ValidationReport v2 Evidence Format (Complete)

- **`ValidationReport.md` upgraded to a v2 evidence structure**: `SimGe.ValidationHarness` now emits a paper-oriented report layout with `Validation Scope`, `Design Baseline Contract`, `Runtime Observation Contract`, `Metric-to-Correlate Validation Matrix`, `Hotspot Analysis`, `Latency Decomposition Evidence`, `Instrument Credibility and Confound Control`, and `Scope, Limits, and Non-Claims`.
- **Telemetry sample documentation split into shared and scenario guides**: Chapter 16 now points to `Architecture/16A_Telemetry_Samples_and_Harness_Usage.md` for shared harness usage and to `Architecture/16B_Telemetry_Sample_Scenarios.md` for concrete `Chat-Headless` and `Restaurant` scenario definitions, keeping the integration chapter architecture-focused.
- **Manifest and sampler metadata surfaced in the report**: `ValidationHarness` now reads run-level manifest metadata (`experiment_id`, scenario, runtime, Fora version, telemetry version, hardware envelope, overhead fields) and reports `MEMORY` / `RTI_STATE` telemetry samples as confound-control evidence.
- **Direct `WHL_IC` dispatch correlate enabled**: Fora client-side `DEC_IC` telemetry now exports `dispatch_resolution_ns` through the `DispatchNotification` sub-phase hook. Canonical `Chat-Headless` and `Restaurant` validation reports therefore promote `WHL_IC` from indirect evidence to `Direct / Supported`.
- **Restaurant report parse scope fixed**: single-run validation now parses only top-level `.fort` files for the active output directory, so root `Restaurant` reports no longer accidentally absorb `VariantA`, `VariantB`, or other nested experiment artifacts.
- **Class-level uncertainty surfaced explicitly**: runtime class tables now emit `Avg Payload ± σ` and `Avg Duration ± σ`; selectivity verdicts now fall back to `Unverifiable (n=...)` when the active steady-state ENC sample count is below the minimum verdict threshold.
- **EventId-correlated decomposition enabled**: validation reports now consume sender/RTI/receiver correlation chains and emit chain-aware latency decomposition rather than only global event averages. Canonical `Chat-Headless` and `Restaurant` reports now show non-zero correlated object and interaction path coverage.
- **Phase-aware correlated reading**: `Section 7` now prefers steady-state correlated chains over all-phase aggregates and reports both steady-state and all-phase EventId coverage, reducing warm-up contamination in latency interpretation.
- **RTI object-path split surfaced in reports**: validation reports now expose `SUB_MATCH.lookup`, `SUB_MATCH.region/finalize`, `DIST_OC.prep`, `DIST_OC.materialization`, and `DIST_OC.network dispatch` as first-class object-path rows. In the current in-process reference RTI, the first two `DIST_OC` components usually remain near the timer floor while network dispatch dominates the observable server-side object cost.
- **Readable property-set evidence**: `PropertySetId` analysis no longer stops at hash output; validation reports now resolve observed attribute/parameter subsets into human-readable member lists for selectivity and operational-drift reading.
- **Logical cycle / iteration coverage**: SimGe samples now annotate repeated semantic loops via `SetScenarioIteration(int)` and validation reports expose decoded `LogicalCycleId` coverage under `Section 6.2A`, making per-round and per-customer runtime reading explicit.
- **`W_i ↔ runtime` correlation surface added**: Section 4 now includes a raw class-by-class table showing design semantic weight `W_i` alongside `Avg ENC`, payload, instance count, and `n`, plus a directional `Spearman ρ` summary for pilot-scale design/runtime correlation reading.
- **Warm-up contamination visibility increased**: Section 3 now reports `ENC_OC (steady-state)` as a first-class event-surface row, and findings emit an automatic `ENC_OC Warm-Up Contamination` drift when setup/unclassified ENC_OC traffic dominates the run.
- **Run-level validation integrity diagnostics added**: findings now emit `FOM SHA Mismatch` (`CRITICAL`) when design-time and runtime FOM hashes diverge, `High Duration Variance Indicates Warm-Up / Phase Mixing` when class duration CV exceeds `1.0`, and `Low Statistical Coverage` when verdict-bearing steady-state ENC coverage remains below `MinSampleForVerdict`.
- **Natural Convergence of FOM SHA-256 Checksums**: Resolved the dynamic timestamp comment mismatch (`<!-- Generated by SimGe at ... -->` added to XML files) between design-time and build-time generation. Both `CFomMetricsMetadataGenerator_Fora` and `ValidationHarness` now strip XML comment nodes before calculating the SHA-256 hash. This enables the design-time, runtime, and manifest FOM checksums to converge naturally on the exact same signature value, eliminating the `FOM SHA Mismatch` warning.
- **Validation and telemetry reliability reports split**: `ValidationReport.md` now focuses on design-metric runtime validation, while `TelemetryReliabilityReport.md` is generated only when `--measure-overhead` is used and carries repeated `OFF/ON` overhead evidence plus instrumentation credibility findings.
- **Harness made sample-extensible and artifact-safe**: `ValidationHarness` now routes sample-specific paths, names, and scenario summary text through a `SampleDefinition` descriptor selected with `--sample <key>`, preserving a generic orchestration/report pipeline for future samples. Normal validation runs also preserve and re-emit linked design artifacts (`*-Module-Analysis-Report.md`, `*-Semantic-OC.md`, `*-Semantic-IC.md`, `*-Topology.md`, `*-Archetype.md`) instead of deleting top-level markdown files during telemetry output cleanup.
- **`--repetitions` defaults to 5**: The pooled-replication default now matches the documented spec floor (Fora Ch.5 §2.4) instead of 1, so default runs (including the validation run emitted after `--measure-overhead`) no longer raise spurious low-coverage Drift 005 / Drift 007 warnings. Pass `--repetitions 1` for a fast single-pass smoke run.

### Maintenance
- **One-Way Converter Hardening**: `ConvertBack` in 22 value/multi-value converters no longer throws `NotImplementedException`/`NotSupportedException`. These converters are display-only (one-way); throwing meant that if a binding's mode ever became two-way, the binding engine would raise a runtime exception. They now return `Binding.DoNothing` (single-value) or `null` (multi-value) so the source is simply not updated. (Adding converter unit tests remains a recommended follow-up.)
- **Converter Filename Fix**: Renamed `BooleanToVisibilityConverter .cs` (which had a trailing space before the extension) to `BooleanToVisibilityConverter.cs`. The stray space produced a fragile path for repo tooling, scripts, packaging and cross-platform checks; the class name and namespace were unaffected and the project uses globbing, so there is no reference impact.
- **Legacy Dead-Code Cleanup**: Removed legacy UI files that were excluded from compilation but still lingered in the repository (`NewOmtItemVM`/`NewOmtItemView`, `NewDimensionVM`, the old `omde/TableView` `NotesView`/`TagsView`, and `LegacyDiagramDefinition`), and purged the now-stale `<Compile Remove>` / `<Page Remove>` entries (including several that already pointed at deleted files such as `OmExplorerVM`, `CodeExplorerVM`, `FamExplorerVM`, and the legacy OMD geometry pipeline). No behavioral change — these types were not compiled into the assembly and had no live references.
- **WPF Temp Files and gitignore Hardening**: Cleaned up legacy WPF temporary project files (`*_wpftmp.csproj`) from the working directory and updated `.gitignore` rules to recursively ignore all temporary compilation files and build artifacts across the entire repository.
- **CI/CD & Release Docs**: Expanded the architecture **CI/CD** chapter (`Architecture/13_CI_CD.md`) to document the build topology and the installer publish→harvest→MSI pipeline (commands, output locations, versioning source, and how dependency changes flow into the MSI). Release Management moved into the architecture set as chapter 17 (`Architecture/17_ReleaseManagement.md`), with its links and the wiki index updated accordingly.
- **Dropped Azure Pipelines**: Removed the unused (and stale) `azure-pipelines.yml` and all Azure CI references from the docs; builds and tests are now run locally with the .NET SDK. The repository is local-only (Azure DevOps remote removed).
- **Installer Resource Cleanup**: Deleted stale, unreferenced setup resources that were never packaged into the MSI — the legacy .NET Framework 4.8 `Resources/Runtime/SimGe.exe.config` (plus `Options.xml`, a `SimGe.runtimeconfig.json` snapshot, and an old MIM `.fed`), and the entire 2017-era `Resources/Samples/Datatypes` sample (which was not part of the installer). The shipped config (`SimGe.dll.config`, harvested from the .NET 10 publish output) is empty, so no stale framework config reaches the MSI. Documented an MSI installed-folder smoke test and a "regenerate bundled samples before release" step in the Release Management and CI/CD chapters.
- **Removed Unused Signing Certificate**: Dropped the dangling `simge_sign.pfx` (a password-protected self-signed certificate that was never wired into any signing step and provided no SmartScreen/Authenticode benefit) — removed its `<None Include>` references from the project files and deleted the scattered copies. Assembly strong-naming continues to use `private.snk`. Code signing, if needed for distribution, should use a CA-issued certificate.
## [0.4.6] - 2026-05-20

### Dashboard
- **Module Analysis Scope Selector**: The Module Analysis Dashboard now has a **Composed / Module Only** segmented control in the toolbar. *Composed* merges the dependency chain via `FomCompositionEngine`; *Module Only* analyzes `module.Content` - the FOM tree with inheritance - without running composition.
- **Module Only Inspector**: In Module Only mode the Classes card now shows a declared / inherited breakdown. Declared classes are explicitly defined in the module; inherited classes are FOM-tree scaffolding nodes grouped by parent class.
- Dashboard UX refresh for the Module Analysis Dashboard: cleaner section rail, tighter header/content composition, and compact detail surfaces.
- Object Model Archetype calibration is now live: threshold changes immediately update the gauge, archetype classification, summary text, and dashboard report language.
- SSI gauge tooling was visually aligned with the archetype card through compact info/calibration actions and a refined calibration panel.
- The Overview summary was reformatted to match the generated report summary style, and the Object Model Archetype row now uses a balanced split layout with the summary on the left and the archetype graph on the right.
- Architectural metric ownership was tightened: `R`, `A`, archetype classification, `DME_w`, `FCI_norm`, `TDF`, and structural propensity scores are now computed in `MetricAnalysisService` as the single numeric source of truth.
- Volume metrics were redesigned around a compact card grid plus a dedicated `Metric Inspector`, replacing inline expansion with a stable right-side detail pane.
- Direct volume metrics now use HLA/FOM-oriented explanations in the inspector, including explicit `Direct metric` computation notes instead of misleading "no formula" placeholders.

### Workspace
- **Floating Windows**: Workspace tabs can now be detached into separate floating windows and docked back into the main tab strip.
- **Tab Context Menu**: Added *Close This* to the right-click context menu alongside the existing *Close All* and *Close All But This* entries.

### OME (Object Model Environment)
- OME FDD Viewer validation workflow was upgraded: DIF, FDD, and OMT schemas are now user-selectable with DIF as the default; validation reports now include richer element-name context for schema errors; and HLA 4 tag export no longer serializes `dataType` when the model value is `NA`.
- OME reference data type editing now accepts reserved referenced-attribute identifiers such as `HLAobjectInstanceHandle` and `HLAobjectInstanceName` even when no physical root attribute node exists in the in-memory model.
- OME MOM integration was hardened: MOM-marked elements now merge generically into the active module, and OME table views now stay in sync with Project Explorer when MOM content is toggled on or off.

### Object Model
- OMT 2025 export/validation alignment was improved: unused HLA 4 tags are now suppressed, `basicData/endian` is always emitted, and standards-based exports no longer serialize `HLAobjectInstanceHandle` / `HLAobjectInstanceName` as normal `HLAobjectRoot` attributes.
- SimGe's internal 2025 MIM/runtime policy was clarified: `HLAobjectInstanceHandle` and `HLAobjectInstanceName` are now treated as reserved reference semantics rather than materialized root attributes, with editor and re-export support preserved for `referenceDataType/referencedAttribute`.

### Documentation
- SimGe Wiki architecture and standards notes were updated for the 2025 MIM/root-attribute policy, the IEEE 1516-2025 OMT schema discrepancy around `referenceDataType/representation`, and the expanded metric ownership/archetype-strength model.
- Model metrics documentation was updated to distinguish computed architectural metrics from direct volume counters, clarify `OC/IC Ratio` ownership, and classify `0/0` semantic-mass cases as `Non-Semantic Support` rather than `Hybrid`.

---

## [0.4.5] - 2026-05-11

### Dashboard - Object Model Archetype Card
- **New Visual Card**: The Architecture section now displays a compact, read-only **Object Model Archetype** card alongside the existing metric cards.
- **Terminology Update**: Renamed "Model Character" to **Object Model Archetype** throughout card labels, analysis report sections, summary text, and internal API (`Archetype` / `ArchetypeDescription` on `OmModelStatistics`).
- **Info Panel**: Clicking the info button expands the Semantic Mass Equilibrium formula block (Section 3.4) with copy-to-clipboard support.
- **SSI Tooltip Alignment**: Corrected Semantic Saturation Index dashboard help and gauge behavior to match the current metric model.

### OME (Object Model Environment)
- **Time Representation Semantics Validation**: `Semantics` is no longer required when a `DataType` is selected, matching the IEEE 1516-2025 DIF/FDD schema (`0..1` cardinality).
- **Services Table**: Fully redesigned to restore WPF row virtualization, group services by IEEE 1516-2025 chapter, support bulk `IsUsed` toggles, and avoid unwanted navigation jumps.
- **Copy Table - Markdown**: The *Copy Table* command now outputs GitHub-flavored Markdown for all table tabs. The Services tab additionally wraps output in `## chapter` group headings.

### Deployment
- **Version Sync Fix**: `SimGe.Setup/Product.wxs` version is now driven from `Directory.Build.props` via `DefineConstants`, eliminating manual version maintenance in the WiX project.

### Code Generator
- **Metric-Driven Variants**: Fora code generation now supports metric-driven specialized outputs behind `EnableMetricDrivenVariants`, including specialized object codecs, delta-tracker hooks, and optional generated dispatch tables.
- **Shared Metric Core**: Code generation metrics are now derived from the same shared analysis layer used by dashboard statistics, reducing drift between dashboard metrics and generator decisions.
- **Generator Strategy Fixes**: Corrected strategy gating so delta-tracker code is emitted only when the corresponding tracker is generated, and dispatch-table output remains disabled in legacy generation unless the metric-driven flag is enabled.

---

## [0.4.4] - 2026-05-02

### OME (Object Model Environment)
- **Parameters Table Remove Button Fix**: The Remove button in the Parameters TableView was bound to `DeletePocCommand` instead of `DeleteOmtElement`. Corrected to use the standard element deletion command.
- **IC/OC PS Default Value**: New Interaction Class and Object Class instances now default `PS` to `Neither` instead of `null`, preventing invalid states in editors and code generators.
- **IC/OC P/S TableView Refresh**: Changing Publish/Subscribe state in the IC or OC editor now immediately reflects in the TableView P/S column.

### Code Generator
- **Fora `FomSharing` Generation Fix**: `Models.simge.cs` no longer emits `HlaSharingType.` for classes with no `PS` value set.
- **Automatic FDD Standard Routing**: `CodeGenerate` now exports the generated FDD using the active code generation target standard.
- **Decoder Dispatch Fix**: Fixed generated `Decoder.simge.cs` object-family dispatch methods to avoid duplicate `Decode(...)` signatures and to route decoding with the actual discovered object class handle.
- **Unsigned Primitive Support**: Fora code generation now maps HLA 4 unsigned integer representations to `byte`, `ushort`, `uint`, and `ulong` and emits matching encoder-decoder helpers.

### Object Model
- **FOM/SOM Rename Persistence Fix**: Renaming a FOM/SOM module from Project Explorer now immediately persists the renamed module metadata and refreshes the `.fom` repository index.
- **OME TableView Refresh Fix**: Copy-paste operations from Project Explorer now correctly refresh open OME TableView hierarchies.
- **Datatype-Aware Copy/Paste**: Project Explorer copy-paste now carries required user-defined datatype dependencies with the pasted OMT element.

### FAME (Federation Architecture Modeling Environment)
- **Full UML Deployment Diagram**: The Deployment Diagram was redesigned around explicit RTI infrastructure, network bus, nested environments, and clearer component architecture.
- **Diagram Rendering Fixes**: Fixed blank-render and null-layout issues, improved legend/canvas layout, and reduced text overlap.
- **FSD Note Icon**: The Federation Structure Diagram now shows note-style module markers for federation FOM and federate SOM references.
- **Toolbar Cleanup**: Federate add/remove actions were consolidated into the dedicated Federate Apps tab.
- **Jump to OMT**: Added quick-navigation buttons from FAME module selectors into OME.
- **Rich Tooltips and Diagnostics**: FSD now exposes richer hover information, visual multiplicity cues, connection interpretation, live diagnostics, and an explicit legend.

### UX & Modernization
- **Workspace Tab Context Menu**: Added *Close All* and *Close All But This* operations for closable workspace tabs.
- **MOM Explorer Integration**: Added a read-only MOM system library under Project Explorer.
- **Native Drag-and-Drop**: OMT elements can now be dragged and dropped between modules using the established copy-paste infrastructure.
- **Technical Debt Cleanup**: Refactored internal repository logic and addressed legacy serialization warnings to prepare for advanced code scaffolding.

### Infrastructure
- **SSI Interpretation Logic Update**: Refined `SSI_n` interpretation for Interaction Class domains and standardized the concrete-population gate.
- **WHL & S_top Calibration**: Improved the Weighted Hierarchy Load and Topological Skewness formulas with epsilon-floor normalization to handle skeletal models and flat hierarchies gracefully.

---

## [0.4.3] - 2026-04-20

### Documentation
- **SimGe Wiki Established**: Created the documentation portal in `docs/SimGeWiki`.
- **Merge Rules Documented**: Added detailed IEEE 1516-2025 Annex C merge-rule documentation.
- **Diagrams Documented**: Added User Manual sections for Directed Interactions Diagram and FOM Modules Dependency Graph.

### Code Generator
- **Fora Compatibility**: Compatible with Fora 0.0.1. Generated federate code now targets the IEEE 1516-2025 HLA Federate Protocol via the [Fora](https://github.com/okantopcu/Fora) client library.
- **Async Lifecycle**: Generated `SimulationManager` provides async federation lifecycle (`connect -> create -> join -> init handles -> resign -> dispose`).
- **UTF-8 Tags**: Tag constants are generated as `ReadOnlySpan<byte>` with C# UTF-8 string literals such as `"NA"u8`.
- **Clean Scaffold**: Manual partial federate class is generated without legacy inheritance for a Fora-only scaffold.

### Diagram Editor
- **New Infrastructure**: Completely redesigned diagram infrastructure.
- **Directed Interactions Diagram**: Added support for visualizing directed interactions between classes.

### Project Explorer
- **OMT Element Copy/Paste**: Added support for copying and pasting user-defined OMT elements between FOM/SOM modules.

---

## [0.4.2] - 2026-04-10

### FOM Module Composition
- **Composition Report**: Added a report for analyzing module composition.
- **Enhanced Operations**: Added support for multiple import, renaming, and duplicating FOM modules.

### Dashboard & Analytics
- **UI Overhaul**: Redesigned dashboard featuring architectural complexity metrics and notes.
- **Architecture Profile Radar**: Added a radar chart for visualizing model architecture profiles.
- **Dependency Graph**: Added the FOM Modules Dependency Graph to the Start Page with image export support.

### OME (Object Model Environment)
- **Table Editor**: Added a consistent "Edit Notes" button across OMT editors with a redesigned `NoteSelector` dialog.
- **Diagram Editor**: Diagrams now display base classes and properties from dependent FOM modules.

### Infrastructure
- **Modernized Preferences**: Added a JSON-based preference system with per-user settings such as reopening the last project.
- **Project Format**: Transitioned `.fam` project files to JSON format.

---

## [0.4.1] - 2026-03-01

### OME (Object Model Environment)
- **Modernized Platform**: Comprehensive update to the OMT editing environment using WPF dialog-based editors.
- **Data Type Management**: Added dedicated editors for simple, enumerated, array, fixed record, variant record, and reference data types.
- **POC Management**: Improved point-of-contact management with structured views and better validation.
- **Standards Infrastructure**: Enhanced FDD and MIM handling; improved restoration of model element types after import.

### User Experience
- **Get Started Splash**: Added a centralized dialog for creating, opening, and browsing projects with quick access to recent samples.

---

## [0.4.0] - 2024-02-24

### Architecture & Standards
- **HLA 4 Ready**: Alignment with IEEE 1516-2025 (HLA 4) modeling practices.
- **Modular FOM**: Automatic composition of base and dependent modules.
- **Model Intelligence**: OMT Intelligence Dashboard featuring architectural complexity metrics and health diagnostics.

---
### Disclaimer
See [Disclaimer](Disclaimer.md) for usage terms and research-only environment conditions.
