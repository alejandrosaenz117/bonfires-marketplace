---
description: AI-powered reachability triage for dependency vulnerabilities. Scans for CVEs then searches your source code to estimate whether each vulnerable API is actually called. Outputs a prioritized LIKELY_REACHABLE / UNCERTAIN / LIKELY_UNREACHABLE report.
argument-hint: [path]
allowed-tools: Read, Glob, Grep
---

# AI Reachability Triage for Dependencies

Usage: `/osv-scanner triage [path]`

Runs a vulnerability scan, then uses AI-powered code analysis to triage findings by likelihood of reachability. This helps you focus on CVEs that actually matter for your codebase.

## ⚠️ Important: Heuristic Triage

This is **LLM-estimated static analysis**, not deterministic call graph tracing. Useful for:
- Quick prioritization of hundreds of CVEs down to actionable items
- Understanding which vulnerabilities are low-risk for your specific codebase
- Guidance on remediation order

**Not a replacement for:**
- Deterministic call graph analysis (available in osv-scanner for Go and Rust via `--call-analysis`)
- Human security review
- Dynamic testing and runtime behavior analysis

Treat **LIKELY_UNREACHABLE** as lower priority, but not safe to ignore completely.

---

## How It Works

1. **Scan** — Run `scan_vulnerable_dependencies` on the provided path (defaults to workspace root)
2. **Fetch details** — For each CVE, retrieve the full advisory to identify which functions/APIs are vulnerable
3. **Search source code** — Use Glob and Grep to find whether those specific functions are imported or called in your codebase
4. **Assign verdicts** — Classify each finding as LIKELY_REACHABLE, UNCERTAIN, or LIKELY_UNREACHABLE with evidence
5. **Output triage report** — Three-tier structured table with reasoning for each verdict

---

## Verdict Definitions

### LIKELY_REACHABLE
The vulnerable function/method is **found in your source code**. This CVE should be prioritized for fixing.
- Evidence: File path and line number where the vulnerable API is used
- Action: Upgrade the package, apply a patch, or change the code to avoid the vulnerable API

### UNCERTAIN
The vulnerability **cannot be statically determined**. This includes:
- Broad vulnerabilities affecting the entire package (not a specific function)
- Dynamic code patterns (eval, dynamic require, reflection)
- Transitive dependencies (dependencies of dependencies)
- Action: Review the advisory and your code path manually; likely mid-to-high priority depending on risk

### LIKELY_UNREACHABLE
The vulnerable package is **not imported** or the vulnerable function is **not found in your code**. This CVE is lower priority.
- Evidence: Package not imported at all, or imported but specific vulnerable API not found
- Caveat: Dynamic patterns or transitive calls might still pose risk
- Action: Monitor for updates; deprioritize unless the dependency is transitive and critical

---

## Parameters

- `path` — Directory or file to scan (defaults to current workspace root)
  - Examples: `/osv-scanner triage .`, `/osv-scanner triage ./src`, `/osv-scanner triage package-lock.json`

---

## Example Output

```
## AI Reachability Triage Report
> ⚠️ LLM-estimated heuristic — not deterministic call graph analysis

### LIKELY_REACHABLE — Fix First
| Package | Version | CVE | Severity | Evidence |
|---------|---------|-----|----------|----------|
| lodash | 4.17.20 | CVE-2021-23337 | HIGH | `_.template()` called in src/render.js:42 |
| express | 4.17.1 | CVE-2022-24999 | MEDIUM | `express()` in server.js:8; uses `qs` vulnerable to prototype pollution |

### UNCERTAIN — Review Manually
| Package | Version | CVE | Severity | Reason |
|---------|---------|-----|----------|--------|
| axios | 0.21.1 | CVE-2021-3749 | HIGH | Broad redirect handling vulnerability; all axios calls are potential vectors |
| node-fetch | 2.6.5 | CVE-2022-33987 | MEDIUM | Dynamic URL construction detected; static analysis cannot determine if vulnerable |

### LIKELY_UNREACHABLE — Low Priority
| Package | Version | CVE | Severity | Evidence |
|---------|---------|-----|----------|----------|
| semver | 5.7.1 | CVE-2022-25883 | MEDIUM | Package only in devDependencies; not called in runtime code |
| jest | 26.6.3 | CVE-2021-23364 | LOW | Test framework; CVE in dev-only utility |
```

---

## Workflow

1. **Scan vulnerabilities** — Calls OSV Scanner's `scan_vulnerable_dependencies` tool
2. **For each CVE found:**
   - Fetch full advisory via `get_vulnerability_details` to identify vulnerable functions/APIs
   - Glob source files in the project to search for the package
   - Grep for the package import/require and the specific vulnerable function names
   - Assign a verdict based on static code analysis
3. **Report** — Output three tables grouped by reachability verdict
4. **Disclaimer** — Always remind that this is heuristic and may have false negatives/positives

---

## Reasoning Rules

- **LIKELY_REACHABLE requires positive evidence** — Don't assume; find the actual call site
- **When in doubt → UNCERTAIN** — False positives (flagging as reachable when unreachable) are better than false negatives (missing real exposures)
- **Dynamic code patterns → UNCERTAIN** — eval, dynamic require, reflection, metaprogramming
- **Transitive dependencies → UNCERTAIN** — Can't statically determine if the parent package uses the vulnerable API
- **devDependencies-only → LIKELY_UNREACHABLE** — Unless the dev code runs in a security-sensitive context
- **Every finding gets a verdict** — No CVE is left without a classification

---

## Best Practices

1. **Run after scanning** — Always run `/osv-scanner scan [path]` first to understand the full vulnerability landscape
2. **Cross-check with deterministic tools** — For Go/Rust, use osv-scanner's native `--call-analysis=go` for authoritative results
3. **Use this for triage, not approval** — LIKELY_UNREACHABLE doesn't mean "safe to ignore forever," just "lower priority today"
4. **Automate follow-up** — Integrate the triage report into your CI/CD to track reachability changes over time
5. **Keep dependencies updated** — Even unreachable vulns become reachable if code patterns change

Keep the report focused and actionable.
