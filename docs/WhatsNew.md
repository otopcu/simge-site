### What's New

SimGe 0.4.8 - June 22, 2026

This release ([Release Notes](https://otopcu.github.io/simge-site/ReleaseNotes/)) strengthens SimGe's modular FOM handling, NETN/RPR analysis, Fora code generation, telemetry validation, and published documentation. It preserves dependency-defined data type references across standalone import/export, restores complex NETN-CBRN generated workloads, expands the dashboard and report surface for NETN metrics, and updates the telemetry sample wiki with Chat, Restaurant, and NETN-CBRN validation run summaries.

- Lossless Modular FOM References (dependency-defined data types survive import, save, composition, and export instead of degrading to missing or `NA` references)
    
- NETN-CBRN Fora Generation Restored (complex arrays, records, inherited SOM members, reserved names, and modular type rebinding now compile and run through the telemetry sample)
    
- NETN Metrics and Dashboard Refresh (updated module analysis reports, direct OC/IC metric pages, and dashboard behavior aligned with NETN-style dependency modules)
    
- Telemetry Validation Improvements (instrumentation benchmark verdicts, clock-alignment residual diagnostics, Restaurant workload scaling, and NETN-CBRN Release & Exposure documentation)
    
- Object Model Editing Safety (case-sensitive OMT name handling, atomic re-parenting, child detach cleanup, note deep-copying, and synchronization-point merge compliance)
    
- Workspace and Documentation Updates (expanded user manual, docs-site links from Help/About, safer async command error surfacing, and new telemetry sample run pages)

[SimGe 0.4.6](https://drive.google.com/file/d/1hAYcyQob2nTizw5tgrtu4wEq2G82_8Ji/view?usp=drive_link) - May 20, 2026

This release ([Release Notes](https://sites.google.com/view/okantopcu/simge/release-notes)) introduces  a new Composed / Module Only analysis scope in the dashboard for clearer module-level inspection, live Object Model Archetype calibration with a redesigned metric inspector, detachable workspace windows for more flexible multi-view workflows, stronger OME validation and reserved-reference editing support, better OMT 2025 export behavior for HLA 4 alignment, and updated wiki guidance reflecting the refined metric and MIM policy model.

- Module Analysis Scope Selector (Composed / Module Only switching, declared vs inherited class breakdown)
    
- Live Archetype and Metric Inspector Refresh (instant calibration feedback, stable inspector, clearer direct-metric explanations)
    
- Floating Workspace Windows (detach and dock back for OME, FAME, and other tabs)
    
- OME Validation and Reference Editing Improvements (schema selection, richer validation context, reserved referenced-attribute support)
    
- OMT 2025 Export Alignment (unused HLA 4 tag suppression, required endian output, reserved root-attribute handling)
    
- Code generator is compatible w/ [Fora Family](https://sites.google.com/view/okantopcu/fora)
