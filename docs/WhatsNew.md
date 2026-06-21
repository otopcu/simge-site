### What's New

SimGe 0.4.8 - June 22, 2026

This release ([Release Notes](https://otopcu.github.io/simge-site/ReleaseNotes/)) strengthens SimGe's modular FOM handling, NETN/RPR analysis, Fora code generation, telemetry validation, and published documentation. It preserves dependency-defined data type references across standalone import/export, restores complex NETN-CBRN generated workloads, expands the dashboard and report surface for NETN metrics, and updates the telemetry sample wiki with Chat, Restaurant, and NETN-CBRN validation run summaries.

- Lossless Modular FOM References (dependency-defined data types survive import, save, composition, and export instead of degrading to missing or `NA` references)

- NETN-CBRN Fora Generation Restored (complex arrays, records, inherited SOM members, reserved names, and modular type rebinding now compile and run through the telemetry sample)

- NETN Metrics and Dashboard Refresh (updated module analysis reports, direct OC/IC metric pages, and dashboard behavior aligned with NETN-style dependency modules)

- Telemetry Validation Improvements (instrumentation benchmark verdicts, clock-alignment residual diagnostics, Restaurant workload scaling, and NETN-CBRN Release & Exposure documentation)

- Object Model Editing Safety (case-sensitive OMT name handling, atomic re-parenting, child detach cleanup, note deep-copying, and synchronization-point merge compliance)

- Workspace and Documentation Updates (expanded user manual, docs-site links from Help/About, safer async command error surfacing, and new telemetry sample run pages)
