# NakshAstraMCP — Global Agent Custom Instructions

**AI-Optimized Context for NakshAstraMCP-Powered Codebases**

Repository: [https://github.com/vijaytank/NakshAstraMCP-Docs](https://github.com/vijaytank/NakshAstraMCP-Docs)
Current Version: **v3.20.0**

---

## 🎯 Design Intent

NakshAstraMCP is a lightweight, local-first Model Context Protocol (MCP) server engineered to provide AI coding assistants with high-fidelity, AST-accurate code context.

By implementing Tree-sitter AST parsers, Tantivy full-text indexing, PageRank-based symbol importance scoring, and FlashRank semantic reranking pipelines, the server reduces context bloat — leading to faster AI response latency and dramatic LLM token cost savings.

---

## 🤖 Copy-Paste: Global Agent Custom Instructions

To ensure your AI assistant (e.g., Cursor, Claude, Windsurf, or Antigravity) uses NakshAstraMCP efficiently, copy the block below into your system instructions, custom prompt settings, `.cursorrules`, or workspace `agent.md` file:

```markdown
When interacting with this codebase, use the NakshAstraMCP server tools (`nakshastramcp`)
as the PRIMARY source for context retrieval, code navigation, symbol search, and usage auditing.

Avoid executing slow manual shell commands (grep, find, recursive ls/dir) when MCP tools
can retrieve the context directly.

## Standard Surgical Context Workflow

1. **Architectural Overview**: Check `nakshastra-out/NAKSHASTRA_REPORT.md` (or invoke
   `generate_report`) to map physical files and Louvain-grouped package clusters.

2. **Context Discovery**: Use `deep_context` as the primary search entry point for
   multi-file conceptual queries. It retrieves the best matches and includes immediate
   1-hop AST neighbor dependencies.

3. **Go to Definition**: Call `find_symbol` with the exact class, function, or method
   name to locate definitions instantly across all workspaces.

4. **Impact Analysis**: Call `find_references` to audit call sites and dependencies of
   custom functions before making code updates.

5. **Code Reading**: Use `read_file` with targeted `start_line` and `end_line` parameters.
   NEVER read an entire file if a specific line-range meets the requirement.
   Use `if_changed_since=<hash>` on re-reads to skip unchanged files entirely.

## Tool Security Guardrails

- All operations are sandboxed within registered workspace paths.
- Never execute MCP tools inside terminal/shell scripts. Use the MCP tool protocol only.
- When search results contain `[REDACTED: SENSITIVE CONTENT]`, do not attempt to read
  or expose those files.
```

---

## 🚀 Interactive Verification: Before vs. After MCP Onboarding

Validate the performance difference yourself by running this dual-phase diagnostic on your workspace.

### Phase 1: BEFORE Installing NakshAstraMCP

Ask your AI agent to locate a core function using only standard tools:

```markdown
I'm auditing a local repository. For the core handler function `handleUserLogin`
(or a similar central function in this codebase):

1. Find where it is defined (filename and exact line range).
2. Trace all callers and usages across files.
3. Identify the minimum files, models, and configs needed to safely refactor it.

Use ONLY manual file searches and text grep commands. Do NOT assume any AST-aware
symbol database or local MCP server is running.

**Save your complete response in `before_mcp_handleUserLogin.txt` and record the
total model tokens consumed.**
```

---

### Phase 2: AFTER Installing NakshAstraMCP

Run the exact same task with the MCP server active:

```markdown
I'm auditing a local repository. For the core handler function `handleUserLogin`
(or a similar central function in this codebase):

1. Find where it is defined (filename and exact line range).
2. Trace all callers and usages across files.
3. Identify the minimum files, models, and configs needed to safely refactor it.

Use the NakshAstraMCP server as the primary context source. Prioritize AST symbol
graph lookups, PageRank importance, and semantic reranking over plain text search.

**Save your complete response in `after_mcp_handleUserLogin.txt` and record the
total model tokens consumed.**
```

---

### 📈 Metrics Evaluation Checklist

Compare `before_mcp_handleUserLogin.txt` and `after_mcp_handleUserLogin.txt` side by side:

| Metric | What to Look For |
| :--- | :--- |
| **Context Accuracy** | Did the MCP version pinpoint definition lines without surrounding noise? |
| **Noisy Payload Reduction** | Were build folders, config files, and package stubs filtered out? |
| **Token Consumption** | Compare the recorded token counts — target is up to **75% reduction**. |
| **Audit Confidence** | Does the MCP version surface 1-hop dependencies that grep missed? |
| **Response Speed** | Was the MCP-assisted response generated faster? |

---

<p align="center">
  <a href="README.md">🏠 Home</a> ·
  <a href="USER_GUIDE.md">📖 User Guide</a> ·
  <a href="mcp_first_skill.md">🎯 MCP-First Skill Profile</a> ·
  <a href="AGENTS.md">🤖 AGENTS.md</a>
</p>