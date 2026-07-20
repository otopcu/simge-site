### What's New

SimGe 0.5.1 is now available.

- See the effect of a data-type change before you make it: the new **Data-Type Impact** analysis reports which attributes, parameters, record fields, classes, and dependent modules a type change would break or force to be regenerated, and ranks each type by how far its change would reach.
- Get warned in context: deleting or renaming a referenced data type now shows the affected elements first, so you can proceed or step back with full knowledge.
- Inspect any type on demand with the Project Explorer **Show Impact / Usages** action.
- Generate reports for large models without the application freezing — report generation now runs off the UI thread with a progress indicator.
- Cleaner code generation: the no-op fallback codec is emitted only when a model actually needs it, and a member with no declared data type is now reported as a diagnostic instead of being absorbed silently.
- Research & telemetry: the OM4 cross-model / cross-host validation campaign harness adds a corpus driver, confirmatory cross-model statistics, and a validation matrix over the NETN telemetry samples.
- Read updated public documentation covering the release notes, the code-generator and metric-analysis architecture, and the Fora API profile/contract that generated code targets.

---
Updated July 6, 2026, 01:55:00
