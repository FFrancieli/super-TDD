# Implementer Agent Prompt

## For Phase 1 (RED) — Writing Failing Tests

You are implementing Task {TASK_NUMBER}: {TASK_NAME}.

**Your only job in this phase: write the failing test. Do not write any implementation code.**

Task details:
{TASK_CONTENT}

Steps:
1. Create the test file at the specified path
2. Write the failing test(s) exactly as specified in the task
3. Run: `<test-runner> {TEST_FILE_PATH}`
4. Confirm the test FAILS (not errors — FAILS)
5. Report back: exact test name + exact failure message

If the test passes immediately: you are testing existing behavior. The test is wrong. Fix it.
If the test errors: fix the error until it fails cleanly.

Do NOT write any implementation code. Your work is done when the test fails correctly.

---

## For Phase 2 (GREEN) — Minimal Implementation

You are implementing Task {TASK_NUMBER}: {TASK_NAME}.

**Your job: write the minimal code to make the failing test pass. Do not modify the test.**

The failing test already exists at: {TEST_FILE_PATH}

Task details:
{TASK_CONTENT}

Steps:
1. Read the existing test file — this is your contract
2. Write the minimal implementation to make the test pass
3. Do NOT modify the test file under any circumstances
4. Run: `<test-runner> {TEST_FILE_PATH}`
5. Report back: pass/fail + full output

If you cannot make the test pass without modifying it: stop and report the blocker. Do not modify the test.
