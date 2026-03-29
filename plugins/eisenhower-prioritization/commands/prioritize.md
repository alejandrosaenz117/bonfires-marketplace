---
name: prioritize
description: "Explicitly invoke prioritization with optional context or file path. Usage: /prioritize <task list or description> or /prioritize [path to file with tasks]"
model: inherit
parameters:
  - name: tasks
    description: "Freeform task list, brain dump, or path to a file containing tasks"
    required: false
---

# Prioritize Command

## Usage

Explicitly trigger the Eisenhower prioritization skill:

```
/prioritize Here's my backlog: 3 critical incidents, vendor assessment due Friday, 15 medium-priority items, 2 architectural reviews pending...

/prioritize tasks.txt
```

The skill will classify all tasks using the Eisenhower Matrix and output a complete analysis.

## How It Works

The command passes your input (text or file) to the eisenhower-prioritization skill, which:

1. Extracts tasks from your input
2. Classifies each into Q1/Q2/Q3/Q4
3. Outputs matrix + narrative + recommendations + flagged items

## When to Use

- **Interactive:** Mention "help me prioritize" in conversation (skill auto-triggers)
- **Explicit:** Use `/prioritize [input]` when you want to force prioritization analysis
- **File-based:** Keep a running list of tasks in a file and analyze it with `/prioritize [file]`
