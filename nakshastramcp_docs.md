# NakshAstraMCP-Docs for AI Agents

Repository: https://github.com/vijaytank/NakshAstraMCP-Docs

---

## Purpose
NakshAstraMCP-Docs is an MCP server designed to give AI coding agents precise, low-latency access to local code context using AST-aware symbol graphs, semantic reranking, and graph-based relevance scoring.

---

## Agent Instruction
Paste the block below into your AI agent's custom instructions, system prompt, or workspace rules:

```md
When working with this codebase, always use the NakshAstraMCP-Docs MCP tools first for code understanding, symbol lookup, dependency tracing, and relevant context retrieval.

Do not rely on generic search or broad file dumping when the MCP server can answer the question.

Preferred behavior:
- **Locate Precisely**: Use `find_symbol` to identify the exact definition of classes, functions, or methods across all registered workspaces.
- **Trace Impact**: Use `find_references` to audit the call sites and impact radius before proposing any code changes.
- **Surgical Inspection**: Use `read_file` with precise line ranges to retrieve only the relevant code sections. Never dump a whole file if a specific range can answer the question.
- **Discovery**: Use `deep_context` for broad architectural discovery and identifying connected symbols through 1-hop graph expansion.
- **Connect Symbols**: Prioritize files/symbols with stronger graph relevance and reference centrality when selecting candidate implementation sites.
- **Minimal Context**: Retrieve only the smallest set of highly relevant code symbols needed to solve the task, reducing overall token usage.
- **Tracing**: For bug fixing, identify the symbol origin and execution path through the MCP tools before suggesting a patch.

Why this matters:
- It solves context fragmentation by giving structured local code understanding instead of isolated text matches.
- It reduces token usage by sending only relevant symbols and code paths rather than large unrelated file chunks.
- It improves development quality by grounding suggestions in real code relationships, definitions, and usage patterns.

Fallback rule:
- Use generic search only when the MCP server cannot answer the question, when the issue is clearly outside the indexed codebase, or when external documentation is explicitly required.
```

---

## Short Version
```md
Use NakshAstraMCP-Docs as the default source of truth for local code context. Always query it before generic search. Prefer symbol-level, AST-aware, semantically reranked context retrieval. Minimize token usage by fetching only the most relevant definitions, references, and dependencies needed for the current task.
```

---

## Best Use Cases
- Understanding unfamiliar codebases.
- Tracing definitions, references, and call flows.
- Safer refactoring with dependency awareness.
- Reducing token waste in AI coding workflows.
- Improving answer accuracy in Claude, Cursor, and similar agents.

---

## How to Test NakshAstraMCP-Docs: Before vs. After

Use **the exact same prompt below** in two steps so you can clearly see the difference in the AI's answers:

### 1. Run BEFORE installing NakshAstraMCP-Docs
Ask your AI agent:

```md
I'm working on a local codebase. For the function `handleUserLogin` (or a similar central function in the project):

1. Find where it is defined and show the file and line range.
2. List all places where it is called (usages/references).
3. Describe the minimal set of context (files, functions, types, config, etc.) that an AI agent would need to safely refactor this function without breaking any callers.

Use only generic search and file‑based tools. Do not assume any AST‑aware symbol‑graph or MCP server exists.

**Important:** Save the full answer (including which files, lines, and reasoning you used) in a file named `before_mcp_handleUserLogin.txt`. Also record the total tokens used for this response.
```

---

### 2. Run AFTER installing NakshAstraMCP-Docs
Use the **exact same question** again, but now the agent has access to the MCP server:

```md
I'm working on a local codebase. For the function `handleUserLogin` (or a similar central function in the project):

1. Find where it is defined and show the file and line range.
2. List all places where it is called (usages/references).
3. Describe the minimal set of context (files, functions, types, config, etc.) that an AI agent would need to safely refactor this function without breaking any callers.

Use NakshAstraMCP-Docs (the local MCP server for code context) as the primary source. Prefer AST‑aware symbol graphs, semantic reranking, and graph‑based relevance over raw file search.

**Important:** Save the full answer (including which MCP tools were used and how the context is different from generic search) in a file named `after_mcp_handleUserLogin.txt`. Also record the total tokens used for this response.
```

---

## Comparison Checklist
When comparing `before_mcp_handleUserLogin.txt` and `after_mcp_handleUserLogin.txt`, evaluate:
- **Accuracy**: Are definitions and references precise?
- **Completeness**: Does the MCP version cover all relevant files and dependencies?
- **Noise Reduction**: Are irrelevant files excluded?
- **Token Usage**: How many tokens were consumed before vs. after?
- **Reasoning Quality**: Is the MCP answer grounded in actual code relationships instead of text matches?

---

By following this process, you can clearly see how NakshAstraMCP-Docs improves context precision, reduces token waste, and enhances AI-assisted coding workflows.