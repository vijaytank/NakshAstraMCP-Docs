# 🛠 NakshAstraMCP Troubleshooting Guide

> **Recovery Center**: Resolve connectivity, indexing, and platform-specific issues.

---

This guide helps resolve common issues encountered while setting up or using NakshAstraMCP.

---

## 🔍 Diagnostic Tool: `doctor`

The first step for any issue is to run the built-in diagnostic tool. This verifies your environment, permissions, and dependencies.

```powershell
nakshastramcp doctor
```

Review the output for any checks marked as **FAILED** or **WARNING**. The doctor performs 12 comprehensive checks covering your runtime, permissions, disk space, and port availability.

---

## 🛠 Common Issues

### 1. Protocol Error: `invalid character '_' looking for beginning of value`
If your IDE (VS Code, Cursor, etc.) shows this error, it's typically because the IDE is trying to read the protocol via `stdio`, but the server is printing an informative banner or log message to the same stream.

**Fix**: Ensure your IDE configuration includes the `--transport stdio` flag. This correctly isolates the protocol stream from informative messages.
```json
"args": ["start", "--transport", "stdio"]
```

### 2. Dual Transport Bridge Port Conflicts
The background HTTP bridge defaults to port `2102`. If this port is in use, the bridge will fail to start, though the host `stdio` session (IDE) will remain functional.

**Fix**: You can specify a custom port for the bridge:
```powershell
nakshastramcp start --port 3000
```
Then, update your follower applications to connect to `http://127.0.0.1:3000/mcp`.

### 3. Server Not Responding / Indexing Issues
If the server appears slow or search results are outdated:
- **Check Status**: Run `nakshastramcp status` to see if indexing is in progress or if there are high memory alerts.
- **Run GC**: If memory usage is high, run `nakshastramcp gc` to trigger an immediate garbage collection.
- **Identify Workspace**: Ensure the workspace is registered correctly. If needed, re-register using `nakshastramcp start --workspace <path>`.

### 4. High Memory Usage
NakshAstraMCP includes a **Memory Guard** that monitors system memory usage. If memory consumption is elevated:
- Run `nakshastramcp gc` to trigger garbage collection immediately.
- Consider reducing the number of registered workspaces.
- Use `.mcpignore` to exclude large directories (e.g., `node_modules/`, `dist/`, `.git/`) from indexing.
- Review the [Hardware Tiers](USER_GUIDE.md#-hardware-tiers) to ensure your system meets the recommended specifications.

### 5. Files Not Being Indexed
If certain files are not appearing in search results:
- Check if a `.mcpignore` file exists in the workspace root that may be excluding them.
- Verify the file type is supported (Python, JavaScript, TypeScript, Java, Kotlin, and common text formats).
- Run `nakshastramcp status` to confirm the workspace is registered and indexing is complete.

### 6. Dashboard Not Loading
If `nakshastramcp ui` does not launch the dashboard:
- Ensure the server is running (`nakshastramcp status`).
- Check that at least one workspace is registered.
- Verify that port 8501 (Streamlit default) is not in use by another application.

---

## 📁 Investigation
If you need to investigate further, use the built-in diagnostic commands:

1. **`nakshastramcp status`**: View overall server health and indexing progress.
2. **`nakshastramcp logs [--follow]`**: Stream the server logs directly to your console. This is the fastest way to identify runtime issues.
3. **`nakshastramcp doctor`**: Perform a comprehensive environment audit.

If the built-in commands are insufficient, log files are maintained in your platform's standard user data directory:
- **Windows**: `%LOCALAPPDATA%\nakshastramcp\logs`
- **Linux**: `~/.local/share/nakshastramcp/logs`
- **macOS**: `~/Library/Application Support/nakshastramcp/logs`

When reporting a bug, please include relevant snippets from these logs. **Note**: Source code snippets are never logged; only indexing counts and performance metrics are tracked.

---

<p align="center">
  <a href="README.md">🏠 Home</a> | 
  <a href="SETUP.md">🚀 Setup Guide</a> | 
  <a href="USER_GUIDE.md">📖 User Guide</a>
</p>
