---
name: gsd:ship
description: Merge completed phase branch into base branch using specific Semantic Commit formatting
argument-hint: "[phase number or milestone, e.g., '4' or 'v1.0']"
allowed-tools:
  - Read
  - Bash
  - Grep
  - Glob
  - Write
  - AskUserQuestion
---
<objective>
Bridge local completion → merge. After /gsd-verify-work passes, ship the work: checkout base branch, squash merge the phase branch, and create a semantic commit following the project standard.

Closes the plan → execute → verify → ship loop.
</objective>

<execution_context>
@~/.claude/get-shit-done/workflows/ship.md
</execution_context>

Execute the ship workflow from @~/.claude/get-shit-done/workflows/ship.md end-to-end.
