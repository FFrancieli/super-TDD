# TDD Superpowe — Design Spec

**Date:** 2026-11-12  
**Author:** Francieli Viane

---

## Overview

A self-contained, standalone replacement for the superpowers plugin for use in environments where external plugins cannot be installed. Covers the full software development lifecycle with TDD as the structural backbone — not an add-on. Every skill in the pipeline is anchored to test-first thinking through a centralized enforcement layer that is always ambient in context.

**Core principle:** TDD is not a step in the workflow. It is the workflow.

---

## Plugin Identity

```
name: tdd-superpowe
location: /Users/francieli/projects/claude-workflows/tdd-superpowe
```

No external dependencies. No superpowers required alongside.

---

## Skill Inventory — 12 Skills

| Skill | Type | Source |
|---|---|---|
| `using-tdd-workflow` | Meta — session init | Adapted from `using-superpowers` |
| `tdd-enforcement` | NEW — enforcement layer | Original |
| `test-driven-development` | TDD mechanics | Enhanced from superpowers |
| `brainstorming` | Design phase | Adapted from superpowers |
| `writing-plans` | Planning phase | Adapted from superpowers |
| `tdd-parallel-execution` | Execution phase | NEW — replaces 3 superpowers skills |
| `finishing-a-development-branch` | Shipping phase | Faithful port |
| `using-git-worktrees` | Workspace isolation | Adapted from superpowers |
| `systematic-debugging` | Bug investigation | Adapted from superpowers |
| `verification-before-completion` | Quality gate | Adapted from superpowers |
| `requesting-code-review` | Code review | Faithful port |
| `receiving-code-review` | Code review | Faithful port |

**Dropped from superpowers (intentional):**
- `subagent-driven-development` — replaced by `tdd-parallel-execution`
- `executing-plans` — replaced by `tdd-parallel-execution`
- `dispatching-parallel-agents` — replaced by `tdd-parallel-execution`
- `writing-skills` — not core to software development

---

## Workflow Pipeline

```
using-tdd-workflow (session start)
  → loads tdd-enforcement eagerly (always ambient)

  DESIGN PHASE
  → brainstorming
      explores requirements, proposes 2-3 approaches
      testability gate: can each component be tested in isolation?
      writes spec to specs/YYYY-MM-DD-<topic>-design.md
      → triggers using-git-worktrees (create isolated workspace)
      → triggers writing-plans

  PLANNING PHASE
  → writing-plans
      breaks work into TDD bite-sized tasks
      marks each task: parallel-safe: yes/no
      language-agnostic: uses <test-runner> placeholder
      writes plan to specs/YYYY-MM-DD-<topic>-plan.md
      → triggers tdd-parallel-execution

  EXECUTION PHASE
  → tdd-parallel-execution
      Phase 1 — RED (parallel)
        all agents write failing tests simultaneously
        ── CHECKPOINT ── verify every test fails for the right reason
      Phase 2 — GREEN (declaration-based parallel)
        parallel-safe: yes → agents implement concurrently
        parallel-safe: no → tasks run sequentially after parallel batch
        ── CHECKPOINT ── verify all tests pass, suite clean
      Phase 3 — REFACTOR (sequential)
        single agent cleans up across all modules
        ── CHECKPOINT ── final quality review
      → triggers finishing-a-development-branch

  SHIPPING PHASE
  → finishing-a-development-branch
      verify tests pass
      present 4 options: merge / PR / keep / discard
      cleanup worktree

  CROSS-CUTTING (available at any point)
  → systematic-debugging
      find root cause
      → handoff to test-driven-development: write failing test before fixing
  → verification-before-completion
      TDD checklist before claiming done
  → requesting-code-review / receiving-code-review

  ENFORCEMENT (ambient, always loaded)
  → tdd-enforcement
      iron law, rationalization counters, red flags, pressure scenarios
```

---

## New Skills

### `tdd-enforcement`

**Role:** Principles and counters. Not mechanics (those live in `test-driven-development`).

**Contents:**
- **Iron law** — "No production code without a failing test first." No exceptions.
- **Rationalization table** — every known excuse mapped to its rebuttal (e.g., "too simple to test" → "simple code breaks; test takes 30 seconds")
- **Red flags list** — self-check triggers that mean STOP and start over
- **Pressure scenarios** — pre-written scenarios for testing compliance under stress (time pressure, sunk cost, authority)

**Loading:** `using-tdd-workflow` loads this skill as its first action before any other response. It is not invoked on demand — it is always present.

**Referenced by:** all skills that touch code decisions cross-reference it, ensuring enforcement is never bypassed regardless of which skill is active.

---

### `tdd-parallel-execution`

**Role:** Execute a written plan using TDD-phased parallel agents. Replaces `subagent-driven-development`, `executing-plans`, and `dispatching-parallel-agents`.

**Input:** A plan doc from `writing-plans` with tasks marked `parallel-safe: yes/no`.

**Three phases with automatic checkpoints:**

**Phase 1 — RED (always parallel)**
- Dispatch one agent per task to write failing tests simultaneously
- Tests are read-only files — no conflicts possible
- CHECKPOINT: orchestrator reviews every test, confirms each fails for the right reason (feature missing, not typo/syntax error) before proceeding to Phase 2
- If any test passes immediately or fails for wrong reason: fix before continuing

**Phase 2 — GREEN (declaration-based parallel)**
- Group tasks: `parallel-safe: yes` → dispatch concurrently; `parallel-safe: no` → run sequentially after
- Each agent implements minimal code to pass its test contract
- Agents operate in isolated worktrees — no file conflicts
- CHECKPOINT: orchestrator verifies all tests pass and full suite is clean before proceeding to Phase 3

