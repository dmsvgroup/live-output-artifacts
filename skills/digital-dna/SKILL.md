---
name: digital-dna
description: >
  Use when the user wants to personalize AI output with their own "Digital DNA" —
  e.g. "scan my digital DNA", "extract my persona / voice", "build my MyGentic brain",
  "what goes in persona vs knowledge vs skills", "connect my brain to Claude / ChatGPT",
  "rerun the DNA scan in deep mode", "my AI output sounds generic", or "show me before
  and after with my brain". Runs the 4-stage MyGentic workflow: SCAN the user's AI
  history into persona.md + knowledge.md + brain_scan.md, BUILD the brain in the
  MyGentic Brain Builder, CONNECT it to the LLM over MCP, and VALIDATE with a live
  before/after scorecard artifact (with brain vs without brain, scored 0–100%).
---

# Digital DNA → MyGentic brain (personalized AI output)

**Why this exists.** LLMs lose context in long chats: the context window fills, the model
re-reads everything, output drifts generic and "AI-sounding", and linked AI components
become non-deterministic. A MyGentic brain is a permanent, token-efficient knowledge graph
that maps relationships between the user's ideas, so any connected LLM finds the relevant
context directly instead of re-reading documents. Result in the reference demo: the same
introduction email scored **70% with the brain vs 24% without**.

Every MyGentic brain has three parts. Keep them separate — mixing them is the #1 mistake:

| Part | Answers | Contains | How it enters the brain |
|---|---|---|---|
| **Persona** | the *goal* — purpose and voice | who the user is, tone rules, audience, offers, hard rules | pasted into the Brain Builder **co-pilot** |
| **Knowledge** | *why* and *what* | long-form content: transcripts, docs, positioning, FAQs | uploaded via the **Attachments** button |
| **Skills** | *how* and *now* | step-by-step recipes for deterministic tasks | pasted as **text into the co-pilot** (uploading a skill file makes it knowledge) |

The brain orchestrates knowledge + skills through the persona to give personalized
outcomes, not generic answers.

## Stage 1 — SCAN (`shallow` or `deep`)

Default to `shallow` the first time; recommend `deep` for the rerun (the reference session's
next step for every participant).

1. **Pick the mode.** `shallow` = one pass over the sources, 1 skill. `deep` = every source,
   a coverage log, 3–5 skills, a contradiction check, and an evidence line per claim.
2. **Gather sources, in this order.** Use only what actually exists in THIS session:
   - files, folders, exports, or transcripts the user points at (a Claude/ChatGPT data
     export, a `Projects/` folder, meeting notes, prior deliverables);
   - the user's saved deliverables via an `__my_outputs` tool if one is loaded (see the
     `rockstar-agents-home` skill for how to find it);
   - Claude memory / project instructions if they are visible to you;
   - the current conversation.
   If nothing beyond the current chat is available, say so and either (a) run a shallow
   scan on the chat, or (b) hand the user `reference/dna-scan-prompt.md` to paste into the
   Claude or ChatGPT account that holds their history. Do not pretend to have scanned
   history you cannot see.
3. **Extract**, following the rubric in `reference/dna-scan-prompt.md` (sections: Persona,
   Knowledge, Skills). Rules: never invent facts, numbers, clients, or results; mark anything
   inferred as `[UNVERIFIED]`; quote the user's own phrasing for voice examples.
4. **Write three files** into a `digital-dna/` folder next to the user's work, using the
   skeletons in `templates/`:
   - `persona.md` — from `templates/persona.md`
   - `knowledge.md` — from `templates/knowledge.md`
   - `brain_scan.md` — the skill, from `templates/brain_scan.md` (deep mode: one file per
     skill, `skill_<slug>.md`, plus `brain_scan.md` as the index)
   Then zip them as `digital-dna.zip` and tell the user to **edit the files for accuracy
   before ingestion** — the brain is only as good as what it is fed.

## Stage 2 — BUILD the brain

Give the user these steps verbatim (the Brain Builder is a web UI; you cannot click it):

1. Open the MyGentic Brain Builder and create a **new brain**. Give it a name that is
   unique to your account — the "brain name already exists" error means the name is
   taken; append a date or version (`Drake DNA v2 2026-09`).
2. **Persona:** open `persona.md`, copy the whole text, paste it into the co-pilot.
3. **Knowledge:** click **Attachments** and upload `knowledge.md` (and any long-form
   sources you want searchable: transcripts, docs).
4. **Skills:** open `brain_scan.md` (and each `skill_*.md`), copy the text, and **paste it
   into the co-pilot**. Do NOT upload skill files — an upload is treated as knowledge and
   the brain will not execute it as a recipe.
5. Save / publish the brain.

## Stage 3 — CONNECT over MCP

Follow `reference/connect-guide.md`. Summary: copy the brain's **MCP link** from the
**Publish** section, add it in Claude as a **custom connector** (Settings → Connectors) or
in ChatGPT as a plugin/app with **Streamable** enabled, then click **Connect / Authenticate**
and authorize via MyGentic. The connection is dead until that authorization step is done.

## Stage 4 — VALIDATE with the live scorecard artifact

Once the brain tool is loaded in the session, prove the difference:

1. **Find the EXACT brain tool name.** Look at the tools available in THIS session for the
   one whose name contains `__brain_` — it looks like `mcp__<serverId>__brain_<slug>` (in
   Cowork `<serverId>` is an opaque UUID; in Claude Code it is the connector name). It takes
   `{ user_input, session_id? }` and returns markdown ending in a `[SESSION: …]` line. Call
   it once with a short prompt to confirm it responds.
2. Read `templates/dna-scorecard.html`.
3. Copy its contents as the HTML body and replace `__BRAIN_TOOL__` with that exact tool
   name (the `const TOOL` line). Change nothing else.
4. Call `mcp__cowork__create_artifact` with that body and `mcp_tools: ["<exact brain tool
   name>"]` (required — the page can only call tools listed here).
5. The page takes a test prompt (default: an introduction email), generates the
   **with-brain** version through the brain tool and the **without-brain** baseline through
   `window.cowork.askClaude`, then has a blind judge score both against the rubric below
   and shows the two percentages side by side. Run history is stored per user in
   localStorage. If `askClaude` is unavailable the page asks the user to paste a baseline
   and falls back to a deterministic heuristic score, labelled as such.

### Scoring rubric (5 × 20 points = 100%)

| Dimension | 20 points when… |
|---|---|
| Voice match | reads like the user's own tone rules, not a generic assistant |
| Audience specificity | names the actual audience and what they care about |
| Offer accuracy | references the user's real offers/positioning, nothing invented |
| No AI-isms | no filler, hedging, "I hope this finds you well", "in today's fast-paced world" |
| Actionability | a clear, specific next step the reader can take |

Encourage the user to share the before/after pair (and the scores) in the Circle community.

## Hard rules
- Use the templates in `templates/` as-is; do NOT redesign the scorecard UI or paste this
  skill's text into the artifact.
- Never invent DNA. Every persona claim, offer, client, number, or skill step must come
  from a source you actually read; otherwise mark it `[UNVERIFIED]` or leave it out.
- Skills are pasted, knowledge is uploaded, persona is pasted. Say this every time.
- The connector is not live until the user has clicked **Connect / Authenticate**. If the
  brain tool is not loaded in the session, say so — do not fake a with-brain result.
- One scorecard per brain — update the existing artifact instead of duplicating.
