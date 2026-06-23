# security-scan

A simple GitHub Copilot CLI **plugin** that scans a repository for security
vulnerabilities and prints a findings report.

It is skills-only (no MCP server, no binaries) and strictly read-only — it uses only Copilot's file-reading and search tools (no shell commands) to inspect dependencies and source code, then reports. It never runs terminal commands and never modifies your code.

## Structure

```
.
├── .github/
│   └── plugin/
│       └── plugin.json          # Plugin manifest
├── agents/
│   └── main.md                  # Entry agent
└── skills/
    └── security-scan/
        └── SKILL.md             # The security scan skill
```

## Agent

| Agent  | Description                                                                       |
| ------ | -------------------------------------------------------------------------------- |
| `main` | Entry agent for the plugin. Reuses the `security-scan` skill; read-only.          |


## Skills

| Skill           | Description                                                         |
| --------------- | ----------------------------------------------------------------- |
| `security-scan` | Scan the repo for vulnerabilities and print a findings report.     |

## Usage

Once the plugin is installed, ask Copilot CLI something like:

```
scan for security vulnerabilities in this repo and print your findings
```

Copilot loads the `security-scan` skill automatically, inspects dependencies and source code, then prints a report with a risk summary, a list of findings (severity, location, recommendation), and any scan gaps.

### Results in CI / workflow logs

When the plugin runs inside the coding-agent runtime (e.g. a GitHub Actions job), the agent's full report is sent to the progress channel rather than stdout, so the workflow log only shows one-line tool-call summaries. To keep the outcome visible there, the skill emits a couple of short marker lines at the end of the scan via the read-only Grep tool, for example:

```
SCAN SUMMARY: 7 finding(s) - 1 Critical, 3 High, 2 Medium, 1 Low
TOP FINDING: [Critical] SQL injection @ routes/login.ts:42
```

These markers are truncated to ~120 characters by the host. The full report is still produced in full for callers that read the progress channel or session state.

## Installing locally

From the Copilot CLI, install this plugin directly from its repository:

```
/plugin install charisk/copilot-plugin
```

Or point the CLI at a local checkout of this directory.
