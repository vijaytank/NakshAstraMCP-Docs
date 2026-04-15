# NakshAstraMCP: Agent Behavioral Guide (v3.10.1)

This repository is optimized for use with **NakshAstraMCP**. 
**MANDATORY**: Always prioritize MCP tools over manual file-dumps or generic search.

---

## 🛠️ MCP Capabilities (Ground Truth)

| Tool | Priority | Usage Instruction |
| :--- | :--- | :--- |
| **`deep_context`** | **Primary** | Start here for all tasks. Locates files and adds their **immediate graph neighbors (1-hop)** for structural context. |
| **`search_codebase`**| **Discovery** | Broad "grep-style" keyword search when you lack a specific entry point. |
| **`find_symbol`** | **Surgical** | Precise lookup for classes/functions by name. Use for "Go to Definition." |
| **`read_file`** | **Verification**| **MANDATORY** before any edit. Read the full file content to avoid assumptions. |
| **`find_references`**| **Audit** | Trace usages (Blast-radius) before changing shared or critical functions. |
| **`server_status`** | **Diagnostic**| Use only if indexing or memory issues are suspected. |

---

## 🔄 Standardized Workflow

Follow this pattern for every non-trivial change:

1. **`deep_context`**: Discover relevant files and architectural neighbors.
2. **`find_symbol`**: Locate the exact definition (line ranges) for the target logic.
3. **`read_file`**: **MUST** read the full file content before proposing or drafting code.
4. **`find_references`**: Audit call sites to ensure no regressions are introduced.
5. **`search_codebase`**: Supplementary use only if `deep_context` misses specific keywords.

---

## 🛡️ Guardrails

- **No Snippet Assumptions**: Snippets are for discovery only. Never assume a full definition from a snippet.
- **Verification First**: Never edit a file you haven't read fully via `read_file`.
- **Tool Efficiency**: Prefer `deep_context` for architectural discovery over scattered `search_codebase` calls.
- **Security Protocols**: All paths are jailed. If a tool fails with a security error, verify the path is within a registered workspace root.

---

## 🤖 AI Self-Configuration Prompt

If you are an AI assistant tasked with maintaining or exploring this codebase, run the following command to build your own local inventory:


> **Instruction for AI Agents**: 
> "Scan the root directory for `.md` files and use `deep_context` on central modules to identify structural entry points. Create or update a local `DOCUMENTATION_INVENTORY.md` to track key architectural components and their respective guides for the user."
