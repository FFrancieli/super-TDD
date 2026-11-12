---
name: spike
description: Use when exploring an idea, testing a library, or writing a one-off script — throwaway work where TDD and the full plan/review cycle are explicitly skipped
---

# Spike

A spike is throwaway work. Its job is to answer a question or produce something immediately useful, not to ship production code.

**Announce at start:** "This is a spike — throwaway work. No plan, no TDD cycle, no review. If you want to keep any of it, that's a new request through the full workflow."

## What's skipped

- No failing test before implementation
- No implementation plan
- No subagent review
- No `verification-before-completion`

Tests are optional. Write one if it helps you think; skip it if it doesn't. A spike that produces a test does not earn the right to bypass TDD when you decide to keep the code — the test still needs to have been written first in a proper RED-GREEN-REFACTOR cycle.

## What's not skipped

- Correctness within the scope of the question being answered
- Saying out loud what assumptions you made
- Stopping when the spike has answered its question

## When done

End with a short handoff:

> **Spike complete.**
> - **What it answered:** [the question the spike was exploring]
> - **What it produced:** [file, function, output — one line each]
> - **What I'd change for production:** [the shortcuts taken that shouldn't survive]
> - **Next step if you want to keep this:** invoke `brainstorming` — keeping spike output is a new request and goes through the full workflow

Do not start building on the spike without that explicit handoff and the user asking to keep it.

## Hard gate

Keeping spike output — using it as a base for real work, committing it, or building on it — is a new request. It goes back through `brainstorming`. The spike label does not carry forward.
