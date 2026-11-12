---
name: using-tdd-workflow
description: Use when starting any conversation — establishes how to find and use skills, requiring skill invocation before ANY response including clarifying questions
---

# Using TDD Workflow

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a skill might apply, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.
</EXTREMELY-IMPORTANT>

## First Action — Every Session

Before any other response, load `tdd-enforcement`. This is not optional.

## Available Skills

| Skill | When to invoke |
|---|---|
| `tdd-enforcement` | Loaded at session start — always ambient |
| `brainstorming` | Before any creative work: new features, components, behavior changes, modifications |
| `writing-plans` | After brainstorming approves a design, before touching code |
| `test-driven-development` | Before writing any implementation code — mechanics of RED-GREEN-REFACTOR |
| `executing-plans` | When executing a written plan — fresh subagent per task with TDD cycle and two-stage review |
| `using-git-worktrees` | Before starting implementation — creates isolated workspace and installs git hooks |
| `systematic-debugging` | When encountering any bug, test failure, or unexpected behavior |
| `dispatching-parallel-agents` | When multiple independent failures exist across different subsystems |
| `verification-before-completion` | Before claiming work is complete, fixed, or passing |
| `requesting-code-review` | After completing tasks or features, before merging |
| `receiving-code-review` | When receiving code review feedback, before implementing suggestions |
| `finishing-a-development-branch` | When all implementation is complete and ready to ship |

## Skill Priority

1. **Process skills first** — `brainstorming`, `systematic-debugging` determine HOW to approach the task
2. **Implementation skills second** — `executing-plans`, `using-git-worktrees` guide execution

"Let's build X" → brainstorming first, then implementation skills.
"Fix this bug" → systematic-debugging first, then test-driven-development for the fix.

## Red Flags

These thoughts mean STOP — you're rationalizing skipping a skill:

| Thought | Reality |
|---------|---------|
| "This is just a simple question" | Questions are tasks. Check for skills. |
| "I need more context first" | Skill check comes BEFORE clarifying questions. |
| "Let me explore the codebase first" | Skills tell you HOW to explore. Check first. |
| "This doesn't need a formal skill" | If a skill exists, use it. |
| "I remember this skill" | Skills evolve. Read current version. |
| "This doesn't count as a task" | Action = task. Check for skills. |
| "I'll just do this one thing first" | Check BEFORE doing anything. |
| "The skill is overkill" | Simple things become complex. Use it. |

## The Rule

**Invoke relevant or requested skills BEFORE any response or action.** Even 1% chance a skill might apply means invoke it. If an invoked skill turns out not to fit the situation, you don't need to follow it.
