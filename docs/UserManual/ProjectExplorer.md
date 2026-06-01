# Project Explorer

The Project Explorer is the central hub for managing your FOM and SOM modules and their contents.

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
