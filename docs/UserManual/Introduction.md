# Introduction & Concepts

This chapter introduces SimGe and the core ideas you need before building object models and federations. If you would rather dive in, jump to the [Quick Start](QuickStart.md).

## What is SimGe?

**SimGe (Simulation Generator)** is a Windows desktop application for designing High Level Architecture (HLA) distributed simulations and generating their code. With SimGe you can:

- Author and edit **object models** (FOM/SOM) with a full table-based editor.
- Compose models from reusable **modules** and visualize their dependencies.
- Design the **federation architecture** — which applications participate and how they connect to the RTI.
- **Import and export** standard HLA model files (FED / FDD / DIF).
- **Generate code** for federate applications, targeting the modern IEEE 1516-2025 / Fora workflow.

SimGe is a free academic research tool aimed at *federation designers* — the simulation developers who plan and build HLA federations.

## HLA in brief

The **High Level Architecture (HLA)** is a standard for building distributed simulations out of independent programs that interoperate at run time:

| Term | Meaning |
|---|---|
| **Federate** | A single participating simulation/application. |
| **Federation** | A set of federates working together toward a common run. |
| **RTI** (Run-Time Infrastructure) | The middleware that connects federates and routes data. |
| **FOM** (Federation Object Model) | The shared data contract for a whole federation — the object classes, interaction classes, and data types everyone agrees on. |
| **SOM** (Simulation Object Model) | The slice of objects/interactions a single federate can publish or subscribe to. |
| **MOM** (Management Object Model) | The standard, built-in objects/interactions used to manage and monitor a federation. |

These models are all expressed with the HLA **Object Model Template (OMT)** — the common tabular structure (object classes, interactions, attributes, parameters, data types, dimensions, and more) that SimGe edits in the [Object Model Editor (OME)](OME.md).

## Modular FOM

SimGe represents object models as composable **modules** rather than one monolithic file. Each module is a self-contained `.sfom` (metadata) + `.xml` (content) pair, and modules combine to form the complete model.

- **Module roles** — *Standalone* (no dependencies), *Dependency* (used by others), *Composed From* (built by combining modules), and *Standard* (read-only system modules such as the MIM/MOM).
- **Dependencies** — a module can build on others; SimGe tracks these links and shows them in the [dependency graph](StartPage.md#fom-modules-dependency-graph).
- **Composition & merge** — when you export or generate code, SimGe merges the composed modules into a single, standard-compliant FOM suitable for the RTI.

Modularity lets you reuse standard distributions (e.g. RPR-FOM, NETN) and keep your own additions in separate, focused modules. See [Modular FOM Concepts](ModularFOM.md) for the full picture.

## Supported HLA standards

SimGe understands three standard generations:

- **HLA 1.3** — legacy support (import/export of `.fed`).
- **IEEE 1516-2010** — widely used production standard.
- **IEEE 1516-2025** — the newest standard and the focus of SimGe's active code-generation path (via the Fora protocol client).

When you import an older model, SimGe can upgrade it to IEEE 1516-2025 so you work in a single, modern representation.

## How the pieces fit together

A typical SimGe workflow moves through these stages:

1. **Create or open a project** — a project (`.fap` file) holds your models, federation design, and settings. See [Creating a Project](CreatingProjects.md).
2. **Build the object model** — add modules and edit them in the [OME](OME.md), or [import](ImportExport.md) an existing FED/FDD.
3. **Validate and analyze** — check the model with [validation](Validation.md), the [dashboard](Dashboard.md), and [metrics & reports](MetricsReports.md).
4. **Design the federation** — define federate applications and their RTI connections in [FAME](FAME.md).
5. **Generate code** — produce federate code with the [Code Generator](CodeGenerator.md).

## Key terms at a glance

| Term | Short definition |
|---|---|
| **OMT** | Object Model Template — the tabular structure of an HLA model. |
| **FOM / SOM / MOM** | Federation / Simulation / Management Object Model. |
| **Module** | A reusable, self-contained part of an object model (`.sfom` + `.xml`). |
| **FAP** | Federation Architecture Project — SimGe's native project file (`.fap`). |
| **OME** | Object Model Editor — where you edit module contents. |
| **FAME** | Federation Architecture Modeling Environment — where you design the federation. |
| **FED / FDD / DIF** | Standard HLA model file formats SimGe imports and exports. |
| **Fora** | The IEEE 1516-2025 RTI client targeted by code generation. |

A fuller [Glossary](Glossary.md) is provided in the appendix.

---

**Next:** [Quick Start: Your First Project](QuickStart.md)
