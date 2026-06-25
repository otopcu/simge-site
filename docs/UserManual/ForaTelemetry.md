# Fora Telemetry & Validation

SimGe can generate **telemetry-instrumented** federate code so a run can be measured and compared against your design-time model. This chapter explains how to turn it on, what gets emitted, and how the results flow back into SimGe. For the generator itself, see [Code Generator](CodeGenerator.md); for inspecting results, see [Telemetry Visualizer](TelemetryVisualizer.md).

## What it is for

Design-time metrics (from the [dashboard](Dashboard.md) and [reports](MetricsReports.md)) describe how a model *should* behave. Telemetry captures how it *actually* behaves at run time — warm-up cost, latency, and where time is spent per event sub-phase. Comparing the two reveals **operational drift** between design and runtime.

## Enabling telemetry generation

Telemetry is a code-generation option, controlled by the **Enable Fora Telemetry** project setting in the Code Generator's **Fora Telemetry Settings** section (see [Code Generator](CodeGenerator.md)).

When enabled, SimGe:

- wraps the generated encoders, decoders, and specialized codecs so their serialization/deserialization runs inside telemetry scopes, and
- emits **design metadata** for the model (per-class design figures such as declared counts and semantic weights) alongside the generated code, so a run can be correlated with the model that produced it.

When disabled, the generated code carries no instrumentation overhead.

## Capturing a run

1. Generate federate code with telemetry enabled.
2. Run the federate(s) under the **SimGe validation harness**, which executes the scenario and records telemetry.
3. The harness produces run artifacts in an output folder:
   - a **`manifest.json`** describing the run (including the FOM checksum used), and
   - one or more **`.fort`** log files (one per federate stream).
   - optional generated reports (e.g. `ValidationReport.md`, `TelemetryReliabilityReport.md`).

> Each manifest is a single replication. Pooling across multiple independent runs is the harness's responsibility — see its documentation.

## Inspecting results

Open the [Telemetry Visualizer](TelemetryVisualizer.md) (**Tools → Experimental**) and load the run's `manifest.json`. The visualizer:

- verifies the run's FOM against the open project (**FOM Matched / Mismatch / Standalone**),
- shows the scenario, federate roster, and class inventory,
- charts warm-up, latency, sub-phase breakdown, and the **Operational Drift** radar,
- surfaces the accompanying Markdown reports.

## Typical workflow

1. Author and validate the FOM in SimGe ([OME](OME.md), [FOM Validation](Validation.md)).
2. Generate federate code with **Enable Fora Telemetry** on.
3. Run under the validation harness to capture telemetry.
4. Load the run in the Telemetry Visualizer and review drift, latency, and hotspots.
5. Adjust the model or generation settings and repeat.

> Deeper specifications of the telemetry/validation integration live in the project's Architecture documentation ("Fora Telemetry and Validation Integration").

---

**Next:** [Preferences & Options](Preferences.md)

---
Updated June 25, 2026, 16:28:09