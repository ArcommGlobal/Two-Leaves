# Two Leaves

A lightweight, local-only chat UI for [Ollama](https://ollama.com). Single HTML file, no install, no server, no data ever leaves your machine. Full markdown rendering, conversation history, and an optional "Smart Speed Mode" that automatically routes short messages to a smaller/faster model.

![status](https://img.shields.io/badge/status-personal%20project-informational)

## Features

- 🪶 **Single file** — just `ollama-chat.html`. No build step, no dependencies to install.
- 💬 **Full markdown rendering** — code blocks, lists, links, bold/italic all render properly (unlike the raw terminal).
- 🧠 **Collapsible reasoning** — if you use a model with `<think>` output (e.g. Qwen3), it renders as a collapsible "Reasoning" section instead of dumping inline.
- 🕑 **Conversation history** — chats autosave locally in your browser and can be reopened, renamed by their first message, or deleted.
- 📋 **Export** — right-click any saved chat to copy it to your clipboard as markdown.
- ⚡ **Smart Speed Mode** — optionally routes short messages (≤12 words) to a smaller/faster model automatically, falling back to your main model for longer prompts.
- 🎨 **Fully local theming** — colors and background are plain CSS/HTML, easy to re-skin.
- 🔒 **100% local** — talks directly to your Ollama server over `localhost`. Nothing is sent anywhere else.

## Requirements

- [Ollama](https://ollama.com) installed and running, with at least one model pulled (e.g. `ollama pull qwen3:8b`).
- A modern browser (Edge or Chrome recommended — the app uses some newer CSS features like `color-mix()`).

## Setup

### 1. Download `ollama-chat.html`

Grab it from this repo and save it anywhere on your computer (Desktop is fine).

### 2. Allow the page to talk to Ollama (one-time setup)

By default, Ollama only accepts requests from a small set of trusted origins, and opening an HTML file directly from disk doesn't always match that list. You need to explicitly allow it.

**Windows (PowerShell):**
```powershell
setx OLLAMA_ORIGINS "*"
```
Then fully quit Ollama (right-click its tray icon → Quit — closing the window isn't enough) and relaunch it so the new setting takes effect.

**macOS:**
```bash
launchctl setenv OLLAMA_ORIGINS "*"
```
Then quit and relaunch the Ollama app.

**Linux (systemd):**
```bash
systemctl edit ollama.service
```
Add:
```
[Service]
Environment="OLLAMA_ORIGINS=*"
```
Then:
```bash
systemctl daemon-reload
systemctl restart ollama
```

> Setting `OLLAMA_ORIGINS=*` allows any local page to talk to your Ollama server. This is safe for normal local use, but be aware of it if you're on a shared machine.

### 3. Open it

Double-click `ollama-chat.html`, or drag it into your browser. The sidebar's status dot should turn green within a second or two. Pick a model from the dropdown and start chatting.

## Making it feel like a native app (optional, Windows)

You can create a desktop shortcut that opens the page in a chromeless browser window (no address bar/tabs), so it behaves like a standalone app:

```powershell
$htmlPath = "C:\path\to\ollama-chat.html"
$edge = "$env:ProgramFiles (x86)\Microsoft\Edge\Application\msedge.exe"
if(-not (Test-Path $edge)){ $edge = "$env:ProgramFiles\Microsoft\Edge\Application\msedge.exe" }

$shortcutPath = "$env:USERPROFILE\Desktop\Two Leaves.lnk"
$ws = New-Object -ComObject WScript.Shell
$shortcut = $ws.CreateShortcut($shortcutPath)
$shortcut.TargetPath = $edge
$shortcut.Arguments = "--app=file:///$($htmlPath -replace '\\','/')"
$shortcut.Save()
```

Run it, then right-click the resulting shortcut → **Pin to taskbar**.

## New models

The model list is fetched when the page loads. If you pull a new model with Ollama while the page is already open, click **"Refresh models"** in the sidebar (under the ⌃ advanced panel) to pick it up.

## Troubleshooting

**Status dot stays red / "Failed to fetch"**
- Confirm Ollama is actually running: run `ollama list` in a terminal. If that fails, Ollama itself isn't running.
- Confirm `OLLAMA_ORIGINS` is set (see Setup step 2) — it can be reset by an Ollama update, so re-check it after updating.
- Make sure you fully quit and relaunched Ollama after setting the environment variable — just closing the window isn't enough on Windows.

**A model you know you have isn't in the dropdown**
- Click "Refresh models" in the sidebar's advanced panel.

## Customizing

Everything — colors, background image, app name, icon — lives as plain CSS variables and inline styles at the top of `ollama-chat.html`. No build tooling required to re-skin it.

## License

MIT — see [LICENSE](LICENSE).
