# Telemetry Visualizer

The Telemetry Visualizer inspects telemetry captured from a simulation run and visualizes how the federation behaved at run time — warm-up, latency, per-phase costs, selectivity, and how the runtime workload differs from the design-time model. It closes the loop between the model you authored in SimGe and what actually happened during execution.

> The visualizer is an **experimental** tool. Open it from **Tools → Experimental**.

## Prerequisite: a captured run

The visualizer reads artifacts produced by the **SimGe validation harness** when you run a telemetry-instrumented federate (see [Fora Telemetry & Validation](ForaTelemetry.md) and [Code Generator](CodeGenerator.md)). A run typically produces:

- a **`manifest.json`** describing the run, and
- one or more **`.fort`** log files (one per federate stream).

Optional sidecar files placed next to the manifest enrich the view: a `scenario.md` / `README.md` description, and generated reports such as `ValidationReport.md` and `TelemetryReliabilityReport.md`.

If you have not generated any run artifacts yet, the visualizer's landing screen explains what it does and how to produce them.

## Loading a run

Browse to and load a run's `manifest.json`. The visualizer reads it together with the sibling `.fort` logs and any report sidecars in the same folder. Each manifest represents a **single replication** of a scenario.

## FOM match verification

A badge shows whether the run's recorded FOM matches your current project:

| Badge | Meaning |
|---|---|
| **FOM Matched** | The run's FOM checksum matches the active merged FOM. |
| **FOM Mismatch** | The checksums differ — the run used a different FOM than the one currently open. |
| **Standalone — no project** | No project is open; the run is inspected on its own. |

A mismatch or standalone state never disables analysis — it only suppresses project-correlated design metrics in favor of the manifest's own snapshot.

## Scenario and Reports tabs

- **Scenario** — an auto-derived, plain-language picture of the run: scenario name/tier, run time and duration, the **federate roster** (from the `.fort` file names), and the **object/interaction class inventory** (from the manifest). An optional `scenario.md` / `README.md` sidecar is shown as free-text notes.
- **Reports** — auto-discovers the Markdown reports next to the manifest (e.g. `ValidationReport.md`). Reports render as formatted Markdown with a **Rendered / Raw** toggle; **Open** launches a report in its default app and **Folder** reveals it in Explorer.

## Charts

High-fidelity native charts visualize the run:

| Chart | Shows |
|---|---|
| **Operational Drift** (radar) | How the runtime workload shifted relative to the design-time model. |
| **JIT Warm-up** (step line) | The just-in-time compiler warm-up curve before steady state. |
| **Latency Histogram** | The distribution of steady-state event latencies. |
| **Sub-phase Breakdown** (stacked) | Where time goes across event sub-phases (encode/serialize/decode/apply). |

Grids complement the charts: a **hotspot burden** grid and an **attribute/parameter selectivity** grid.

## Exporting

- **Save as PNG…** is available for each canvas chart (Operational Drift, JIT warm-up, latency histogram, sub-phase breakdown).
- **Copy Table (MD)** copies the hotspot and selectivity grids as GitHub-flavored Markdown for pasting into docs or issues.

The toolbar groups these as flat **Copy / Open / Folder / Save** buttons with tooltips.

## Reading a run

1. Check the **FOM match** badge so you know the run corresponds to your model.
2. Read the **Scenario** tab to understand what was run.
3. Use **JIT Warm-up** to separate cold-start cost from steady state, then **Latency Histogram** and **Sub-phase Breakdown** for steady-state behavior.
4. Use **Operational Drift** and the selectivity/hotspot grids to see where runtime diverges from the design, then revisit those classes in the [OME](OME.md) or [Code Generator](CodeGenerator.md) settings.

> Each manifest is one replication. Pooling across independent runs is a job for the validation harness, not something inferred from a single manifest; the visualizer surfaces the pooled-replication count and instrumentation overhead from the accompanying reports when available.

---

**Next:** [FAME — Federation Architecture Modeling Environment](FAME.md)
