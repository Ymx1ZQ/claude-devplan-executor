---
name: devplan-executor
description: Executes a Markdown dev plan milestone by milestone, fully autonomously from start to finish. Defaults to TDD (Test Driven Development); can be invoked in IDD (Implementation Driven Development) mode. Use when the user has a DEVPLAN.md and wants to implement it completely without interruptions.
---

# Dev Plan Executor — Router

This skill has two execution modes:

- **TDD (Test Driven Development) — DEFAULT.** Write tests first based
  on the business requirement, run them red, implement until green,
  simplify, docs, devplan, commit & push. Use for milestones with clear,
  testable requirements.
- **IDD (Implementation Driven Development).** Implement first, write
  tests covering the finished code, simplify, docs, devplan, commit &
  push. Use for exploratory work (spikes, prototypes, investigations).

## Mode selection

Parse the first token of the args:

- `TDD <devplan-path>` or just `<devplan-path>` → TDD (default)
- `IDD <devplan-path>` → IDD

If no mode token is present, use TDD.

## Routing

1. Announce the mode at the very start: `Mode: TDD` or `Mode: IDD`.
2. Read the corresponding playbook file with the Read tool:
   - TDD → `TDD.md` (in this skill directory)
   - IDD → `IDD.md` (in this skill directory)
3. Follow that playbook end-to-end. Do not load the other playbook.
