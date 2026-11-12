# Rationalization Counters

Detailed rebuttals for the most common TDD rationalization patterns.

---

## "Too Simple to Test"

**The rationalization:** The function is so simple (one line, obvious logic) that writing a test is overkill.

**The counter:** Complexity is not the criterion. Behavior is. A one-line function can have edge cases (null inputs, empty strings, zero values, negative numbers). The test takes 30 seconds. If the function ever breaks — from a refactor, a dependency change, a language upgrade — the 30-second investment saves hours of debugging. "Simple" functions are also the ones most likely to be changed carelessly later, precisely because they seem harmless.

---

## "I'll Test After to Verify It Works"

**The rationalization:** I'll write the implementation, then write tests to confirm it does what I expect.

**The counter:** Tests written after implementation almost always pass immediately. A test that passes immediately proves nothing — it cannot distinguish correct behavior from a test that simply mirrors the implementation. You never saw it catch a bug. Test-first forces you to watch the test FAIL before you write code — that moment of failure is the proof that the test actually tests something real.

---

## "Deleting X Hours of Work Is Wasteful"

**The rationalization:** I've already written 3 hours of code. Deleting it and starting over with TDD wastes those hours.

**The counter:** Sunk cost fallacy. Those hours are gone regardless of what you do next. The choice now is: (a) delete and rewrite test-first, spending more hours but gaining high confidence, or (b) keep the code and add tests after, spending less time now but keeping code you can't fully trust. Option (b) is technical debt — it looks faster but costs more later in bugs, debugging, and re-work. The "waste" is keeping code without proof it works.

---

## "Tests After Achieve the Same Goals"

**The rationalization:** Whether I write tests before or after, I end up with tests. Same result.

**The counter:** Tests-after answer "What does this code do?" Tests-first answer "What should this code do?" Tests written after are biased by the implementation — you test what you built, not what was required. You verify the edge cases you remember, not the ones you discover. Tests written first force you to define the API before building it, and to discover edge cases before they become bugs in production. The sequence is not ritual — it changes what gets tested.

---

## "It's About Spirit Not Ritual"

**The rationalization:** I understand the SPIRIT of TDD (confidence, coverage, good design). I don't need to follow the letter (test first, watch it fail).

**The counter:** The letter IS the spirit. The ritual exists because the spirit cannot be reliably achieved without it. Watching the test fail is not ceremony — it is proof that the test tests something real. Writing the test first is not arbitrary order — it changes what you design and discover. "Following the spirit" while skipping the letter is an unfalsifiable rationalization that lets you skip TDD whenever it's inconvenient, which is exactly when it matters most.
