---
name: security-scan
description: Scan the current repository for security vulnerabilities and print a findings report. Use this when asked to scan for security issues, find vulnerabilities, or perform a security review of the repo.
allowed-tools: Glob Grep Read View
---

# Security Vulnerability Scan

Review this repository for security vulnerabilities and report what you find.

**Read-only:** Use only file read and search tools. Do not run shell or terminal commands, and do not modify any files. If a check would require tooling (e.g. `npm audit`), note it as a gap rather than running it.

## Output

Produce the following sections in order:

1. **Summary** — one paragraph on the overall risk posture.
2. **Findings** — each with severity (Critical / High / Medium / Low), location (`file:line`), description, and recommendation.
3. **Gaps** — checks that could not be performed by static inspection alone.
4. **JSON findings** — a fenced `json` code block containing an array of every finding. Each object must have exactly these fields:
   - `title` — short name for the finding
   - `description` — what the issue is and why it matters
   - `location` — `"file:line"` string (use `"unknown"` if no specific location)
   - `severity` — one of `"Critical"`, `"High"`, `"Medium"`, `"Low"`

   Example:
   ```json
   [
     {
       "title": "SQL injection in login handler",
       "description": "User input is directly concatenated into a SQL query without sanitization, allowing an attacker to read or modify the database.",
       "location": "routes/login.ts:42",
       "severity": "Critical"
     }
   ]
   ```
   Emit an empty array `[]` if there are no findings.

If no issues are found, say so explicitly and note what you checked.

## Surfacing results in the run log

When this skill runs inside the coding-agent runtime (e.g. CI), your report text is **not** written to the workflow log — only one-line tool-call summaries are. To make the outcome visible there, after you finish the report, emit summary markers by calling the **Grep** tool purely for its logged summary line. (The host logs the `pattern` you pass; the search result itself is irrelevant and will likely match nothing — that is expected.)

Make these calls last, after the report:

1. **Counts** — call Grep with `pattern` set to:
   `SCAN SUMMARY: <N> finding(s) - <c> Critical, <h> High, <m> Medium, <l> Low`
2. **One example** — call Grep again with `pattern` set to:
   `TOP FINDING: [<severity>] <short title> @ <file:line>`
   Pick the highest-severity finding. If there are no findings, skip this call and use `SCAN SUMMARY: 0 findings` for step 1.

Keep each marker on a single line and **under 120 characters** — the host truncates anything longer. Use plain text (avoid regex metacharacters like `[](){}` where you can; they don't break logging but keep the line readable). These Grep calls are only a logging mechanism — still produce the full report described above.
