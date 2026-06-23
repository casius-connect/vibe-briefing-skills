# Briefing style for the AI daily briefing

Paste this to the Replit agent to shape the `/brief` output.

> Make the /brief output read like my personal AI briefing, in the exact shape and
> tone below. The brief covers roughly the last 7 days of stored stories and picks
> the five with the most weight, not just the most recent. Return ONLY the briefing.
> Never show the source list, the raw fetched articles, or any of the underlying data.

## Format

# AI Briefing, <start date> to <end date>

TOP LINE
One flowing paragraph (about 120 to 200 words) that weaves the week's biggest
threads into a single story, not a list. Finish with one italic line that starts
"The theme this week:" and captures the through-line.

Then the five biggest stories, numbered 1 to 5, each like this:

**N. <a confident, specific headline>**
*<Source, Source, date>*
**What happened:** two to four sentences of plain fact: the key players, the
numbers, who reported it.
**Why it matters:** two to four sentences of analysis: the implication and where it
is trending. When it continues an earlier story, say so ("this follows last week's ...").
**Sources:**
- <Publication>, "<headline>", (<link>), <date>
- (two to four sources per story)

## Rules

- NO em dashes anywhere. Use commas, colons, parentheses, or "and". Do not use long
  dashes in date ranges either: write the range with the word "to", like
  "19 to 21 June".
- British English: optimise, colour, favour, organise.
- No corporate jargon (leverage, synergy, paradigm, holistic, best in class).
- Lead with the story, never a definition.
- Keep it tight: I want the whole week in about two minutes of reading.
- Show only the briefing. No raw data, no feed list, no JSON.

## Short example of the tone and shape

# AI Briefing, 16 to 23 June 2026

TOP LINE
This week the centre of gravity in AI moved from model launches to who controls the
infrastructure underneath them. Two of the biggest players locked in a multi-year
compute deal, a third quietly shipped the capability everyone will copy by autumn,
and a regulator finally put real numbers on the risk. The smaller stories all point
the same way: the people building the models and the people paying for the
electricity are no longer the same companies. The theme this week: *the AI race is
becoming an infrastructure race, and power is moving to whoever owns the compute and
the distribution.*

**1. A frontier lab ships its long promised model, and the market reprices overnight**
*The Verge, Reuters, 19 June 2026*
**What happened:** The lab released the model on Thursday, claiming a large jump on
its own benchmarks. Reuters confirmed the figures, and a rival responded within a day.
**Why it matters:** This is the capability the rest of the field has been waiting on,
and it puts the rival on the back foot. It follows last week's preview, so the trend
is clear: the gap at the top is widening, not closing.
**Sources:**
- The Verge, "headline here", (https://...), 19 June 2026
- Reuters, "headline here", (https://...), 19 June 2026
