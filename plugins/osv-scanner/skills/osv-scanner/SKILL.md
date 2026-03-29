---
name: osv-scanner
description: This skill should be triggered when the user asks about dependency security, vulnerability scanning, or package safety. Examples: "check my dependencies for vulnerabilities", "scan my packages", "are my dependencies safe", "dependency audit", "check for CVEs", "security audit", "vulnerable packages", "scan dependencies for vulnerabilities".
---

# Scan Dependencies for Vulnerabilities

Scans a project's dependencies for known vulnerabilities using the OSV database.

The user is asking about dependency security. Help them identify vulnerable packages and estimate which vulnerabilities actually reach their code.

## Your Task

1. **Identify the scanning scope**: Confirm which directory or file to scan (default to workspace root)
2. **Choose scan mode**: Decide between **quick scan** (list vulnerabilities) or **reachability triage** (estimate which vulnerabilities your code actually calls)
3. **Scan dependencies**: Use appropriate tool (scan or triage)
4. **Report findings**: Present results grouped by severity, with actionable next steps
5. **Suggest remediation**: Recommend upgrades, suppression, or monitoring based on reachability

## Decision Tree: Scan vs. Triage

```
Do you need to know if vulnerable code is *actually called* in your project?
│
├─→ YES (or unsure): Use TRIAGE
│   ├─ Shows: LIKELY_REACHABLE, UNCERTAIN, LIKELY_UNREACHABLE
│   ├─ Time: ~2-5 min (grep-based reachability analysis)
│   ├─ When: Deciding whether to upgrade or ignore a CVE
│   ├─ Example: "We have 42 vulnerabilities, but triage shows only 3 are
│   │           actually called — we can suppress the other 39 safely."
│   └─ Command: `/osv-scanner triage [path]`
│
└─→ NO (just need the list): Use SCAN
    ├─ Shows: All vulnerabilities, sorted by severity
    ├─ Time: <30 seconds
    ├─ When: Compliance/audit inventory, rapid security posture check
    ├─ Example: "Report all CVEs in dependencies for the board."
    └─ Command: `/osv-scanner scan [path]`
```

## Output Format

### Scan Output
Present findings in severity tiers (CRITICAL, HIGH, MEDIUM, LOW, INFO) with:
- Package name and current version
- Vulnerability ID and CVSS score
- Advisory summary (1-2 sentences)
- Link to full advisory
- Remediation (upgrade version or suppress reason)

**Example (abbreviated):**

```
CRITICAL (2 vulnerabilities)
├─ express 4.17.1
│  ├─ CVE-2022-24999 (CVSS 7.5)
│  │  Open Redirect in query parser
│  │  Fix: Upgrade to express 4.18.0+
│  │  https://osv.dev/CVE-2022-24999
│  └─ CVE-2024-1086 (CVSS 8.1)
│     Authentication bypass in middleware
│     Fix: Upgrade to express 4.21.0+

HIGH (5 vulnerabilities)
├─ lodash 4.17.20
│  ├─ CVE-2021-23337 (CVSS 6.1)
│  │  Prototype pollution in defaultsDeep
│  │  Fix: Upgrade to lodash 4.17.21+
│  │  https://osv.dev/CVE-2021-23337
```

### Triage Output
Present findings in three verdict tiers with evidence:

```
LIKELY_REACHABLE (2 vulnerabilities) — Your code calls these
├─ express 4.17.1 / CVE-2022-24999
│  Evidence: query-parser.js:45 calls express.query()
│  Action: UPGRADE IMMEDIATELY (4.18.0+)

UNCERTAIN (8 vulnerabilities) — Might be called, hard to detect
├─ lodash 4.17.20 / CVE-2021-23337
│  Evidence: Dynamic code pattern detected; grep found _.defaultsDeep in
│            middleware/config.js:12 but call path unclear
│  Action: INVESTIGATE manually or upgrade as preventive measure

LIKELY_UNREACHABLE (29 vulnerabilities) — Your code doesn't call these
├─ protobuf 3.14.0 / CVE-2021-22570
│  Evidence: Grep found no usage of affected API
│  Action: SUPPRESS (no upgrade needed for this project)
```

## Remediation Logic

**LIKELY_REACHABLE:** Upgrade immediately. This is a real risk.

**UNCERTAIN:** Either:
- Upgrade to be safe (if impact is low)
- Investigate manually (if upgrade is costly or breaking)
- Suppress with documented reason (if truly unreachable after review)

**LIKELY_UNREACHABLE:** Safe to suppress. Document the reason in config.

## Triage Heuristics

The triage tool uses grep-based analysis to estimate reachability:

- **Positive evidence required for LIKELY_REACHABLE:** Exact file:line citation showing vulnerable API is called
- **Transitive dependencies always UNCERTAIN:** Can't reliably trace calls through intermediate packages
- **Dynamic code patterns flagged:** If your code uses `eval()`, `require(variable)`, or similar, verdict might be uncertain
- **CRITICAL/HIGH CVEs in LIKELY_UNREACHABLE always flagged for review:** Never auto-suppress high-severity findings

Keep the response focused and actionable.
