# LMSctrl

A standalone, single-file dashboard for managing and testing models served by [LM Studio](https://lmstudio.ai). Zero dependencies, no build step, no package manager.

## Features

- **Model Management** — Browse all downloaded models with metadata (architecture, quantization, size, capabilities). Load and unload models with configurable context length, flash attention, KV cache offloading, and eval batch size.
- **Quick Test** — Send prompts to loaded models with streaming responses, adjustable temperature and max tokens, optional system prompts, and real-time performance stats (TTFT, tokens/s, total time).
- **Three API Modes** — Switch between LM Studio native (`/api/v1`), OpenAI-compatible (`/v1`), and Anthropic-compatible (`/anthropic/v1`) endpoints.
- **Endpoint Reference** — Built-in reference for all LM Studio API endpoints.
- **Dark / Light / System Theme** — Persisted to localStorage.
- **Launch Button** — Open LM Studio directly from the dashboard via the `lmstudio://` URL scheme.

## Getting Started

### Prerequisites

- [LM Studio](https://lmstudio.ai) installed and running with the local server enabled (default: `localhost:1234`)

### Run

Open the HTML file directly in a browser:

```
open lms-dashboard_2.html
```

That's it. No server, no install, no build.

> **CORS note:** If the browser blocks API calls from a `file://` URL, either enable CORS in LM Studio (`lms server start --cors`) or serve the file with any static server (e.g. `npx serve .`, `python3 -m http.server`).

### LM Studio CLI

If you have the `lms` CLI installed, you can manage the server from your terminal:

```
lms server start              # start on default port
lms server start --cors       # enable CORS for browser access
lms server status             # check if server is running
```

## API Modes

| Mode | Chat Endpoint | Format |
|------|--------------|--------|
| **LMS Native** | `POST /api/v1/chat` | `{ input, model, temperature, max_output_tokens }` |
| **OpenAI** | `POST /v1/chat/completions` | `{ messages, model, temperature, max_tokens }` |
| **Anthropic** | `POST /anthropic/v1/messages` | `{ messages, model, temperature, max_tokens }` |

Click the API badge in Quick Test to cycle between modes.

## Project Structure

The entire application is a single HTML file with three inline sections:

```
lms-dashboard_2.html
  ├── <style>   — CSS (theming via custom properties, CSS Grid layout)
  ├── <html>    — Static DOM structure
  └── <script>  — Application logic (state, API, rendering, streaming)
```

## License

MIT — with a restriction on resale. See [LICENSE](LICENSE) for details.
