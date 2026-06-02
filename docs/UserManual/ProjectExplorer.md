# Project Explorer

The Project Explorer is the central hub for managing your FOM and SOM modules and their contents.

## The project tree

Under the project root, the tree groups everything into two top-level areas:

- **Object Models** — the object-model repository:
    - **FOM Modules** — your [FOM modules](ModularFOM.md) and their content trees.
    - **SOM Modules** — your SOM modules.
    - **MOM** — the read-only standard [Management Object Model](ModularFOM.md#fom-som-and-mom) library (see [MOM Explorer](#mom-explorer-system-library) below).
- **Federate Applications** — the federation's [federate applications](FAME.md) and the code generated for them.

Double-clicking a module opens it in the [OME](OME.md); double-clicking a node in the dependency graph or an explorer item behaves the same way. Right-clicking any item opens a context menu tailored to that item.

## Context menus

Each kind of item has its own right-click menu. Most menus also include **Expand** / **Collapse**.

### Project root

| Command | What it does |
|---|---|
| **Rename Project…** | Renames the project and its folder. See [Opening & Saving](OpeningSaving.md). |

### "Object Models" folder

| Command | What it does |
|---|---|
| **Load SimGe OM…** | Loads an existing object-model index into the project. |
| **Create New OM…** | Creates a new (FOM) object model. |
| **Clear All OM** | Removes all object models from the project. |

### "FOM Modules" folder

| Command | What it does |
|---|---|
| **Add New FOM Module…** | Creates a new, empty module. See [Managing Modules → Adding modules](ManagingModules.md#adding-modules). |
| **Add Existing FOM Module(s)…** | Brings existing module file(s) into the project. See [Managing Modules → Adding modules](ManagingModules.md#adding-modules). |
| **Module Dependencies…** | Opens the [Module Dependencies tool](ManagingModules.md#editing-dependencies). |
| **Remove All Modules** | Clears every module in one batch. See [Managing Modules → Removing](ManagingModules.md#removing). |

### "SOM Modules" folder

| Command | What it does |
|---|---|
| **Create New SOM…** | Creates a new SOM module. |

### A module (FOM / SOM)

| Command | What it does |
|---|---|
| **Open in Editor** | Opens the module in the [OME](OME.md) (read-only for dependency/standard modules). |
| **Rename Module…** | Renames the module and its files. See [Managing Modules → Renaming](ManagingModules.md#renaming). |
| **Duplicate Module…** | Copies the module under a new name. See [Managing Modules → Duplicating](ManagingModules.md#duplicating). |
| **Paste OMT Element** | Pastes a copied element into the module. See [Copy and Paste OMT Elements](#copy-and-paste-omt-elements). |
| **Set as Project FOM** | Marks this module as the project's primary FOM. |
| **Merge Modules…** | Merges modules into one. See [Managing Modules → Merging](ManagingModules.md#merging). |
| **Module Dependencies…** | Edits this module's dependencies. See [Managing Modules → Editing dependencies](ManagingModules.md#editing-dependencies). |
| **Remove Module** | Removes the module from the project. See [Managing Modules → Removing](ManagingModules.md#removing). |
| **Export FDD… / Export FED…** | Exports the module to a standard file. See [Importing & Exporting](ImportExport.md). |
| **Properties** | Shows the module's properties panel. |

### An OMT element (inside a module's content)

| Command | What it does |
|---|---|
| **Rename** | Renames the element. |
| **Copy** | Copies the element. See [Copy and Paste OMT Elements](#copy-and-paste-omt-elements). |
| **Delete** | Deletes the element. |
| **Properties** | Shows the element's properties panel. |

### "Federate Applications" folder

| Command | What it does |
|---|---|
| **Generate All Code** | Generates code for every federate application. See [Code Generator](CodeGenerator.md). |

### A federate application

| Command | What it does |
|---|---|
| **Generate Code** | Generates code for this federate. See [Code Generator](CodeGenerator.md). |
| **Open Latest Generated Code Folder** | Opens the most recent output folder in Windows Explorer. |
| **Rename** | Renames the federate application. See [FAME](FAME.md). |
| **Delete** | Removes the federate application. |

### Generated code (run folder / file)

| Command | What it does |
|---|---|
| **Open in File Explorer** | Reveals the generated folder or file in Windows Explorer. |

## MOM Explorer (System Library)
SimGe includes a dedicated "MOM" folder under the "Object Models" root node in the Project Explorer. This acts as a System Library providing the standard IEEE 1516-2025 Management Object Model (MOM).
- **Read-Only**: System-defined elements within the MOM module are robustly protected against modification or deletion.
- **Editor Access**: Double-clicking the MOM module opens it in the OMT Table Editor in a read-only state, allowing for deep inspection of standard structures.
- **Utilization**: You can view the properties of the newest HLA 4 MOM objects, interactions, and unsigned data types. These standard elements can also be copied (via right-click or drag-and-drop) into your custom FOM/SOM modules.

---

## Drag-and-Drop OMT Elements
You can easily transfer OMT elements (such as Objects, Interactions, and Data Types) between modules using native drag-and-drop within the Project Explorer.
1. Click and hold the left mouse button on a source OMT element.
2. Drag the element over a target FOM or SOM module node.
3. **Visual Feedback**: The cursor will change to a "plus" (+) sign when hovering over valid, editable targets. It will show a "prohibited" sign when hovering over invalid targets (such as the read-only MOM module or the Project Root).
4. Release the mouse button to drop the element. 

SimGe automatically invokes the copy-paste validation, ensuring the element is placed in the correct OMT section and resolving any name conflicts via a confirmation dialog.

---

## Copy and Paste OMT Elements

Use this feature to copy a user-defined OMT element from one FOM/SOM module to another.

### Requirements

- The source element must be inside a FOM or SOM module.
- The source element must be `UserDefined`.
- The target module must be a FOM or SOM with editable `UserDefined` content.
- Built-in, MOM, MIM, read-only, and non-module tree items cannot be copied or used as paste targets.

### Copy

1. In Project Explorer, expand a FOM/SOM module and its `Content` tree.
2. Right-click a `UserDefined` OMT element.
3. Select `Copy`.
4. Review the confirmation dialog. It lists what will be copied, including child tree elements and owned details such as enum values.
5. Confirm to store the copied element.

### Paste

1. Right-click the target FOM/SOM module.
2. Select `Paste OMT Element`.
3. SimGe places the copied element in the matching OMT section.

#### Examples:

- Enumerated data type -> `EnumeratedDataTypes`
- Simple data type -> `SimpleDataTypes`
- Object class -> `Objects` / `HLAobjectRoot`
- Interaction class -> `Interactions` / `HLAinteractionRoot`
- Dimension -> `Dimensions`

### Name Conflicts

If the target already contains an element with the same name, SimGe asks whether to paste with a generated copy name, such as `StatusType_copy`.

### Clipboard Behavior

The paste buffer is cleared after every paste attempt, whether the operation succeeds, fails, or is cancelled. To paste again, copy the element again.

---

## Recovering Missing Module Files

Each FOM/SOM module is stored on disk as two files in your project's `Fom` folder: a small metadata file (`.sfom`) and the model content file (`.xml`). If a project is moved, a file is renamed or deleted, or a shared drive is offline, SimGe may not find these files when you open the project.

### Spotting a broken module

Modules whose files are missing are marked with a red warning badge (a circled `!`):

- In the **Project Explorer** tree, next to the module name.
- On the **Start Page** dependency graph, on the module's node.

Hover the badge (or the node) to see a tooltip describing what is missing and a reminder that double-clicking starts recovery.

### What happens when you open a broken module

When you double-click a module whose files are missing, SimGe first tries to find them automatically in the usual project locations (the project folder, its `Fom` subfolder, and the configured FOM folder). If both files turn up there, the module is repaired silently and opens as normal.

If they still cannot be found, a **Module File Missing** dialog appears with these choices:

| Option | What it does |
|---|---|
| **Locate…** | Opens a file browser so you can point SimGe at the module's `.sfom` file. |
| **Remove Module** | Removes the broken module from the project (after a confirmation). Use this when the file is gone for good. |
| **Repair SimGe…** | Shown only for the bundled sample projects — guides you to repair the installation to restore the original sample files. |
| **Cancel** | Closes the dialog without opening the module. |

### Locating a file

1. Choose **Locate…** and browse to the module's `.sfom` metadata file.
2. SimGe checks that the selected file really belongs to this module. If it belongs to a *different* module, you are warned and asked to confirm before continuing, so module links are not silently mixed up.
3. The module is reloaded from the chosen location and its contents reappear under the correct **FOM Modules** or **SOM Modules** folder in the Project Explorer.
4. If the metadata is found but its content (`.xml`) file is not next to it, SimGe tells you exactly which file to place beside the `.sfom` and try again.

The browse dialog remembers the folder you last used during the current session, so locating several files in a row is quicker. It resets to the default folder the next time you start SimGe.

> **Tip:** Because the project path was repaired, SimGe marks the project as having unsaved changes. Save the project to make the corrected location permanent.

### A note on sample projects

The bundled sample projects (e.g. **Chat**, **STMS**) are installed in a shared, read-only location, so **Save** is disabled for them. To keep your edits — including a repaired module path — use **Save As** to store your own copy in a writable folder such as your Documents. **Save As** is always available for samples.
