# Preferences & Options

Application preferences control SimGe's start-up behavior and the recent-projects list. They apply to the whole application, independent of any open project.

## Opening Preferences

Open the **Preferences** workspace from the application. Preferences are grouped into sections you select on the left.

## Startup

Control what happens when SimGe launches and how a project opens.

| Setting | Effect |
|---|---|
| **Reopen last project** | When enabled, SimGe reopens the most recently used project on launch instead of starting empty. |
| **Show Explorer on startup** | Shows the Project Explorer panel automatically when a project opens. |
| **Show Properties on startup** | Shows the Properties panel automatically when a project opens. |

## Recently used projects (MRU)

| Setting | Effect |
|---|---|
| **Maximum recent files** | How many recent projects SimGe remembers and lists in the Get Started dialog and menus. |

See [Opening & Saving Projects](OpeningSaving.md) for how the recent list is used.

## How preferences are stored

Preferences are saved to a per-user settings file as you change them. SimGe makes this robust:

- **Atomic writes** — settings are written to a temporary file and then swapped into place, so a crash or full disk cannot leave a half-written file.
- **Corruption recovery** — if the settings file cannot be read, SimGe backs it up as **`.corrupt-{timestamp}`**, alerts you, and starts from clean defaults. Reapply your preferences; the backup is kept beside the settings file if you want to inspect it.

## Default options

SimGe ships with a **`DefaultOptions.xml`** in its installation directory that seeds default settings. It is loaded from the install location regardless of where SimGe is launched from, so defaults are consistent.

> Theme switching (light/dark) and panel layout are controlled from the application shell itself, not from this Preferences surface.

---

**Next:** [Keyboard Shortcuts & Tips](Shortcuts.md)
