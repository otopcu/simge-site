# Model Metrics & Reports

SimGe can quantify a model with **metrics** and produce a formatted, printable **report** of its full contents. Metrics drive the [FOM Dashboard](Dashboard.md) and the analysis reports; the model report documents every table for review or archiving.

## Model metrics

Metrics fall into two groups:

- **Direct volume counters** — straightforward inventory counts taken directly from the model:
  - object-class count, interaction-class count,
  - attribute count, parameter count,
  - data-type count, dimension count,
  - the **OC/IC ratio** (object classes ÷ interaction classes).
- **Derived architectural metrics** — computed indicators over the class hierarchies (for example, hierarchy depth/breadth and complexity/architecture measures), evaluated separately for the Object Class and Interaction Class domains.

A single analysis engine computes these values once, so the [dashboard](Dashboard.md) cards and the reports always agree.

> In a merged (composed) analysis, breakdowns may distinguish elements you declared locally from those that come from base modules, while the direct totals remain raw counts of the analyzed content.

## The model report

The report renders a model's full OMT content as a formatted document using the built-in report viewer. It covers the model's tables, including:

- object classes and interaction classes,
- attributes and parameters,
- dimensions, time representations, tags, transportations, update rates, switches, services, notes,
- data types (basic representations, simple, enumerated, array, fixed-record, and variant-record).

The report opens in its own **Reports** workspace, where you can review it on screen, print it, or export it through the viewer's standard controls.

## Generating a report

1. Select the module you want to document.
2. Open its **Report** (Reports workspace).
3. The report builds from the module's current content; if you edit the model, regenerate to refresh it.

## When to use metrics and reports

- Use the **[dashboard](Dashboard.md)** for quick, interactive assessment while you work.
- Use a **report** when you need a complete, presentable document of the model — for design reviews, archiving, or sharing with stakeholders.
- Use both together: the dashboard points you at what to inspect; the report captures the full detail.

---

**Next:** [Telemetry Visualizer](TelemetryVisualizer.md)
