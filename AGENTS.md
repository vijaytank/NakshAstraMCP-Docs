# NakshAstraMCP — Agent Instructions (v3.20.0)

> This file is automatically provisioned in your workspace root by `nakshastramcp start --workspace .`.
> It instructs AI coding assistants to use NakshAstraMCP tools efficiently and safely.

**MANDATORY**: Always prioritize MCP tools over manual file-dumps or generic shell commands.

---

## 🛠️ MCP Capabilities (Ground Truth)

| Tool | Priority | Usage Instruction |
| :--- | :---: | :--- |
| **`deep_context`** | 🥇 **Primary** | Start here for all tasks. Locates files and adds their **immediate graph neighbors (1-hop)** for structural context. Strongly prefer over scattered `search_codebase` calls. |
| **`search_codebase`** | 🥈 **Discovery** | Broad "grep-style" keyword search when you lack a specific entry point. |
| **`find_symbol`** | 🥉 **Surgical** | Precise lookup for classes/functions by name. Use for "Go to Definition." |
| **`read_file`** | 📖 **Verification** | **MANDATORY** before any edit. Read the target block using `start_line`/`end_line` to avoid assumptions. Use `if_changed_since=<hash>` on re-reads to skip unchanged files (saves 100% tokens). |
| **`find_references`** | 🔎 **Audit** | Trace call-sites (blast-radius) before changing custom functions. Avoid on standard library utilities to save tokens. |
| **`generate_report`** | 🗺️ **Map** | Produce a macro-level architectural report of the workspace. |
| **`server_status`** | 🩺 **Diagnostic** | Use only if indexing or memory issues are suspected. |

---

## 📂 Workspace Artifacts

NakshAstraMCP generates architectural insights in:

```
nakshastra-out/
├── NAKSHASTRA_REPORT.md   ← Architectural walkthrough: PageRank hubs, Louvain clusters, blast-radius warnings
└── graph.json             ← Raw symbol relationship graph (JSON), renderable in external diagram tools
```

**Always check `NAKSHASTRA_REPORT.md` first** for macro architectural context before starting complex tasks.

---

## 🔄 Standardized Workflow

Follow this pattern for every non-trivial task:

```
1. generate_report   (Optional) → Read NAKSHASTRA_REPORT.md for macro context
         │
2. deep_context      → Discover relevant files + 1-hop architectural neighbors
         │
3. find_symbol       → Locate the exact definition (line range) for the target logic
         │
4. read_file         → Read the target code block (with enclosing context) BEFORE editing
         │
5. find_references   → Audit call sites of custom functions BEFORE making changes
```

---

## 🛡️ Guardrails

> [!IMPORTANT]
> Violating these rules causes inefficiency, bugs, and wasted tokens.

| Rule | Detail |
| :--- | :--- |
| ❌ **No Snippet Assumptions** | Snippets are for discovery only. Never assume a full definition from a snippet — always use `read_file` with line ranges. |
| ❌ **No Full File Dumps** | Never read an entire file into context if a specific line range meets the requirement. |
| ❌ **No Terminal MCP Execution** | Never attempt to run MCP tools as shell commands (e.g., `nakshastramcp_generate_report`). Call them strictly via the MCP tool protocol. |
| ✅ **Verification First** | Never edit code you haven't read in its local context via `read_file`. |
| ✅ **Blast-Radius Audit** | Always run `find_references` on custom functions before modifying their signatures. |
| ✅ **Tool Efficiency** | Prefer `deep_context` for multi-file discovery over scattered `search_codebase` calls. |
| ✅ **Truncate Log Reads** | When analyzing logs, focus on relevant failure stack traces or tail lines only — never read entire log streams. |
| ✅ **Security Protocols** | All paths are jailed. If a tool fails with a security error, verify the path is inside a registered workspace. |

---

<p align="center">
  <a href="README.md">🏠 Home</a> ·
  <a href="USER_GUIDE.md">📖 User Guide</a> ·
  <a href="mcp_first_skill.md">🎯 MCP-First Skill Profile</a>
</p>
