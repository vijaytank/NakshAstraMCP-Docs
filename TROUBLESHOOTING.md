# 🛠️ NakshAstraMCP Troubleshooting Guide

> **Recovery Hub**: Resolve connectivity, path normalizations, socket issues, and indexing delays.

---

## 🩺 Background Diagnostics: `doctor`

For any configuration, permission, or library anomalies, immediately invoke the built-in system auditor:

```powershell
nakshastramcp doctor
```

The system doctor executes 12 system-level audits covering:
*   **Python Runtime Integrity** (Checks for correct path bindings)
*   **Port Allocations** (Checks if port `2102` is available for the HTTP bridge)
*   **System Disk Space** (Validates temp cache space)
*   **Path Privileges** (Ensures permissions to write index DBs in `~/.mcp_index/`)

---

## 🛠️ Common Anomalies & Fixes

### 1. Connection Error: `invalid character '_' looking for beginning of value`
**Symptom**: Your AI-native editor (e.g. VS Code, IntelliJ, or Cursor) fails to bind to the MCP server.
*   **Root Cause**: The client editor attempts to parse standard JSON-RPC packets via `stdio` transport, but the server is printing diagnostic banners or startup notices on the same channel.
*   **Resolution**: Explicitly specify the `--transport stdio` flag in your editor configuration block to cleanly separate logging signals from JSON payloads.
    ```json
    "args": ["start", "--transport", "stdio"]
    ```

### 2. Dual Transport Bridge Conflicts (Port 2102 in Use)
**Symptom**: Your host stdio IDE connection starts successfully, but secondary followers cannot reach the HTTP bridge.
*   **Root Cause**: Another service (or an orphaned background NakshAstraMCP process) is currently listening on port `2102`.
*   **Resolution**: Specify a custom port during host startup:
    ```powershell
    nakshastramcp start --port 3000
    ```
    Then configure follower editors (e.g., VS Code Extensions) to target:
    ```
    http://127.0.0.1:3000/mcp
    ```

### 3. Missing or Stale Search Results
**Symptom**: The AI assistant returns incomplete context or cannot find a newly added function/class.
*   **Resolution**:
    1.  Verify if the workspace is registered and indexing is completed:
        ```powershell
        nakshastramcp status
        ```
    2.  Check for active ignore patterns in your `.mcpignore` file that might be excluding the files.
    3.  Manually force a full re-index of the repository workspace:
        ```powershell
        nakshastramcp start --workspace . --force
        ```

### 4. Elevated Memory Consumption
**Symptom**: Background processes use more than 1 GB of RAM on massive codebases.
*   **Resolution**:
    *   Create a concise `.mcpignore` file in the project root to exclude massive binary directories, virtual environments, or output logs (e.g., `dist/`, `.venv/`, `node_modules/`).
    *   Manually invoke immediate memory garbage collection:
        ```powershell
        nakshastramcp gc
        ```
    *   Reduce the number of concurrently active workspaces in your LRU index registry.

### 5. Addon Compilation Issues
**Symptom**: Running `nakshastramcp provision --lang rust` returns compilation or validation errors.
*   **Resolution**: Ensure you've supplied the absolute filesystem path to the compiled grammar dynamic library (`.dll` on Windows, `.so` on Linux/macOS) and that the file has not been locked by other background scanners.

---

## 📁 Log Locations & Diagnostics

If issues persist, stream background logs directly to your terminal:
```powershell
nakshastramcp logs --follow
```

Diagnostic rotating logs are maintained natively in standard system data directories:
*   **Windows Platform**: `%LOCALAPPDATA%\nakshastramcp\logs\`
*   **macOS Platform**: `~/Library/Application Support/nakshastramcp/logs/`
*   **Linux Platform**: `~/.local/share/nakshastramcp/logs/`

> **Note**: For absolute privacy, NakshAstraMCP never records source code, variable values, or secrets to logs; only file paths, metrics, and server states are tracked.

---

<p align="center">
  <a href="README.md">🏠 Home</a> | 
  <a href="SETUP.md">🚀 Setup Guide</a> | 
  <a href="USER_GUIDE.md">📖 User Guide</a>
</p>
