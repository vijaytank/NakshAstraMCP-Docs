# 🛠️ NakshAstraMCP — Troubleshooting Guide

> **Recovery Hub**: Diagnose and resolve connectivity problems, indexing delays, port conflicts, and environment configuration issues.

---

## 📋 Table of Contents

1. [First Step — Run the Doctor](#-first-step--run-the-doctor)
2. [Common Issues & Fixes](#️-common-issues--fixes)
   - [JSON Parsing Error on stdio Connection](#1-json-parsing-error-invalid-character--looking-for-beginning-of-value)
   - [Port 2102 Already in Use](#2-dual-transport-bridge-conflict-port-2102-in-use)
   - [Missing or Stale Search Results](#3-missing-or-stale-search-results)
   - [Elevated Memory Consumption](#4-elevated-memory-consumption)
   - [Grammar Addon Compilation Failures](#5-grammar-addon-compilation-failures)
   - [Server Fails to Start — Already Running](#6-server-fails-to-start--already-running-error)
   - [Windows Long Path Issues](#7-windows-long-path-issues)
   - [FlashRank Model Download Fails](#8-flashrank-model-download-fails)
3. [Log Locations & Streaming](#-log-locations--streaming)
4. [Getting Further Help](#-getting-further-help)

---

## 🩺 First Step — Run the Doctor

For **any** configuration, permission, or library anomaly — always start here:

```powershell
nakshastramcp doctor
```

The doctor runs **13 automated pre-flight checks** covering: Python runtime integrity, FTS5 availability, Tree-sitter API health, grammar integrity hashes, FlashRank model cache status, Tantivy engine, port availability, disk space, and Windows Long Path settings.

Review the output. Fix any checks marked `FAIL ❌` (critical failures) before investigating further. See the [complete doctor reference in the Setup Guide → Doctor Pre-Flight Diagnostics Reference](SETUP.md).

---

## 🛠️ Common Issues & Fixes

### 1. JSON Parsing Error: `invalid character '_' looking for beginning of value`

**Symptom**: Your AI client (e.g., Claude Desktop, Cursor, VS Code) reports a JSON parsing error when connecting to the server.

**Root Cause**: The MCP client uses `stdio` transport and expects pure JSON-RPC frames on stdout. When the transport flag is missing or wrong, the server may print diagnostic banners or startup text onto stdout, which the client tries to parse as JSON — and fails.

**Fix**: Explicitly add `--transport stdio` to your IDE's MCP configuration:

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

> [!NOTE]
> In `stdio` mode, all startup banners and info messages are routed to `stderr`, keeping `stdout` clean for JSON-RPC communication.

---

### 2. Dual Transport Bridge Conflict: Port 2102 in Use

**Symptom**: The host stdio IDE connection starts successfully, but secondary follower clients cannot reach the HTTP bridge. Logs show `Port 2102 is already in use`.

**Root Cause**: Another service or an orphaned background NakshAstraMCP process is already listening on port `2102`.

**Fix Option A** — Kill the conflicting process:

```powershell
# Windows: Find and kill the process using port 2102
netstat -ano | findstr :2102
taskkill /PID <PID> /F
```
```bash
# macOS / Linux
lsof -ti :2102 | xargs kill -9
```

**Fix Option B** — Use a different port:

```powershell
nakshastramcp start --port 3000
```

Then update your follower clients to target `http://127.0.0.1:3000/mcp`.

---

### 3. Missing or Stale Search Results

**Symptom**: The AI assistant returns incomplete context or cannot find a recently added function, class, or file.

**Resolution**:

1. Verify the workspace is registered and indexing has completed:
   ```powershell
   nakshastramcp status
   ```

2. Check if your `.mcpignore` file is accidentally excluding the files:
   ```powershell
   # Open the mcpignore in your project root
   cat .\.mcpignore
   ```

3. Force a complete re-index:
   ```powershell
   nakshastramcp start --workspace . --force
   ```

4. If using the real-time watcher, confirm it's active (check the logs):
   ```powershell
   nakshastramcp logs --lines 30
   ```

---

### 4. Elevated Memory Consumption

**Symptom**: Background processes use significantly more than expected RAM on massive codebases (> 1 GB).

**Resolution**:

1. Create or expand your `.mcpignore` to exclude large binary directories, virtual environments, and output logs:
   ```gitignore
   dist/
   build/
   node_modules/
   .venv/
   __pycache__/
   *.csv
   *.parquet
   *.db
   ```

2. Trigger immediate garbage collection:
   ```powershell
   nakshastramcp gc
   ```

3. Lower the maximum number of simultaneous active workspace contexts in `config.toml`:
   ```toml
   mem_lru_size = 3
   ```

4. Lower the Memory Guard threshold to trigger cleanups sooner:
   ```toml
   mem_threshold_mb = 512
   ```

---

### 5. Grammar Addon Compilation Failures

**Symptom**: Running `nakshastramcp provision --lang rust --lib ./tree_sitter_rust.dll` returns validation or compilation errors.

**Resolution**:

- Ensure you're providing the **absolute path** to a pre-compiled Tree-sitter grammar dynamic library:
  - Windows: `.dll`
  - Linux: `.so`
  - macOS: `.dylib`
- Verify the binary file is not locked or in use by another process.
- Ensure the grammar was compiled for the same platform and Python ABI as your NakshAstraMCP installation.
- Check that `nakshastramcp doctor` shows **Build Tools (C++)** as available if you need to recompile grammars.

---

### 6. Server Fails to Start — "Already Running" Error

**Symptom**: `nakshastramcp start` exits with `ERROR: NakshAstraMCP is already running (PID: XXXX)`.

**Root Cause**: A stale lock file (`server.lock`) is present from a previous crash or ungraceful shutdown.

**Fix**:

```powershell
# Force clear the stale lock and restart:
nakshastramcp start --force
```

Or stop the existing instance first:

```powershell
nakshastramcp stop
nakshastramcp start
```

---

### 7. Windows Long Path Issues

**Symptom**: Indexing fails silently for files in deeply nested directories (paths > 260 characters).

**Root Cause**: Windows restricts path lengths to 260 characters by default. NakshAstraMCP's `doctor` command checks for this under the **Windows Long Paths** check.

**Fix**: Enable long path support via PowerShell (requires Administrator):

```powershell
# Enable Windows Long Paths
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" `
  -Name "LongPathsEnabled" -Value 1 -Type DWord
```

Or via Group Policy Editor: `Computer Configuration → Administrative Templates → System → Filesystem → Enable Win32 long paths`.

---

### 8. FlashRank Model Download Fails

**Symptom**: `nakshastramcp doctor` shows `FAIL` on the **FlashRank Model Cache** check. Reranking produces errors or is silently skipped.

**Root Cause**: The `ms-marco-TinyBERT-L-2-v2` model couldn't be downloaded (network restriction, proxy, or corporate firewall).

**Fix**:

1. Download the model on a machine with open internet access and copy the cache directory.
2. Or, if your corporate environment blocks the model hub, set the environment variable to use a local model path:
   ```powershell
   $env:FLASHRANK_CACHE_DIR = "C:\path\to\local\models"
   ```
3. Rerun `nakshastramcp doctor` to confirm the model cache check passes.

---

## 📁 Log Locations & Streaming

Stream live logs directly to your terminal:

```powershell
# Stream last 50 lines:
nakshastramcp logs

# Follow (tail -f mode):
nakshastramcp logs --follow

# Show last 100 lines:
nakshastramcp logs --lines 100
```

Rotating log files are stored locally in your system data directory:

| Platform | Log Directory |
| :--- | :--- |
| **Windows** | `%LOCALAPPDATA%\nakshastramcp\logs\` |
| **macOS** | `~/Library/Application Support/nakshastramcp/logs/` |
| **Linux** | `~/.local/share/nakshastramcp/logs/` |

> [!NOTE]
> For absolute privacy, NakshAstraMCP **never** records source code content, variable values, or secret values in logs. Only file paths, timing metrics, and server state are tracked.

---

## 💬 Getting Further Help

If you've followed this guide and the issue persists:

1. **Run** `nakshastramcp doctor` and collect the full output.
2. **Stream** `nakshastramcp logs --lines 100` and note any `ERROR` or `WARNING` lines.
3. **Visit** [GitHub Discussions](https://github.com/vijaytank/NakshAstraMCP-Docs/discussions) — post your OS, version (`nakshastramcp --version`), and the relevant log lines.
4. **For bugs**: Open a [GitHub Issue](https://github.com/vijaytank/NakshAstraMCP-Docs/issues) using the bug report template.

---

<p align="center">
  <a href="README.md">🏠 Home</a> ·
  <a href="SETUP.md">🚀 Setup Guide</a> ·
  <a href="USER_GUIDE.md">📖 User Guide</a> ·
  <a href="DISCUSSIONS_WELCOME.md">💬 Discussions</a>
</p>
