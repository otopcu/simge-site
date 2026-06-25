# Glossary

Definitions of the HLA, OMT, and SimGe-specific terms used throughout this manual.

## HLA and federations

**HLA (High Level Architecture)**
: The IEEE standard for building distributed simulations from independent, interoperating programs.

**Federate**
: A single participating simulation or application in a federation.

**Federation**
: A set of federates running together toward a common goal, sharing data through the RTI.

**Federation execution**
: A specific running instance of a federation, identified by name.

**RTI (Run-Time Infrastructure)**
: The middleware that connects federates and routes their data and interactions at run time.

## Object models

**OMT (Object Model Template)**
: The common tabular structure HLA uses to describe a model — object classes, interaction classes, attributes, parameters, data types, dimensions, and more.

**FOM (Federation Object Model)**
: The shared data contract for an entire federation: the classes, interactions, and data types every federate agrees on.

**SOM (Simulation Object Model)**
: The subset of objects and interactions a single federate can publish or subscribe to.

**MOM (Management Object Model)**
: The standard, built-in objects and interactions used to manage and monitor a federation. In SimGe it is a read-only system library module.

**MIM (Management and Initialization Module)**
: The standard base module that supplies the MOM and core definitions every model builds on.

**Object class**
: A class of simulated entities with attributes (e.g. an aircraft with position and heading).

**Interaction class**
: A class of transient messages exchanged between federates, carrying parameters.

**Attribute / Parameter**
: A named, typed data field of an object class (attribute) or interaction class (parameter).

## Modular FOM

**Module**
: A self-contained, reusable part of an object model, stored as a `.sfom` (metadata) + `.xml` (content) pair.

**Module role**
: How a module participates in composition — *Standalone*, *Dependency*, *Composed From*, or *Standard*.

**Dependency**
: A link from one module to another it builds on. *Resolved* when the target is present; *unresolved / orphan* when it is missing.

**Composition / Merge**
: Combining several modules into a single, standard-compliant FOM. SimGe merges automatically on export and code generation.

## File formats

**FAP (Federation Architecture Project)**
: SimGe's native project file (`.fap`), an XML document holding settings and model references.

**FED**
: The legacy HLA 1.3 federation execution data file (`.fed`).

**FDD (FOM Document Data)**
: The IEEE 1516 FOM document (`.xml`), used by the 2010 and 2025 standards.

**DIF**
: The IEEE 1516-2025 dependency/interface document.

**`.fom`**
: SimGe's object-model index file that lists a project's modules.

**`.sfom` / `.xml`**
: A module's metadata file and its content file, respectively.

## SimGe workspaces and tooling

**OME (Object Model Editor)**
: The workspace where you edit a module's OMT tables and elements.

**FAME (Federation Architecture Modeling Environment)**
: The workspace where you design the federation — its federate applications and RTI connections.

**FAM (Federation Architecture Module)**
: The artifact describing the federation's federate applications and execution (`.fam`).

**Fora**
: The IEEE 1516-2025 RTI client targeted by SimGe's code generation.

**Telemetry**
: Run-time measurements captured from an instrumented federate (warm-up, latency, per-phase costs), inspected in the Telemetry Visualizer.

**Validation harness**
: The tool that runs an instrumented federate and produces the telemetry artifacts (`manifest.json`, `.fort` logs) the visualizer reads.

**MRU (Most Recently Used)**
: The list of projects you opened recently, shown for quick reopening.

---

**Back to:** [User Manual overview](../UserManual.md)

---
Updated June 25, 2026, 16:28:09