---
name: osv-scanner
description: This skill should be triggered when the user asks about dependency security, vulnerability scanning, or package safety. Examples: "check my dependencies for vulnerabilities", "scan my packages", "are my dependencies safe", "dependency audit", "check for CVEs", "security audit", "vulnerable packages", "scan dependencies for vulnerabilities".
---

# Scan Dependencies for Vulnerabilities

Scans a project's dependencies for known vulnerabilities using the OSV database.

The user is asking about dependency security. Help them:

1. **Identify the scanning scope** — Ask which directory or file to scan (default to current workspace root)
2. **Scan dependencies** — Use `scan_vulnerable_dependencies` to check for known vulnerabilities
3. **Report findings** — Present vulnerabilities grouped by severity, with links to advisories
4. **Suggest next steps** — Recommend upgrades or suppression via config if needed

Keep the response focused and actionable.
