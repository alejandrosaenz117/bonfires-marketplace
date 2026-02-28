![Bonfires Marketplace Banner](./banner.png)

# Bonfires Marketplace

A marketplace of Claude Code plugins for security-focused development: adversarial code review, dependency vulnerability scanning, and reachability triage.

## Installation

**1) Register the marketplace:**

```bash
/plugin marketplace add alejandrosaenz117/bonfires-marketplace
```

**2) Install a plugin:**

```bash
/plugin install devils-advocate@bonfires-marketplace
/plugin install osv-scanner@bonfires-marketplace
```

## What's Inside

### The Devil's Advocate

The tenth man. When consensus forms, it is a sign of danger. This plugin peers into the fog where failure waits. The collapse. The breach. The systems failing under weight they cannot see. It reveals where light fails and why the walls will break.

**Invoke:** Mention "adversarial review" or "challenge the plan" as a skill, or use `/devils-advocate [file|description|recent]` command.

See [plugins/devils-advocate/README.md](plugins/devils-advocate/README.md) for full documentation.

### OSV Scanner

Integrates OSV Scanner as an MCP server, giving Claude direct access to the OSV vulnerability database. Scans your dependencies for known CVEs, fetches full advisories, and uses grep-based reachability triage to estimate which vulnerabilities actually live in your code paths.

**Invoke:** Mention "check my dependencies for vulnerabilities" or "scan for CVEs" as a skill, or use `/osv-scanner scan [path]` and `/osv-scanner triage [path]` commands.

See [plugins/osv-scanner/README.md](plugins/osv-scanner/README.md) for full documentation.

## Philosophy

See the darkness before it sees you. Challenge consensus. Find what will break before you go hollow.

## License

MIT. See [LICENSE](LICENSE).

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding plugins and contributing.
