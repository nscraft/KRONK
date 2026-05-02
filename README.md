# KRONK-up

> Local-first task time tracker with ClickUp sync.

KRONK-up is a self-contained, single-file HTML app — no build step, no dependencies, no server. Track time locally in your browser, then push entries directly to ClickUp tasks when you're ready.

It is a superset of **KRONK**, the base time tracker. Both files live in this repo; KRONK is standalone, KRONK-up adds the ClickUp layer on top.

---

## Features

### Core (KRONK)
- **Task log** — add named tasks with an optional Parent Task field
- **Start / Stop timer** — one active task at a time; switching auto-stops the previous
- **Multiple time entries per task** — each start/stop cycle creates a new entry
- **Inline time editing** — click any Start or End time on a completed entry to correct it
- **Parent Task suggestions** — dropdown auto-populates from existing task and parent names; free-text also accepted
- **Live duration counter** — running entries tick in real time
- **Export** — download all data as CSV or JSON to your local Downloads folder
- **Speech-to-text** — click the microphone to dictate a task name (Chrome)
- **Zero persistence by default** — data lives in memory; export before closing

### ClickUp Integration (KRONK-up)
- **API key auth** — connect with a personal ClickUp API token; no OAuth, no backend
- **Workspace → Space → List selector** — choose exactly which list to work against
- **Link tasks** — search your ClickUp list by name and link to an existing task, or create a new one directly from KRONK-up
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
- Your ClickUp API key is stored in your browser's `localStorage` under the key `kronkup_cu`
- Task data (names, times, entries) lives only in browser memory and is cleared on page close unless exported
- To remove stored credentials: open DevTools → Application → Local Storage → delete `kronkup_cu`

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Enter` in task name field | Add task |
| `Enter` in API key field | Connect to ClickUp |

---

## Browser Compatibility

| Feature | Requirement |
|---|---|
| Core time tracking | Any modern browser |
| Speech-to-text | Chrome / Edge (Web Speech API) |
| ClickUp sync | Any modern browser (CORS supported by ClickUp API) |

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
| `POST` | `/task/{id}/time` | Log a time entry |

---

## License

MIT
