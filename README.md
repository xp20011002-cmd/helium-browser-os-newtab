# HeliumOS

A browser extension (Manifest V3) that turns the **new tab** into a small operating system with an **Aero (Windows 7)** theme: desktop, windows, file system, terminal, code editor, image viewer, audio/video player and much more.

Built for [Helium](https://github.com/imputnet/helium), but it works in any Chromium-based browser (Chrome, Edge, Brave, etc.). No frameworks, no dependencies: just HTML, CSS and JavaScript.

> **Status:** experimental (v0.13). This is not a real OS: it runs inside the browser sandbox.

<!-- Add screenshots here: docs/screenshot-desktop.png, docs/screenshot-files.png -->

## Features

**Desktop and windows**
- Windows that can be moved, resized, minimized, maximized and closed, with animations
- Aero Snap: drag a window to the left/right edge (half screen) or to the top (maximized)
- Taskbar with buttons grouped by app, pinned apps, hover previews, calendar and "Show desktop"
- Two-column Start menu with search for programs and files
- Movable desktop icons: drag them on a grid, select several with a rectangle or Ctrl+click, drag files from the desktop onto the Recycle Bin or a folder
- Startup animation, login screen with optional password, and lock screen
- **Files:** multi-select, copy, cut, paste and **undo / redo** for file operations
- **Real folders from your computer** mounted in Files (File System Access API) and an **encrypted Vault** (AES-256) for passwords, notes and files
- **Global search** (Ctrl+K or Alt+S): apps, windows, commands, settings, files (names and contents), notifications, calculations and the web, all in a single box
- **Custom apps:** write an HTML file and install it as a system app, with the `helium` API and permissions
- "Run" (Alt+R), window switching with Alt+` and `.lnk` shortcuts on the desktop
- Virtual desktops (up to 6) with Task View (the ▦ button next to Start)
- Notifications: balloons on the desktop, a notification center (🔔) with history, "Do not disturb", optional sound and reminders
- Context menus (right-click) on the desktop, in Files and on the taskbar

**Included apps**

| App | What it does |
|---|---|
| Files | Explorer with navigation pane, back/forward, search, sortable columns, drag & drop, import/export |
| Notepad | Simple text editor (Ctrl+S) |
| Code | Editor with syntax highlighting (JS, CSS, HTML, Python), runs `.js` and previews `.html` in an isolated sandbox |
| Terminal | `ls`, `cd`, `cat`, `echo > file`, `mkdir`, `rm`, `mv`, `cp`, `open`, `edit` and more (`help`) |
| Images | Browse between images, zoom, "Set as background", "Edit in Paint" |
| Paint | Pencil, brush, eraser, spray, line, rectangle, ellipse, fill, color picker, text; undo/redo, color palettes, save PNG/JPG to `/Pictures` |
| Solitaire | Klondike with draw 1 or 3, mouse dragging, double-click to foundations, undo, auto-complete, score, timer and statistics |
| Recycle Bin | Deleted items are moved here; restore to the original location or delete permanently |
| Task Manager | List of windows (with "Close window") and a performance graph |
| Music | Audio and video with playlist and spectrum visualizer |
| Browser | Web pages in an iframe |
| Calculator | Simple calculator |
| Vault | Passwords, notes and files encrypted with your password (AES-256-GCM), password generator, auto-lock |
| Custom apps | HTML/JS files installed as apps, run in isolation in a system window (see below) |
| Settings | Theme, background, icons, custom apps, account and password, notifications, backups |

**Customization**
- Custom background (fill, fit, stretch, tile, center) or from your own images
- Custom icons for each app (image or emoji), on the desktop and in the taskbar
- Window color, transparency (Aero glass) or the "Flat" theme, user name and picture
- User password (PBKDF2 hash, ordinary access control only, not encryption) and configurable startup animation

## Files: multi-select, copy, paste, undo

- **Multi-select:** Ctrl+click adds or removes an item, Shift+click selects a range, Ctrl+A selects everything, and the arrow keys (with Shift) extend the selection. The status bar shows how many items you selected and how much space they take.
- **Copy / Cut / Paste:** Ctrl+C, Ctrl+X, Ctrl+V, the command bar buttons, or right-click. Cut items appear faded. When pasting, if the name already exists, " - copy", " - copy (2)" is added automatically (or " (2)" when moving). A folder cannot be pasted into itself.
- **Drag:** you can drag several items at once onto a folder, onto the Recycle Bin or onto a folder on the desktop.
- **Delete:** Delete (or right-click) moves all selected items to the Recycle Bin, with a single confirmation.
- **Undo / Redo:** Ctrl+Z and Ctrl+Y (or the ↶ ↷ buttons, or the commands in global search) for: copy, paste, move, drag, move to Recycle Bin, creating files and folders, rename and import. Keeps the last 50 operations. Permanent deletions (from the Recycle Bin or from a mounted disk) cannot be undone. A notification after deletion also has an "Undo" button.
- **On the desktop:** select file icons (with Ctrl+click or a rectangle) and use the same keys: Ctrl+C, Ctrl+X, Ctrl+V (pastes onto the desktop), Delete, Ctrl+Z and Ctrl+Y.
- **With mounted disks:** pasting onto a mounted disk writes the real files, and undoing deletes them from the disk. Copying between a disk and the rest of the system works in both directions; "Cut" between different worlds copies, and the original stays.

## Real folders from your computer (mounted disks)

In Files, in the left pane under "Disks", click **＋ Mount folder…** (or Settings → System). Pick a folder on your computer and mount it with **read and write** or **read-only** access. It appears under `/Mount/<name>`, and its files are the real ones:

- what you save there (Notepad, Code, Paint, import, creating files and folders, renaming files) is written to disk;
- files are read on demand, so a large folder does not fill up memory; images, music and text files open as usual;
- **deletion is permanent** (it does not go to the Recycle Bin) and requires a separate confirmation;
- copying between a disk and the rest of the system works in both directions (by dragging in Files), while moving between disks is not supported: the original stays;
- moving or renaming **folders** on the disk is not supported; files can be moved and renamed within the same disk;
- after restarting the browser, the disk appears "disconnected" (🔌) until you click it: the browser asks for permission again;
- the file list is read when mounting (maximum 5000 items, depth 10; `node_modules` and `.git` are ignored). For changes made from outside: right-click the disk → "Reload file list";
- disk contents are not saved in the browser and are not included in backups.

Requires a Chromium browser (Chrome, Edge, Helium…). In Terminal: `mount` lists disks, and `mount add` opens the mount dialog.

## Encrypted Vault

The **Vault** app (🔐) keeps **passwords, notes and files** encrypted with a separate password.

- **Encryption:** AES-256-GCM, key derived from the vault password with PBKDF2-SHA256 (600,000 iterations, random salt), new initialization vector on every save. Any modification of the encrypted data is detected. Only the encrypted blob is kept in the browser; the password is never stored anywhere.
- **Items:** passwords (username, password, web address, notes, strength meter, opening the address in Browser), notes, and files up to ~5 MB (the vault has a maximum of ~30 MB). Right-click a file → "Add to encrypted vault…".
- **Password generator:** cryptographically random, length 8–64, uppercase and lowercase letters, digits, symbols (no ambiguous characters).
- **Safe usage:** auto-lock (1–60 min or never), lock when the window is closed and when you lock the screen, delays after wrong passwords, clipboard cleared 20 s after copying a password.
- **Backups:** "Export encrypted vault" (`.vault`) and "Import". The general backup in Settings also includes the vault, still in encrypted form.
- **Limits:** if you forget the password, the contents cannot be recovered. The vault does not protect against malware or keyloggers on your computer, and while it is open the data exists unencrypted in the page's memory. Vault contents do not appear in global search.

## Global search

Open it with **Ctrl+K**, **Alt+S**, the 🔍 button in the taskbar, "Global search" in Start, or right-click on the desktop. Results appear instantly, grouped by category:

- **Calculation:** `12*7+3`, `2^10`, `200*15%`, `(1+2)*3` give you the result directly; Enter copies it.
- **Open windows:** Enter takes you to the window, even if it is on another virtual desktop.
- **Apps:** including your own; it also understands legacy names (`cmd`, `explorer`, `mspaint`).
- **Commands:** lock the screen, new desktop, "Do not disturb", empty the Recycle Bin, change theme, new file on desktop…
- **Settings:** "password", "background", "notifications", "backup"… take you straight to the right page.
- **Files and folders:** by name, plus **in the contents** of text files, with a snippet around the word found.
- **Notifications and reminders** from history.
- **Web:** an address opens in Browser, and any other text offers "Search the web".

Search ignores case and diacritics (`fisier` finds "fișier"), accepts multiple words and fuzzy matching for short names (`cnt` → "counter"). What you choose often moves up in the ranking, and with no text you see **recents**. **Filters:** `a:` apps, `f:` files, `s:` settings, `c:` commands, `w:` windows, `n:` notifications (for example `f:report`). **Shift+Enter** on a file shows it in its folder. In Terminal there is `search <text>`.

## Custom apps

An app is an **HTML** file (with CSS and JavaScript) that runs in a system window, in a sandbox isolated from everything else. It can be installed so it appears on the desktop, in Start and in the taskbar.

**How to make one**
1. In the **Code** app click "App template" (or Settings → Apps → "New app") and write the code.
2. **▶ Run** tries it out, with access to `helium`, like a real app.
3. **📦 Install…** lets you choose the name, icon, dimensions, permissions and where it appears.
4. From Files, right-click an `.html` → "Open as app" (without installing) or "Install as app…".

`.js`, `.css` and image files from the same folder are included automatically if you reference them with a relative path. Libraries from the internet cannot be loaded: paste their code into the folder. Settings → Apps lists the installed apps (Open, Edit, Properties, Uninstall).

**The `helium` API** (all functions return promises):

| Function | What it does |
|---|---|
| `helium.notify(title, message)` | System notification |
| `helium.storage.get/set/remove/keys` | Own persistent storage (maximum 1 MB) |
| `helium.files.read/write/list/remove` | Files in the app's own folder `/Apps/<id>` |
| `helium.fs.read/write/list/remove` | The whole file system (asks for your confirmation) |
| `helium.run(command)` | Opens an app, file or address, like "Run" (requires access to the whole system) |
| `helium.system()` | `{ version, accent, theme, user, app }` |
| `helium.window.setTitle/resize/close/focus` | Control of the app's window |

**Safety:** apps reach the rest of the system only through `helium`, and permissions are checked by the system, not by the app. By default they have no internet access (a CSP policy blocks `fetch`, external images and fonts) and can only see their own folder. Internet access and access to the whole file system are granted explicitly, from Properties. Only install code you understand.

New users get the "Counter" and "Notes" examples and a guide (`Aplicatii-proprii.md`) in `/Documents`. Existing users can add them from Settings → Apps → "Add examples".

## Run, window switching and shortcuts

**Keys** (Win+R and Alt+Tab are intercepted by the operating system and the browser, so HeliumOS uses alternatives):

| Keys | Action |
|---|---|
| **Ctrl+K** or **Alt+S** | **Global search** |
| **Alt+R** (or Meta+R, if it reaches the page) | **Run** dialog |
| **Alt+`** or **Alt+Q** (while holding Alt) | Switch between windows, like Alt+Tab; Shift reverses direction |
| **Alt+Shift+←/→** | Switch virtual desktop |
| **Alt+Shift+↑** | Task View |

- **Run** understands app names (`paint`, `calculator`, `cmd`, `explorer`…), commands (`lock`, `restart`), paths (`/Documents`, `hello.js`), web addresses (`example.com`) and desktop shortcuts. It remembers the last 15 commands. It is also available in Start, in the taskbar menu and in Terminal (`run …`).
- **Window switching** shows the windows of the current desktop in order of use, including minimized ones, and previews the chosen window (the others become translucent). A quick press returns to the previous window.
- **Shortcuts (`.lnk`)** are small files that point to an app, a file or folder, or a web address. They are created from right-click (on an app or a file → "Create shortcut"), from "New shortcut…" (desktop or Files), from the 🔗 button in Browser, or from Terminal (`lnk <target> [name]`). They have their own icon with an arrow, can have their icon changed, and the `.lnk` extension is not shown. If the target disappears, you get a notification with the option to delete the shortcut.

## Virtual desktops

- The ▦ button in the taskbar (or Alt+Shift+↑) opens **Task View**: thumbnails of the desktops, the windows of the current desktop and "+ New desktop" (maximum 6).
- Click a desktop to activate it. Double-click its name to rename it, and ✕ removes it (its windows move to the neighboring desktop).
- Move a window by dragging it onto a desktop or from right-click on its title bar.
- Keys: **Alt+Shift+←/→** or **Ctrl+Alt+←/→** switch between desktops, and **1–6** change desktop while Task View is open. Some systems reserve these combinations.
- Terminal: `desk` (list), `desk 2` (switch), `desk new` (create).
- **Different background on each desktop:** enable "Different background on each virtual desktop" in Settings → Personalization or choose "Change background…" from right-click on a desktop in Task View. A desktop without its own background uses the shared one. The background changes with a smooth transition when you switch desktops, and the thumbnails in Task View show it.
- Desktop icons are shared across all desktops, and the taskbar shows the windows of the current desktop.

## Notifications

- They appear as balloons in the bottom right and are kept in the notification center (🔔 in the taskbar).
- Events that generate notifications: imports, move to Recycle Bin (with an "Undo" button), images saved from Paint, password change, low-space warning and backup reminder.
- **Reminders:** from the 🔔 panel or from Terminal: `remind 10m take the bread out`. They ring while HeliumOS is open in a tab; missed ones appear the next time it is opened.
- **From code:** in the Code app, a `.js` file can call `notify("Title", "Message")`.
- **System notifications** (optional): in Settings → Notifications, with the optional "notifications" permission, they also appear when the tab is not in front.

## Installation

1. Download or clone the repository.
2. Open the extensions page (`helium://extensions` or `chrome://extensions`).
3. Enable **Developer mode**.
4. Click **Load unpacked** and choose the project folder.
5. Open a new tab.

## How files are used

- Files are stored locally, in the browser (**IndexedDB**). Nothing is sent over the internet.
- You can drag files from your computer onto the desktop (they end up in `/Desktop`) or into a folder in the Files app.
- The limit is 20 MB per file. Large files make saving slower.
- If you clear the browser's data, the files disappear. Use **Settings → System → Export** for backups.

## Permissions

| Permission | Reason |
|---|---|
| `chrome_url_overrides.newtab` | Replaces the new tab page |
| `storage` | Remembers, per browser session, whether you are logged in and whether the startup animation has run |
| `notifications` (optional) | Requested only if you enable system notifications |
| `declarativeNetRequest` + `<all_urls>` | Removes the `X-Frame-Options` / `Content-Security-Policy` headers **only** for frames initiated by the extension, so the Browser app can display sites in an iframe |
| `sandbox.html` page | Runs code and HTML previews in an isolated origin, with no access to the extension |

The network rule applies only to HeliumOS windows, not to the rest of your browsing.

## Project structure

```
manifest.json     extension configuration (MV3)
newtab.html       the desktop page
os.css            styles (Flat theme + Aero theme)
os.js             the system: windows, files, apps, Start
background.js     network rule for iframes
sandbox.html/js   isolated environment for "Run" in the code editor
```

## Known limitations

- Some sites may refuse to load in the Browser app even with the rule above.
- It has no real processes, no networking and no access to your computer's files beyond import/export.
- The password does not encrypt files: anyone with access to the browser profile can bypass it. If you forget it, the only way out is a reset.

## Future ideas

- Sticky notes
- Minesweeper
- Custom wallpaper on the lock screen

## Contributing

Issues and pull requests are welcome. The code is written in plain JavaScript with no build step, so you can edit the files directly and press "Reload" on the extension.

## License

Choose a license (for example MIT) and add it in a `LICENSE` file.