**Phase 3 — REFACTOR (sequential)**
- Single agent reviews all implemented modules together
- Removes duplication, improves names, extracts helpers
- No new behavior added
- CHECKPOINT: final quality review before triggering `finishing-a-development-branch`

**Supporting files:**
- `implementer-prompt.md` — template for dispatching implementation agents
- `checkpoint-prompt.md` — template for checkpoint verification

---

### `using-tdd-workflow`

**Role:** Session initialization meta-skill. Adapted from `using-superpowers`.

**Key changes from `using-superpowers`:**
- First action before any other response: load `tdd-enforcement`
- All superpowers skill references updated to tdd-superpowe skill names
- Skill trigger list updated to reflect the 12 skills in this plugin

---

## Adapted Skills

### `test-driven-development`
**Change:** Rationalization table and red flags list move to `tdd-enforcement`. The TDD skill retains only mechanics.

**Keeps:**
- RED-GREEN-REFACTOR cycle (the loop, what each phase means)
- Good/bad test examples
- Verification steps (watch it fail, watch it pass)
- "When stuck" table
- Bug fix example
- Verification checklist

**Moves to `tdd-enforcement`:**
- Iron law declaration
- Rationalization table (all excuses + rebuttals)
- Red flags list
- "Why order matters" arguments

**Adds:**
- Reference to `tdd-enforcement` at the top: "REQUIRED: `tdd-enforcement` is always loaded. The iron law, rationalization counters, and red flags live there."

---

### `brainstorming`
**Change:** Adds a testability gate before design approval.

Before the user approves a design section, ask:
> "Can each component in this design be tested in isolation? Are there hidden dependencies that would make testing difficult?"

If the answer is no, revisit the design. Hard-to-test design = hard-to-use design.

**Also removed:** reference to `using-git-worktrees` as an external skill name — updated to this plugin's skill name.

---

### `writing-plans`
**Changes:**
1. Each task includes a `parallel-safe: yes/no` declaration with brief justification
2. All test commands use language-agnostic `<test-runner>` placeholder (auto-detected from project: `npm test`, `pytest`, `go test ./...`, `cargo test`, etc.)
3. Execution handoff updated: points to `tdd-parallel-execution` (not `subagent-driven-development` or `executing-plans`)

**Task structure addition:**
```markdown
**parallel-safe:** yes
**reason:** touches only src/auth.ts and tests/auth.test.ts
```

---

### `systematic-debugging`
**Change:** Explicit TDD handoff after root cause identification.

After the root cause is confirmed, the skill now says:

> "Root cause identified. Before writing any fix: write a failing test that reproduces this bug. Follow the test-driven-development skill from this point."

This makes the debugging → TDD handoff mandatory, not optional. Bug fixes without a test are a TDD violation — same iron law applies.

---

### `verification-before-completion`
**Change:** TDD checklist items added to the existing checklist.

Additional items:
- [ ] Every new function/method has a test written before the implementation
- [ ] Watched each test fail before implementing
- [ ] Each test failed for the expected reason (feature missing, not typo)
- [ ] No test passed immediately without implementation

---

### `using-git-worktrees`
**Change:** Installs a `pre-commit` git hook after worktree creation and baseline verification.

After `npm install` / dependency setup and baseline tests pass, detect test runner and install:
```bash
# .git/hooks/pre-commit
#!/bin/sh
<test-runner>
```

This enforces "tests must pass before you commit" at the git level — independent of agent behavior.

---

## Faithful Ports (no changes)

- `finishing-a-development-branch` — verify tests → 4 options → merge/PR/keep/discard → cleanup worktree
- `requesting-code-review` — code review workflow
- `receiving-code-review` — responding to review feedback

---

## File Structure

```
tdd-superpowe/
  .claude-plugin/
    plugin.json
    marketplace.json
  skills/
    using-tdd-workflow/
      SKILL.md
    tdd-enforcement/
      SKILL.md
      pressure-scenarios.md
      rationalization-counters.md
    test-driven-development/
      SKILL.md
      testing-anti-patterns.md
    brainstorming/
      SKILL.md
    writing-plans/
      SKILL.md
    tdd-parallel-execution/
      SKILL.md
      implementer-prompt.md
      checkpoint-prompt.md
    finishing-a-development-branch/
      SKILL.md
    using-git-worktrees/
      SKILL.md
    systematic-debugging/
      SKILL.md
      condition-based-waiting.md
      condition-based-waiting-example.ts
      defense-in-depth.md
      root-cause-tracing.md
      find-polluter.sh
    verification-before-completion/
      SKILL.md
    requesting-code-review/
      SKILL.md
      code-reviewer.md
    receiving-code-review/
      SKILL.md
  specs/
    2026-11-12-tdd-superpowe-design.md   ← this file
  README.md
```

---

## Key Design Decisions

| Decision | Choice | Reason |
|---|---|---|
| Enforcement approach | Centralized `tdd-enforcement` skill, always ambient | Single source of truth; all skills reference it; can be strengthened without touching other skills |
| Parallelization safety | Declaration-based (`parallel-safe: yes/no` in plan) | Planning phase has full context to reason about dependencies; execution phase does not |
| Parallel phase checkpoints | Automatic between every phase | TDD iron law requires watching tests fail — cannot skip |
| Language agnosticism | `<test-runner>` placeholder throughout | Plugin should work for any project type |
| Code review TDD lens | Not added | Enforcement happens upstream; review focuses on quality and correctness |
| Git worktrees | Kept | Required for safe parallel agent execution (no file conflicts) |
| `writing-skills` | Dropped | Not core to software development |
| Docs location | `specs/` | Simple, clean, no superpowers-specific naming |
| Debugging → TDD | Mandatory handoff | Bug fixes are covered by the same iron law |
