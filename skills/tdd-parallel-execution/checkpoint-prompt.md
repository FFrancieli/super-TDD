# Checkpoint Verification

## Phase 1 Checkpoint (before GREEN)

Review the following agent reports and answer for each task:

{AGENT_REPORTS}

For each task, verify:
- [ ] Test exists at the specified path
- [ ] Test FAILED (not errored, not passed)
- [ ] Failure message indicates feature is missing (not a typo or syntax error)
- [ ] Failure is deterministic (run twice if unsure)

**Decision:**
- ALL tasks pass verification → proceed to Phase 2
- ANY task fails verification → list which tasks need fixes, with specific instructions

---

## Phase 2 Checkpoint (before REFACTOR)

Run the full test suite: `<test-runner>`

Verify:
- [ ] All tests pass (0 failures)
- [ ] No errors in output
- [ ] No warnings that indicate broken behavior

Report the full output. If anything fails, list which tasks need fixes.

**Decision:**
- Suite fully clean → proceed to Phase 3
- Any failures → list tasks to return for fixes

---

## Phase 3 Checkpoint (before finishing)

After refactoring, run the full test suite: `<test-runner>`

Verify:
- [ ] All tests still pass (refactor did not break anything)
- [ ] Output clean

**Decision:**
- Clean → trigger `finishing-a-development-branch`
- Any failures → refactor agent must fix before finishing
