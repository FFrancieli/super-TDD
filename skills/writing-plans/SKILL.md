---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for the codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, commands, expected output. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume they are a skilled developer, but know almost nothing about the toolset or problem domain. Assume they don't know good test design very well.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Save plans to:** `specs/YYYY-MM-DD-<feature-name>.md`

**Execute plans with:** `executing-plans`

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** REQUIRED: Use `executing-plans` to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

---
```

## Task Structure

Each task must include a parallel-safe declaration and complete content — no placeholders.

```markdown
### Task N: [Component Name]

**parallel-safe:** yes/no
**reason:** [why this task is or isn't safe to run concurrently — which files it touches]

**Files:**
- Create: `exact/path/to/file`
- Modify: `exact/path/to/existing:123-145`
- Test: `tests/exact/path/to/test`

- [ ] **Step 1: Write the failing test**

[actual test code]

- [ ] **Step 2: Run test to verify it fails**

Run: `<test-runner> tests/path/to/test`
Expected: FAIL with "[exact expected error message]"

- [ ] **Step 3: Write minimal implementation**

[actual implementation code]

- [ ] **Step 4: Run test to verify it passes**

Run: `<test-runner> tests/path/to/test`
Expected: PASS

- [ ] **Step 5: Commit**

```bash
git add tests/path/file src/path/file
git commit -m "feat: add specific behavior"
```
```

## Test Runner Resolution

`<test-runner>` resolves to the project's test command. Detect from project files:

| File present | Test runner |
|---|---|
| `package.json` | `npm test` or `npx jest` |
| `pyproject.toml` or `requirements.txt` | `pytest` |
| `go.mod` | `go test ./...` |
| `Cargo.toml` | `cargo test` |
| `Gemfile` | `bundle exec rspec` |

## parallel-safe Declaration Rules

Mark `parallel-safe: yes` if and only if:
- Task creates/modifies files no other concurrent task touches
- Task does not depend on output of another concurrent task
- Test file is separate from all other concurrent tasks

Mark `parallel-safe: no` if:
- Task touches a shared file (config, shared utility, index)
- Task must run after another specific task
- Task modifies files another task also modifies

## No Placeholders

Every step must contain actual content:
- Never "TBD", "TODO", "implement later"
- Never "Add appropriate error handling"
- Never "Write tests for the above" without actual test code
- Never "Similar to Task N" — repeat the code
- Every code step must show the code

## Remember
- Exact file paths always
- Complete code in every step
- Exact commands with expected output
- DRY, YAGNI, TDD, frequent commits

## Execution Handoff

After saving the plan:

**"Plan complete and saved to `specs/<filename>.md`. Ready to execute with `executing-plans` — parallel RED phase, then parallel GREEN phase for independent tasks, then sequential REFACTOR. Shall I proceed?"**
