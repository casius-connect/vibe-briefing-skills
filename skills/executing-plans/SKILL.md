---
name: executing-plans
description: "Builds a feature from an existing written plan, one task at a time, verifying each step and committing before moving on. Use when a plan already exists and the user says 'build it', 'implement the plan', 'start building', 'work through the plan', or 'make it'. This is the BUILD stage after writing-plans. Stops and asks instead of guessing when blocked."
---

# Executing plans

Build from an agreed plan with discipline, one task at a time.
Do not barrel through the whole plan in one pass.

## Process
1. Read the plan in full. Review it critically first: flag anything unclear,
   missing, or risky, and raise it before writing code.
2. Turn the plan into a task checklist.
3. For each task, in order:
   - Build only what that task describes
   - Run the verification the plan specifies (test, command, or manual check)
   - Commit with a clear message
   - Only then move to the next task
4. Report progress after each task so I can course-correct.

## Stop and ask, do not guess, when:
- A dependency is missing
- A test or check fails twice
- An instruction is unclear or the plan has a gap
- The approach needs rethinking

## Rules
- One task at a time, verify before moving on
- Commit frequently, small commits
- Never start on the main branch without asking first
- If the plan relies on a decision you disagree with, say so before building
