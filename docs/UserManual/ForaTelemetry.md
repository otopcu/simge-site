# Fora Telemetry & Validation

SimGe can generate **telemetry-instrumented** federate code so a run can be measured and compared against your design-time model. This chapter explains how to turn it on, what gets emitted, how to capture a run, and how to scale from a single run to a cross-model validation campaign. For the generator itself, see [Code Generator](CodeGenerator.md); for inspecting results, see [Telemetry Visualizer](TelemetryVisualizer.md).

## What it is for

Design-time metrics (from the [dashboard](Dashboard.md) and [reports](MetricsReports.md)) describe how a model *should* behave. Telemetry captures how it *actually* behaves at run time — warm-up cost, latency, and where time is spent per event sub-phase. Comparing the two reveals **operational drift** between design and runtime, and lets you ask whether a design-time metric (breadth, semantic weight, archetype balance) actually predicts a runtime cost.

## Enabling telemetry generation

Telemetry is a code-generation option, controlled by the **Enable Fora Telemetry** project setting in the Code Generator's **Fora Telemetry Settings** section (see [Code Generator](CodeGenerator.md)).

When enabled, SimGe:

- wraps the generated encoders, decoders, and specialized codecs so their serialization/deserialization runs inside telemetry scopes, and
- emits **design metadata** for the model (per-class design figures such as declared counts and semantic weights) alongside the generated code, so a run can be correlated with the model that produced it.

When disabled, the generated code carries no instrumentation overhead.

## Capturing a run

Runs are executed by the **SimGe validation harness**, a command-line tool (`SimGe.ValidationHarness`) that generates the code, builds the scenario, launches the Fora RTI and federates, and records telemetry. A minimal single-sample run:

```powershell
dotnet run --project Telemetry/SimGe.ValidationHarness/SimGe.ValidationHarness.csproj `
    -c Telemetry -- --sample netn-mrm --repetitions 5
```

- `--sample <key>` selects a registered scenario. Run `--help` to see the current list.
- `--repetitions N` runs the scenario `N` independent times and **pools** them into one snapshot, smoothing launch-order variance. Only runs sharing the same resolved-FOM checksum are pooled; mismatched runs are excluded.

Each run produces artifacts in its output folder:

- a **`manifest.json`** describing the run (resolved-FOM checksum, hardware identity, scenario seed, clock-alignment status),
- one or more **`.fort`** log files (one per federate stream),
- a **`ValidationReport.md`** (per-metric runtime evidence and verdicts), and
- a **`CampaignSampleSummary.json`** — a machine-readable summary the campaign aggregator consumes.

> The full harness contract, switches, overhead-measurement mode, and per-scenario behaviour are documented in the project's internal architecture documentation.

## Inspecting results

Open the [Telemetry Visualizer](TelemetryVisualizer.md) (**Tools → Experimental**) and load the run's `manifest.json`. The visualizer:

- verifies the run's FOM against the open project (**FOM Matched / Mismatch / Standalone**),
- shows the scenario, federate roster, and class inventory,
- charts warm-up, latency, sub-phase breakdown, and the **Operational Drift** radar,
- surfaces the accompanying Markdown reports.

## Cross-model / cross-host validation campaign

A single run shows how *one* model behaves on *one* machine. To ask whether a design-time metric predicts runtime cost **in general**, the harness has a corpus campaign mode that runs several models and pools the evidence into one confirmatory report.

```powershell
dotnet run --project Telemetry/SimGe.ValidationHarness/SimGe.ValidationHarness.csproj `
    -c Telemetry -- --corpus netn-cbrn,netn-mrm,netn-entity --repetitions 5 `
    --corpus-output "C:/path/to/Corpus"
```

This runs each model through the same pipeline into `<corpus-output>/<key>/`, then emits, at the corpus root:

- **`ConfirmatoryValidationMatrix.md`** — one verdict per registered hypothesis (**Confirmed**, **Weak / Directional**, **Inconclusive**, or **Rejected**), with the pooled effect size and a Benjamini–Hochberg-corrected p-value, and
- **`CorpusSummary.csv`** — one row per cell for import into R, Python, or a spreadsheet.

Two design points worth knowing as a user:

- **Hosts are never blended.** x86-64 and ARM64 runs of the same model are kept as separate cells and only compared, not merged. This is what lets you gather runs from two machines and aggregate them offline with `--corpus-aggregate <dir>`.
- **A small corpus cannot over-claim.** With only a few models, the matrix caps agreement at *Weak / Directional*; a *Confirmed* verdict requires the full corpus. The thresholds are fixed in advance and never chosen after seeing results.

The complete procedure — prerequisites, reproduction, the cross-host merge, adding a new model, and troubleshooting — along with the frozen hypotheses, thresholds, and non-claims, is covered in the project's internal architecture documentation.

## Reproducibility

Every admitted observation is tied to a reproducible envelope so a result can be re-derived:

- the **resolved-FOM SHA-256** is stamped into the design artifacts, the generated code, and the manifest, so the design-versus-runtime join is exact;
- the **scenario seed** (`--scenario-rng-seed <n>`, exported with `--emit-operational-load-factors`) is recorded in the manifest and verified identical across pooled replications;
- the **hardware identity** in reports is taken from the manifest (the machine that produced the telemetry), not from the machine generating the report, so offline and cross-host report generation stay correctly attributed.

## Typical workflow

1. Author and validate the FOM in SimGe ([OME](OME.md), [FOM Validation](Validation.md)).
2. Generate federate code with **Enable Fora Telemetry** on.
3. Run under the validation harness to capture telemetry (`--sample`, `--repetitions`).
4. Load the run in the Telemetry Visualizer and review drift, latency, and hotspots.
5. For a generalisation claim, run the corpus campaign (`--corpus`) and read the confirmatory matrix.
6. Adjust the model or generation settings and repeat.

> Deeper specifications of the telemetry/validation integration live in the project's internal architecture documentation.

---

**Next:** [Preferences & Options](Preferences.md)

---
Updated July 6, 2026, 01:55:00
