# Atlaso — remote MCP server

**Atlaso is an AI memory layer: one shared, persistent memory across every AI tool you connect.**

Tell it once in one tool, and it's already there in the next. Atlaso recalls the
context that matters before each turn and captures the durable facts after — your
decisions, your preferences, and where you left off.

This repository documents Atlaso's **hosted MCP server**. Atlaso is a managed
service, so there is nothing to self-host and no server code here — you connect
to the endpoint below and authorize in your browser.

- **Endpoint:** `https://mcp.atlaso.ai/mcp`
- **Transport:** Streamable HTTP
- **Auth:** OAuth 2.1 with dynamic client registration — your MCP client walks you
  through it; there are no API keys to paste
- **Website:** https://www.atlaso.ai
- **Docs:** https://docs.atlaso.ai

## Why this exists
Claude Desktop, Zed, Cline and other MCP clients have no memory of their own that reaches your coding tools. This hosted server gives any MCP client the same memory your coding tools share: decisions made in long chats, research and notes, preferences that should follow you. Memory works when the model calls the tools (remember, recall, forget); a skill can make that routine. Add `https://mcp.atlaso.ai/mcp` as a connector and sign in. [Claude Desktop](https://www.atlaso.ai/for/claude-desktop) · [Any MCP tool](https://docs.atlaso.ai/tools/custom-mcp)

## Connect

Any MCP client that supports remote servers can connect. Add
`https://mcp.atlaso.ai/mcp` as a connector and authorize in the browser when
prompted.

**Claude Desktop / claude.ai** — Settings → Connectors → Add custom connector →
paste the endpoint → Connect → authorize.

**Cursor, VS Code, and other MCP clients** — add it as a remote MCP server:

```json
{
  "mcpServers": {
    "atlaso": {
      "url": "https://mcp.atlaso.ai/mcp"
    }
  }
}
```

**Terminal-based tools** (Claude Code, Codex, OpenCode, Antigravity) get a richer
integration than MCP alone — automatic recall before each turn and automatic
capture after, via hooks. One command sets all of them up:

```bash
curl -fsSL https://atlaso.ai/install.sh | bash    # macOS / Linux
irm https://atlaso.ai/install.ps1 | iex           # Windows
```

That installs the `atlaso` CLI, links the machine (creating your account if you
don't have one), then asks which of your AI tools should share the memory.

## Tools

| Tool | What it does |
| --- | --- |
| `recall(query, limit=5)` | Search your memory for notes relevant to `query`. Returns a ranked list of `{id, content}`. Read-only. |
| `remember(text, polarity)` | Save a durable note — a decision, preference, or gotcha. `polarity` is one of `positive`, `open`, `cautionary`, `negative`. Returns the new id. |
| `recent(limit=10)` | List the most recent memories, newest first. Read-only. |
| `forget(id)` | Permanently delete a memory by id. Destructive and not undoable. |
| `status()` | Connection state, how many memories are stored and pending, and your memory health score. Read-only. |

Every recall carries a verdict on whether a memory is settled or contested, so
the model isn't acting on shaky context.

## Memory scopes

Memories are either **personal** (they follow you everywhere) or **project**
(scoped to the repository or workspace you were in). Both are recalled
automatically in the right place — you don't manage scopes by hand.

## Privacy

Secrets are scrubbed before anything is stored. Data is encrypted in transit and
at rest. Your memory is never used to train AI models and never sold. On the free
plan it is never sent to any LLM at all. You can delete any single memory, or your
whole account, at any time.

## Pricing

Free for one device and one tool. Pro is **$10/month** for unlimited devices and
tools sharing one memory, plus Ambient Memory, background enrichment, Ask your
memory, and JSON export. Build is **$25/month** and adds a developer memory API.

## Registry

Listed in the official MCP Registry as `io.github.imashishkh21/atlaso`. The
[`server.json`](./server.json) in this repository is the published manifest.

---

Atlaso is a product of Atlaso Labs Inc. This repository is documentation for the
hosted service; the service itself is not open source.
