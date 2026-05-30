# 🚀 NakshAstraMCP Setup Guide

> **Get Started**: Build a high-performance local AI code context server in under 5 minutes.

---

## 💻 System Specifications

NakshAstraMCP scales dynamically to adapt to your workstation's hardware:

| Specification | Minimum | Recommended |
| :--- | :--- | :--- |
| **OS Platform** | Windows 10+, macOS 12+, Linux (glibc 2.31+) | Windows 11, macOS 14+ |
| **CPU Arch** | Dual-core CPU | Quad-core (or better) CPU |
| **System RAM** | 4 GB | 8 GB or higher |
| **Python Version** | 3.11 or higher | Python 3.13 |
| **Storage Space** | 500 MB (Temporary indexing cache) | 1 GB+ (SSD recommended) |

---

## 📥 Installation

NakshAstraMCP is packaged and distributed as a **Secure Binary Wheel** to ensure complete local code privacy, optimal execution speeds, and seamless dependencies.

### Option A: Recommended Installation via `uv`
We highly recommend using [uv](https://astral.sh/uv), the ultra-fast Python tool manager, to install and isolate NakshAstraMCP:

**📥 [Download v3.19.0 Secure Wheel (Windows)](https://github.com/vijaytank/NakshAstraMCP-Docs/releases/download/v3.19.0/nakshastramcp-3.19.0-cp313-cp313-win_amd64.whl)**

```powershell
# 1. Install uv globally
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# 2. Install the secure wheel
uv tool install https://github.com/vijaytank/NakshAstraMCP-Docs/releases/download/v3.19.0/nakshastramcp-3.19.0-cp313-cp313-win_amd64.whl --force
```

### Option B: Installation via standard Python pip
If you prefer a standard pip installation:
```powershell
python -m pip install .\nakshastramcp-3.19.0-cp313-cp313-win_amd64.whl
```

---

## 🩺 System Verification

Once installed, verify that your environment, path variables, and required dependencies are perfectly configured by running the system health auditor:

```powershell
nakshastramcp doctor
```
Review the diagnostic checks (including path permissions, port conflicts, and Tree-sitter compilers). Ensure all critical indicators are green before registering code bases.

---

## ⚙️ Repository Onboarding

To enable semantic indexing and AST relationship graphing, register your project workspaces:

### 1. Registering a Workspace
Open your terminal inside your project directory and execute:
```powershell
nakshastramcp start --workspace .
```
The server will initialize a secure database directory locally in your user profile space (`~/.mcp_index/`) to cache code graphs. It **never** modifies your source code files.

### 2. Filtering Files via `.mcpignore`
Exclude temporary outputs, dependency folders, or media from being scanned by creating a `.mcpignore` file in your workspace root. The ignore rules are identical to `.gitignore`:

```
# Exclude build files
dist/
build/
node_modules/
.git/

# Exclude bulk datasets
*.csv
*.parquet
*.db

# Exclude temp environments
.venv/
__pycache__/
```
The workspace watcher detects updates to `.mcpignore` in real time, automatically purging excluded records from your local index.

---

## 🛡️ Privacy & Secure Environment

*   **Local First Strategy**: No codebase analysis, function definitions, or metadata ever leave your physical machine.
*   **Privilege Level**: Runs entirely within user-space context; administrative permissions are never requested.
*   **Offline First**: Operational tools are functional without an active internet connection.

---

<p align="center">
  <a href="README.md">🏠 Home</a> | 
  <a href="USER_GUIDE.md">📖 User Guide</a> | 
  <a href="TROUBLESHOOTING.md">🛠️ Troubleshooting</a>
</p>
