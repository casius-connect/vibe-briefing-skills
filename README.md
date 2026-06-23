# Vibe-briefing skills

The take-home kit for the live build: an AI daily briefing that reads your feeds,
writes a what-happened and why-it-matters on each story, and remembers everything
in Oracle so you can ask it later.

Three things in here:

1. **Skills** the Agent loads while you build (`skills/`).
2. **A Flask starter** so you are not staring at a blank page (`flask-starter/`).
3. **The session slides**, if you want to follow the structure later (`deck/`).

---

## Before you start: an Oracle database (free)

The backend skill wires the app to an **Oracle Autonomous Database** and to
**OCI Generative AI**: Select AI writes the summaries inside the database, and
Oracle AI Agent Memory stores and searches everything. Every AI call stays
inside Oracle, so there are no third-party AI keys to manage.

You do not need an existing Oracle account. Sign up for the **OCI Free Trial**
and you get **US$300 in cloud credits for 30 days**, which is plenty to set all
of this up and run it through the session:

1. Create a free account at [oracle.com/cloud/free](https://www.oracle.com/cloud/free/).
   You get the $300 trial credits, and an Always Free tier afterwards.
2. Provision an **Autonomous Database** (choose Oracle AI Database 26ai). Select
   AI and AI Vector Search are built in.
3. Check that **OCI Generative AI** is available in your region (the skill uses
   Cohere models through OCI GenAI). GenAI usage draws from your trial credits.
4. Download the database **wallet** and set your connection details. The
   `vibe-briefing-backend` skill has the exact wiring and the five-route
   contract.

Running the database in the Autonomous Database Free container image is possible,
but the AI still calls OCI GenAI, so you would need an OCI account anyway and
would have to swap the wallet connection for a local one. The free trial is the
simpler route.

---

## 1. Add the skills (Replit, Settings, Personalization)

A skill is a short instruction file that teaches the Replit Agent how to do one
thing well. You add them once, then the Agent pulls in the right one when it is
relevant, or you call one by name.

1. In Replit, open **Settings**, then **Personalization**.
2. Add the skills **one by one**: for each folder under `skills/` in this repo,
   add its `SKILL.md` as a new skill. Six in total.
3. That is it. Replit reads each skill's name and description, and loads the full
   instructions only when they fit what you are doing. You can also call one
   directly by typing its name in the Agent chat, for example `/brainstorming`.

The six skills, and what each one is for:

| Skill | What it does |
|---|---|
| `brainstorming` | Design before code. The Agent interviews you, one question at a time, instead of generating in the wrong direction. |
| `writing-plans` | Turns the design into a small, checkable build plan. |
| `executing-plans` | Builds the plan one task at a time, verifying each before moving on. |
| `vibe-briefing-backend` | The locked Oracle and Agent Memory backend contract, so voice-driven changes cannot break the data layer. |
| `flask-ui-design` | The Tailwind and DaisyUI look-and-feel rules for this app. |
| `frontend-design` | The taste layer that keeps the UI from looking like generic AI slop. |

These are the Replit-clean versions. The Claude Code machinery (git worktrees,
subagents, wikilinks) has been stripped out, so they just work in a single agent.

If you are forking `vibe-briefing-backend` for your own database, swap the
project specifics (the Select AI profile, the model names, the wallet and DSN)
for your own Oracle AI Database values. The contract stays the same.

---

## 2. Flask starter (templates)

Drag the two files from `flask-starter/templates/` into your project's
`templates/` folder:

- `base.html`: the shared layout (nav, container, fonts, DaisyUI theme).
- `index.html`: an example page that extends `base.html`.

Then point a Flask route at it:

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route("/")
def home():
    return render_template("index.html")
```

The CDNs in `base.html` are fine for prototyping in Replit. For production, move
Tailwind and DaisyUI to a proper build step.

---

## 3. The session slides

`deck/index.html` is the companion deck from the session: what we are building,
the opening prompt, Oracle Agent Memory, and the brainstorm, plan, execute,
harden spine. Open it in a browser (clone the repo, then open the file) to walk
back through the structure on your own.

## Starter Prompt
I want a personal AI daily briefing app, not a generic news summary, but a
briefing written for my role: an AI Developer Advocate at Oracle.

- I add RSS feeds for the sources I care about.
- Each day it fetches them, summarises every story, and stores the summaries
  so I can query them later.
- On demand it writes a digest: a top-line narrative, a daily theme, and the
  five most significant stories.
- Each story: What happened, Why it matters, Your angle (my content, talk,
  and demo opportunities).
- Briefings are saved, so I can read past days.

Help me design and build this. Let's talk it through first, not code:
stack, storage and retrieval, writing "Your angle", MVP first.
