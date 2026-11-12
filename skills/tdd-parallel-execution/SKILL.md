---
name: tdd-parallel-execution
description: Use when executing implementation plans with independent tasks — runs TDD phases in parallel with automatic checkpoints between each phase
---

# TDD Parallel Execution

## Overview

Execute implementation plans using TDD phases as parallelization boundaries. All agents write failing tests simultaneously (RED), then independent modules are implemented concurrently (GREEN), then a single agent refactors (REFACTOR).

**Core principle:** Test contracts define safe parallelism boundaries. Once failing tests exist, independent modules can be implemented concurrently — each agent has a clear, isolated contract to satisfy.

**REQUIRED: `tdd-enforcement` is always loaded. The iron law applies to every dispatched agent.**

## When to Use

- You have a written plan from `writing-plans` with `parallel-safe: yes/no` task declarations
- You want maximum execution speed with TDD discipline intact

## Input

A plan at `specs/YYYY-MM-DD-<feature>-plan.md` with tasks marked `parallel-safe: yes` or `parallel-safe: no`.

## The Three Phases

### Phase 1 — RED (always parallel)

Dispatch one agent per task to write failing tests simultaneously. Tests are created as new files — no conflicts possible regardless of `parallel-safe` declarations.

Each agent:
1. Reads its task from the plan
2. Creates the test file with the specified failing tests
3. Runs: `<test-runner> <test-file>`
4. Reports back: test name + exact failure message

**── AUTOMATIC CHECKPOINT before Phase 2 ──**

Orchestrator reviews every agent report. For each task:
- Did the test FAIL (not error, not pass)?
- Did it fail because the feature is missing (not a typo or syntax error)?

If any test **passed immediately**: that agent must fix its test — it is testing existing behavior, not new behavior.

If any test **errored**: that agent must fix the error until the test fails cleanly.

**Do not proceed to Phase 2 until ALL tests fail correctly.**

---

### Phase 2 — GREEN (declaration-based parallel)

Group tasks by `parallel-safe` declaration:
- `parallel-safe: yes` → dispatch all as concurrent agents
- `parallel-safe: no` → run sequentially after the parallel batch completes

Each agent:
1. Reads its task (the test file already exists from Phase 1)
2. Writes minimal implementation code to pass the test
3. Does NOT modify the test file
4. Runs: `<test-runner> <test-file>`
5. Reports back: pass/fail + output

**── AUTOMATIC CHECKPOINT before Phase 3 ──**

Orchestrator runs the full test suite: `<test-runner>`

- All tests pass?
- Suite is clean (no errors, no warnings)?

If any test fails: that agent returns to fix its implementation (never the test).
If suite has errors: investigate before continuing.

**Do not proceed to Phase 3 until ALL tests pass and suite is clean.**

---

### Phase 3 — REFACTOR (sequential)

A single agent reviews all implemented modules together:
- Remove duplication across modules
- Improve names
- Extract shared helpers
- No new behavior added

After refactoring: `<test-runner>` — all tests must still pass.

**── AUTOMATIC CHECKPOINT before finishing ──**

- All tests pass after refactor?
- Output clean?

If yes: trigger `finishing-a-development-branch`.

---

## Agent Prompts

See:
- `implementer-prompt.md` — template for dispatching Phase 1 and Phase 2 agents
- `checkpoint-prompt.md` — template for checkpoint verification

## Red Flags

**Never:**
- Skip a checkpoint ("it's obviously fine")
- Let Phase 2 agents modify test files
- Start Phase 3 before Phase 2 checkpoint passes
- Start Phase 2 before ALL Phase 1 tests fail correctly
- Let a parallel agent modify files from another agent's task

**If a checkpoint fails:**
- Return only the failing agents to fix their work
- Re-run only those agents' tests before proceeding
- Do not re-run the entire phase from scratch
