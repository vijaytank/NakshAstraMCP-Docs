# 🛡️ Security Policy — NakshAstraMCP

We are committed to delivering a highly secure, private-first experience for local codebase context retrieval. All processing is local-only — your code never leaves your machine.

---

## ✅ Supported Versions

Security updates and maintenance patches are actively applied to these releases:

| Version | Status |
| :--- | :--- |
| **v3.20.x** (Current) | ✅ **Active — Fully Supported** |
| **v3.19.x** | ✅ Active Security Support |
| **v3.16.x – v3.18.x** | ✅ Maintained (critical patches only) |
| **v3.0.0 – v3.15.x** | ❌ Outdated — Please upgrade |
| **< v3.0.0** | ❌ Unsupported |

> [!IMPORTANT]
> Always run the latest release. Outdated versions may contain unpatched vulnerabilities and will not receive security fixes.

---

## 🔒 Reporting a Security Vulnerability

We prioritize local privacy and workspace protection. If you identify a security issue, vulnerability, sandbox escape, or path traversal risk — **do not open a public GitHub issue.**

### Private Disclosure Process

1. **Email**: Send your report directly to [vijaytank132@gmail.com](mailto:vijaytank132@gmail.com)
2. **Subject Line**: Use exactly: `[SECURITY VULNERABILITY] - NakshAstraMCP`
3. **Include in your report**:
   - A detailed description of the suspected vulnerability and its potential impact.
   - Clear, step-by-step reproduction instructions (with minimal sample code or commands where applicable).
   - Your operating system, terminal environment, and NakshAstraMCP version:
     ```powershell
     nakshastramcp --version
     ```

---

## ⚡ Our Response Protocol

| Stage | Commitment |
| :--- | :--- |
| **Acknowledgment** | We will confirm receipt of your private email within **48 hours** |
| **Investigation** | We will reproduce and analyze the reported behavior internally |
| **Resolution** | We will implement a patch and notify you when a secure hotfix release is available for download |
| **Confidentiality** | Your identity will remain completely confidential throughout the entire process |

---

## 🔐 Built-In Security Architecture

NakshAstraMCP is designed with security-first principles:

| Feature | Description |
| :--- | :--- |
| **Path Jail** | All file read and index operations are validated against registered workspace roots — no traversal outside boundaries |
| **Secret Detection** | Integrated scanner (using `detect-secrets` with regex fallback) prevents API keys, tokens, and passwords from being indexed |
| **User-Space Only** | No administrator or root privileges are ever requested |
| **Local-Only Processing** | No codebase data, AST graphs, or analysis results leave your machine |
| **Grammar Lock (TOFU)** | SHA-256 hashes of Tree-sitter grammar wheels are verified on every startup to detect tampering |
| **SSL Environment Repair** | Invalid SSL certificate paths from other tools (e.g., PostgreSQL) are automatically removed from the environment to prevent SSL failures |

---

<p align="center">
  <a href="README.md">🏠 Home</a> ·
  <a href="SETUP.md">🚀 Setup Guide</a> ·
  <a href="DISCUSSIONS_WELCOME.md">💬 Discussions</a>
</p>
