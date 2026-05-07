# 🚀 NakshAstraMCP Setup Guide

> **Quick Start**: Get your local context engine running in under 5 minutes.

---

## 💻 System Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| **OS** | Windows 10+, macOS 12+, Linux (glibc 2.31+) | Windows 11, macOS 14+ |
| **CPU** | 2 cores | 4+ cores |
| **RAM** | 4 GB | 8 GB+ |
| **Python** | 3.11+ (bundled with wheel) | 3.13 |
| **Disk** | 500 MB (for indexing data) | 1 GB+ |

---

## 📥 Installation

### 1. Install via uv (Recommended)
NakshAstraMCP is distributed as a **Secure Binary Wheel** for maximum privacy and performance.

**📥 [Download v3.10.1 Secure Wheel (Windows)](https://github.com/vijaytank/NakshAstraMCP-Docs/releases/download/3.0.0/nakshastramcp-3.10.1-cp313-cp313-win_amd64.whl)**

We recommend using [uv](https://astral.sh/uv) for installation:

```powershell
# Install uv
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# Install NakshAstraMCP
uv tool install https://github.com/vijaytank/NakshAstraMCP-Docs/releases/download/3.0.0/nakshastramcp-3.10.1-cp313-cp313-win_amd64.whl --force
```

### 2. Prerequisite Check
Run the built-in `doctor` command to verify your environment is correctly configured:
```powershell
nakshastramcp doctor
```

---

## ⚙️ Initial Configuration

### Registering a Workspace
Navigate to your project root and run:
```powershell
nakshastramcp start --workspace .
```
The server will create a local index in your user data directory without modifying your source code.

### Configuring `.mcpignore`
You can control which files and directories are excluded from indexing by creating a `.mcpignore` file in your workspace root. The syntax is identical to `.gitignore`:

```
# Exclude build artifacts
dist/
build/
node_modules/

# Exclude large data files
*.csv
*.parquet

# Exclude specific directories
vendor/
__pycache__/
```

---

## 🛡 Security Note
NakshAstraMCP is designed with a "Local First" philosophy. It does not require administrative privileges to index your local files, and it does not connect to any external servers during normal operation.

---

<p align="center">
  <a href="README.md">🏠 Home</a> | 
  <a href="USER_GUIDE.md">📖 User Guide</a> | 
  <a href="TROUBLESHOOTING.md">🛠 Troubleshooting</a>
</p>
