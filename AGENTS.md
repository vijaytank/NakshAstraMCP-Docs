# NakshAstraMCP: Agent Instructions (v3.13.0)

This repository is optimized for use with **NakshAstraMCP**. 
**MANDATORY**: Always prioritize MCP tools over manual file-dumps or generic search.

---

## 🛠️ MCP Capabilities (Ground Truth)

| Tool | Priority | Usage Instruction |
| :--- | :--- | :--- |
| **`deep_context`** | **Primary** | Start here for all tasks. Locates files and adds their **immediate graph neighbors (1-hop)** for structural context. Strongly prefer over scattered `search_codebase` calls. |
| **`search_codebase`**| **Discovery** | Broad "grep-style" keyword search when you lack a specific entry point. |
| **`find_symbol`** | **Surgical** | Precise lookup for classes/functions by name. Use for "Go to Definition." |
| **`read_file`** | **Verification**| **MANDATORY** before any edit. Surgically read the relevant target block range (using line limits) to avoid assumptions. |
| **`find_references`**| **Audit** | Trace usages (Blast-radius) before changing custom functions. Avoid calling on common utilities / standard libraries to save tokens. |
| **`generate_report`** | **Map** | Produce a macro-level architectural report of the workspace. |
| **`server_status`** | **Diagnostic**| Use only if indexing or memory issues are suspected. |

---

## 📂 Workspace Artifacts

NakshAstraMCP generates architectural insights in the following directory:
- **`nakshastra-out/`**: Contains synthesis reports and interactive graphs:
  - `nakshastra-out/NAKSHASTRA_REPORT.md`: A comprehensive, macro-level architectural walkthrough of the repository, including key file clusters, PageRank matrices, and import dependencies.
  - `nakshastra-out/graph.json`: The raw symbol relationship graph in JSON format, which can be visualized.

---

## 🔄 Standardized Workflow

Follow this pattern for every non-trivial change:

1. **`generate_report`**: (Optional) Check `nakshastra-out/NAKSHASTRA_REPORT.md` for macro-view.
2. **`deep_context`**: Discover relevant files and architectural neighbors.
3. **`find_symbol`**: Locate the exact definition (line ranges) for the target logic.
4. **`read_file`**: **MUST** read the relevant target code block or line range (with enclosing context) before proposing edits.
5. **`find_references`**: Audit call sites only on custom functions (avoid common libraries/utils) to prevent token waste.

---

## 🛡️ Guardrails

- **No Snippet Assumptions**: Snippets are for discovery only. Never assume a full definition from a snippet.
- **Verification First**: Never edit code blocks you haven't read fully in their local context. Use line-range reads surgically to save tokens.
- **Tool Efficiency**: Prefer `deep_context` for architectural discovery over scattered `search_codebase` calls.
- **Log and Output Truncation**: When analyzing logs or test outputs, do not read complete log streams. Focus exclusively on relevant failure stack traces or tail lines to minimize token footprint.
- **Security Protocols**: All paths are jailed. If a tool fails with a security error, verify the path is within a registered workspace.

---

## 🤖 AI Self-Configuration Prompt
If you are an AI assistant tasked with maintaining or exploring this codebase, execution of the following sequence is required for initial synchronization. MANDATORY: You must use NakshAstraMCP tools specifically for this discovery; generic searches are prohibited.

> **Instruction for AI Agents**: 

- **Run Below Prompt once when setting up new repository**

"Perform an initial environment discovery by scanning the root directory for .md files and using deep_context on central modules to identify structural entry points. Create or update a local DOCUMENTATION_INVENTORY.md to track key architectural components and their respective guides for the user. CRITICAL: Upon successful completion, add a direct reference/link to the DOCUMENTATION_INVENTORY.md within the relevant documentation section of AGENTS.md, then remove this 'AI Self-Configuration Prompt' section entirely, while strictly preserving all other behavioral guidelines and MCP instruction sets within the file."

---
IMPORTANT INSTRUCTIONS (follow these strictly):
• DO NOT write any greeting, introduction, or conclusion. Start with the answer immediately. No 'Great question!' or 'I hope this helps!' - if you do, you have FAILED.

