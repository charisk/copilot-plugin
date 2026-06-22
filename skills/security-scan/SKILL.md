---
name: security-scan
description: Scan the current repository for security vulnerabilities and print a findings report. Use this when asked to scan for security issues, find vulnerabilities, or perform a security review of the repo.
---

# Security Vulnerability Scan

Scan the repository for security vulnerabilities and print a clear findings report. Do not modify any code — this skill is read-only and only reports.

## Process

1. **Understand the project.** Identify the languages, frameworks, and package managers in use (e.g. `package.json`, `requirements.txt`, `go.mod`, `Gemfile`, `pom.xml`).

2. **Check dependencies for known vulnerabilities.** Run the ecosystem's native auditing tool when available, for example:
   - Node.js: `npm audit` (or `pnpm audit` / `yarn audit`)
   - Python: `pip-audit`
   - Go: `govulncheck ./...`
   - Ruby: `bundle audit`
   If a tool is not installed, note it as a gap rather than failing.

3. **Scan source code for common issues.** Review the code for:
   - Hardcoded secrets, API keys, tokens, or passwords
   - Injection risks (SQL, command, template) from unsanitized input
   - Insecure deserialization and unsafe `eval`/`exec` usage
   - Weak or misused cryptography (e.g. MD5/SHA1 for passwords, hardcoded keys)
   - Path traversal and unsafe file handling
   - Missing authentication/authorization checks on sensitive operations
   - Insecure defaults (debug mode, permissive CORS, disabled TLS verification)

4. **Check configuration and CI.** Review Dockerfiles, CI workflows, and environment configs for exposed secrets or insecure settings.

## Output

Print a findings report with this structure:

- **Summary** — one paragraph on overall risk posture.
- **Findings** — a list, each with:
  - **Title**
  - **Severity** (Critical / High / Medium / Low)
  - **Location** (file and line where applicable)
  - **Description** — what the issue is and why it matters
  - **Recommendation** — how to fix it
- **Gaps** — any scans that could not run (e.g. missing tooling).

If no issues are found, say so explicitly and list what was checked.
