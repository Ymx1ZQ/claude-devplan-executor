# devplan-executor

A [Claude Code](https://claude.ai/code) skill that executes a Markdown dev plan milestone by milestone, fully autonomously — no interruptions, no confirmation prompts.

## What it does

When invoked, `devplan-executor` reads your `DEVPLAN.md` (or any Markdown dev plan) and executes every pending milestone in order. It supports two modes:

- **TDD (Test Driven Development) — DEFAULT.** For each milestone: state the business requirement → write tests at all applicable levels → confirm they fail (red) → implement until green → simplify → docs → devplan → commit & push.
- **IDD (Implementation Driven Development).** For each milestone: implement → write tests covering the finished code → simplify → docs → devplan → commit & push. Use when the milestone is exploratory.

In both modes the skill:

1. Discovers and writes tests at all relevant levels (unit, integration, functional, e2e — whatever the project uses)
2. Runs `/simplify` to clean up code and tests
3. Updates documentation
4. Updates the devplan checkboxes
5. Commits and pushes after each milestone
6. Moves immediately to the next milestone without asking

It stops only on unresolvable blocking errors.

## Requirements

- [Claude Code](https://claude.ai/code) CLI installed and authenticated
- A project with a `DEVPLAN.md` (or equivalent Markdown plan file)
- Git initialized and a remote configured (for push)

## Installation

### Option 1 — Clone directly into your Claude skills directory

```bash
git clone https://github.com/Ymx1ZQ/claudecode-devplan-executor \
  ~/.claude/skills/devplan-executor
```

Claude Code auto-discovers skills in `~/.claude/skills/`. No extra configuration needed.

### Option 2 — Manual copy

Copy the entire skill directory into `~/.claude/skills/devplan-executor/`.
The skill consists of:

- `SKILL.md` — entry point and mode router (TDD vs IDD)
- `TDD.md` — full Test Driven Development playbook (default mode)
- `IDD.md` — full Implementation Driven Development playbook
- `README.md` — this file

`SKILL.md` reads only the playbook for the chosen mode, so the agent
follows a single self-contained set of instructions per run.

## Usage

Open Claude Code in your project directory and run:

```
/devplan-executor
```

This invokes the skill in TDD mode (default) on the active devplan.

You can also pass arguments:

```
/devplan-executor devplan/v0.9-wip.md          # TDD mode (default)
/devplan-executor TDD devplan/v0.9-wip.md      # TDD mode (explicit)
/devplan-executor IDD devplan/v0.9-wip.md      # IDD mode (implementation-first)
```

Claude will read the dev plan, then execute every pending milestone autonomously until done.

### Choosing a mode

- **Use TDD (default)** for milestones with clear, testable requirements: bug fixes, new features with defined behavior, refactors with preserved contracts.
- **Use IDD** for exploratory work where the requirement cannot be stated as a contract upfront: spikes, prototypes, investigations, code archaeology. The skill can also fall back to IDD per-milestone automatically when it cannot articulate the requirement clearly in TDD mode.

### Tips

- Make sure your dev plan has clear, actionable milestones before invoking
- Pending milestones should use `- [ ]` checkboxes; completed ones use `- [x]`
- The skill respects your project's existing test structure (unit, integration, e2e, etc.)
- If you need to pause mid-run, just interrupt Claude Code (`Ctrl+C`)

## Devplan format

The skill works with any Markdown file that has milestone headings and checkbox task lists. A minimal example:

```markdown
# My Project Dev Plan

## M1: Add user authentication
- [ ] Implement login endpoint
- [ ] Add JWT token generation
- [ ] Write unit tests

## M2: Add password reset flow
- [ ] Send reset email
- [ ] Implement token validation
```

No specific format is required beyond readable headings and checkboxes.

## License

MIT
