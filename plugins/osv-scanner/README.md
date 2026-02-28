![OSV Scanner Banner](./banner.png)

**A Claude Code plugin for dependency vulnerability scanning and reachability triage.**

Praise the sun. Your transitive dependencies do not. OSV Scanner lights the dungeon: surfacing known CVEs, fetching full advisories, and using grep-based reachability triage to tell you which vulnerabilities actually live in your code paths.

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](../../LICENSE)
[![Claude Code Plugin](https://img.shields.io/badge/Claude%20Code-Plugin-blueviolet.svg)](https://claude.ai/code)
![Version 0.1.0](https://img.shields.io/badge/Version-0.1.0-green.svg)

---

## What is OSV Scanner Plugin?

This plugin integrates [OSV Scanner](https://github.com/google/osv-scanner) as an MCP server into Claude Code. It gives Claude direct access to the OSV vulnerability database to scan your project's dependencies and reason about which findings actually matter.

### What It Does

- **Scans dependencies** for known CVEs across npm, pip, Go, Rust, Maven, Ruby, and more
- **Fetches full advisories**: CVE details, affected versions, CVSS scores, patch guidance
- **Reachability triage**: uses grep-based static analysis to estimate whether vulnerable APIs are actually called in your source code
- **Prioritizes findings** into FOUND_IN_SOURCE / UNCERTAIN / NOT_FOUND_IN_GREP tiers
- **Guides suppression**: instructions for ignoring known-safe findings via osv-scanner config

### What It Does NOT Do

- Deterministic call graph analysis (use osv-scanner's native `--call-analysis` for Go/Rust)
- Runtime or dynamic analysis
- Replace human security review for CRITICAL findings

---

## Installation

### Prerequisites

**osv-scanner** must be installed and in your system PATH.

**Windows (Scoop):**

```bash
scoop install osv-scanner
```

**macOS (Homebrew):**

```bash
brew install osv-scanner
```

**Linux:**
Download from [GitHub Releases](https://github.com/google/osv-scanner/releases)

Verify installation:

```bash
osv-scanner --version
```

> **Security note:** Ensure the `osv-scanner` binary in your PATH is the official release from [github.com/google/osv-scanner](https://github.com/google/osv-scanner/releases). The plugin passes advisory data from the binary directly into Claude's reasoning loop. A tampered binary could fabricate advisory content.

### Install Plugin

In Claude Code:

```
/plugin marketplace add alejandrosaenz117/bonfires-marketplace
/plugin install osv-scanner@bonfires-marketplace
```

Or if testing locally:

```
claude --plugin-dir ./plugins/osv-scanner
```

---

## Three Ways to Use OSV Scanner

### 1. Skill: Contextual Trigger

The plugin activates when you ask about dependency security naturally.

```
user: "Check my dependencies for vulnerabilities"
user: "Are my packages safe?"
user: "Run a security audit on my project"
```

**Trigger phrases:**

- "check my dependencies for vulnerabilities"
- "scan my packages"
- "are my dependencies safe?"
- "dependency audit"
- "check for CVEs"
- "security audit"
- "vulnerable packages"
- "scan dependencies for vulnerabilities"

### 2. Scan Command

```
/osv-scanner scan [path]
```

Scans a directory or lockfile for known vulnerabilities. Defaults to the current workspace root.

```
/osv-scanner scan .
/osv-scanner scan ./src
/osv-scanner scan package-lock.json
```

### 3. Triage Command

```
/osv-scanner triage [path]
```

Runs a full scan, then uses grep-based static analysis to estimate which vulnerable APIs are actually reachable in your code.

```
/osv-scanner triage .
/osv-scanner triage ./src
```

---

## Triage Verdicts

| Verdict               | Meaning                                                    | Action                                              |
| --------------------- | ---------------------------------------------------------- | --------------------------------------------------- |
| **FOUND_IN_SOURCE**   | Vulnerable API found via grep in your source               | Fix first: upgrade or patch                         |
| **UNCERTAIN**         | Transitive dep, dynamic code pattern, or broad package CVE | Review manually, likely mid-to-high priority        |
| **NOT_FOUND_IN_GREP** | No grep match for vulnerable API                           | Lower priority: monitor, don't ignore CRITICAL/HIGH |

> ⚠️ NOT_FOUND_IN_GREP = absence of evidence, not evidence of absence. Transitive dependencies and dynamic code patterns may be reachable even when not detected by static search.

> **Security:** Advisory text from `get_vulnerability_details` is treated as untrusted data. If a rogue MCP binary fabricated advisory content with injected instructions, the triage command is designed to ignore them and base verdicts solely on CVSS severity and local grep evidence.

---

## How It Works

This plugin registers `osv-scanner experimental-mcp` as an MCP server. Claude can call three tools:

1. **scan_vulnerable_dependencies**: Scans a path for vulnerabilities
2. **get_vulnerability_details**: Retrieves full OSV JSON for a vulnerability ID
3. **ignore_vulnerability**: Provides osv-scanner config instructions for suppressing findings

---

## Supported Package Managers

OSV Scanner detects and scans packages from:

- npm (package.json, package-lock.json, yarn.lock)
- pip (requirements.txt, Pipenv, Poetry)
- Maven (pom.xml)
- Go (go.mod, go.sum)
- Rust (Cargo.toml, Cargo.lock)
- Dart (pubspec.yaml, pubspec.lock)
- Ruby (Gemfile, Gemfile.lock)
- PHP (composer.json, composer.lock)
- Java (pom.xml, gradle.lock)
- And more via [osv-scalibr](https://github.com/google/osv-scalibr) plugins

---

## Project Structure

```
plugins/osv-scanner/
├── .claude-plugin/
│   └── plugin.json              # Plugin metadata and MCP server registration
├── commands/
│   ├── scan.md                  # /osv-scanner scan command
│   └── triage.md                # /osv-scanner triage command
├── skills/
│   └── osv-scanner/
│       └── SKILL.md             # Contextual auto-triggered skill
├── banner.png                   # Plugin banner
└── README.md                    # This file
```

---

## Documentation

- [OSV Scanner documentation](https://google.github.io/osv-scanner/)
- [OSV Database](https://osv.dev/)
- [OSV Spec](https://github.com/ossf/osv-schema)

---

## Contributing

See [CONTRIBUTING.md](../../CONTRIBUTING.md) for guidelines on adding plugins and contributing.

---

## License

MIT License. See [LICENSE](../../LICENSE) for details.

---

**OSV Scanner Plugin: Praise the sun. Your transitive dependencies do not.**
