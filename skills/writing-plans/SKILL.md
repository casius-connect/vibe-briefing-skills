---
name: writing-plans
description: "Breaks an agreed design or spec into a bite-sized, step-by-step build plan with exact files, code, and verification steps. Use after the design is settled and before building, when the user says 'make a plan', 'break this down', 'what are the steps', 'plan the build', or 'turn this into tasks'. This is the PLAN stage between brainstorming and executing-plans. Do not use to write the actual feature code, only the plan."
---

# Writing implementation plans

Turn an agreed design into a step-by-step plan. Assume the engineer is skilled
but knows nothing about this codebase or domain. Document everything.
DRY, YAGNI, test-driven, frequent commits. Save to docs/plan-<feature>.md

## Header
# <Feature> implementation plan
Goal: <one sentence>
Architecture: <2-3 sentences>
Tech stack: <key libraries>

## Each task
### Task N: <component>
Files: create / modify (exact paths) / test

One-action steps (2-5 min each):
1. Write the failing test (include the real test code)
2. Run it, confirm it fails (command + expected output)
3. Write the minimal code to pass (include the real code)
4. Run the test, confirm it passes
5. Commit (give the commit message)

## Rules
- Exact file paths, always
- Complete code in the plan, never "add validation"
- Exact commands with expected output
- One action per step
