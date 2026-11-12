---
name: tdd-driven-development
description: Use when executing implementation plans with independent tasks in the current session
---

# TDD-Driven Development

Execute a plan by dispatching a fresh subagent per task, each following the full TDD cycle, with two-stage review after each task.

**Core principle:** Fresh subagent per task + TDD cycle enforced + two-stage review (spec then quality) = high quality, disciplined iteration.

**REQUIRED: `tdd-enforcement` is always loaded. The iron law applies to every dispatched subagent.**

## When to Use

- You have a written plan from `writing-plans`
- Tasks are mostly independent
- You want TDD discipline enforced at every step

## The Process

```dot
digraph process {
    rankdir=TB;

    "Read plan, extract all tasks, create TodoWrite" [shape=box];
    "More tasks remain?" [shape=diamond];
    "Dispatch implementer subagent" [shape=box];
    "Subagent asks questions?" [shape=diamond];
    "Answer questions" [shape=box];
    "Subagent runs TDD cycle\n(RED→GREEN→REFACTOR), commits" [shape=box];
    "Dispatch spec reviewer" [shape=box];
    "Spec compliant?" [shape=diamond];
    "Implementer fixes spec gaps" [shape=box];
    "Dispatch code quality reviewer" [shape=box];
    "Quality approved?" [shape=diamond];
    "Implementer fixes quality issues" [shape=box];
    "Mark task complete" [shape=box];
    "Dispatch final code reviewer" [shape=box];
    "finishing-a-development-branch" [shape=box style=filled fillcolor=lightgreen];

    "Read plan, extract all tasks, create TodoWrite" -> "More tasks remain?";
    "More tasks remain?" -> "Dispatch implementer subagent" [label="yes"];
    "More tasks remain?" -> "Dispatch final code reviewer" [label="no"];
    "Dispatch implementer subagent" -> "Subagent asks questions?";
    "Subagent asks questions?" -> "Answer questions" [label="yes"];
    "Answer questions" -> "Dispatch implementer subagent";
    "Subagent asks questions?" -> "Subagent runs TDD cycle\n(RED→GREEN→REFACTOR), commits" [label="no"];
    "Subagent runs TDD cycle\n(RED→GREEN→REFACTOR), commits" -> "Dispatch spec reviewer";
    "Dispatch spec reviewer" -> "Spec compliant?";
    "Spec compliant?" -> "Implementer fixes spec gaps" [label="no"];
    "Implementer fixes spec gaps" -> "Dispatch spec reviewer";
    "Spec compliant?" -> "Dispatch code quality reviewer" [label="yes"];
    "Dispatch code quality reviewer" -> "Quality approved?";
    "Quality approved?" -> "Implementer fixes quality issues" [label="no"];
    "Implementer fixes quality issues" -> "Dispatch code quality reviewer";
    "Quality approved?" -> "Mark task complete" [label="yes"];
    "Mark task complete" -> "More tasks remain?";
    "Dispatch final code reviewer" -> "finishing-a-development-branch";
}
```

## Prompt Templates

- `implementer-prompt.md` — dispatch implementer subagent (enforces TDD cycle)
- `spec-reviewer-prompt.md` — dispatch spec compliance reviewer
- `code-quality-reviewer-prompt.md` — dispatch code quality reviewer

## Example Workflow

```
[Read plan: specs/feature-plan.md]
[Extract all 4 tasks with full text]
[Create TodoWrite with all tasks]

Task 1: User login

[Dispatch implementer with full task text]
Implementer: "Should login be case-insensitive for email?"
You: "Yes, lowercase before comparison."
Implementer:
  - RED: wrote login.test.ts, confirmed FAIL
  - GREEN: minimal implementation, tests PASS
  - REFACTOR: extracted validation helper
  - Committed

[Dispatch spec reviewer]
Spec reviewer: ✅ All requirements met

[Dispatch code quality reviewer]
Code reviewer: ✅ Approved

[Mark Task 1 complete]

Task 2: User logout
...
```

## Red Flags

**Never:**
- Skip spec compliance review before code quality review
- Proceed with open issues from either reviewer
- Let implementer self-review replace actual review
- Accept "close enough" on spec compliance
- Skip re-review after fixes
- Move to next task while current has open issues

**If subagent asks questions:** Answer before letting them proceed.
**If reviewer finds issues:** Implementer fixes → reviewer re-reviews → only then proceed.
