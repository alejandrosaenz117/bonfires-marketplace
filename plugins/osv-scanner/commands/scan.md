---
name: osv-scanner scan
description: Scan a directory or file for vulnerable dependencies
tools:
  - MCP:osv-scanner/scan_vulnerable_dependencies
  - MCP:osv-scanner/get_vulnerability_details
model: inherit
---

# Scan for Vulnerable Dependencies

Usage: `/osv-scanner scan [path]`

Scans a directory or lockfile for known vulnerabilities in dependencies.

**Parameters:**
- `path` — Directory or file to scan (defaults to current workspace root)

**Examples:**
- `/osv-scanner scan .`
- `/osv-scanner scan ./src`
- `/osv-scanner scan package-lock.json`

**Steps:**

1. Parse the provided path (default to workspace root if none given)
2. Call `scan_vulnerable_dependencies` with the path
3. Present results grouped by severity (critical, high, medium, low)
4. For each vulnerability, show:
   - Package name and version
   - CVE/advisory ID and summary
   - Link to full advisory (if available)
5. Optionally fetch details for critical vulnerabilities using `get_vulnerability_details`

Keep output concise and actionable.
