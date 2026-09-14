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

The dashboard and analysis reports share the same analysis engine. Compare results for the same model, dependency closure, scope, and calibration settings.

## Metric definitions and published reference

The definitions and methodology for **Weighted Hierarchy Load (WHL)**, **Topological Skewness (S_top)**, **Depth Variation Index (CV_D)**, and **Polymorphic Potential (PP)** are given in Okan Topçu's [*Structural and topological complexity analysis in HLA object models*](https://doi.org/10.1016/j.simpat.2026.103328), *Simulation Modelling Practice and Theory*, 152 (2026), 103328.

Use the paper for the structural metric definitions and their analytical basis. This manual focuses on selecting an analysis scope, using the dashboard, and reading the resulting reports; it does not repeat the paper's derivations. These structural indicators describe pre-execution risk conditions; runtime cost and strategy benefits require measured evidence.

SimGe also displays semantic indicators, archetypes, and runtime validation results. These additional surfaces are not all definitions from that paper. Use the dashboard's information panels and report notes for their interpretation and current scope.

## Reading an analysis report

The **module analysis report**, available through the [dashboard](Dashboard.md), summarizes metrics, integrity findings, and engineering guidance. It differs from the printable OMT content report described below.

- **Object Model Archetype** describes the balance of object and interaction semantic mass. **Domain Dominance (A)** expresses distance from a balanced model; it is not confidence in the archetype label. Read the direction from the semantic-mass ratio **R** and the archetype label. A low dominance value is expected for a balanced Hybrid model.
- **Data-Type Impact** identifies the elements and, where available, modules affected by a datatype change.
- **Code-Generation Hints** are advisory suggestions. A hint does not activate a generator option; inspect the [generation settings and generated README](CodeGenerator.md).

When comparing reports across exports, use the same model and analysis scope. Corrected datatype bindings can change semantic masses and archetype labels without a change to the metric formula. The RPR/NETN corpora and their sample reports are repository research resources; they are not included in the SimGe installer.

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

Report generation runs in the background, so the application stays responsive while a large model is rendered; a status-bar indicator shows progress and the report appears when it is ready.

## Data-type impact analysis

The analysis report includes a **Data-Type Impact** section that answers a practical question before you change a model: *if I change this datatype, what else is affected?* It works from the model's datatype reference graph and covers two kinds of change:

- **Removal or rename** — identifies the references that depend on the type's identity. Deletion leaves unresolved type names that block code generation until repaired. The editor updates in-module references during a rename; references in dependent modules and already-generated code still require review.
- **Wire-format shift** — changing a type's representation or encoding does not break references, but it forces every codec that embeds the type to be regenerated and re-coordinated with other federates that share the FOM.

For each datatype the section reports its **blast radius** — the total number of elements (attributes, parameters, record fields, variant discriminants/alternatives, and the classes that carry them) reached transitively by such a change — and ranks the declared types by that reach. A high blast radius marks a load-bearing type to change carefully; a low one is comparatively safe to edit.

The same analysis drives the interactive impact warning shown when you delete a datatype in the [Object Model Editor](OME.md).

Composed analysis reports include a **Dependent Modules** column when ownership information is available. It identifies the modules containing affected elements; single-module reports omit this column. For an on-demand preview, right-click a datatype in the [Project Explorer](ProjectExplorer.md) and choose **Show Impact / Usages**.

## When to use metrics and reports

- Use the **[dashboard](Dashboard.md)** for quick, interactive assessment while you work.
- Use a **report** when you need a complete, presentable document of the model — for design reviews, archiving, or sharing with stakeholders.
- Use both together: the dashboard points you at what to inspect; the report captures the full detail.

---

**Next:** [Telemetry Visualizer](TelemetryVisualizer.md)

---
Updated September 14, 2026
