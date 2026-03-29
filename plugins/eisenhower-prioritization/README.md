# Eisenhower Prioritization

Eisenhower Matrix-based task prioritization for any team or workload.

## What It Does

Ingests a brain dump of tasks, projects, or work items—in any format—and applies the Eisenhower Matrix to categorize work into four quadrants:

- **Q1 (Urgent + Important):** Do now. Blocking issues, hard deadlines, critical business impact.
- **Q2 (Important, Not Urgent):** Schedule. Strategic investments: automation, process improvements, capability building.
- **Q3 (Urgent, Not Important):** Delegate or automate. Routine approvals, interruptions, low strategic value.
- **Q4 (Neither):** Delete. Obsolete processes, low-value meetings, waste.

## Usage

**As a skill (keyword trigger):**
```
"Help me prioritize my backlog"
"I'm overwhelmed with work, what should I focus on?"
"Here's my task list, help me triage this"
```

**As a command (explicit invocation):**
```
/prioritize Here's my sprint: 3 critical issues, vendor assessment due Friday, 20 medium-priority tasks...
```

## Output

The skill produces four outputs:

1. **Eisenhower Matrix** — 2×2 grid showing task distribution
2. **Narrative Summary** — interpretation of the distribution and team health
3. **Actionable Recommendations** — per-quadrant guidance (do, schedule, delegate, delete)
4. **Flagged Items** — ambiguous tasks with clarifying questions

## Prerequisites

None. Works out-of-the-box with Claude Code.

## Installation

```bash
/plugin install eisenhower-prioritization@bonfires-marketplace
```

Then use as a skill or with `/prioritize [tasks]`.
