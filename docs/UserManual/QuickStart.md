# Quick Start: Your First Project

This guided first session takes about ten minutes. You will open a bundled sample, explore it, edit a module, and save your own copy. New to the concepts? Read [Introduction & Concepts](Introduction.md) first.

## 1. Launch SimGe

When SimGe starts, the **SimGe — Get Started** dialog appears with these choices:

| Choice | What it does |
|---|---|
| **Create New Project** | Opens the New Project wizard ([details](CreatingProjects.md)). |
| **Start Empty** | Closes the dialog and starts with no project. |
| **Open Samples → Chat / STMS** | Loads one of the bundled sample projects. |
| **Open a Project** | Browses for an existing `.fap` project file. |
| **Recent Projects** | Reopens a project you used recently. |

For this walkthrough, click **Open Samples → Chat**.

> You can reopen this dialog's choices at any time from the **Start Page** workspace.

## 2. Get your bearings

Once the Chat sample loads, the main window shows three things:

- **Project Explorer** (left) — a tree of your project: the **Object Models** root with **FOM Modules**, **SOM Modules**, and the read-only **MOM** library, plus **Federate Applications**.
- **Workspaces** (center) — tabbed editing areas. The **Start Page** opens here first.
- **Status bar** (bottom) — save state, busy activity, and contextual hints.

On the **Start Page**, find the **FOM Modules Dependency Graph**: it shows the sample's modules and how they depend on one another. Hover a node for a summary. (More in [Start Page](StartPage.md).)

## 3. Open a module in the editor

In the Project Explorer, expand **Object Models → FOM Modules**, then **double-click** a module (for example, the Chat FOM module).

The module opens in the **Object Model Editor (OME)** as a new workspace tab. The OME presents the model as a set of tables — **Objects**, **Interactions**, **Attributes**, **Parameters**, **Datatypes**, and more.

- Select the **Objects** table to see the object classes.
- Double-click an object class to open its editor and inspect its attributes.

See [OME](OME.md) for a full tour of every table and editor.

## 4. Make a small change

1. In the OME, open the **Objects** table and double-click a class.
2. Change a value (for example, an attribute's order or transportation), then confirm with **OK**.
3. Notice the status bar switch to **Unsaved changes**.

> Editing tip: dependency and standard (MOM) modules open **read-only** by design; edit your own FOM/SOM modules instead.

## 5. Save your own copy

The bundled samples are installed in a shared, read-only location, so **Save** is disabled for them. To keep your edits:

1. Choose **Save As**.
2. Pick a writable folder (your Documents folder is offered by default).
3. SimGe writes your editable copy there; from now on, ordinary **Save** works.

See [Opening & Saving Projects](OpeningSaving.md) for the details.

## 6. Where to go next

| Goal | Chapter |
|---|---|
| Understand modules and dependencies | [Modular FOM Concepts](ModularFOM.md) |
| Add, rename, or merge modules | [Managing Modules](ManagingModules.md) |
| Edit object/interaction models in depth | [OME](OME.md) |
| Bring in an existing FED/FDD | [Importing & Exporting](ImportExport.md) |
| Design which applications join the federation | [FAME](FAME.md) |
| Generate federate code | [Code Generator](CodeGenerator.md) |

---

**Next:** [The SimGe Workspace](Workspaces.md)
