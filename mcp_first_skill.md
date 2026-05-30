# NakshAstraMCP-First Developer Rule Profile (v3.19.0)

You are configured to use **NakshAstraMCP** as your primary engineering intelligence engine.
You MUST strictly follow these rules, priorities, and workflow protocols for all development, refactoring, and debugging tasks.

---

## 🛠️ Mandatory Tool Priorities

You are PROHIBITED from using generic shell grep searches or plain file dumping. You MUST use NakshAstraMCP tools first:

1.  **`deep_context` (Primary Search & Discovery)**:
    *   **Usage**: Use this for high-level questions, system design planning, and module mapping.
    *   **Behavior**: It semantically retrieves symbols and expands search boundaries to immediate 1-hop physical neighbor dependencies.
2.  **`find_symbol` (Surgical Definition Target)**:
    *   **Usage**: Locate the exact physical file and line range where a class, function, or method is defined.
3.  **`find_references` (Blast-Radius Audit)**:
    *   **Usage**: Trace all callers, usages, and import dependencies of a custom target symbol before making changes.
4.  **`read_file` (Surgical Code Reader)**:
    *   **Usage**: Read targeted, precise line-number ranges. 
    *   **Rule**: Never read an entire file if a specific range (e.g. `start_line` to `end_line`) can answer the query.
5.  **`search_codebase` (Keyword Fallback)**:
    *   **Usage**: Use only as a last resort if no structural symbol or AST node indexes match your query.

---

## 🛡️ Strategic Guardrails

*   ❌ **NO Definition Guessing**: Never assume the content or signature of a class or function based on search snippets. You must surgically read the file boundaries using `read_file` before suggesting any code changes.
*   ❌ **NO Plain File Dumps**: Never print or read the entire content of a file. Use specific `start_line` and `end_line` ranges to keep token usage minimal and prompt density high.
*   ✅ **Mandatory Call-Site Auditing**: You are strictly forbidden from modifying any function, parameter list, or class signature unless you have audited all call sites using `find_references` to ensure no callers break.
*   ✅ **Architectural Synthesis Alignment**: Read `nakshastra-out/NAKSHASTRA_REPORT.md` (or invoke `generate_report`) during planning to align with physical modules and PageRank centrality scoring.
*   ✅ **Sandboxed Local Execution**: Verify all paths remain strictly inside registered workspace boundaries. Never execute MCP tools inside terminals or shell scripts.

---

## 🔄 Standard Operational Workflows

### 🐛 Debugging Loop
1.  **Locate**: Run `find_symbol` to identify the buggy handler or class file.
2.  **Audit**: Run `find_references` on the target symbol to trace incoming variables, parameters, and execution paths.
3.  **Read**: Run `read_file` with precise line numbers around the bug origin.
4.  **Resolve**: Draft the targeted correction and audit caller structures before applying.

### 📦 Safe Refactoring Flow
1.  **Map**: Run `deep_context` to understand surrounding dependencies.
2.  **Locate**: Run `find_symbol` to fetch structural nesting boundaries.
3.  **Audit**: Run `find_references` to trace all callers and import modules.
4.  **Refactor**: Propose structural changes with clear line diff highlights.

### 🚀 Feature Implementation Flow
1.  **Discover**: Run `deep_context` to identify parent entry modules.
2.  **Target**: Use `find_symbol` and `find_references` to target insertion points with the highest PageRank centrality scores.
3.  **Implement**: Code the feature, then build corresponding regression test suites.
