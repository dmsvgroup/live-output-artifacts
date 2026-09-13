---
description: Run the Digital DNA workflow — scan your AI history into persona / knowledge / skills files, build and connect a MyGentic brain, then prove it with a live before-vs-after scorecard artifact
---

Run the **Digital DNA** workflow from the `digital-dna` skill. Argument: `shallow`
(default) or `deep`, optionally followed by a stage: `scan`, `build`, `connect`, `validate`.
With no stage, run the stages in order and stop at the first one that needs the user.

## Do exactly this
1. **SCAN** — read `skills/digital-dna/reference/dna-scan-prompt.md` for the extraction
   rubric. Gather only sources that exist in THIS session (files the user points at, an
   `__my_outputs` tool if loaded, visible memory, the current chat). Write `digital-dna/persona.md`,
   `digital-dna/knowledge.md`, `digital-dna/brain_scan.md` from the skeletons in
   `skills/digital-dna/templates/`, zip them as `digital-dna.zip`, and tell the user to edit
   for accuracy before ingestion. Never invent DNA; mark inferences `[UNVERIFIED]`. If no
   history is reachable, say so and hand the user the scan prompt to paste into the
   Claude/ChatGPT account that holds it.
2. **BUILD** — give the Brain Builder steps: persona → pasted into the co-pilot; knowledge →
   uploaded via **Attachments**; skills → pasted as text (uploading makes them knowledge).
   "Brain name already exists" = pick a unique name.
3. **CONNECT** — walk through `skills/digital-dna/reference/connect-guide.md`: copy the MCP
   link from **Publish**, add as a Claude custom connector or a ChatGPT connector with
   **Streamable** on, then **Connect / Authenticate** via MyGentic.
4. **VALIDATE** — find the EXACT loaded tool whose name contains `__brain_`
   (`mcp__<serverId>__brain_<slug>`; UUID serverId in Cowork — read it live, never hardcode).
   Read `skills/digital-dna/templates/dna-scorecard.html`, replace `__BRAIN_TOOL__` with that
   name (the `const TOOL` line only), and call `mcp__cowork__create_artifact` with that body
   and `mcp_tools: ["<exact brain tool name>"]`. The page generates with-brain (via the
   brain) and without-brain (via `askClaude`) versions of a test prompt and blind-scores both
   0–100% on voice, audience, offers, AI-isms, and actionability.

## Hard rules
- Do NOT redesign the scorecard or paste these instructions into the artifact.
- Do NOT fake a with-brain result when the brain tool is not loaded — say the connector
  needs to be authenticated first.
- One scorecard per brain — update the existing artifact instead of duplicating.
