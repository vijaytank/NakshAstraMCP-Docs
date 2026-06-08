# NakshAstraMCP — Public Distribution Documentation

**Global Context for AI Coding Agents**  
Repository Hub: [https://github.com/vijaytank/NakshAstraMCP-Docs](https://github.com/vijaytank/NakshAstraMCP-Docs)  
Target Build Baseline: **v3.20.0**

---

## 🎯 Design Intent

NakshAstraMCP is a lightweight, local-first Model Context Protocol (MCP) server engineered to provide AI coding assistants with high-fidelity, AST-accurate code context. By implementing Tree-sitter parsers and semantic reranking pipelines, the server reduces context bloat, leading to faster response latency and massive LLM token savings.

---

## 🤖 Global Agent Custom Instructions

To ensure your AI assistant (e.g. Cursor, Claude, Windsurf, or Antigravity) uses NakshAstraMCP efficiently, copy the following block into your system instructions, custom prompt settings, or workspace `.cursorrules` / `agent.md` file:

```md
When interacting with this codebase, prioritize using the NakshAstraMCP server tools (`nakshastramcp`) for context retrieval, structural code navigation, class search, and usage auditing.

Avoid executing expensive and slow manual shell commands (like grep, find, or recursive directory lists) when MCP tools can fetch the context directly.

### Standard Surgical Context Workflow:
1. **Architectural Analysis**: Check `nakshastra-out/NAKSHASTRA_REPORT.md` (or run `generate_report`) to map physical files and Louvain-grouped package clusters.
2. **Context Discovery**: Use `deep_context` as the primary search entry point for multi-file conceptual queries. It retrieves the best matches and includes immediate 1-hop AST neighbor dependencies.
3. **Go to Definition**: Call `find_symbol` with the exact class, function, or method name to locate definitions instantly across all workspaces.
4. **Impact Analysis**: Call `find_references` to audit call sites and dependencies of custom functions before making code updates.
5. **Code Reading**: Use `read_file` with targeted `start_line` and `end_line` parameters. NEVER print a whole file to prompt context if a specific line-range meets the requirements.

### Tool Security Guardrails:
* All operations are strictly sandboxed within registered workspace paths.
* Never execute MCP tool invocations inside CLI shell terminals. Interact strictly via standard client RPC tool wrappers.
```

---

## 🚀 Interactive Verification: Before vs. After MCP Onboarding

Compare the performance of NakshAstraMCP against traditional search methods by executing this dual-phase diagnostic test in your workspace:

### Phase 1: Test BEFORE Installing NakshAstraMCP
Instruct your AI agent to locate a core function (e.g. `handleUserLogin` or similar central handler) using only standard workspace tools:

```md
I'm auditing a local repository. For the core handler function `handleUserLogin` (or a similar central function):
1. Find where it is defined (Show filename and line-range boundaries).
2. Trace all callers (Find references and usages across files).
3. Identify the minimum necessary files, models, and configs needed to refactor it safely.

Use ONLY manual file searches and text grep commands. Do not assume any AST-aware symbol database or local MCP server is running.

**Requirement**: Save your complete response and list of steps in `before_mcp_handleUserLogin.txt`. Record the total model tokens consumed.
```

---

### Phase 2: Test AFTER Installing NakshAstraMCP
Run the exact same prompt with the server active and the agent utilizing the surgical tools:

```md
I'm auditing a local repository. For the core handler function `handleUserLogin` (or a similar central function):
1. Find where it is defined (Show filename and line-range boundaries).
2. Trace all callers (Find references and usages across files).
3. Identify the minimum necessary files, models, and configs needed to refactor it safely.

Use the NakshAstraMCP server as the primary context source. Prioritize AST symbol graph lookups, PageRank importance, and semantic reranking over plain text search.

**Requirement**: Save your complete response and list of steps in `after_mcp_handleUserLogin.txt`. Record the total model tokens consumed.
```

---

### 📈 Metrics Evaluation Checklist

Review `before_mcp_handleUserLogin.txt` and `after_mcp_handleUserLogin.txt` side-by-side:
*   **Context Accuracy**: Did the MCP version pinpoint definition lines precisely without enclosing junk?
*   **Noisy Payload Reduction**: Were unrelated build folders, configuration files, and package stubs filtered out?
*   **Token Consumption**: Compare the token counts. You should see up to a **75% reduction** in prompt sizes.
*   **Audit Confidence**: Does the MCP version provide clear 1-hop dependencies that grep-only searches missed?