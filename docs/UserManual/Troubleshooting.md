# Troubleshooting

Solutions to the problems users hit most often. If your issue is not here, check the chapter for the feature involved.

## A module shows a warning badge / "file missing"

A module's backing files (`.sfom` metadata or `.xml` content) could not be found on disk — for example, the project was moved or a file was renamed/deleted.

**What to do:** double-click the flagged module to start recovery. SimGe first tries to find the files automatically; if it cannot, it offers **Locate…**, **Remove Module**, or (for samples) **Repair SimGe**. See [Project Explorer → Recovering Missing Module Files](ProjectExplorer.md#recovering-missing-module-files).

## "Access denied" when saving

You are most likely saving a **bundled sample**, which lives in a read-only, machine-wide location (`C:\ProgramData\SimGe\Samples`).

**What to do:** use **Save As** to store an editable copy in a writable folder such as your Documents. Save As stays available for samples and defaults to Documents. See [Opening & Saving Projects](OpeningSaving.md).

## Save is disabled

The status bar reads **Read-only sample**, or the project has validation errors.

**What to do:**

- For a sample, use **Save As** (above).
- If settings are invalid, SimGe lists what to fix; correct the flagged project or code-generator settings, then save. See [Project Structure & Settings](ProjectSettings.md).

## A project will not open

Common causes:

- The `.fap` file or its `Fom` folder was moved/deleted.
- The project folder is on a drive that is currently unavailable.

**What to do:** confirm the `.fap` and its sibling `Fom` / `Fam` / `Source` folders are present together. If only some modules are missing, the project still opens and flags those modules for [recovery](ProjectExplorer.md#recovering-missing-module-files).

## Preferences seem to have reset

SimGe protects against corrupted settings files. If a settings file cannot be read, SimGe backs it up as **`.corrupt-{timestamp}`**, alerts you, and starts from clean defaults. Writes are atomic, so a crash or full disk should not leave a half-written settings file.

**What to do:** reapply your preferences; the corrupt backup is kept beside the settings file if you need to inspect it. See [Preferences & Options](Preferences.md).

## Import or validation errors

- **On import**, SimGe validates the source and reports problems as it reads it. Fix the source file or choose the correct standard. See [Importing & Exporting](ImportExport.md).
- **On export/validate**, work through the items in the results window — typically unresolved dependencies or required fields. See [FOM Validation](Validation.md).

## Unresolved (orphan) dependencies

A module references another module that is not present (removed, not yet added, or a name mismatch). The dependency is shown with a warning in the dependency graph and the module's dependency list.

**What to do:** add or relink the missing module, or edit the dependencies. See [Managing Modules](ManagingModules.md) and [Modular FOM Concepts → Dependencies](ModularFOM.md#dependencies).

## A sample's files are missing after install

If a bundled sample reports missing files, the install may be incomplete or modified.

**What to do:** use **Windows Settings → Apps → SimGe → Modify/Repair** to restore the bundled files, then reopen the sample. See [Installation & Updates](Installation.md).

## Telemetry Visualizer shows "FOM Mismatch"

The loaded run was captured against a different FOM than the project you currently have open. This is informational — analysis still works, but project-correlated design metrics are suppressed. Open the matching project, or inspect the run standalone. See [Telemetry Visualizer](TelemetryVisualizer.md).

---

**Next:** [Glossary](Glossary.md)
