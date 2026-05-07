
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
- **Security Protocols**: All paths are jailed. If a tool fails with a security error, verify the path is within a registered workspace.


---


## 🤖 AI Self-Configuration Prompt
If you are an AI assistant tasked with maintaining or exploring this codebase, execution of the following sequence is required for initial synchronization. MANDATORY: You must use NakshAstraMCP tools specifically for this discovery; generic searches are prohibited.

> **Instruction for AI Agents**: 

- **Run Below Prompt once when setting up new repository**

"Perform an initial environment discovery by scanning the root directory for .md files and using deep_context on central modules to identify structural entry points. Create or update a local DOCUMENTATION_INVENTORY.md to track key architectural components and their respective guides for the user. CRITICAL: Upon successful completion, add a direct reference/link to the DOCUMENTATION_INVENTORY.md within the relevant documentation section of agent.md, then remove this 'AI Self-Configuration Prompt' section entirely, while strictly preserving all other behavioral guidelines and MCP instruction sets within the file."

---
IMPORTANT INSTRUCTIONS (follow these strictly):
• DO NOT write any greeting, introduction, or conclusion. Start with the answer immediately. No 'Great question!' or 'I hope this helps!' - if you do, you have FAILED.
