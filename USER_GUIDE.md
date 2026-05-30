# 📖 NakshAstraMCP User Guide

> **Maximize AI Developer Context**: Master hybrid search, multi-client bridges, real-time watchers, and visual graph diagnostics.

---

## 🚀 Client Configuration

Once NakshAstraMCP is installed, add it to your preferred AI coding environments to start querying context automatically.

### 1. Claude Desktop (macOS / Windows)

#### Option A: Manual Configuration
Add the server definition to your `claude_desktop_config.json`:
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
*Note: Once a workspace path is registered using the CLI, NakshAstraMCP's centralized database remembers it. Your IDE config only needs the plain `start` action.*

#### Option B: Easy Automation via Claude CLI
You can easily provision and register NakshAstraMCP inside Claude using the official Claude CLI tool:

*   **For HTTP Bridge Transport** (Requires the background server to be actively running in another terminal window):
    ```bash
    claude mcp add --transport http nakshastramcp http://127.0.0.1:2102/mcp
    ```
*   **For Direct STDIO Transport** (The server must be stopped/inactive in the terminal; Claude will manage its lifecycle):
    ```bash
    claude mcp add --transport stdio nakshastramcp nakshastramcp -- start --transport stdio
    ```

### 2. Cursor IDE
1. Open **Settings** -> **Models** -> **MCP**.
2. Click **+ Add New MCP Server**.
3. Name: `NakshAstra`
4. Transport: `stdio`
5. Command: `nakshastramcp`
6. Arguments: `start`, `--transport`, `stdio`

### 3. Antigravity IDE
Configure the server block inside your `mcp_config.json`:
```json
{
  "mcpServers": {
    "nakshastramcp": {
      "command": "nakshastramcp",
      "args": [
        "start",
        "--transport",
        "stdio"
      ],
      "type": "stdio",
      "disabled": false
    }
  }
}
```

### 4. Factory AI

#### Option A: Manual HTTP Transport Setup
Configure the server block inside your Factory client configuration settings:
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
*(Note: Requires the NakshAstraMCP background host server to be actively running in another window.)*

#### Option B: Manual STDIO Transport Setup
If adding NakshAstraMCP directly using standard STDIO transport via Factory CLI or client UI:
*   **Server Name**: `nakshastramcp`
*   **Server Type**: `stdio`
*   **Command**: `nakshastramcp start --transport stdio`
*(Note: The background server must be stopped/inactive in the terminal; Factory will manage the process lifecycle.)*

---

## 🌉 Dual Transport Connection Bridge

NakshAstraMCP features a **Dual Transport Bridge**. When started via stdio inside your main IDE, the host session automatically spawns a local, streamable HTTP connection on port `2102`.

### Practical Use Case
If you use **Antigravity** (Host) for terminal workflows and **VS Code** (Follower) with specialized extensions, both can query the same local codebase context graph simultaneously without thread lock contention.

### Configuring Followers
Point follower clients to the active background bridge:
*   **Type**: `streamable-http`
*   **Bridge URL**: `http://127.0.0.1:2102/mcp`

---

## 🤖 Automated Agent Orchestration

Starting with v3.16.0, NakshAstraMCP simplifies onboarding for new team members and external subagents:
*   **Zero-Config Onboarding**: When registering a repository via `nakshastramcp start --workspace .`, the server automatically provisions a custom `AGENTS.md` instructions file in your workspace root.
*   **Prompt Alignment**: The file teaches AI agents to prioritize surgical tools (`read_file` with line boundaries, `find_symbol`) over expensive plain text grepping.
*   **Non-Destructive Overwrites**: If an older custom `AGENTS.md` already exists, it is backed up to `AGENTS_Backup.md` safely.
*   **MCP-First Skill File Profile**: Users can load the standard [🎯 MCP-First Skill Profile](mcp_first_skill.md) into AI assistants (like Claude, Cursor, Antigravity, and Windsurf) to enforce precise, AST-aware, token-optimized workflow habits across all codebase tasks.

---

## 🩺 Surgical Context Tools

NakshAstraMCP registers high-precision tools on your AI client to extract code relationships cleanly while reducing token waste.

### Primary Tools

