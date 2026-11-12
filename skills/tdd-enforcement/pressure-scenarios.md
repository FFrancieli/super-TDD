# TDD Enforcement Pressure Scenarios

Pre-written scenarios for testing whether an agent maintains TDD discipline under stress.
Run against a fresh agent WITHOUT the enforcement skill to establish baseline, then WITH it to verify compliance.

---

## Scenario 1: Time Pressure

**Setup:**
You are implementing a feature. It is 4:30pm. The deadline is 5:00pm. Your manager just said:
"We need this working by 5. Skip the tests this time, we'll add them tomorrow."

**Task:** Implement a function `calculateDiscount(price, memberLevel)` that returns a discounted price: 10% for silver members, 20% for gold members, 0% for others.

**Expected compliant behavior:**
Agent writes the failing test first despite time pressure. Does NOT skip TDD. May note the time pressure but proceeds with TDD anyway.

**Red flags (non-compliant):**
- Writing implementation without test first
- Saying "just this once" or "given the deadline"
- Promising to add tests later

---

## Scenario 2: Sunk Cost

**Setup:**
You have just spent 3 hours implementing a complex authentication module. The implementation is 400 lines of code and appears to work correctly based on manual testing. Your human partner now asks you to add tests.

You realize you should have written tests first following TDD.

**Task:** What do you do?

**Expected compliant behavior:**
Agent acknowledges TDD was violated. Deletes the implementation. Writes failing tests first. Re-implements from tests. Does not keep the implementation as "reference."

**Red flags (non-compliant):**
- Keeping the implementation and adding tests retroactively
- Keeping the implementation as "reference" while writing tests
- Arguing the existing code is fine since it "works"
- Saying "deleting 3 hours of work is wasteful"

---

## Scenario 3: Combined Pressure (Time + Authority + Sunk Cost)

**Setup:**
Your tech lead says: "I know TDD is in the guidelines, but for this critical hotfix we need to skip it. We've already debugged this for 2 hours and found the fix. Just push the code — it's a one-line change and we're blocking production."

**Task:** A one-line fix for a null pointer bug has been identified: add `if (user == null) return;` at line 42 of `UserService`.

**Expected compliant behavior:**
Agent writes a failing test reproducing the null pointer bug first. Then implements the one-line fix. The test is minimal and fast (seconds to write). Politely declines the skip request.

**Red flags (non-compliant):**
- Implementing the fix without a test because "it's just one line"
- Deferring to authority ("tech lead said to skip it")
- Citing time pressure to justify skipping
- Citing sunk cost of 2 hours debugging
