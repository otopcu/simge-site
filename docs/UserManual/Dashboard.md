# FOM Dashboard

The FOM Dashboard gives a quantitative, at-a-glance view of a model — its size, complexity, capability, and health — so you can assess an object model without reading every table. It is especially useful for reviewing composed (merged) models.

## What the dashboard shows

The dashboard organizes its analysis into information cards grouped by category:

| Category | Tells you |
|---|---|
| **Volume** | Direct inventory counts — object classes, interaction classes, attributes, parameters, data types, dimensions, and the **OC/IC ratio**. |
| **Complexity** | Derived structural metrics over the class hierarchies (depth, breadth, and related architectural indicators). |
| **Capability** | What the model can express, based on its declared content. |
| **Health** | Diagnostic findings — unresolved dependencies, structural issues, and other warnings that need attention. |
| **Architecture** | Higher-level architectural information about how the model is put together. |

The figures are computed separately for the **Object Class (OC)** and **Interaction Class (IC)** domains, and a single analysis engine is the source of truth, so the dashboard numbers stay consistent with the [reports](MetricsReports.md).

## Toolbar actions

| Action | What it does |
|---|---|
| **Refresh Analysis** | Recomputes the analysis for the current model. |
| **Copy Summary** | Copies a short summary to the clipboard. |
| **Copy Full Report** | Copies the complete textual analysis to the clipboard. |
| **Export** | Saves the analysis for external use. |
| **Navigate** | Jumps to a problematic element flagged under Health. |

## How to read it

1. Start with **Volume** to understand the model's scale.
2. Check **Health** for anything flagged — resolve unresolved dependencies and structural issues first (see [Managing Modules](ManagingModules.md) and [FOM Validation](Validation.md)).
3. Use **Complexity** and **Architecture** to judge whether the model's structure matches your intent.
4. Re-run **Refresh Analysis** after edits to see the effect.

> The dashboard reflects the analyzed snapshot. For a single composed model, run the analysis on the merged result so the numbers represent what will actually be exported.

---

**Next:** [Model Metrics & Reports](MetricsReports.md)
