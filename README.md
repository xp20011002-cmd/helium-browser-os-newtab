# HeliumOS

Browser extension (Manifest V3) that turns the **new tab** into a small operating system with an **Aero (Windows 7)** theme: desktop, windows, file system, terminal, code editor, image viewer, audio/video player and much more.

Built for [Helium](https://github.com/imputnet/helium), but works in any Chromium-based browser (Chrome, Edge, Brave etc.). No frameworks, no dependencies: just HTML, CSS and JavaScript.

> **Stage:** experimental (v0.7). Not a real OS: it runs inside the browser sandbox.

<!-- Add screenshots here: docs/screenshot-desktop.png, docs/screenshot-files.png -->

## Features

**Desktop and windows**
- Windows that can be moved, resized, minimized, maximized and closed, with animations
- Aero Snap: drag a window to the left/right edge (half screen) or to the top (maximized)
- Taskbar with buttons grouped by application, pinned apps, hover preview, calendar and “Show desktop”
- Two-column Start menu with program and file search
- Movable desktop icons: drag them onto a grid, select multiple with a rectangle or Ctrl+click, drag files from the desktop onto the Recycle Bin or into a folder
- Boot animation, login screen with optional password and lock screen
- Virtual desktops (up to 6) with Task View (▦ button next to Start)
- Notifications: balloons on the desktop, notification center (🔔) with history, Do Not Disturb, optional sound and reminders
- Context menus (right-click) on the desktop, in Files and on the taskbar

**Included applications**

| Application | What it does |
|---|---|
| Files | Explorer with navigation pane, back/forward, search, sortable columns, drag & drop, import/export |
| Notepad | Simple text editor (Ctrl+S) |
| Code | Editor with syntax highlighting (JS, CSS, HTML, Python), run `.js` and preview `.html` in an isolated sandbox |
| Terminal | `ls`, `cd`, `cat`, `echo > file`, `mkdir`, `rm`, `mv`, `cp`, `open`, `edit` and more (`help`) |
| Images | Browse between images, zoom, “Set as wallpaper”, “Edit in Paint” |
| Paint | Pencil, brush, eraser, spray, line, rectangle, ellipse, fill, eyedropper, text; undo/redo, color palettes, save PNG/JPG to `/Pictures` |
| Solitaire | Klondike with draw 1 or 3, mouse drag, double-click to foundations, undo, auto-complete, scoring, timer and statistics |
| Recycle Bin | Deleted items go here; restore to original location or permanently delete |
| Task Manager | List of windows (with “Close window”) and performance graph |
| Music | Audio and video with playlist and spectrum visualizer |
| Browser | Web pages in an iframe |
| Calculator | Simple calculator |
| Settings | Theme, wallpaper, icons, account and password, notifications, backups |

**Customization**
- Custom wallpaper (fill, fit, stretch, tile, center) or from your own images
- Custom icons for each application (image or emoji), on the desktop and in the taskbar
- Window color, transparency (Aero glass) or “Flat” theme, user name and picture
- User password (PBKDF2 hash, ordinary access only, not encryption) and configurable boot animation

## Virtual desktops

- The ▦ button on the taskbar (or Alt+Shift+↑) opens **Task View**: desktop thumbnails, windows of the current desktop and “+ New desktop” (maximum 6).
- Click a desktop to activate it. Double-click the name to rename it, ✕ removes it (its windows move to a neighboring desktop).
- Move a window by dragging it onto a desktop or from the right-click menu on its title bar.
- Keys: **Alt+Shift+←/→** or **Ctrl+Alt+←/→** switch desktops, and **1–6** switch desktop while Task View is open. Some systems reserve these combinations.
- Terminal: `desk` (list), `desk 2` (switch), `desk new` (create).
- Desktop icons are shared across all desktops, and the taskbar shows the windows of the current desktop.

## Notifications

- Shown as balloons in the bottom-right corner and kept in the notification center (🔔 on the taskbar).
- Events that generate notifications: imports, moving to the Recycle Bin (with “Undo” button), images saved from Paint, password change, space warning and backup reminder.
- **Reminders:** from the 🔔 panel or from the Terminal: `remind 10m take the bread out`. They sound while HeliumOS is open in a tab; missed ones appear on the next open.
- **From code:** in the Code app, a `.js` file can call `notify("Title", "Message")`.
- **System notifications** (optional): in Settings → Notifications, with the optional “notifications” permission, they also appear when the tab is not in the foreground.

## Installation

1. Download or clone the repository.
2. Open the extensions page (`helium://extensions` or `chrome://extensions`).
3. Enable **Developer mode**.
4. Click **Load unpacked** and select the project folder.
5. Open a new tab.

## How files work

- Files are stored locally in the browser (**IndexedDB**). Nothing is sent to the internet.
- You can drag files from your computer onto the desktop (they end up in `/Desktop`) or into a folder in the Files app.
- Limit is 20 MB per file. Large files make saves slower.
- If you clear browser data, the files disappear. Use **Settings → System → Export** for backups.

## Permissions

| Permission | Reason |
|---|---|
| `chrome_url_overrides.newtab` | Replaces the new tab page |
| `storage` | Remembers per browser session whether you are logged in and whether the boot animation has run |
| `notifications` (optional) | Requested only if you enable system notifications |
| `declarativeNetRequest` + `<all_urls>` | Removes `X-Frame-Options` / `Content-Security-Policy` headers **only** for frames initiated by the extension, so the Browser app can display sites in an iframe |
| `sandbox.html` page | Runs code and HTML previews in an isolated origin, with no access to the extension |

The network rule applies only to windows inside HeliumOS, not to the rest of browsing.

## Project structure

```
manifest.json     extension configuration (MV3)
newtab.html       desktop page
os.css            styles (Flat theme + Aero theme)
os.js             system: windows, files, apps, Start
background.js     network rule for iframes
sandbox.html/js   isolated environment for “Run” in the code editor
```

## Known limitations

- Some sites may refuse to load in the Browser app even with the rule above.
- No real processes, network or access to the computer’s files outside import/export.
- The password does not encrypt files: anyone with access to the browser profile can bypass it. If you forget it, the only way is a reset.

## Ideas for the future

- Minesweeper and Solitaire
- Minesweeper
- Different wallpaper on each virtual desktop
- Custom wallpaper on the lock screen

## Contributing

Issues and pull requests are welcome. The code is written in plain JavaScript, with no build step, so you can edit the files directly and press “Reload” on the extension.

## License

Choose a license (for example MIT) and add it in a `LICENSE` file.
