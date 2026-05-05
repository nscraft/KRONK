# KRONK-up

> Local-first task time tracker with ClickUp sync.

KRONK-up is a self-contained, single-file HTML app — no build step, no dependencies, no server. Track time locally in your browser, then push entries directly to ClickUp tasks when you're ready.

It is a superset of **KRONK**, the base time tracker. Both files live in this repo; KRONK is standalone, KRONK-up adds the ClickUp layer on top.

---

## Features

### Core (KRONK)
- **Task log** — add named tasks with an optional Parent Task field
- **Task typeahead** — search existing task names as you type; press `Tab` to start the top match
- **Auto-start on create** — creating a task immediately starts its timer; any previously running timer is stopped automatically
- **Start / Stop timer** — one active task at a time; switching auto-stops the previous
- **Sticky STOP button** — a STOP button appears in the header whenever a timer is running, so you can stop from anywhere without scrolling
- **Multiple time entries per task** — each start/stop cycle creates a new entry
- **Inline time editing** — click any Start or End time on a completed entry to correct it
- **Parent Task suggestions** — dropdown auto-populates from existing task and parent names; free-text also accepted
- **Live duration counter** — running entries tick in real time
- **Auto-stop on tab close** — if you close the tab, any running timer is stopped and saved automatically; minimizing or switching windows keeps the timer running
- **Export** — download all data as CSV or JSON to your local Downloads folder
- **Speech-to-text** — click the microphone to dictate a task name; the task is created and timed automatically when you finish speaking (Chrome)
- **Persistent data** — tasks, entries, and times are saved to `localStorage` and restored on next open

### ClickUp Integration (KRONK-up)
- **API key auth** — connect with a personal ClickUp API token; no OAuth, no backend
- **Workspace → Space → List selector** — choose exactly which list to work against
- **Link tasks** — search your ClickUp list by name and link to an existing task, or create a new one directly from KRONK-up
- **Parent task picker** — set a ClickUp parent task per row; linked tasks are created as subtasks automatically
- **Push time entries** — send individual task entries or all linked tasks at once to ClickUp's time tracking
- **Sync indicators** — each entry shows whether it has been pushed (✓) or is pending (○)
- **Re-push on edit** — editing a pushed entry's time marks it unsynced so it can be corrected in ClickUp
- **Persistent config** — API key, selected Space, and List are saved in `localStorage` and restored on next open
- **Auto-reconnect** — if a key was saved, KRONK-up reconnects silently on load
- **Enhanced export** — CSV and JSON include ClickUp task name and per-entry sync status

---

## Getting Started

KRONK-up is a single HTML file. No installation required.

```
git clone https://github.com/your-username/kronk-up.git
cd kronk-up
```

Open `KRONK-up.html` in a browser. That's it.

> **Note:** The ClickUp API is called directly from the browser. Because ClickUp's API supports CORS for personal tokens, no proxy or server is needed.

---

## ClickUp Setup

### 1. Get your personal API token

1. Log in to [app.clickup.com](https://app.clickup.com)
2. Click your avatar → **Settings**
3. Navigate to **Apps**
4. Copy your **Personal API Token** (starts with `pk_`)

### 2. Connect in KRONK-up

1. Open `KRONK-up.html`
2. Paste your token into the **API Key** field in the ClickUp config bar
3. Click **Connect** — your workspace name will appear in the status badge
4. Select a **Space**, then a **List** from the dropdowns

### 3. Link and push

| Action | How |
|---|---|
| Link a task to ClickUp | Click **Link** in the ClickUp column → search your list → select a task |
| Create a new ClickUp task | Click **Link** → **Create new ClickUp task** |
| Push time entries | Click **↑N** on a linked task, or **Push All** in the header |
| Unlink a task | Hover the task row → click **✕** in the ClickUp column |

---

## File Structure

```
kronk-up/
├── KRONK.html        # Standalone time tracker (no ClickUp)
├── KRONK-up.html     # KRONK + ClickUp integration
└── README.md
```

Both files are fully self-contained. They share no external scripts or stylesheets beyond the Google Fonts CDN link for IBM Plex Mono.

---

## Data & Privacy

- **No data is sent anywhere except ClickUp** — and only when you explicitly click Push
- Task data (names, times, entries) is saved in `localStorage` and survives page close and refresh
- Your ClickUp API key and workspace config are stored separately under the key `kronkup_cu`

| App | localStorage key | Contents |
|---|---|---|
| KRONK | `kronk_tasks` | All tasks and time entries |
| KRONK-up | `kronkup_tasks` | All tasks and time entries |
| KRONK-up | `kronkup_cu` | API key, workspace, space, and list selection |

To clear all stored data: open DevTools → Application → Local Storage → delete the relevant keys.

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Enter` in task name field | Create a custom task and start its timer |
| `Tab` in task name field | Start the top matching typeahead task |
| `Enter` in API key field | Connect to ClickUp |

---

## Browser Compatibility

| Feature | Requirement |
|---|---|
| Core time tracking | Any modern browser |
| Speech-to-text | Chrome / Edge (Web Speech API) |
| ClickUp sync | Any modern browser (CORS supported by ClickUp API) |

---

## Microphone Permissions

Modern browsers do not persist microphone permissions for files opened directly from disk (`file://` URLs). If Chrome or Edge re-prompts you for microphone access every session, serve KRONK from a local web server instead. Once running on `localhost`, the browser will remember your choice.

**Python** (3.x):
```bash
cd path/to/kronk
python -m http.server 8000
```
Open `http://localhost:8000/KRONK.html` (or `KRONK-up.html`).

**Node.js** (via npx, no install required):
```bash
npx serve path/to/kronk
```

**VS Code** — install the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension, right-click the HTML file, and choose **Open with Live Server**.

> This only affects speech-to-text. All other features work identically whether opened as a file or served from localhost.

---

## ClickUp API Reference

KRONK-up uses the following ClickUp v2 endpoints:

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `/team` | Verify API key, get workspace |
| `GET` | `/team/{id}/space` | List spaces |
| `GET` | `/space/{id}/folder` | List folders |
| `GET` | `/folder/{id}/list` | Lists within a folder |
| `GET` | `/space/{id}/list` | Folderless lists |
| `GET` | `/list/{id}/task` | Fetch tasks for linking |
| `POST` | `/list/{id}/task` | Create a new task |
| `PUT` | `/task/{id}` | Convert task to subtask |
| `POST` | `/task/{id}/time` | Log a time entry |

---

## License

MIT
