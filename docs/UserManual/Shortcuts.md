# Keyboard Shortcuts & Tips

A reference for the keyboard shortcuts available in SimGe, plus a few workflow tips. Most top-level actions (New, Open, Save, Import, Export) are run from the menus and toolbars; the shortcuts below cover the surfaces where keys speed things up.

## Text editing

Standard Windows text-editing shortcuts work in editable fields:

| Shortcut | Action |
|---|---|
| **Ctrl + X** | Cut |
| **Ctrl + C** | Copy |
| **Ctrl + V** | Paste |

## Viewers (FDD / FED / generated code)

In the document and code viewers:

| Shortcut | Action |
|---|---|
| **Ctrl + C** | Copy the displayed content to the clipboard |
| **Ctrl + E** | Export the content to a file |

## OME editors

When editing collections inside the [Object Model Editor](OME.md) — attributes, parameters, enumerators, fixed-record fields, and variant alternatives:

| Shortcut | Action |
|---|---|
| **Delete** | Remove the selected row (attribute, parameter, enumerator, field, or alternative) |

## Diagrams

Diagram surfaces share a common navigation model. The full table is in [Diagrams → Interaction, Navigation, and Toolbar Controls](Diagrams.md#interaction-navigation-and-toolbar-controls); the essentials:

| Shortcut | Action |
|---|---|
| **Ctrl + Mouse Wheel** | Zoom in/out toward the cursor |
| **Ctrl + Left-drag** | Pan the canvas (selection preserved) |
| **Click** | Select a single node |
| **Shift + Click** | Toggle a node in a multi-selection |
| **Ctrl + A** | Select all nodes |
| **Esc** or click empty canvas | Clear the selection |
| **Delete** | Delete the selected node(s) after confirmation |

## Dialogs

| Shortcut | Action |
|---|---|
| **Enter** | Confirm the default action (e.g. **OK** / primary button) |
| **Esc** | Cancel or close the dialog |

## Tips

- **Double-click to open** — double-click a module in the Project Explorer (or a node in a diagram) to open it in the [OME](OME.md).
- **Floating windows** — detach a workspace into its own window to compare two side by side; see [The SimGe Workspace](Workspaces.md).
- **Save As for samples** — bundled samples are read-only; use **Save As** to keep an editable copy (see [Opening & Saving Projects](OpeningSaving.md)).
- **Validate before exporting** — run [validation](Validation.md) after big edits or merges to catch issues early.
- **Watch the status bar** — it shows save state (*Unsaved changes* / *All changes saved* / *Read-only sample*) and contextual hints for the active workspace.

---

**Back to:** [User Manual overview](../UserManual.md)
