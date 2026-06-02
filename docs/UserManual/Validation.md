# FOM Validation

Validation checks that an object model is well-formed and standard-compliant before you export it or hand it to an RTI. Catching problems in SimGe is far cheaper than discovering them at federation start-up.

## What validation checks

Validation combines two kinds of checks:

- **Schema validation** — the generated document conforms to the official HLA OMT schema for the chosen standard.
- **Structural rules** — model-level consistency beyond the raw schema (for example, references that must resolve and required identification fields).

## Standards covered

SimGe can validate against the standard you are targeting:

| Target | Validates |
|---|---|
| **IEEE 1516-2010 FDD** | The 2010 FOM Document Data schema. |
| **IEEE 1516-2025 FDD** | The current FDD schema. |
| **DIF (2025)** | The 2025 dependency/interface document. |

## Running validation

Validation is available where you produce a standard document:

- In the **FDD/FED preview** surface, choose the target standard and use **Validate**. SimGe builds the merged document and checks it.
- Validation also runs **automatically during import**, so problems in a source file are reported as it is read (see [Importing & Exporting](ImportExport.md)).

Because SimGe authors models as [modules](ModularFOM.md), validation runs against the **merged** result — the same composition that export and code generation use — so what you validate is what you ship.

## Reading the results

Validation results open in a dedicated results window:

- A clean run reports success.
- Problems are listed with enough detail to locate them (the offending element and the rule or schema constraint involved).
- The window is color-coded by outcome so you can tell success from warnings and errors at a glance.

Work through the listed items, fix them in the [OME](OME.md), and re-validate until the model is clean.

## Common issues and fixes

| Symptom | Likely cause | Fix |
|---|---|---|
| **Unresolved dependency** warnings | A referenced module is missing or its name does not match. | Add or relink the module; see [Managing Modules](ManagingModules.md) and [Modular FOM Concepts → Dependencies](ModularFOM.md#dependencies). |
| **Schema errors** on export | A field required by the chosen standard is empty or malformed. | Complete the required fields in the relevant OME table, then re-validate. |
| **Datatype / reference errors** | An element points at a datatype or parent that no longer exists. | Repoint it in the OME to a valid target. |

## When to validate

- Before **exporting** a FOM for an RTI.
- After a large **edit** or a **merge** of modules.
- After **importing**, to confirm the upgraded model is clean.

---

**Next:** [Diagrams](Diagrams.md)
