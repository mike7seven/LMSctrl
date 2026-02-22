# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

LMSctrl is a standalone, single-file HTML dashboard for managing and testing models served by LM Studio. It has zero dependencies, no build step, and no package manager — the entire application lives in `lms-dashboard_2.html`.

## Development

**Run locally:** Open `lms-dashboard_2.html` directly in a browser, or serve it:
```
npx serve .
```

There is no build system, test runner, or linter. Changes are made directly to the HTML file and tested by refreshing the browser.

## Architecture

The app is a single HTML file (~1160 lines) with three inline sections:

1. **`<style>`** — All CSS (theming via CSS custom properties, CSS Grid layout)
2. **HTML markup** — Static DOM structure
3. **`<script>`** — All application logic

### Script Organization

The script block is divided into labeled sections:
- `STATE` — Single global state object `S` (connection info, models, UI state)
- `UTILS` — Helper functions (`$`, `$$`, `baseUrl`, `hdrs`, `apiFetch`)
- `THEME` — Dark/light/system theme toggle, persisted to `localStorage`
- `LOGGING` — Activity log panel rendering
- `API` — `apiFetch` wrapper, API mode cycling (`lms` / `openai` / `anthropic`)
- `CONNECT / POLL` — Server connection, `setInterval`-based polling for model state
- `SIDEBAR` — Collapsible sidebar with model tree and navigation
- `MODEL RENDERING` — `renderModels()` builds model cards with load/unload/config controls
- `LOAD / UNLOAD` — Model load (with config panel) and unload via LM Studio API
- `TEST` — Quick Test chat view with SSE streaming (`readSSE`, `readAnthropicSSE`)
- `NAV / TABS` — View switching between Models, Quick Test, and Endpoints tabs
- `INIT` — Entry point, restores session, binds events, auto-connects

### State Management

All state lives in a single mutable object `S`. There is no reactivity — state changes require manually calling render functions (e.g., `renderModels()`, `renderLog()`). Persistence uses `sessionStorage` (host, token) and `localStorage` (theme).

### API Integration

The app communicates with a local LM Studio server (default `localhost:1234`). Three API modes are supported:

| Mode | Chat endpoint | SSE parser |
|------|--------------|------------|
| `lms` | `POST /api/v1/chat` | `readSSE` |
| `openai` | `POST /v1/chat/completions` | `readSSE` |
| `anthropic` | `POST /anthropic/v1/messages` | `readAnthropicSSE` |

Model listing always uses `GET /api/v1/models`. Streaming uses the Fetch Streams API with manual line-buffered SSE parsing.

### Styling

CSS custom properties define two themes (`[data-theme="dark"]` and `[data-theme="light"]`) on `<html>`. Design tokens (accent colors, fonts) are in `:root`. Layout uses CSS Grid. Fonts: JetBrains Mono (body) and Syne (headings) from Google Fonts.
