# 📖 NakshAstraMCP User Guide

> **Master your workflow**: Deep analysis, multi-client connectivity, and visualization.

---

> **Vision**: Empower AI agents with high-fidelity local code context at zero infrastructure cost.

---

## 📋 Prerequisites
- **OS**: Windows 10+ (tested on v11), macOS 12+, or Linux (glibc 2.31+).
- **Hardware**: See the [Hardware Tiers](#-hardware-tiers) section for recommended specifications.

---

## 🚀 Getting Started

### 1. Installation
Ensure [uv](https://astral.sh/uv) is installed, then install the universal wheel:
```powershell
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
uv tool install https://github.com/vijaytank/NakshAstraMCP-Docs/releases/download/3.0.0/nakshastramcp-3.13.0-cp313-cp313-win_amd64.whl --force
```

### 2. Configuration for AI Clients

#### Claude Desktop
Add the following to your `claude_desktop_config.json`:
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

#### Cursor IDE
1. Open Settings -> Models -> MCP.
2. Add New MCP Server.
3. Name: `NakshAstra`.
4. Command: `nakshastramcp`.
5. Arguments: `start`, `--transport`, `stdio`.

#### Antigravity (mcp_config.json)
Add the following to your `mcp_config.json`:
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

---

## 🌉 Multi-Client Connectivity (Dual Transport Bridge)

NakshAstraMCP introduces a **Dual Transport Bridge**, allowing one host instance to serve multiple clients simultaneously.

### Use Case
You are working in **Antigravity** (Host) while also using **VS Code** (Follower) for specific extensions. Both can connect to the same NakshAstraMCP instance.

### Configuration for Follower Applications
When the host is active, other tools can connect to the bridge:
- **Type**: `streamable-http`
- **URL**: `http://127.0.0.1:2102/mcp`

---

## 💻 Hardware Tiers

NakshAstraMCP automatically adapts its engine capabilities based on your available hardware:

| Tier | Specs | Capabilities |
|------|-------|-------------|
| **Minimal** | 2 cores / 4 GB RAM | Core search engine, aggressive memory management |
| **Recommended** | 4 cores / 8 GB RAM | + Semantic reranking + High-performance indexing |
| **Optimal** | 8+ cores / 16 GB RAM | Full graph analysis + Deep reranking |
| **Massive Repo** | 8+ cores / 16 GB+ RAM | Optimized for repositories with 50k+ files |

**Performance SLA**: p95 query latency under 500ms on a 10,000-file repository.

---

### 🤖 Automated Agent Orchestration
Starting with v3.13.0, NakshAstraMCP automatically manages agent behavior within your workspace.

- **Zero-Config Onboarding**: When you register a workspace via `nakshastramcp start --workspace .`, the server automatically provisions a project-specific `AGENTS.md` file.
- **AI Guidance**: This file instructs LLM agents to prioritize NakshAstraMCP tools over generic search methods, ensuring consistent performance and lower API costs.
- **Safety First**: If you already have an `AGENTS.md` file, NakshAstraMCP will safely rename it to `AGENTS_Backup.md` before provisioning the optimized version.

---

## 🩺 Surgical Intelligence Tools

NakshAstraMCP v3.13.0 introduces high-precision tools designed for minimal token usage and maximum accuracy.

### `search_codebase`
Performs a hybrid search (Tantivy BM25 + FlashRank Reranking) across your registered repositories.
- When `workspace_path` is omitted, **all registered workspaces** are searched and results are merged by score.
- **Usage**: "Find where the authentication logic is handled."

### `deep_context`
Analyzes cross-file relationships and provides structural snippets with 2-hop graph expansion.
- Supports **multi-workspace** search — results are merged and sorted by relevance score.
- **Usage**: "Explain how the DataManager class is initialized."

### `generate_report`
Manually triggers a full architectural analysis of your workspace.
- Generates `NAKSHASTRA_REPORT.md` and `graph.json` in the `nakshastra-out/` directory.
- **Usage**: "Refresh the architectural map for this project."

### Precision Retrieval Tools

- **`read_file`**: Read precise line ranges of a file. AI agents use this to avoid dumping entire large files.
- **`find_symbol`**: Locate exactly where a class, function, or method is defined across all registered workspaces.
- **`find_references`**: Trace every usage of a symbol to understand the impact of potential changes.
- **`generate_report`**: Produce a macro-level architectural report of the workspace.
- **`server_status`**: Returns internal health metrics, memory usage, and indexing status.

---

---

## 🗺️ Knowledge Mapping (Architectural Intelligence)

NakshAstraMCP v3.13.0 features a high-fidelity **Knowledge Mapping** engine that parses code relationships and visualizes architectural dependencies.

### 📊 Automated Reports
Every full scan automatically generates a `nakshastra-out/` directory in your workspace root containing:
- **`NAKSHASTRA_REPORT.md`**: A human-readable architectural digest structured for maximum readability:
    - **High-Impact Files**: Physical codebase files identified as central hubs via PageRank.
    - **High-Impact Symbols**: Key logical abstractions (classes, functions, methods, properties) displaying clickable line-range hyperlinks (e.g. `file:///absolute/path/to/file#Lstart-Lend`) for instant IDE navigation.
    - **Module Communities**: Tightly-coupled functional clusters grouped using Louvain community detection.
- **`graph.json`**: A portable export of the code-relationship graph for use in external visualization tools.

### 🧠 AST Call Graph & Community Detection
The engine analyzes package imports, documentation links, and granular code symbol calls to construct an AST-level relationship map:
- **Semantic Calls**: Traces exact class and function interactions between files and internal helper methods.
- **Clean Louvain Clusters**: Clustering is computed exclusively over workspace source files, filtering out external package stubs to keep analysis pristine.
- **Directory Frequency Naming**: Module communities are automatically named by their dominant parent directory (e.g., `ui`, `data`, `network`), offering immediate conceptual clarity.

### 🛠️ Manual Report Generation
You can manually trigger a report refresh at any time via the CLI:
```powershell
nakshastramcp report .
```
This is useful after major refactors or when you've added new source files that you want to integrate into the architectural map.

---

## 🛠️ Important Commands

| Category | Action | Command |
| --- | --- | --- |
| **Lifecycle** | **Start** | `nakshastramcp start --workspace <path>` |
| | **Stop** | `nakshastramcp stop` |
| | **Restart** | `nakshastramcp restart` |
| **Diagnostics** | **Health Check** | `nakshastramcp doctor` |
| | **Logs** | `nakshastramcp logs [--follow]` |
| | **Status** | `nakshastramcp status` |
| **Advanced** | **Visual UI** | `nakshastramcp ui` |
| | **Provision** | `nakshastramcp provision --lang <name> --lib <path>` |
| | **Cleanup** | `nakshastramcp gc` |

---

## 🔄 Background vs. CLI Management

NakshAstraMCP is designed to be a **"Set it and Forget it"** tool:

1. **Background Management (Daily Use)**:
   When configured in an IDE like **Antigravity**, **Cursor**, or **VS Code**, the server is managed automatically. The IDE spawns the process when it starts and terminates it when it closes. You do not need to manage the server manually.

2. **CLI Management (Advanced Use)**:
   For debugging, health checks, or manual workspace registration, use the commands listed above. The CLI is optional and provided for advanced users.

---

## 🚫 `.mcpignore` Configuration

Control which files and directories are excluded from indexing by placing a `.mcpignore` file in your workspace root. The syntax is identical to `.gitignore`:

```
# Exclude build artifacts
dist/
build/
node_modules/

# Exclude large data files
*.csv
*.parquet
*.sqlite

# Exclude specific directories
vendor/
__pycache__/
.git/
```

> **Tip**: Changes to `.mcpignore` take effect immediately — the real-time watcher picks them up automatically.

---

## 🧩 Adding New Language Support (Addons)

NakshAstraMCP supports [Tree-sitter](https://tree-sitter.github.io/tree-sitter/) grammars. While popular languages like Python, JS/TS, Java, and Kotlin are built-in, you can add any other language at runtime.

### Provisioning a Language
1.  **Obtain a compiled grammar** (`.dll` for Windows, `.so` for Linux/macOS).
2.  **Provision the language**:
    ```powershell
    nakshastramcp provision --lang go --lib ./tree_sitter_go.dll
    ```
3.  **Verify**: The server will validate the binary and copy it to its internal store. No restart is required for the indexing engine to pick up the new file types.

---

## ⚙️ Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `NAKSH_TRANSPORT` | `streamable-http` | Override transport mode (`stdio` or `streamable-http`) |
| `NAKSH_MEM_THRESHOLD_MB` | `1024` | Memory guard threshold in MB |
| `NAKSH_SNIPPET_LIMIT` | `10` | Max results per search query |
| `NAKSH_LOG_LEVEL` | `INFO` | Logging verbosity (`DEBUG`, `INFO`, `WARNING`, `ERROR`) |

---

## 🛡 Security & Privacy
- **Local-Only**: Your code never leaves your machine.
- **Secret Detection**: Automatically prevents indexing of API keys and sensitive strings.
- **Access Control**: The server is locked to the workspace roots you've explicitly registered.
- **Zero Telemetry**: No data collection. Fully local execution.

---

<p align="center">
  <a href="README.md">🏠 Home</a> | 
  <a href="SETUP.md">🚀 Setup Guide</a> | 
  <a href="TROUBLESHOOTING.md">🛠 Troubleshooting</a>
</p>
