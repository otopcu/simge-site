# The SimGe Workspace

This chapter describes the SimGe main window — its menus, toolbar, panels, workspace tabs, and status bar.

## Main window layout

| Area | Role |
|---|---|
| **Menu bar** (top) | All commands, grouped by topic (see [Menus](#menus)). |
| **Toolbar** (below the menu) | One-click access to the most common commands (see [Toolbar](#toolbar)). |
| **Project Explorer** (side panel) | The unified project tree — see [Project Explorer](ProjectExplorer.md). |
| **Properties** (side panel) | Properties of the item selected in the tree or a workspace. |
| **Workspaces** (center) | Tabbed editing areas — see [Workspaces](#workspaces). |
| **Status bar** (bottom) | Live feedback, save state, and activity (see [Status bar](#status-bar)). |

The **Project Explorer** and **Properties** panels can be shown or hidden from the **View** menu, and any workspace can be floated into its own window.

## Menus

| Menu | Contains |
|---|---|
| **File** | New Project, Load…, Close…, Save, Save As, **Sample Projects** (Chat / STMS), Exit. See [Opening & Saving](OpeningSaving.md). |
| **Edit** | Cut, Copy, Paste. |
| **View** | **Project Workspace ▸** (open any workspace — see below), **Project Explorer**, **Properties Window**. |
| **Object Model** | Load Object Model, Remove All Object Models, Create New FOM/SOM, **Import ▸** Any FDD (1.3 / 2010 / 2025), **Export ▸** FED / FDD. See [Importing & Exporting](ImportExport.md). |
| **Federation Architecture** | Create New Federation Architecture. See [FAME](FAME.md). |
| **Code Generator** | Generate Code. See [Code Generator](CodeGenerator.md). |
| **RID Editor** | RID file commands *(reserved / not yet enabled)*. |
| **Tools** | Project Settings, Preferences…, **FOM Dependencies…**, **Experimental ▸ Telemetry Visualizer…** |
| **Help** | SimGe User Manual, About SimGe (see [Installation & Updates](Installation.md)). |

## Toolbar

The toolbar mirrors the most-used commands in grouped icon buttons:

- **Project** — open the Chat / STMS samples, New Project, Load, Close, Save.
- **Object Model** — Create FOM, Load Object Model, Remove all object models, Import any FDD, Export FDD/FED.
- **Federation** — Create Federation Architecture.
- **Code** — Generate Code.
- **Settings** — Project Settings, Preferences.

## Workspaces

The central area holds **workspace tabs**. Open a workspace from **View → Project Workspace**, from the toolbar/menus, or by double-clicking an item in the Project Explorer.

| Workspace | What it is | Chapter |
|---|---|---|
| **Start Page** | Landing page: recent projects, samples, metadata, and the dependency graph. | [Start Page](StartPage.md) |
| **Object Model Editor (OME)** | Edits one FOM/SOM module — its tables, diagram editor, viewers, and dashboard. | [OME](OME.md), [Diagram Editor](Diagrams.md) |
| **FAME** | Designs the federation architecture and federate applications. | [FAME](FAME.md) |
| **Code Generator** | Generates federate code. | [Code Generator](CodeGenerator.md) |
| **Report Generator** | Renders a printable model report. | [Model Metrics & Reports](MetricsReports.md) |
| **Telemetry Visualizer** | Inspects simulation-run telemetry (Tools → Experimental). | [Telemetry Visualizer](TelemetryVisualizer.md) |
| **Project Settings** | Project and code-generator settings. | [Project Structure & Settings](ProjectSettings.md) |
| **Preferences** | Application-wide preferences. | [Preferences & Options](Preferences.md) |

> The generated **code viewer** and the **FED / FDD viewers** open from the Project Explorer or from within the OME workspace rather than as standalone top-level tabs.

## Tab controls

Each workspace tab has two action buttons in its header:

| Button | Description |
|--------|-------------|
| ↗ (Open in floating window) | Detaches the workspace into a separate floating window |
| ✕ (Close) | Closes the workspace |

Right-clicking a tab opens a context menu with additional options:

| Menu Item | Description |
|-----------|-------------|
| **Close This** | Closes the current tab |
| **Close All** | Closes all open workspace tabs |
| **Close All But This** | Closes every tab except the one you right-clicked |

## Floating windows

You can move any workspace into its own independent window — useful when working with multiple monitors or when you want to compare two workspaces side by side.

### Detaching a workspace

1. Click the **↗** icon on the tab you want to detach.
2. The tab disappears from the main tab strip and the workspace opens in a new floating window.
3. The floating window is fully functional — all editing, Project Explorer interactions, and tool operations continue to work as normal.

> You can detach multiple workspaces at the same time. Each gets its own independent window.

### Bringing windows to the front

- Click any floating window's title bar to bring it to the front.
- Click the main application window to bring it in front of any floating windows.
- All windows are independent — none is forced to stay on top of another.

### Docking a workspace back

To return a floating workspace to the main tab strip:

1. Click the **← Dock Back** button in the floating window's toolbar.
2. The floating window closes and the workspace reappears as the active tab in the main window.

### Closing a floating workspace

To close (discard) a floating workspace entirely, click the standard **✕** close button on the floating window's title bar. The workspace is closed and removed — it does not return to the main tab strip.

## Status bar

The status bar along the bottom is organized into zones:

- **Primary status** (left) — the latest message or the active workspace's hint, colour-coded by category (info / success / warning / error).
- **Save state** — a persistent badge: **Unsaved changes**, **All changes saved**, **Read-only sample**, or **No project**.
- **Busy activity** — a progress message and bar shown while a long operation runs.
- **Workspace / project info** (right) — the active workspace and project context.

## Tips

- If you open a module that is already detached in a floating window, SimGe brings that floating window to the front instead of opening a duplicate.
- Closing a project closes all workspaces, including any that are currently in floating windows.
- Use **View → Project Workspace** to jump to any workspace without hunting through the tree.

---

**Next:** [Start Page](StartPage.md)
