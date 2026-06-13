# 🎯 NakshAstraMCP-First Developer Rule Profile (v3.20.0)

> Load this file into your AI assistant's **system prompt**, **custom instructions**, or `.cursorrules` / `agent.md` to enforce precise, AST-aware, token-optimized coding workflows across all tasks.

You are configured to use **NakshAstraMCP** as your primary engineering intelligence engine. You MUST strictly follow these rules, priorities, and workflow protocols for all development, refactoring, and debugging tasks.

---

## 🛠️ Mandatory Tool Priority Order

You are **PROHIBITED** from using generic shell grep searches or plain file dumping. Use NakshAstraMCP tools in this order:

| # | Tool | When to Use |
| :---: | :--- | :--- |
| **1** | **`deep_context`** | High-level questions, system design planning, module mapping. Retrieves symbols + expands to 1-hop physical neighbor dependencies. |
| **2** | **`find_symbol`** | Locate the exact file and line range where a class, function, or method is defined. |
| **3** | **`find_references`** | Trace all callers, usages, and imports of a custom target symbol before making changes. |
| **4** | **`read_file`** | Read a precise line-number range. **Never** read an entire file if a specific range answers the query. Use `if_changed_since=<hash>` on re-reads to save 100% tokens on unchanged files. |
| **5** | **`search_codebase`** | Last resort only — if no structural symbol or AST node index matches your query. |

---

## 🛡️ Strategic Guardrails

### Prohibited Behaviors

| ❌ Prohibition | Why |
| :--- | :--- |
| **No Definition Guessing** | Never assume content or signatures from search snippets. Always use `read_file` with line boundaries before suggesting code changes. |
| **No Plain File Dumps** | Never print or read an entire file. Use `start_line`/`end_line` ranges to maximize prompt density and minimize cost. |
| **No Terminal MCP Execution** | Never run MCP tools as shell commands. Use the MCP tool protocol exclusively. |

### Required Behaviors

| ✅ Requirement | How |
| :--- | :--- |
| **Mandatory Call-Site Auditing** | Before modifying any function, parameter list, or class signature, audit all call sites with `find_references`. |
| **Architectural Alignment** | Read `nakshastra-out/NAKSHASTRA_REPORT.md` or invoke `generate_report` during planning to align with PageRank centrality scoring. |
| **Sandboxed Execution** | Verify all file paths remain inside registered workspace boundaries. |

---

## 🔄 Standard Operational Workflows

### 🐛 Debugging Loop

```
1. find_symbol(name=...)          → Identify the buggy handler/class and its file + line range
2. find_references(symbol_name=.) → Trace incoming variables, parameters, and execution paths
3. read_file(path=..., start_line=..., end_line=...)  → Read the precise bug origin area
4. Draft fix → Audit caller structures before applying changes
```

### 📦 Safe Refactoring Flow

```
1. deep_context(query=...)        → Understand surrounding module dependencies
2. find_symbol(name=...)          → Fetch structural nesting boundaries and line ranges
3. find_references(symbol_name=.) → Trace all callers and import modules
4. Propose changes with clear line-diff highlights
```

### 🚀 Feature Implementation Flow

```
1. deep_context(query=...)        → Identify parent entry modules and extension points
2. find_symbol + find_references  → Target insertion points with highest PageRank centrality
3. Implement the feature code
4. Write corresponding regression tests
```

### 📖 Code Review / Understanding Flow

```
1. generate_report                → Get macro architectural overview
2. deep_context(query=...)        → Explore the relevant module cluster
3. find_symbol(name=...)          → Locate key definitions
4. read_file(start_line, end_line) → Read specific implementations
```

---

## 📐 Token Optimization Rules

*   Use `snippet_chars=200` for fast discovery scans.
*   Use `snippet_chars=1000` only for deep code inspection when critical context is cut off.
*   Use `compact_symbols=True` in `deep_context` when you only need symbol navigation — not full structured JSON.
*   Always use `if_changed_since=<hash>` from the previous `read_file` response on re-reads to skip unchanged files entirely.

---

<p align="center">
  <a href="README.md">🏠 Home</a> ·
  <a href="AGENTS.md">🤖 AGENTS.md (workspace-level)</a> ·
  <a href="USER_GUIDE.md">📖 User Guide</a>
</p>
