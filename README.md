# tdd-superpowe

A self-contained TDD-first development workflow for Claude Code. A full replacement for superpowers with test-driven discipline enforced at every stage — not as an add-on, but as the structural backbone.

Synced to superpowers 7.0.2.

## Skills

| Skill | Purpose |
|---|---|
| `using-tdd-workflow` | Session entry point — loaded at every session start via hook |
| `tdd-enforcement` | Iron law, rationalization counters, red flags — always ambient |
| `brainstorming` | Design before code, with testability gate and tdd-enforcement reference |
| `writing-plans` | Implementation plans with TDD mandate and task dependency ordering |
| `test-driven-development` | RED-GREEN-REFACTOR mechanics with Good/Bad code examples |
| `executing-plans` | Sequential execution — ledger system, scripts, 7 reference files, TDD cycle per task, two-stage review |
| `systematic-debugging` | Root cause investigation → mandatory TDD handoff for the fix |
| `dispatching-parallel-agents` | Parallel investigation of multiple independent failures |
| `using-git-worktrees` | Isolated workspace + pre-commit hook installation |
| `verification-before-completion` | Evidence before claims — TDD compliance checklist |
| `requesting-code-review` | Dispatch reviewer with spec-coverage analysis |
| `receiving-code-review` | Respond to review with technical rigor |
| `finishing-a-development-branch` | Verify → 3 options → ship |

## Deployment

Skills are loaded via symlinks in `~/.claude/skills/`:

```bash
for dir in /path/to/tdd-superpowe/skills/*/; do
  ln -sf "$dir" ~/.claude/skills/"$(basename "$dir")"
done
```

The `using-tdd-workflow` skill fires at every session start via a `SessionStart` hook registered in `~/.claude/settings.json`:

```json
{
  "hooks": {
    "SessionStart": [{
      "matcher": "startup|clear|compact",
      "hooks": [{"type": "command", "command": "/path/to/tdd-superpowe/hooks/session-start", "shell": "bash", "async": false}]
    }]
  }
}
```

## Structure

```
skills/
  brainstorming/          SKILL.md, visual-companion.md, builder-check-prompt.md
  executing-plans/        SKILL.md, implementer-prompt.md, task/spec/quality reviewer prompts
    scripts/              sdd-workspace, task-brief, review-package, sdd-repo
    references/           fix-loop.md, preflight.md, resuming.md, batching.md,
                          cannot-verify.md, handling-reports.md, final-review.md
  systematic-debugging/   SKILL.md + root-cause-tracing.md, defense-in-depth.md,
                          condition-based-waiting.md, test pressure scenarios
  tdd-enforcement/        SKILL.md, pressure-scenarios.md, rationalization-counters.md
  [10 more skills]/       SKILL.md (+ supporting files where applicable)
hooks/
  session-start           Injects using-tdd-workflow at session start
specs/                    Design specs and implementation plans
```

## Design Docs

`specs/2026-11-12-tdd-superpowe-design.md` — full design spec
`specs/2026-11-12-tdd-superpowe-plan.md` — implementation plan
