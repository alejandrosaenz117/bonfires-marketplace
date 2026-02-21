# Changelog

All notable changes to The Devil's Advocate plugin are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.2.0] - 2026-02-20

### Changed

- **Documentation clarification**: Updated README and main documentation to clearly state that The Devil's Advocate must be explicitly invoked—it does not auto-trigger
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
