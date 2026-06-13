# 🚀 NakshAstraMCP — Setup Guide

> **Goal**: Get a high-performance local AI code context server running on your machine in under 5 minutes.

---

## 📋 Table of Contents

1. [System Requirements](#-system-requirements)
2. [Installation](#-installation)
3. [System Verification — Doctor](#-system-verification--doctor)
4. [Doctor Pre-Flight Diagnostics Reference](#-doctor-pre-flight-diagnostics-reference)
5. [Workspace Onboarding](#️-workspace-onboarding)
6. [Agent Orchestration — Auto-Provisioning](#-agent-orchestration--auto-provisioning)
7. [Privacy & Security Guardrails](#️-privacy--security-guardrails)

---

## 💻 System Requirements

NakshAstraMCP scales dynamically to adapt to your workstation's hardware:

| Specification | Minimum | Recommended |
| :--- | :--- | :--- |
| **OS Platform** | Windows 10+, macOS 12+, Linux (glibc 2.31+) | Windows 11, macOS 14+ |
| **CPU Architecture** | Dual-core CPU | Quad-core (or better) |
| **System RAM** | 4 GB | 8 GB or higher |
| **Python Version** | 3.11 or higher | Python 3.13 |
| **Storage Space** | 500 MB (indexing cache) | 1 GB+ (SSD recommended) |

---

## 📥 Installation

NakshAstraMCP is packaged and distributed as a **Secure Binary Wheel** (`.whl`) to ensure complete local code privacy, optimal execution speeds, and seamless dependency bundling.

### Option A — Recommended: Install via `uv` ✅

[`uv`](https://astral.sh/uv) is an ultra-fast Python tool manager that isolates NakshAstraMCP cleanly into a global tool environment.

**1. Install `uv`:**

```powershell
# Windows (PowerShell)
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```
```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**2. Download and Install the Wheel:**

📥 **[Download v3.20.0 Secure Wheel → Releases Page](https://github.com/vijaytank/NakshAstraMCP-Docs/releases/tag/v3.20.0)**

```powershell
# Windows wheel (Python 3.13, AMD64)
uv tool install https://github.com/vijaytank/NakshAstraMCP-Docs/releases/download/v3.20.0/nakshastramcp-3.20.0-cp313-cp313-win_amd64.whl --force
```

> [!TIP]
> After installation, `nakshastramcp` will be available as a global command. Restart your terminal if the command is not found.

### Option B — Standard pip

If you prefer a standard Python installation:

```powershell
# After downloading the .whl file locally:
python -m pip install .\nakshastramcp-3.20.0-cp313-cp313-win_amd64.whl
```

---

## 🩺 System Verification — Doctor

Before registering codebases, verify that your environment is correctly configured:

```powershell
nakshastramcp doctor
```

This runs **13 automated pre-flight environment checks**. You will see a structured pass/fail report in your terminal. Ensure all critical checks show `PASS ✅` before proceeding.

---

## 🔬 Doctor Pre-Flight Diagnostics Reference

The `doctor` command executes the following checks in order:

| # | Check Name | Critical? | What It Verifies |
| :---: | :--- | :---: | :--- |
| 1 | **SQLite FTS5** | ✅ Critical | Verifies your SQLite build includes FTS5 full-text search support |
| 2 | **Tree-sitter v0.22+ API** | ✅ Critical | Validates the modern Tree-sitter Python API is functional |
| 3 | **Grammar Compiler** | ✅ Critical | Ensures JavaScript and TypeScript language parser wheels are loadable |
| 4 | **Grammar Lock (TOFU)** | ⚠️ Optional | TOFU SHA-256 hash verification — detects unexpected grammar wheel changes |
| 5 | **FlashRank Library** | ⚠️ Optional | Checks for the FlashRank semantic reranking library |
| 6 | **detect-secrets Library** | ⚠️ Optional | Validates the secret scanner (falls back to regex patterns if missing) |
| 7 | **FlashRank Model Cache** | ⚠️ Optional | Pre-warms the `ms-marco-TinyBERT-L-2-v2` model (downloads if missing) |
| 8 | **Tantivy Engine** | ⚠️ Optional | Verifies the Tantivy full-text search engine is importable and operable |
| 9 | **Watchdog Monitor** | ⚠️ Optional | Validates the filesystem watcher can be started and stopped cleanly |
| 10 | **psutil Memory Polling** | ⚠️ Optional | Confirms psutil can query system RAM (required for Memory Guard) |
| 11 | **Stale Lock Detection** | ⚠️ Optional | Scans for stale `.lock` files older than 1 hour in the index directory |
| 12 | **Windows Long Paths** | ⚠️ Optional | Checks if the Windows Long Path registry key is enabled (avoids 260-char limit) |
| 13 | **Build Tools (C++)** | ⚠️ Optional | Detects presence of `cl`, `gcc`, `g++`, or `clang` for custom grammar compilation |

> [!NOTE]
> Only checks marked **✅ Critical** block server startup on failure. Optional checks that fail produce warnings but do not prevent operation — the server degrades gracefully.

---

## ⚙️ Workspace Onboarding

### 1. Register a Workspace

Open your terminal inside any project directory and run:

```powershell
nakshastramcp start --workspace .
```

The server will:
1. Register the workspace in its secure central registry (`repos.json`).
2. Create a local index database in your user data directory — **never touching your source files**.
3. Begin background AST indexing of all supported files.
4. Provision an `AGENTS.md` instruction file in your workspace root (see below).

> **Index storage location (platform-specific):**
> - **Windows**: `%LOCALAPPDATA%\nakshastramcp\`
> - **macOS**: `~/Library/Application Support/nakshastramcp/`
> - **Linux**: `~/.local/share/nakshastramcp/`

### 2. Filter Files via `.mcpignore`

Exclude build artifacts, dependency folders, or binary/media files from being indexed by creating a `.mcpignore` in your workspace root. The syntax is identical to `.gitignore`:

```gitignore
# Build outputs
dist/
build/
out/

# Dependency folders
node_modules/
.venv/
__pycache__/

# Version control
.git/

# Large binary/data files
*.csv
*.parquet
*.db
*.whl

# IDE metadata
.idea/
.vscode/
```

> [!TIP]
> The workspace watcher detects updates to `.mcpignore` in real time and automatically purges excluded records from the local index — no server restart required.

---

## 🤖 Agent Orchestration — Auto-Provisioning

When you run `nakshastramcp start --workspace .`, the server automatically provisions behavioral instruction files for AI agents:

*   **`AGENTS.md`**: A standardized agent instruction file written to your workspace root. It teaches AI assistants to use NakshAstraMCP tools efficiently — prioritizing surgical lookups over expensive shell commands.
*   **Non-Destructive**: If your workspace already contains an `AGENTS.md`, it is safely backed up to `AGENTS_Backup.md` before being replaced.
*   **MCP-First Skill Profile**: You can also load the [🎯 MCP-First Skill Profile](mcp_first_skill.md) directly into your AI assistant's system prompt or custom instructions for maximum precision.

---

## 🛡️ Privacy & Security Guardrails

| Guarantee | Detail |
| :--- | :--- |
| **Local-Only Execution** | No codebase analysis, AST graphs, function definitions, or metadata ever leave your machine |
| **User-Space Privilege** | Runs entirely without administrator or root permissions |
| **Offline-First** | All search and indexing tools are fully functional without an internet connection |
| **Secret Redaction** | Files flagged as containing secrets (API keys, tokens) are indexed as `[REDACTED]` — never exposed in search results |
| **Path Jail** | All file operations are validated against registered workspace roots; no traversal outside boundaries |

---

<p align="center">
  <a href="README.md">🏠 Home</a> ·
  <a href="USER_GUIDE.md">📖 User Guide</a> ·
  <a href="TROUBLESHOOTING.md">🛠️ Troubleshooting</a> ·
  <a href="AGENTS.md">🤖 Agent Guide</a>
</p>
