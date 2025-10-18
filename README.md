# bgsh - background shell

Give Codex (and other coding agents) the ability to run long-running commands in the background and inspect their logs on demand.

---

## Installation

**Step 1:** Install globally
```bash
npm i -g bgsh
```

**Step 2:** Add to your `~/.codex/AGENTS.md` file:


```
## bgsh - background shell

Use the `bgsh` command to run any shell command in the background and inspect its logs on demand.

Usage:
- `bgsh run <cmd>` — runs `<cmd>` in a detached background process; returns a bgsh session id immediately.
- `bgsh status` — list all active bgsh sessions.
- `bgsh logs <session-id>` — print the full log for a session.
- `bgsh kill <session-id>` — terminate a running background session.
You can pipe the logs to standard tools like tail, grep, etc.

Eg: `bgsh logs 3493 | tail -n 100`
```
---

## Requirements
- Python 3.7 or higher (usually pre-installed on macOS/Linux)
- macOS or Linux (Windows not currently supported)
---

## Usage

### 1. Run a command in the background
```bash
bgsh run pnpm dev
```

Example output:
```
npm install is running in the background with session-id a4f2.
Use `bgsh logs a4f2` to read the logs.
```

### 2. View logs for a session

```bash
bgsh logs a4f2
```

Example output:
```
$ vite

  ROLLDOWN-VITE v7.1.14  ready in 158 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
```

This streams the complete log output for the specified session.

### 3. List active sessions

```bash
bgsh status
```

Example output:
```
SESSION  PID    STARTED (UTC)         COMMAND
a4f2     12345  2025-01-15T10:30:00Z  npm run dev
b8e9     12346  2025-01-15T10:31:00Z  python train_model.py
```

### 4. Kill a background session

```bash
bgsh kill a4f2
```

This sends a SIGTERM signal to the process and marks the session as stopped.

---

## Storage

By default, bgsh stores session metadata and logs in `~/.bgsh/`:
- Session metadata: `~/.bgsh/sessions/`
- Logs: `~/.bgsh/logs/`

You can customize the storage location by setting the `BGSH_HOME` environment variable:

```bash
export BGSH_HOME=~/.config/bgsh  # or any custom path
```

---

## Use Cases

- Giving codex the ability to start/stop dev servers in the background on demand
- Any command you want to run without keeping a terminal open