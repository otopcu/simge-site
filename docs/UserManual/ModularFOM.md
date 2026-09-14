# Modular FOM Concepts

SimGe represents object models as **modules** that combine to form a complete model, rather than one large file. This chapter explains the concepts; see [Managing Modules](ManagingModules.md) for the day-to-day operations.

## Why modular?

A modular object model lets you:

- **Reuse** standard distributions (such as RPR-FOM or NETN) without copying their content into your own model.
- **Separate concerns** — keep your additions in small, focused modules.
- **Compose** a federation's full FOM from several modules, then merge them only when you export or generate code.

This mirrors the IEEE 1516 *modular FOM* idea: standards ship as modules, and a federation picks the ones it needs.

## Modules and their files

Each module is a self-contained unit stored as two files in the project's **Fom** folder:

- a **`.sfom`** metadata file (role, dependencies, identity, and the content file name), and
- a **`.xml`** content file (the actual object classes, interactions, data types, and so on).

The model content itself is edited in the [Object Model Editor (OME)](OME.md).

## FOM, SOM, and MOM

| Kind | Role in SimGe |
|---|---|
| **FOM** | Federation Object Model content — the shared classes/interactions for the federation. Most of your modules are FOM modules. |
| **SOM** | Simulation Object Model content — what a single federate publishes/subscribes. Shown under **SOM Modules**. |
| **MOM** | The standard Management Object Model, provided as a built-in, **read-only** system library module. You can inspect it and copy standard elements from it, but not modify it. |

## Module roles

Every module has a role that describes how it participates in composition:

| Role | Meaning |
|---|---|
| **Standalone** | Has no dependencies — a self-sufficient base module. |
| **Dependency** | Used by other modules; opened **read-only** in the editor so a consumed module is not changed by accident. |
| **Composed From** | Built by combining other modules. |
| **Standard** | A built-in, read-only standard module (e.g. the MIM/MOM). |

## Dependencies

A module can depend on others — for example, an extension that builds on RPR-Base. SimGe tracks these links and uses them to:

- order and lay out the [dependency graph](StartPage.md#fom-modules-dependency-graph) on the Start Page,
- arrange modules hierarchically in the Project Explorer,
- compute the correct merge order on export/generation.

### Resolved vs. unresolved (orphan) dependencies

A dependency is **resolved** when the module it points to is present in the project. If the target is missing — for instance, you removed it or have not added it yet — the link becomes an **unresolved (orphan) dependency**. SimGe shows unresolved dependencies with a warning (red styling in the dependency graph and in the module's dependency list) so you can add or relink the missing module.

## Identification and naming

A module has three related names that serve different purposes:

| Name | Purpose |
|---|---|
| **Display name** | The short label shown in the Project Explorer. |
| **FOM name** | The HLA model-identification name from the model's Identification table. Dependency relationships are matched on this name. |
| **File name** | The on-disk content file (`.xml`). |

Keeping these consistent matters when standard distributions are involved, because dependencies are resolved by the FOM name.

## Dependency naming interoperability (RPR / NETN)

Different standard FOM distributions sometimes refer to the same base module by different identification strings (short names, versioned file names, or long SISO model-identification names). SimGe aligns these variants when matching dependencies so that valid links are not turned into orphans. The details and the underlying interoperability issue are captured in the project's standards note *"Modular FOM Dependency Naming Interoperability (RPR / NETN)."*

## Composition and merge

While you edit, modules stay separate. **Composition** merges the selected modules and their dependencies into a unified model using the merge rules. Use a composed model when you need the full dependency closure in one FOM; code generation also uses composition to resolve model dependencies.

Individual module export remains available. In the FDD viewer, choosing **Composed dependency closure** for validation does not replace the model exported by the viewer. See [Importing & Exporting](ImportExport.md#exporting) for this distinction.

---

**Next:** [Managing Modules](ManagingModules.md)

---
Updated September 14, 2026
