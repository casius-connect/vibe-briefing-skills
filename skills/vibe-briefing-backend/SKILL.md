---
name: vibe-briefing-backend
description: "The locked backend contract for the VIBEBRIEF daily AI briefing app, all-Oracle and in-database. Use whenever building or changing any backend route, database call, summary, embedding, memory, story-linking, or search, or the Oracle and Agent Memory wiring. Pins the connection, Select AI generation, and the Oracle AI Agent Memory (oracleagentmemory) four-memory model so voice-driven changes cannot break the data layer. The frontend (look, layout, copy) is NOT governed by this skill and stays free to change."
---

# VIBEBRIEF backend contract

This skill governs the BACKEND only. The Oracle wiring must be exactly as below,
because it is hard to get right and easy to break by improvising. The FRONTEND
(HTML, Tailwind, layout, copy, what the page looks like) is Casius's to direct
live, change it freely.

## Hard rules (never break these)
- ALL AI goes to ORACLE only: either in the database (Select AI / DBMS_VECTOR) or
  to OCI GenAI via Oracle AI Agent Memory. NEVER OpenAI, Anthropic, Cohere-direct,
  or any third-party AI API.
- NEVER add an AI API key to the app. The only secrets are the Oracle connection
  and the OCI api-key credentials used for GenAI.
- With Oracle AI Agent Memory, ALWAYS prefix the model `oci/` (e.g.
  `oci/cohere.command-r-08-2024`). OAMP embeds app-side through LiteLLM, and a bare
  model name routes to a third party and demands a key. `oci/` hits OCI GenAI.
- NEVER use LangChain or LangGraph. (OAMP depends on LiteLLM internally; that is
  OAMP's own dependency, not a framework we add.)
- ALWAYS read Oracle CLOBs to a Python string before returning them in JSON.
- Oracle does the clever parts. The app is thin routes over it.

## Connection (reuse, do not rewrite)
Use the existing `db.py` python-oracledb pool (mTLS wallet, env-var driven). It
exposes `get_conn()` and the pool object. Oracle AI Agent Memory takes that pool
directly: `OracleAgentMemory(connection=pool, ...)`. Do not open a second
connection path.

## The memory model (Oracle AI Agent Memory)
Install: `pip install oracledb oracleagentmemory` (the driver is `oracledb`, NOT
`python-oracledb`). It stores all memory inside Oracle. Map the four types to the briefing:
- EPISODIC = each news story as an event (what happened, source, when). This is
  what powers cross-day story-linking.
- SEMANTIC = the world model it builds over time (who the players are, trends).
- PROCEDURAL = the briefing STYLE (Casius's voice, one analogy per story,
  British English, 5 stories, "what happened / why it matters"). Seed this once.
- WORKING = today's in-progress briefing (the day's thread).

### Real API (use these calls, do not invent others)
```python
from oracleagentmemory.apis.searchscope import SearchScope
from oracleagentmemory.core import OracleAgentMemory
from oracleagentmemory.core.embedders.embedder import Embedder
from oracleagentmemory.core.llms.llm import Llm

# VERIFIED 2026-06-22 against VIBEBRIEF. OAMP embeds/extracts app-side via LiteLLM,
# so prefix models with "oci/" to hit Oracle's OCI GenAI (never a bare model name,
# which routes to a third party and demands a key). oci_* auth = api-key creds for
# the account with GenAI access; GenAI on-demand region is us-chicago-1 (NOT Phoenix).
oci_kwargs = dict(
    oci_user=OCI_USER, oci_tenancy=OCI_TENANCY, oci_fingerprint=OCI_FINGERPRINT,
    oci_key_file=OCI_KEY_FILE, oci_compartment_id=OCI_COMPARTMENT_ID,
    oci_region="us-chicago-1",
)
embedder = Embedder(model="oci/cohere.embed-english-v3.0", **oci_kwargs)
llm = Llm(model="oci/cohere.command-r-08-2024", **oci_kwargs)
memory = OracleAgentMemory(connection=pool, embedder=embedder, llm=llm,
                           schema_policy="create_if_necessary")  # creates OAMP tables on first run

thread = memory.create_thread(user_id="casius")
thread.add_memory("<a summarised story, or a fact, or a style rule>")

results = memory.search(query="<topic>", scope=SearchScope(user_id="casius"))
for r in results:
    # record_type is "memory" for add_memory. Do NOT switch to thread.add_messages
    # to get episodic/semantic: that extractor path currently 400s on OCI chat.
    print(r.record.record_type, r.content)
```

