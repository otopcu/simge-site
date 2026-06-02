# Opening & Saving Projects

This chapter covers opening existing projects and saving your work. For creating a brand-new project, see [Creating a Project](CreatingProjects.md).

## Opening a project

A SimGe project is a **`.fap`** file (Federation Architecture Project). You can open one in several ways:

- **SimGe — Get Started** dialog → **Open a Project**, then browse to the `.fap`.
- The application **Open Project** command.
- **Recent Projects** — pick a recently used project from the Get Started dialog or the menu.
- **Double-click a `.fap` file** in Windows Explorer (the installer registers this association).

When a project opens, its models hydrate, the Project Explorer fills in, and the Start Page shows the dependency graph.

> If a module's files cannot be found while opening, SimGe flags the module and offers recovery rather than failing — see [Project Explorer → Recovering Missing Module Files](ProjectExplorer.md#recovering-missing-module-files).

## Recently used projects (MRU)

SimGe remembers the projects you open most recently and lists them in the **Get Started** dialog and the menu, with a relative "last opened" time. Selecting an entry reopens that project. Entries that no longer exist on disk are shown as *file not found*.

## Saving

Use **Save** to write the current project to disk. A single save persists:

- the project file (`‹ProjectName›.fap`),
- the object-model index (`‹ProjectName›OM.fom`),
- each module as its metadata + content pair (`.sfom` + `.xml`) in the **Fom** folder.

The status bar reflects save state:

| State | Meaning |
|---|---|
| **Unsaved changes** | The project has edits that are not yet saved. |
| **‹ProjectName› saved.** | Transient confirmation shown right after a successful save. |
| **All changes saved** | Everything is persisted. |
| **Read-only sample** | A bundled sample is open; in-place Save is disabled (use Save As). |

### Save is blocked by validation errors

If the project settings or code-generator settings contain validation errors (for example, an invalid project name), SimGe blocks the save and shows which settings to fix first. Correct them, then save again.

## Save As (snapshot)

**Save As** writes the project to a new name and location, effectively creating a copy/snapshot. After a successful Save As, the project now points at the new location, so subsequent ordinary **Save** operations go there.

Common uses:

- Keeping an editable copy of a **read-only sample** (Save As stays available for samples and defaults to your Documents folder).
- Branching a project to try changes without touching the original.

## If a save fails

SimGe reports clear reasons instead of failing silently:

| Reported problem | What to do |
|---|---|
| **Access denied** | The target location is not writable (e.g. a sample under `ProgramData`). Use **Save As** to a writable folder. |
| **Directory not found** | The project's folder was moved or deleted. Use **Save As** to re-establish a valid location. |
| **Path is empty/invalid** | The project name or location is not set correctly — check the project settings. |

See [Project Structure & Settings](ProjectSettings.md) for what lives where, and [Troubleshooting](Troubleshooting.md) for more recovery guidance.

---

**Next:** [Project Structure & Settings](ProjectSettings.md)
