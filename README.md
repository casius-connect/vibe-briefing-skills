# Replit bundle

Two things to drag into your Replit project.

## 1. Skills  ->  /.agents/skills/

In Replit: turn on **Show Hidden Files**, open (or create) the `/.agents/skills/`
folder, then drag in every folder from `skills/` here:

- brainstorming/      design before code, one question at a time
- writing-plans/      turn a design into a bite-sized build plan
- executing-plans/    build the plan one task at a time, verify each
- flask-ui-design/    the Tailwind + DaisyUI look-and-feel rules for this app
- frontend-design/    the anti-"AI slop" taste layer

Each is a folder with a SKILL.md inside. Replit reads the name + description of
every skill and loads the full one only when relevant. Invoke a skill directly
by typing `/brainstorming` (etc.) in the Agent chat.

These are the Replit-clean versions: the Claude Code machinery (git worktrees,
subagents, wikilinks) has been stripped out, so they just work in a single agent.

## 2. Flask starter  ->  templates/

Drag the two files from `flask-starter/templates/` into your project's
`templates/` folder:

- base.html    the shared layout (nav, container, fonts, DaisyUI theme)
- index.html   an example page that extends base.html

Then point a Flask route at it:

    from flask import Flask, render_template
    app = Flask(__name__)

    @app.route("/")
    def home():
        return render_template("index.html")

The CDNs in base.html are fine for prototyping in Replit. For production, move
Tailwind and DaisyUI to a proper build step.
