---
name: flask-ui-design
description: "The visual stack and design rules for this Flask app. Tailwind CSS plus DaisyUI inside Jinja templates, a shared base.html layout, a restrained palette, and two non-generic fonts. Use whenever building, styling, or restyling any page, template, view, navbar, form, card, button, or layout in this Flask/Jinja project, or when the user says 'make it look good', 'design the page', 'style this', 'the UI', or 'make it prettier'. Keeps every screen consistent and intentional."
---

# Flask UI design

This app renders HTML with Jinja templates. Make them look good and consistent.

## Stack (do not deviate without asking)
- Tailwind CSS for styling (utility classes directly in the templates)
- DaisyUI for ready-made components (buttons, cards, navbar, modals)
- One base layout (templates/base.html) that every page extends
- Keep all pages inside a centred container with a sensible max width

## Layout
- templates/base.html holds the <head>, the nav, and a {% block content %}
- Every page does {% extends "base.html" %} and fills the content block
- Page content sits in a container: mx-auto max-w-5xl px-4 py-8
- Responsive by default: stack on mobile, grid on wider screens

## Visual rules
- Pick ONE restrained palette and stick to it (neutral base + one accent).
  Set it once as the DaisyUI theme, reuse everywhere.
- Two fonts maximum: one for headings, one for body. Load from Google Fonts.
  Avoid Inter, Roboto and Arial.
- Generous whitespace. Consistent rounded corners and one shadow style.
- Clear hierarchy: confident headings, calm body text, one primary button per view.
- Buttons, inputs and cards always use the same DaisyUI classes, never ad-hoc styles.

## Avoid the generic AI look
- No centred-everything gradient hero with three identical feature cards.
- Make at least one intentional choice per screen: an asymmetric layout,
  a distinctive accent, real copy instead of placeholder text.
- If a screen looks like every other AI-built app, redesign it.

## When unsure about aesthetics
Apply the frontend-design skill for taste-level calls (font pairing, colour,
spacing, breaking out of templated defaults).
