# Eisenhower Prioritization Skill — Testing Guide

This guide is for manual skill testing during development. It is git-ignored and not part of the release.

## Test Cases

### Test 1: Simple Brain Dump (Basic Classification)

**Input:**
```
3 critical incidents in production
15 medium-severity findings from scanning tool
Need to build automated triage system
Vendor assessment due Friday
Routine code review approvals taking too much time
Obsolete documentation pages
```

**Expected Output:**
- **Q1:** 3 critical incidents, vendor assessment (deadline)
- **Q2:** Automated triage system (strategic investment)
- **Q3:** Routine code review approvals (urgent but low-value)
- **Q4:** Obsolete documentation (delete)

**Narrative should note:** High Q1 volume signals reactive posture; Q2 automation is key leverage point.

---

### Test 2: Ambiguous Task Handling

**Input:**
```
Active incident containment
Critical vulnerability in core dependency (we use it)
Training program for team capability building
Internal system audit (no timeline given)
Compliance deadline next month
```

**Expected Output:**
- **Q1:** Active incident, critical vulnerability, compliance deadline
- **Q2:** Training program (enablement / long-term)
- **Q2 or Q3? [?]:** Internal audit (flagged — no deadline, but important system; ask: is this blocking deployment or is it routine review?)

**Should flag the audit as ambiguous** and offer clarifying question.

---

### Test 3: Structured Task List Input

**Input:**
```
TASK-123 [P0] Database backup system failing - BLOCKING PRODUCTION
TASK-456 [P2] Document disaster recovery procedures
TASK-789 [P1] Update network security rules - due EOW
TASK-111 [P3] Implement logging standards
TASK-222 [P3] Monthly compliance checklist
TASK-333 Critical vulnerability in payment service
```

**Expected Output:**
- **Q1:** TASK-123 (blocking), TASK-333 (critical/payment), TASK-789 (hard deadline)
- **Q2:** TASK-111 (platform-level improvement)
- **Q3:** TASK-222 (routine compliance)
- **Q4:** TASK-456 (low-priority docs)

---

### Test 4: Strategic Insight (Recognizing Patterns)

**Input:**
```
70% of my time is spent on alert triage
5 critical vulnerabilities in dependencies
Need to build platform standards
Vendor questionnaires (3 pending, all due this quarter)
Manual review of every task is exhausting
```

**Expected Output:**
- **Q1:** Critical vulnerabilities, vendor questionnaires
- **Q2:** Platform standards, automated triage (high leverage)
- **Q3:** Manual review (should be automated)
- **Q4:** None

**Narrative must highlight:** "You're burning out your team on Q3 work (alert triage, manual reviews). Automating these in Q2 would free ~40 hours/week for strategic work. Platform standards enable scaling."

---

## Manual Testing Workflow

### Before each test:

1. Open Claude Code
2. Install or activate the plugin:
   ```bash
   /plugin install eisenhower-prioritization@bonfires-marketplace
   ```
   OR
   ```bash
   claude --plugin-dir ./plugins/eisenhower-prioritization
   ```

3. Copy test case input

### Run test:

4. Paste input into Claude with one of these prompts:
   - "Help me prioritize these tasks"
   - "I need to triage my backlog: [input]"
   - `/prioritize [input]`

5. Observe output:
   - Does the matrix correctly classify tasks?
   - Does the narrative identify high-leverage Q2 items?
   - Are recommendations concrete and actionable?
   - Are ambiguous items flagged with clarifying questions?

### Evaluation checklist:

- [ ] Matrix is correctly formatted (2×2 grid with clear quadrants)
- [ ] All input tasks are accounted for (nothing lost)
- [ ] Classifications match heuristics (hard deadline = Q1/Q3, strategic = Q2)
- [ ] Narrative interprets distribution (e.g., "you're reactive" if Q1 > 50%)
- [ ] Recommendations are specific, not generic
- [ ] Ambiguous tasks are flagged, not forced
- [ ] Output matches the spec (4 components: matrix, narrative, recommendations, flagged items)

---

## Known Limitations (v0.1.0)

- No automated risk scoring (requires user context for now)
- No explicit "team maturity" assessment (user can provide context)
- Ambiguity flagging relies on judgment; some edge cases may slip through

These are tracked as extensibility hooks for future versions.
