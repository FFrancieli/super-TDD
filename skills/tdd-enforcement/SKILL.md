---
name: tdd-enforcement
description: Use when tempted to skip writing a test first, to rationalize not following TDD, or when under pressure to deliver quickly without tests
---

# TDD Enforcement

**Violating the letter of the rules is violating the spirit of the rules.**

## The Iron Law

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

No exceptions. Write code before the test? Delete it. Start over.

**No exceptions:**
- Don't keep it as "reference"
- Don't "adapt" it while writing tests
- Don't look at it
- Delete means delete

## Rationalization Table

| Excuse | Reality |
|--------|---------|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I'll test after" | Tests passing immediately prove nothing. |
| "Tests after achieve same goals" | Tests-after = "what does this do?" Tests-first = "what should this do?" |
| "Already manually tested" | Ad-hoc ≠ systematic. No record, can't re-run. |
| "Deleting X hours is wasteful" | Sunk cost fallacy. Keeping unverified code is technical debt. |
| "Keep as reference, write tests first" | You'll adapt it. That's testing after. Delete means delete. |
| "Need to explore first" | Fine. Throw away exploration, start with TDD. |
| "Test hard = design unclear" | Listen to test. Hard to test = hard to use. |
| "TDD will slow me down" | TDD faster than debugging. Pragmatic = test-first. |
| "Manual test faster" | Manual doesn't prove edge cases. You'll re-test every change. |
| "Existing code has no tests" | You're improving it. Add tests for existing code. |
| "TDD is dogmatic, I'm being pragmatic" | TDD IS pragmatic. Finds bugs before commit. |
| "Tests after achieve the same purpose" | No. Tests-after verify what you built. Tests-first define what should exist. |
| "It's about spirit not ritual" | Violating the letter is violating the spirit. |
| "This is different because..." | It never is. Same rules apply. |

## Red Flags — STOP and Start Over

- Code before test
- Test after implementation
- Test passes immediately without implementation
- Can't explain why the test failed
- Tests added "later"
- Rationalizing "just this once"
- "I already manually tested it"
- "Tests after achieve the same purpose"
- "It's about spirit not ritual"
- "Keep as reference" or "adapt existing code"
- "Already spent X hours, deleting is wasteful"
- "TDD is dogmatic, I'm being pragmatic"
- "This is different because..."

**All of these mean: Delete code. Start over with TDD.**

## Why Order Matters

**Tests written after code pass immediately. Passing immediately proves nothing:**
- Might test wrong thing
- Might test implementation, not behavior
- Might miss edge cases you forgot
- You never saw it catch the bug

**Manual testing is ad-hoc:**
- No record of what you tested
- Can't re-run when code changes
- Easy to forget cases under pressure
- "It worked when I tried it" ≠ comprehensive

**Sunk cost is not a reason:**
- Time already spent is gone regardless
- Keeping unverified code = guaranteed technical debt
- Delete and rewrite TDD: more hours, high confidence
- Keep and test after: less time, low confidence, likely bugs

**TDD IS pragmatic:**
- Finds bugs before commit (faster than debugging after)
- Prevents regressions (tests catch breaks immediately)
- Documents behavior (tests show how to use code)
- Enables refactoring (change freely, tests catch breaks)

## Pressure Scenarios

See `pressure-scenarios.md` for pre-written scenarios to test TDD compliance under stress.

## Detailed Rebuttals

See `rationalization-counters.md` for in-depth counters to each rationalization.
