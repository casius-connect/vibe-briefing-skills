---
name: brainstorming
description: "Turns a rough idea into a validated design through one-question-at-a-time dialogue, before any code is written. Use at the very start of anything new or fuzzy: a new feature, app, page, component, or behaviour change, and when the user says 'I want to build', 'I have an idea', 'let us design', 'help me think this through', or the requirements are unclear. This is the DESIGN stage, before writing-plans. Do not use once a design is already agreed."
---

# Brainstorming: ideas into designs

## Process
1. Understand first. Look at what already exists in the project. Then ask
   questions ONE at a time. Prefer multiple choice. Never more than one
   question per message. Focus on purpose, constraints, success criteria.
2. Explore approaches. Propose 2-3 options with trade-offs. Lead with your
   recommendation and say why.
3. Present the design in small sections (200-300 words). After each section,
   ask whether it looks right before continuing. Cover architecture,
   components, data flow, error handling, testing.
4. Stay flexible. Go back and clarify whenever something does not fit.

## When the design is agreed
Write it to docs/design-<topic>.md, then ask:
"Ready to turn this into an implementation plan?"

## Principles
- One question at a time
- Multiple choice preferred
- YAGNI: cut every feature that is not needed
- Always offer 2-3 alternatives before settling
- Validate the design section by section
