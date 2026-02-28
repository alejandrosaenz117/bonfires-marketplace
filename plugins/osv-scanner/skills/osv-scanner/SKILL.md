---
name: osv-scanner
model: inherit
tools:
  - MCP:osv-scanner/scan_vulnerable_dependencies
  - MCP:osv-scanner/get_vulnerability_details
triggers:
  - keywords:
      - "check dependencies"
      - "check my dependencies"
      - "scan for vulnerabilities"
      - "scan dependencies"
      - "are my dependencies safe"
      - "are my packages vulnerable"
      - "check for vulnerabilities"
      - "check for CVEs"
      - "dependency audit"
      - "dependency security"
      - "vulnerable packages"
      - "vulnerable dependencies"
      - "security audit"
      - "audit my dependencies"
---

# Scan Dependencies for Vulnerabilities

Scans a project's dependencies for known vulnerabilities using the OSV database.

The user is asking about dependency security. Help them:

1. **Identify the scanning scope** — Ask which directory or file to scan (default to current workspace root)
2. **Scan dependencies** — Use `scan_vulnerable_dependencies` to check for known vulnerabilities
3. **Report findings** — Present vulnerabilities grouped by severity, with links to advisories
4. **Suggest next steps** — Recommend upgrades or suppression via config if needed

Keep the response focused and actionable.
