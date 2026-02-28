<div align="center">

# Claude Inspector

**See what Claude Code actually sends to the API.**

MITM proxy that intercepts Claude Code CLI traffic in real-time<br>
and visualizes all 5 prompt augmentation mechanisms.

[Getting Started](#getting-started) · [Proxy Mode](#-proxy-mode) · [How It Works](#how-it-works)

</div>

---

<p align="center">
  <img src="public/screenshots/proxy-request.png" width="100%" alt="Proxy — Request view with mechanism filter chips and search" />
</p>

<p align="center">
  <img src="public/screenshots/proxy-analysis.png" width="100%" alt="Proxy — Analysis view auto-detecting all 5 mechanisms" />
</p>

## Getting Started

```bash
git clone https://github.com/kangraemin/claude-inspector.git
cd claude-inspector
npm install
npm start
```

## Proxy Mode

Intercept **real** Claude Code CLI traffic via a local MITM proxy.

```
Claude Code CLI  →  Inspector (localhost:9090)  →  api.anthropic.com
```

**1.** Click **Start Proxy** in the app<br>
**2.** Run Claude Code through the proxy:

```bash
ANTHROPIC_BASE_URL=http://localhost:9090 claude
```

**3.** Every API request/response is captured in real-time.

### What you can do

| Tab | Description |
|-----|-------------|
| **Messages** | Browse `messages[]` by role — filter by user/assistant/system, full-text search (`Cmd+F`) |
| **Request** | Raw request JSON with collapsible tree, mechanism filter chips (CLAUDE.md, Slash Cmd, Skill...) |
| **Response** | Full response including SSE stream auto-reassembly |
| **Analysis** | Auto-detects which of the 5 mechanisms are present and explains each one |

## How It Works

Claude Code enhances every API call with up to **5 prompt augmentation mechanisms** — but these are invisible during normal usage.

| Mechanism | Injection Point |
|-----------|----------------|
| **CLAUDE.md** | `messages[].content` → `<system-reminder>` |
| **Output Style** | `system[]` additional block |
| **Slash Command** | `messages[].content` → `<command-message>` |
| **Skill** | `tool_result` after Skill `tool_use` |
| **Sub-Agent** | Separate isolated API call via Task tool |

Claude Inspector sits between Claude Code and the Anthropic API, capturing the full request/response payload — so you can see exactly what gets injected and where.

> **Privacy**: All traffic stays on your machine. The proxy runs on `localhost` only. No data is sent anywhere except directly to `api.anthropic.com`.

## Tech Stack

- **Electron** — cross-platform desktop (macOS `hiddenInset` titlebar)
- **Vanilla JS** — zero frameworks, zero build steps
- **Node `http`/`https`** — lightweight MITM proxy with SSE stream reassembly
- **highlight.js** + **marked** — syntax highlighting & markdown rendering

## Build

```bash
npm run dist         # .dmg + .exe
npm run dist:mac     # macOS only (arm64 + x64)
npm run dist:win     # Windows only (NSIS)
```

## Related

Built on top of the research from [Reverse Engineering Claude Code — How Skills Different from Agents, Commands, and Styles](https://).

## License

MIT
