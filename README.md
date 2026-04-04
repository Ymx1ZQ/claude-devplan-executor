# devplan-executor

A [Claude Code](https://claude.ai/code) skill that executes a Markdown dev plan milestone by milestone, fully autonomously — no interruptions, no confirmation prompts.

## What it does

When invoked, `devplan-executor` reads your `DEVPLAN.md` (or any Markdown dev plan) and:

1. Implements each milestone in order
2. Writes and runs unit tests; discovers and writes higher-level tests where appropriate
3. Runs `/simplify` to clean up code and tests
4. Updates documentation
5. Updates the devplan checkboxes
6. Commits and pushes after each milestone
7. Moves immediately to the next milestone without asking

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

Copy `SKILL.md` into `~/.claude/skills/devplan-executor/SKILL.md`.

## Usage

Open Claude Code in your project directory and run:

```
/devplan-executor
```

Claude will read the dev plan, then execute every pending milestone autonomously until done.

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