*   **`deep_context`** — Semantically searches all active workspaces, extracts matched symbols, and expands search boundaries by mapping immediate 1-hop AST imports and neighbors.
*   **`search_codebase`** — Hybrid keyword search across all indexed workspaces. Automatically blends score results.
*   **`find_symbol`** — High-precision index lookup to locate definitions (class, function, method structures) by exact identifier name.
*   **`find_references`** — Scans all registered source directories to locate exact call sites and usage metrics for custom functions.
*   **`read_file`** — Surgical reader that fetches specific line-number ranges (e.g. `start_line` to `end_line`) to avoid dumping massive source files into LLM prompts.
*   **`generate_report`** — Triggers a comprehensive architectural synthesis, outputting a detailed code relationship map (`NAKSHASTRA_REPORT.md`) and Graph visualization data (`graph.json`) in the workspace `nakshastra-out/` folder.
*   **`server_status`** — Provides diagnostic insights, indexing metrics, active workspaces, and runtime resource limits.

---

## 🗺️ High-Fidelity Knowledge Mapping

NakshAstraMCP includes a background **Architectural Report Engine** to synthesize code relationship graphs.

### 📊 Artifact Generation
When a scan is triggered manually or automatically, the engine creates a `nakshastra-out/` folder inside your project containing:
1.  **`NAKSHASTRA_REPORT.md`**: A clean, structured overview showing:
    *   **High-Impact Files**: Code files calculated to be key architectural hubs.
    *   **High-Impact Symbols**: Central logical abstractions with precise, clickable IDE line links (e.g. `file:///path/to/file#L10-L45`).
    *   **Module Communities**: Logical clusters grouped automatically by package communities and named after their dominant parent directories (e.g. `auth`, `db`, `models`).
2.  **`graph.json`**: A raw symbol dependency map in JSON format that can be parsed or rendered in external diagram tools.

---

## 🛠️ Unified CLI Command Reference

Manage server lifecycles, configuration, and environment cleanup directly via the CLI:

| Command | Action | Description |
| :--- | :--- | :--- |
| `nakshastramcp start --workspace <path>` | Start / Register | Initializes, indexes, and monitors a target code repository. |
| `nakshastramcp stop` | Stop Server | Gracefully terminates active background server processes. |
| `nakshastramcp restart` | Restart Server | Flushes active sessions and restarts background transports. |
| `nakshastramcp status` | Diagnostic Status | View database indexes, workspace lists, and server health. |
| `nakshastramcp doctor` | Environment Audit | Runs 12 comprehensive runtime checks to detect configuration issues. |
| `nakshastramcp logs [--follow]` | Process Logs | Streams real-time error logs and performance statistics to terminal. |
| `nakshastramcp ui` | Launch Dashboard | Opens the **Nebula Graph UI** to interactively visualize structural charts. |
| `nakshastramcp gc` | Clean Indexes | Triggers immediate garbage collection and deletes old database indexes. |
| `nakshastramcp provision --lang <name> --lib <path>` | Add Language | Dynamically provision custom tree-sitter grammars at runtime. |

---

## 🧩 Adding Custom Language Support (Addons)

While core languages (Python, JavaScript, TypeScript, Java, and Kotlin) are natively supported, you can configure new language syntax grammars dynamically.

### Step-by-Step Grammar Provisioning
1.  **Obtain Binary Grammar**: Fetch or compile a Tree-sitter binary compiled grammar library (`.dll` on Windows, `.so` on Linux/macOS) for your target language (e.g. Go).
2.  **Provision Grammar**: Run the CLI provision command:
    ```powershell
    nakshastramcp provision --lang go --lib .\tree_sitter_go.dll
    ```
    *NakshAstraMCP validates the library in a sandboxed subprocess and adds it to its grammars repository.*
3.  **Associate Extensions**: Inform the indexer engine of extension mappings:
    ```powershell
    nakshastramcp start --config-lang ".go=go"
    ```

---

## ⚙️ Process Environment Configurations

Fine-tune internal settings using environment variables:

| Environment Variable | Default | Description |
| :--- | :--- | :--- |
| `NAKSH_TRANSPORT` | `streamable-http` | Overrides server transport modes (`stdio` or `streamable-http`). |
| `NAKSH_MEM_THRESHOLD_MB` | `1024` | Triggers Memory Guard cleanup if the process RAM footprint exceeds this value. |
| `NAKSH_SNIPPET_LIMIT` | `10` | Limits maximum result records returned in search payloads. |
| `NAKSH_LOG_LEVEL` | `INFO` | Adjusts diagnostic logging verbosity (`DEBUG`, `INFO`, `WARNING`, `ERROR`). |

---

<p align="center">
  <a href="README.md">🏠 Home</a> | 
  <a href="SETUP.md">🚀 Setup Guide</a> | 
  <a href="TROUBLESHOOTING.md">🛠️ Troubleshooting</a>
</p>
