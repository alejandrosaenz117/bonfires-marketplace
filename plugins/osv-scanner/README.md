# OSV Scanner Plugin

Integrates [OSV Scanner](https://github.com/google/osv-scanner) as an MCP server into Claude Code. Scans project dependencies for known vulnerabilities using the OSV database.

## Features

- **Scan dependencies**: Check your project for known vulnerabilities in package lockfiles and manifests
- **Detailed vulnerability info**: Fetch full CVE/advisory details by ID
- **Security audit workflow**: Multi-step guided dependency security review
- **Configuration guidance**: Instructions for suppressing findings via osv-scanner config

## Installation

### Prerequisites

**osv-scanner** must be installed and in your system PATH.

Install via package manager:

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

## Usage

### Contextual Skill (Auto-trigger)

Ask Claude naturally about dependency security:
- "Check my dependencies for vulnerabilities"
- "Scan my project for CVEs"
- "Are my dependencies safe?"
- "Run a security audit on my packages"

### Explicit Command

```
/osv-scanner scan [path]
```

Scans a directory or file for vulnerabilities. Defaults to the current workspace root.

Examples:
```
/osv-scanner scan .
/osv-scanner scan ./src
/osv-scanner scan package-lock.json
```

### Get Vulnerability Details

Ask Claude or use the MCP tool directly:
- "Tell me more about CVE-2024-1234"
- "Get details for GHSA-xxxx-xxxx-xxxx"

## How It Works

This plugin registers the built-in `osv-scanner experimental-mcp` command as an MCP server. Claude can call three tools:

1. **scan_vulnerable_dependencies** — Scans a path for vulnerabilities
2. **get_vulnerability_details** — Retrieves full OSV JSON for a vulnerability ID
3. **ignore_vulnerability** — Provides osv-scanner config instructions for suppressing findings

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

## Documentation

- [OSV Scanner documentation](https://google.github.io/osv-scanner/)
- [OSV Database](https://osv.dev/)
- [OSV Spec](https://github.com/ossf/osv-schema)
