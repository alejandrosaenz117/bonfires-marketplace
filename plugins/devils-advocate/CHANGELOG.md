# Changelog

All notable changes to The Devil's Advocate plugin are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.2.0] - 2026-02-20

### Added

- **Auto-engagement hooks**: Plugin now includes `hooks/hooks.json` that automatically triggers The Devil's Advocate after planning sessions complete
  - Fires when the Plan subagent finishes (after plan acceptance)
  - Uses prompt-based hook to inject adversarial review directive
  - Hooks are bundled with plugin and activated automatically on installation
  - Users no longer need to manually request adversarial review after planning

## [0.1.0] - 2026-02-20

### Added

- Initial release of The Devil's Advocate plugin
- Three invocation surfaces:
  - **Skill** (contextual auto-trigger): Activates on adversarial review phrases like "what could go wrong?" and "challenge the plan"
  - **Agent** (proactive chaining): Auto-triggers after planning/review agents complete
  - **Command** (`/devils-advocate`): Explicit on-demand invocation for adversarial analysis
- Four-section review output structure:
  - The Contradiction: assumption vs. reality that violates it
  - The Fragility Vector: security flaw + architectural enabler + cascade impact
  - The Black Swan Scenario: specific, exploitable failure scenario
  - The Mitigation Strategy: concrete architectural hardening
- Comprehensive fragility vectors, Black Swan scenario templates, and mitigation patterns in `skills/devils-advocate/references/review-rubric.md`
- Model cost strategy: `inherit` for skill/agent (respects user's baseline model), `opus` for explicit `/devils-advocate` command
- Full documentation: README, contribution guidelines, security disclosure policy
- Test suite with 30+ test cases across 11 categories (security, architecture, agentic patterns, mandate, git, surfaces, structure, LGTM enforcement, AI & scale, Claude Code, deployment scale)

---

**Repository**: [alejandrosaenz117/bonfires-marketplace](https://github.com/alejandrosaenz117/bonfires-marketplace)
**License**: MIT
