# Changelog

All notable changes to plugins in the Bonfires Marketplace are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## OSV Scanner Plugin

## [0.1.0] - 2026-02-27

### Added

- Initial release of the OSV Scanner plugin
- MCP server registration via `osv-scanner experimental-mcp` (stdio transport, no runtime dependencies)
- Three invocation surfaces:
  - **Skill** (contextual trigger): activates on dependency security phrases like "check my dependencies for vulnerabilities", "security audit", "vulnerable packages", and more (8 trigger phrases)
  - **Scan command** (`/osv-scanner scan [path]`): scans a directory or lockfile for known CVEs
  - **Triage command** (`/osv-scanner triage [path]`): grep-based reachability triage to estimate which vulnerable APIs are actually called
- Three MCP tools exposed to Claude: `scan_vulnerable_dependencies`, `get_vulnerability_details`, `ignore_vulnerability`
- Reachability triage with three verdict tiers: FOUND_IN_SOURCE, UNCERTAIN, NOT_FOUND_IN_GREP
- Triage reasoning rules:
  - Positive evidence required for FOUND_IN_SOURCE (exact file:line citations only)
  - Transitive dependencies always classified as UNCERTAIN
  - Dynamic code pattern detection before assigning NOT_FOUND_IN_GREP
  - Mandatory review flag for CRITICAL/HIGH CVEs in NOT_FOUND_IN_GREP tier
  - Advisory text treated as untrusted data (defense against prompt injection via rogue MCP binary)
  - CVSS severity score cannot be overridden by advisory body text
- Full documentation: README with banner, tagline, What It Does/Does NOT Do, installation prerequisites, usage examples, triage verdicts table, security notes, Contributing, and License sections
- Security note on binary integrity: tampered binary could fabricate advisory content
- Supported package managers: npm, pip, Go, Rust, Maven, Ruby, PHP, Java, Dart, and more via osv-scalibr

---

## The Devil's Advocate Plugin

## [0.2.0] - 2026-02-20

### Changed

- **Documentation clarification**: Updated README and main documentation to clearly state that The Devil's Advocate must be explicitly invoked. It does not auto-trigger
  - Removed misleading claims about automatic activation after planning sessions
  - Removed references to non-existent agent chaining feature
  - Clarified two explicit invocation methods: skill trigger and command

### Removed

- **Hooks experiment**: Removed `hooks/hooks.json` as hook-based auto-triggering was not working as intended

## [0.1.0] - 2026-02-20

### Added

- Initial release of The Devil's Advocate plugin
- Two invocation surfaces:
  - **Skill** (contextual trigger): Activates on adversarial review phrases like "what could go wrong?" and "challenge the plan"
  - **Command** (`/devils-advocate`): Explicit on-demand invocation for adversarial analysis
- Four-section review output structure:
  - The Contradiction: assumption vs. reality that violates it
  - The Fragility Vector: security flaw + architectural enabler + cascade impact
  - The Black Swan Scenario: specific, exploitable failure scenario
  - The Mitigation Strategy: concrete architectural hardening
- Comprehensive fragility vectors, Black Swan scenario templates, and mitigation patterns in `skills/devils-advocate/references/review-rubric.md`
- Model cost strategy: `inherit` for skill (respects user's baseline model), `opus` for explicit `/devils-advocate` command
- Full documentation: README, contribution guidelines, security disclosure policy
- Test suite with 30+ test cases across 11 categories (security, architecture, agentic patterns, mandate, git, surfaces, structure, LGTM enforcement, AI & scale, Claude Code, deployment scale)

---

**Repository**: [alejandrosaenz117/bonfires-marketplace](https://github.com/alejandrosaenz117/bonfires-marketplace)
**License**: MIT