## Routes (the contract)
Keep routes thin. Five routes:

1. `/fetch` (POST) — parse an RSS URL with `feedparser`, return the 20 newest
   items as JSON {title, link, summary}. No DB.

2. `/summarise` (POST) — one article in, {summary} out. Generation runs IN the
   database with Select AI. Bind the prompt, run:
   ```sql
   SELECT DBMS_CLOUD_AI.GENERATE(
     prompt => :prompt, profile_name => 'VIBE_GENAI', action => 'chat'
   ) AS response FROM dual
   ```
   Prompt: "Summarise this article in two short sections. WHAT HAPPENED: one
   sentence. WHY IT MATTERS: one sentence. Title: <title>. Link: <link>".
   Read the returned CLOB to a string.

3. `/save` (POST) — store each summarised story as a memory AND detect follow-ups:
   - BEFORE saving, `memory.search(query=<story headline+summary>, scope=...)`.
     If a prior EPISODIC result is highly similar, this story is a FOLLOW-UP:
     return its linked headline so the UI can say "follow-up from <headline>".
   - Then `thread.add_memory(<headline + summary>)` so OAMP embeds and stores it.
     This is the WORKING path. Do NOT use `thread.add_messages(...)` for
     auto-classification: it currently fails on OCI chat (see Status). add_memory
     stores a generic "memory" record, which still powers the semantic search and
     the follow-up linking above.

4. `/brief` (GET) — the daily briefing. Gather today's stored stories, pull
   relevant SEMANTIC memories for trend context and the PROCEDURAL style memory,
   then run ONE Select AI `GENERATE` that returns a top-line summary + the 5 top
   stories, written in the procedural style (the analogy lens). Read CLOB to str.
   Do NOT show the source list in the output.

5. `/search` (POST) — `memory.search(query, scope=SearchScope(user_id="casius"))`,
   render results grouped by `record.record_type`.

## Seed the style once (procedural memory)
At setup, add the briefing style as a memory so the agent remembers HOW Casius
likes it, do not hardcode it only in a prompt:
```python
thread.add_memory(
  "STYLE RULE: write my daily briefing in my voice. Five top stories. For each: "
  "what happened, why it matters, and one analogy from a different domain. Link "
  "stories to earlier ones when they are follow-ups. British English. No em dashes. "
  "Give a one-line top-line summary of where the day is trending."
)
```

## Status (verified 2026-06-22)
WORKS via OCI GenAI, no external key: DB connect, OAMP construct (oci/ models +
schema_policy), `add_memory` (embeds), `search` (vector), and the follow-up linking.
Summaries and the `/brief` synthesis use in-DB Select AI, also working. That is
enough for the full experience: a briefing that remembers and links stories.

KNOWN ISSUE (deferred, not on the critical path): OAMP's message extraction
(`thread.add_messages`, the auto four-type classifier) fails with an OCI chat 400
through LiteLLM ("correct format of request"), even though a direct chat to the
same model works. So OAMP sends a param OCI rejects. Do NOT rely on auto
episodic/semantic/procedural tags yet; use `add_memory` (record_type "memory").
Smoke-tests in `.datacamp-secrets`: `oamp_smoketest_oci.py` (green),
`oamp_spike_types.py` (reproduces the 400).

The rehearsed raw-SQL happy-path app (DBMS_VECTOR + VECTOR_DISTANCE, no OAMP) stays
the live safety net. This OAMP backend is the richer "my version" + take-home.

## Public take-home note
For the audience fork, replace the VIBEBRIEF specifics (profile `VIBE_GENAI`,
the model names, the wallet/DSN) with their own Oracle AI Database values. The
contract and the rules above stay identical.
