# security-scan

A simple GitHub Copilot CLI **plugin** that scans a repository for security
vulnerabilities and prints a findings report.

It is skills-only (no MCP server, no binaries) — it drives Copilot's built-in
tools to inspect dependencies and source code, then reports. It never modifies
your code.

## Structure

```
.
├── .github/
│   └── plugin/
│       └── plugin.json          # Plugin manifest
└── skills/
    └── security-scan/
        └── SKILL.md             # The security scan skill
```

## Skills

| Skill           | Description                                                         |
| --------------- | ----------------------------------------------------------------- |
| `security-scan` | Scan the repo for vulnerabilities and print a findings report.     |

## Usage

Once the plugin is installed, ask Copilot CLI something like:

```
scan for security vulnerabilities in this repo and print your findings
```

Copilot loads the `security-scan` skill automatically, inspects dependencies and
source code, then prints a report with a risk summary, a list of findings
(severity, location, recommendation), and any scan gaps.

## Installing locally

From the Copilot CLI, install this plugin directly from its repository:

```
/plugin install charisk/copilot-plugin
```

Or point the CLI at a local checkout of this directory.
