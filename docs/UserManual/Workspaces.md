# Workspaces

Workspaces are the main editing areas in SimGe. Each open module, diagram, or tool (such as an OME or FAME) occupies its own workspace tab at the top of the application window.

---

## Tab Controls

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

---

## Floating Windows

You can move any workspace into its own independent window — useful when working with multiple monitors or when you want to compare two workspaces side by side.

### Detaching a Workspace

1. Click the **↗** icon on the tab you want to detach.
2. The tab disappears from the main tab strip and the workspace opens in a new floating window.
3. The floating window is fully functional — all editing, Project Explorer interactions, and tool operations continue to work as normal.

> You can detach multiple workspaces at the same time. Each gets its own independent window.

### Bringing Windows to the Front

- Click any floating window's title bar to bring it to the front.
- Click the main application window to bring it in front of any floating windows.
- All windows are independent — none is forced to stay on top of another.

### Docking a Workspace Back

To return a floating workspace to the main tab strip:

1. Click the **← Dock Back** button in the floating window's toolbar.
2. The floating window closes and the workspace reappears as the active tab in the main window.

### Closing a Floating Workspace

To close (discard) a floating workspace entirely, click the standard **✕** close button on the floating window's title bar. The workspace is closed and removed — it does not return to the main tab strip.

---

## Tips

- If you open a module that is already detached in a floating window, SimGe brings that floating window to the front instead of opening a duplicate.
- Closing a project closes all workspaces, including any that are currently in floating windows.
