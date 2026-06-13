# 📖 NakshAstraMCP — User Guide

> **Maximize AI Developer Context**: Master client configuration, CLI control, multi-client bridges, configuration tuning, and visual graph diagnostics.

---

## 📋 Table of Contents

1. [Client Configuration](#-client-configuration)
2. [Dual Transport Bridge](#-dual-transport-connection-bridge)
3. [Automated Agent Orchestration](#-automated-agent-orchestration)
4. [MCP Tool Reference](#-mcp-tool-reference)
5. [Unified CLI Command Reference](#️-unified-cli-command-reference)
6. [Advanced CLI Flags](#-advanced-cli-flags)
7. [Configuration System (`config.toml`)](#️-configuration-system-configtoml)
8. [Environment Variables](#-environment-variables)
9. [Adding Custom Language Support](#-adding-custom-language-support-addons)
10. [Knowledge Mapping & Report Generation](#️-high-fidelity-knowledge-mapping)
11. [How the Search Engine Works](#-how-the-search-engine-works)

---

## 🚀 Client Configuration

Once NakshAstraMCP is installed, connect it to your preferred AI coding environment. The server remembers registered workspaces centrally — your IDE config only needs the `start` command.

### 1. Claude Desktop (macOS / Windows)

**Config file locations:**
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`

#### Option A: Manual JSON Configuration

```json
{
  "mcpServers": {
    "nakshastramcp": {
      "command": "nakshastramcp",
      "args": ["start"]
    }
  }
}
```

#### Option B: Claude CLI Automation

```bash
# HTTP Transport (requires background server already running):
claude mcp add --transport http nakshastramcp http://127.0.0.1:2102/mcp

# STDIO Transport (Claude manages the process lifecycle):
claude mcp add --transport stdio nakshastramcp nakshastramcp -- start --transport stdio
```

---

### 2. Cursor IDE

1. Open **Settings** → **Models** → **MCP**.
2. Click **+ Add New MCP Server**.
3. Fill in the fields:
   - **Name**: `NakshAstra`
   - **Transport**: `stdio`
   - **Command**: `nakshastramcp`
   - **Arguments**: `start`, `--transport`, `stdio`

---

### 3. Antigravity IDE

Add the following block to your `mcp_config.json`:

```json
{
  "mcpServers": {
    "nakshastramcp": {
      "command": "nakshastramcp",
      "args": ["start", "--transport", "stdio"],
      "type": "stdio",
      "disabled": false
    }
  }
}
```

---

### 4. Windsurf

Add to your Windsurf MCP configuration:

```json
{
  "mcpServers": {
    "nakshastramcp": {
      "command": "nakshastramcp",
      "args": ["start", "--transport", "stdio"]
    }
  }
}
```

---

### 5. Factory AI

#### Option A: HTTP Transport (background server required)

```json
{
  "mcpServers": {
    "nakshastramcp": {
      "type": "http",
      "url": "http://127.0.0.1:2102/mcp",
      "disabled": false
    }
  }
}
```

#### Option B: STDIO Transport

- **Server Name**: `nakshastramcp`
- **Server Type**: `stdio`
- **Command**: `nakshastramcp start --transport stdio`

---

### 6. VS Code (via HTTP Bridge)

When NakshAstraMCP is already running as a host in another IDE, connect VS Code extensions directly to the HTTP bridge — no second process required:

- **URL**: `http://127.0.0.1:2102/mcp`
- **Type**: `streamable-http`

---

## 🌉 Dual Transport Connection Bridge

NakshAstraMCP features a **Dual Transport Bridge**. When started via `stdio` inside your main IDE, the host session automatically spawns a local streamable HTTP connection on port `2102`.

```
┌─────────────────────────────┐
│   Antigravity / Cursor IDE  │  ← Host (stdio)
│   nakshastramcp start       │
└────────────┬────────────────┘
             │ spawns
             ▼
     Port 2102 HTTP Bridge
     http://127.0.0.1:2102/mcp
             │
    ┌────────┴────────┐
    ▼                 ▼
  VS Code          Factory AI
 (Follower)       (Follower)
```

**Practical Use Case**: Use Antigravity as your primary coding host and VS Code with a specialized extension simultaneously — both query the same live codebase graph without performance degradation or index contention.

**Configuring follower clients:**

| Field | Value |
| :--- | :--- |
| **Type** | `streamable-http` |
| **Bridge URL** | `http://127.0.0.1:2102/mcp` |

> [!NOTE]
> If port `2102` is already in use, start with a custom port: `nakshastramcp start --port 3000`. Then update follower clients to use `http://127.0.0.1:3000/mcp`.

---

## 🤖 Automated Agent Orchestration

Starting with v3.16.0, NakshAstraMCP simplifies onboarding for AI agents and team members:

*   **Zero-Config Onboarding**: When registering a workspace via `nakshastramcp start --workspace .`, the server automatically provisions a custom `AGENTS.md` file in your workspace root.
*   **Prompt Alignment**: The file instructs AI agents to prioritize surgical MCP tools (`read_file` with line ranges, `find_symbol`) over expensive shell grep commands.
*   **Non-Destructive Backups**: If an older `AGENTS.md` already exists, it is safely backed up to `AGENTS_Backup.md`.
*   **MCP-First Skill Profile**: Load the [🎯 MCP-First Skill Profile](mcp_first_skill.md) into your AI assistant's system prompt to enforce AST-aware, token-optimized workflows across all tasks.

---

## 🩺 MCP Tool Reference

NakshAstraMCP registers these precision tools on your AI client. Each tool has a specific purpose — use the right tool for each task to minimize token consumption:

| Tool | Priority | Best For | Parameters |
| :--- | :---: | :--- | :--- |
| **`deep_context`** | 🥇 Primary | High-level questions, architectural discovery, module-to-module relationships | `query`, `workspace_path?`, `rerank?`, `snippet_chars?`, `expand_neighbors?`, `compact_symbols?` |
| **`search_codebase`** | 🥈 Discovery | Broad keyword/grep-style search across all indexed files | `query`, `workspace_path?`, `snippet_chars?` |
| **`find_symbol`** | 🥉 Surgical | Locate exact class/function/method definition with file + line range | `name`, `workspace_path?` |
| **`read_file`** | 📖 Reader | Read a specific file or line range before any edit | `path`, `workspace_path?`, `start_line?`, `end_line?`, `if_changed_since?` |
| **`find_references`** | 🔎 Audit | Trace all call sites and usages of a custom function/class | `symbol_name`, `workspace_path?` |
| **`generate_report`** | 🗺️ Map | Trigger full architectural synthesis → `NAKSHASTRA_REPORT.md` + `graph.json` | `workspace_path?` |
| **`server_status`** | 🩺 Diagnostic | Check workspace health, memory usage, and uptime | *(none)* |

### Key Tool Parameters Explained

**`deep_context` — Advanced Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `query` | `string` | required | Natural language or keyword search query |
| `workspace_path` | `string` | all workspaces | Restrict search to a specific workspace path |
| `rerank` | `bool` | `true` | Enable FlashRank semantic reranking |
| `snippet_chars` | `int` | `200` | Snippet character window (`200` = fast scan, `1000` = deep read) |
| `expand_neighbors` | `bool` | `true` | Include 1-hop AST graph neighbors in results |
| `compact_symbols` | `bool` | `false` | Return compact text symbol list instead of structured JSON |

**`read_file` — Advanced Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `path` | `string` | required | Relative file path within the workspace |
| `start_line` | `int` | 1 | First line to read (1-indexed) |
| `end_line` | `int` | EOF | Last line to read |
| `if_changed_since` | `string` | — | SHA-256 hash of previous read; returns `{"status": "unchanged"}` if file is unchanged — saves 100% tokens on re-reads |

---

## 🛠️ Unified CLI Command Reference

Manage server lifecycle, workspaces, and environment directly from your terminal:

| Command | Description |
| :--- | :--- |
| `nakshastramcp start --workspace <path>` | Register, index, and start monitoring a codebase |
| `nakshastramcp stop` | Gracefully terminate the background server process |
| `nakshastramcp restart` | Flush sessions and restart all background transports |
| `nakshastramcp status` | View registered workspaces and server health |
| `nakshastramcp doctor` | Run 13 pre-flight environment checks |
| `nakshastramcp logs [--follow]` | Stream real-time logs to terminal (`--follow` for tail mode) |
| `nakshastramcp deregister --workspace <path>` | Stop server, remove workspace, and clean orphaned index data |
| `nakshastramcp gc` | Garbage collect orphaned index directories for deleted projects |
| `nakshastramcp ui [--workspace <path>]` | Launch the **Nebula Graph UI** for interactive visualization |
| `nakshastramcp report [<path>]` | Generate architectural report (`NAKSHASTRA_REPORT.md`) for a workspace |
| `nakshastramcp provision --lang <name> --lib <path>` | Provision a custom Tree-sitter grammar at runtime |
| `nakshastramcp test-tools` | Run in-process MCP tool diagnostics (debug EOF/transport issues) |
| `nakshastramcp --version` | Print the installed version |

---

## 🎛️ Advanced CLI Flags

### `nakshastramcp start` — Full Flag Reference

| Flag | Short | Type | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| `--workspace` | `-w` | path | — | Register and index this workspace path (can be repeated for multiple) |
| `--transport` | — | string | `streamable-http` | Transport mode: `stdio` or `streamable-http` |
| `--port` | `-p` | int | `2102` | HTTP bridge port (useful if 2102 is occupied) |
| `--host` | — | string | `127.0.0.1` | HTTP bridge bind host |
| `--config-lang` | — | string | — | Custom language extension mappings: `".go=go,.rs=rust"` or JSON `{"go": "go"}` |
| `--force` | `-f` | bool | `false` | Force start by clearing any stale lock file |

### `nakshastramcp stop` — Flags

| Flag | Short | Description |
| :--- | :--- | :--- |
| `--force` | `-f` | Remove stale lock file even if no running process is found |

### `nakshastramcp logs` — Flags

| Flag | Short | Description |
| :--- | :--- | :--- |
| `--lines` | `-n` | Number of recent log lines to display (default: 50) |
| `--follow` | `-f` | Stream logs continuously (like `tail -f`) |

---

## ⚙️ Configuration System (`config.toml`)

NakshAstraMCP uses a layered configuration system. Settings are resolved in this **precedence order** (highest to lowest):

```
CLI Flags  >  Environment Variables (NAKSH_*)  >  config.toml  >  System Defaults
```

### Config File Location (Platform-Specific)

| Platform | Path |
| :--- | :--- |
| **Windows** | `%LOCALAPPDATA%\nakshastramcp\config.toml` |
| **macOS** | `~/Library/Application Support/nakshastramcp/config.toml` |
| **Linux** | `~/.local/share/nakshastramcp/config.toml` |

### Complete `config.toml` Reference

```toml
# ── Search Settings ─────────────────────────────────────────────────────────
# Maximum time (ms) the search pipeline may spend before returning results.
search_timeout_ms = 500

# Maximum time (ms) the FlashRank semantic reranker may use.
rerank_timeout_ms = 150

# Maximum number of result records returned per search query.
snippet_limit = 10

# ── Memory Management ────────────────────────────────────────────────────────
# RAM threshold (MB) before Memory Guard triggers garbage collection.
mem_threshold_mb = 1024

# How often (seconds) the Memory Guard polls system RAM usage.
mem_poll_interval_s = 30.0

# Maximum number of simultaneously active workspace contexts (LRU eviction).
mem_lru_size = 5

# ── Machine Learning / FlashRank ─────────────────────────────────────────────
# Cross-encoder model used for semantic reranking.
model_name = "ms-marco-TinyBERT-L-2-v2"

# ── Performance ──────────────────────────────────────────────────────────────
# How long (seconds) search result caches are valid before re-querying.
search_cache_ttl_s = 300

# Filesystem watcher debounce period (ms) — prevents re-indexing during rapid saves.
debounce_ms = 500

# ── Retrieval Quality ────────────────────────────────────────────────────────
# Alpha weight in the combined relevance formula:
#   score = α × lexical_score + (1 - α) × pagerank_score
# 0.7 = favor lexical match; lower values weight architectural importance more.
scoring_alpha = 0.7

# ── Logging ──────────────────────────────────────────────────────────────────
# Log verbosity level: DEBUG, INFO, WARNING, ERROR
log_level = "INFO"

# Maximum size (bytes) of a single log file before rotation.
log_max_bytes = 5000000  # 5 MB

# Number of rotated log backups to keep.
log_backup_count = 3

# ── Transport ─────────────────────────────────────────────────────────────────
# Default transport: "streamable-http" or "stdio"
transport = "streamable-http"

# HTTP bridge port.
port = 2102

# HTTP bridge bind host (127.0.0.1 = localhost only).
host = "127.0.0.1"

# ── Custom Language Mappings ──────────────────────────────────────────────────
# Associate file extensions with provisioned Tree-sitter grammars.
# Requires grammar to be provisioned first via: nakshastramcp provision --lang go --lib ./tree_sitter_go.dll
[custom_languages]
".go" = "go"
".rs" = "rust"
```

---

## 🌍 Environment Variables

All settings can be overridden at runtime using `NAKSH_*` prefixed environment variables. These take precedence over `config.toml` but are overridden by CLI flags.

| Variable | Default | Description |
| :--- | :--- | :--- |
| `NAKSH_TRANSPORT` | `streamable-http` | Transport mode: `stdio` or `streamable-http` |
| `NAKSH_PORT` | `2102` | HTTP bridge port |
| `NAKSH_HOST` | `127.0.0.1` | HTTP bridge bind host |
| `NAKSH_MEM_THRESHOLD_MB` | `1024` | Memory Guard cleanup threshold (MB) |
| `NAKSH_SNIPPET_LIMIT` | `10` | Maximum search result records per query |
| `NAKSH_LOG_LEVEL` | `INFO` | Logging verbosity (`DEBUG`, `INFO`, `WARNING`, `ERROR`) |
| `NAKSH_SEARCH_TIMEOUT_MS` | `500` | Search pipeline timeout (ms) |
| `NAKSH_RERANK_TIMEOUT_MS` | `150` | FlashRank reranker timeout (ms) |
| `NAKSH_SCORING_ALPHA` | `0.7` | Relevance/PageRank blend weight |
| `NAKSH_DEBOUNCE_MS` | `500` | Filesystem watcher debounce period (ms) |
| `NAKSH_SEARCH_CACHE_TTL_S` | `300` | Search result cache TTL (seconds) |

---

## 🧩 Adding Custom Language Support (Addons)

Core languages — **Python, JavaScript, TypeScript, TSX, Java, Kotlin, and Swift** — are natively supported with no configuration. To add additional languages:

### Step-by-Step Grammar Provisioning

**1. Obtain the grammar binary:**

Fetch or compile a Tree-sitter binary grammar (`.dll` on Windows, `.so` on Linux/macOS, `.dylib` on macOS) for your target language.

**2. Provision the grammar:**

```powershell
nakshastramcp provision --lang go --lib .\tree_sitter_go.dll
```

Optional flags for `provision`:
- `--query <path>`: Path to a custom `symbols.scm` query file for symbol extraction.
- `--ext <extension>`: File extension to map (e.g., `.go`). If omitted, you will be shown how to configure this in `config.toml`.

NakshAstraMCP validates the grammar library in an isolated subprocess to prevent crashes from malformed binaries.

**3. Associate file extensions (in `config.toml`):**

```toml
[custom_languages]
".go" = "go"
```

Or pass inline via CLI:

```powershell
nakshastramcp start --config-lang ".go=go,.rs=rust"
```

---

## 🗺️ High-Fidelity Knowledge Mapping

### Artifact Generation

When triggered, the Architectural Report Engine creates a `nakshastra-out/` folder inside your workspace containing:

1. **`NAKSHASTRA_REPORT.md`**: A structured Markdown overview showing:
   - **High-Impact Files**: Hub files ranked by PageRank centrality score.
   - **High-Impact Symbols**: Central class/function definitions with clickable line links (e.g., `file:///path/to/file#L10-L45`).
   - **Module Communities**: Louvain-detected clusters of tightly coupled files (named by dominant directory).
   - **Blast Radius Warnings**: Flags "God Node" files whose modification would cascade across the most modules.

2. **`graph.json`**: Raw symbol dependency map in JSON format for rendering in external diagram tools.

### Triggering a Report

**Via MCP tool (from AI assistant):**
```
generate_report
```

**Via CLI (standalone):**
```powershell
nakshastramcp report .
```

---

## 🔬 How the Search Engine Works

Understanding the search pipeline helps you tune parameters and choose the right tool.

### The 5-Stage Search Pipeline

When you call `deep_context` or `search_codebase`, the engine runs this pipeline:

```
Query Input
    │
    ▼
1. Query Sanitization
   (Remove FTS5/Tantivy special chars; camelCase decomposition as fallback)
    │
    ▼
2. Tantivy Full-Text Search  ← Primary engine (sub-millisecond)
   [Fallback if empty]
    │
    ▼
3. SQLite FTS5 BM25 Search   ← Secondary engine
   [Fallback if empty]
    │
    ▼
4. Substring LIKE Search     ← Last resort
    │
    ▼
5. FlashRank Semantic Reranking  ← Reorders by conceptual intent (150ms max)
```

### `deep_context` — Combined Relevance Scoring

After search, `deep_context` performs additional graph-aware scoring for each symbol:

```
Combined Score = Dampening × (α × Lexical Score + (1 - α) × PageRank Score)
```

Where:
- **`α`** (`scoring_alpha`, default `0.7`): Blends lexical match strength with architectural importance.
- **Dampening** (`0.4` for graph neighbors, `1.0` for primary hits): Reduces weight of indirectly expanded files.
- **Query Match Boost** (`2.0×`): Applied when the symbol name directly contains the search query term.
- **Top-25 Deduplication**: Final results are deduplicated and capped at 25 symbols for token efficiency.

### 1-Hop AST Graph Expansion

The `deep_context` tool goes further than text matching: after finding primary file hits, it queries the Symbol Graph to identify **1-hop neighbors** — files that import or are imported by the matched files. This pulls in the architectural context a keyword search would miss.

> [!TIP]
> Pass `expand_neighbors=False` to `deep_context` for faster responses when you only need direct file matches and don't need surrounding module context.

---

<p align="center">
  <a href="README.md">🏠 Home</a> ·
  <a href="SETUP.md">🚀 Setup Guide</a> ·
  <a href="TROUBLESHOOTING.md">🛠️ Troubleshooting</a> ·
  <a href="AGENTS.md">🤖 Agent Guide</a>
</p>
