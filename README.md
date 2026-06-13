<div align="center" markdown="1">

<img src="assets/logo.png" alt="NakshAstraMCP - AI Code Context Engine Banner" width="100%">

# NakshAstraMCP

**The ultimate high-performance local code context engine for AI-native software development.**

[![License](https://img.shields.io/badge/License-Proprietary-EB5757.svg?style=for-the-badge&logoWidth=40)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Active-27AE60.svg?style=for-the-badge)](#)
[![OS Support](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-2F80ED.svg?style=for-the-badge)](#)
[![Release](https://img.shields.io/badge/Release-v3.20.0-8E44AD.svg?style=for-the-badge)](https://github.com/vijaytank/NakshAstraMCP-Docs/releases/tag/v3.20.0)

</div>

---

## What is NakshAstraMCP?

**NakshAstraMCP** is an ultra-fast, local-first **Model Context Protocol (MCP)** server built to empower AI coding assistants — including **Claude**, **Cursor**, **Windsurf**, and **Antigravity IDE** — with deep, AST-accurate, structural understanding of your codebases.

Unlike generic text searches or broad file dumps that inflate LLM token costs and dilute context quality, NakshAstraMCP parses class hierarchies, function boundaries, and cross-file reference graphs to supply AI agents with the *exact* context needed to solve complex programming tasks — safely and precisely.

---

## 🧭 Documentation Hub

| 🚀 [Setup Guide](SETUP.md) | 📖 [User Guide](USER_GUIDE.md) | 🤖 [Agent Guide](AGENTS.md) |
| :---: | :---: | :---: |
| *Step-by-step Installation & Platform Setup* | *Advanced Usage, CLI Reference & Configuration* | *AI Agent Behavioral Instructions* |

| 📜 [License](LICENSE) | 🛡️ [Security Policy](SECURITY.md) | 💬 [Community Discussions](DISCUSSIONS_WELCOME.md) |
| :---: | :---: | :---: |
| *Usage Terms* | *Local Privacy & Safety* | *Q&A, Ideas & Feedback* |

| 🎯 [MCP-First Skill](mcp_first_skill.md) | 🛠️ [Troubleshooting](TROUBLESHOOTING.md) | 📋 [Global Agent Config](nakshastramcp_docs.md) |
| :---: | :---: | :---: |
| *AI Assistant Rule Profile* | *Diagnosis & Recovery* | *Custom AI Instructions Block* |

---

## ⚡ Performance Benchmarks

*   **Search Latency**: **~0.68ms (p95)** across medium-to-large codebases.
*   **Semantic Alignment**: CPU-bound FlashRank reranker aligns results to programming intent.
*   **Memory Footprint**: Runs under **< 150 MB idle RAM** with automatic garbage collection.
*   **Language Coverage**: Deep AST parsing for **Python, JavaScript, TypeScript (TSX), Java, Kotlin, and Swift** out of the box — plus runtime addon support for Go, Rust, Ruby, and more.
*   **Token Savings**: Reduces LLM context payload sizes and costs by up to **75%**.

### 📊 Context Retrieval: With vs. Without NakshAstraMCP

Tested on a commercial codebase with over 10,000 source files:

| Metric | With NakshAstraMCP | Manual Search | Efficiency Gain |
| :--- | :--- | :--- | :--- |
| **Context Fidelity** | High: specific AST symbols & 1-hop neighbors | Low: scattered keyword-only search | **High-Precision Context** |
| **LLM Token Cost** | **$0.09** | **$0.37** | **75% Cost Reduction** |
| **Wall Clock Time** | **1m 21s** | **2m 05s** | **35% Speed Increase** |

<br>

<div align="center">
  <img src="assets/dashboard_search.png" alt="NakshAstraMCP Search Interface Dashboard" width="90%">
  <p><em>Sleek multi-repository hybrid search with instant lexical routing.</em></p>
</div>

---

## ✨ Core Features

| Feature | Description |
| :--- | :--- |
| 🔍 **Multi-Repo Hybrid Search** | Search and merge context across all your projects simultaneously. |
| 🧠 **Semantic Reranking** | FlashRank cross-encoder (CPU-based) re-orders results by conceptual intent. |
| 🌳 **AST-Aware Snippets** | Returns complete, syntactically valid functions/classes — no arbitrarily sliced text. |
| 📊 **PageRank Relevance** | Grades code importance based on cross-file call frequency and import patterns. |
| 🤖 **Agent Orchestration** | Auto-provisions `AGENTS.md` instructions and MCP-First Skill Profile for AI assistants. |
| 🛡️ **Path Jail** | Strictly sandboxed to registered workspace roots — protects sensitive environments. |
| 👁️ **Real-Time Watcher** | Debounced filesystem updates with automatic mass-update safeguards. |
| 🧩 **Runtime Language Addons** | Add new Tree-sitter grammars (e.g., Go, Rust) without rebuilding. |
| 🧹 **Memory Guard** | Background process prevents RAM leaks during long indexing runs. |
| 📈 **Nebula UI Dashboard** | Streamlit visualization tool for interactive codebase graph exploration. |
| 🌉 **Dual Transport Bridge** | Serve multiple AI clients simultaneously from a single background session. |

<div align="center">
  <img src="assets/dashboard_stats.png" alt="NakshAstraMCP System Monitoring Analytics Dashboard" width="90%">
  <p><em>Real-time indexing statistics and memory usage tracking.</em></p>
</div>

---

## 🚀 Quick Start

### Step 1 — Install `uv` (Fast Python Package Manager)

```powershell
# Windows (PowerShell)
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```
```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### Step 2 — Install the Secure Binary Wheel

**📥 [Download v3.20.0 Secure Wheel from the Releases Page](https://github.com/vijaytank/NakshAstraMCP-Docs/releases/tag/v3.20.0)**

```powershell
# Recommended (uv) — auto-isolates the tool globally:
uv tool install https://github.com/vijaytank/NakshAstraMCP-Docs/releases/download/v3.20.0/nakshastramcp-3.20.0-cp313-cp313-win_amd64.whl --force
```

### Step 3 — Register & Index Your Workspace

```powershell
# Navigate to your project root, then:
nakshastramcp start --workspace .
```

### Step 4 — Verify Health

```powershell
nakshastramcp doctor   # Runs 13 pre-flight environment diagnostics
nakshastramcp status   # View active workspaces and server state
```

> **Next step**: Connect NakshAstraMCP to your AI client. See the [User Guide → Client Configuration](USER_GUIDE.md).

---

## 💻 System Requirements

| Tier | Hardware | Features Enabled |
| :--- | :--- | :--- |
| **Minimal** | 2 CPU Cores / 4 GB RAM | Core keyword search, aggressive Memory Guard cleanups |
| **Recommended** | 4 CPU Cores / 8 GB RAM | + Tantivy FTS, CPU FlashRank Semantic Reranking |
| **Optimal** | 8+ CPU Cores / 16 GB RAM | + PageRank graph calculations, deep AST relationship mapping |

---

## 🌉 Multi-Client Bridge

NakshAstraMCP features a **Dual Transport Bridge** that lets multiple IDEs and AI clients share one background session with zero performance contention.

*   **Host Session**: Your primary editor (e.g., Antigravity or Cursor) spawns the host via `stdio` transport.
*   **HTTP Bridge**: The host automatically exposes a streamable HTTP connection on port `2102`.
*   **Secondary Clients**: Other tools (VS Code extensions, scripts) can connect simultaneously via:
    *   **URL**: `http://127.0.0.1:2102/mcp`
    *   **Type**: `streamable-http`

---

## 🛡️ Security & Privacy

*   **100% Local Execution**: No code, index data, or metadata ever leaves your machine.
*   **Secret Detection**: Integrated secret scanner blocks API keys and passwords from being indexed.
*   **Jailed Paths**: Strict sandboxing blocks symbolic link exploits and out-of-workspace traversal.
*   **User-Space Only**: Runs entirely within user-space — no administrator/root privileges required.

---

<div align="center">
  <p>&copy; 2026 Vijay Tank. All rights reserved.</p>
  <p>
    <a href="SETUP.md">🚀 Get Started</a> ·
    <a href="USER_GUIDE.md">📖 User Guide</a> ·
    <a href="TROUBLESHOOTING.md">🛠️ Troubleshooting</a> ·
    <a href="SECURITY.md">🛡️ Security</a> ·
    <a href="https://github.com/vijaytank/NakshAstraMCP-Docs/discussions">💬 Discussions</a>
  </p>
</div>
