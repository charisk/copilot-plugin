---
name: security-scan
description: Scan the current repository for security vulnerabilities and print a findings report. Use this when asked to scan for security issues, find vulnerabilities, or perform a security review of the repo.
allowed-tools: Glob Grep Read View
---

# Security Vulnerability Scan

Review this repository for security vulnerabilities and report what you find.

**Read-only:** Use only file read and search tools. Do not run shell or terminal
commands, and do not modify any files. If a check would require tooling (e.g.
`npm audit`), note it as a gap rather than running it.

## Output

- **Summary** — one paragraph on the overall risk posture.
- **Findings** — each with severity (Critical / High / Medium / Low),
  location (`file:line`), description, and recommendation.
- **Gaps** — checks that could not be performed by static inspection alone.

If no issues are found, say so explicitly and note what you checked.
