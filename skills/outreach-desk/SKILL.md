---
name: outreach-desk
description: >
  Use when the user wants their email outreach command center — e.g. "open the outreach
  desk", "what's on for today", "review drafts", "who replied", "draft the next contact",
  "export for RocketFuel". A claude.ai Artifact (not a Cowork artifact) that runs the AI
  Outreach Engine daily loop: approval queue with the 1→5→10 ladder and the deliverability
  ramp cap, a working CRM with offer/angle matching, Gmail replies and draft creation,
  Google Calendar with meeting prep, projects, a video library, and an agent activity feed.
---

# Outreach Desk (claude.ai Artifact with runtime capabilities)

`templates/outreach-desk.html` is the page source. It is published with the Artifact tool
and these capabilities:

```
{ "db": {}, "sample": {}, "downloads": true,
  "mcp": { "servers": [
    { "server": "Gmail", "tools": ["search_threads", "get_thread", "create_draft"] },
    { "server": "Google Calendar", "tools": ["list_events"] },
    { "server": "Claude N8N", "tools": ["search_workflows", "search_workflow_executions"] } ] } }
```

- **db** holds `settings/main`, `contacts/*`, `queue/*` (drafts awaiting approval),
  `projects/*`, `videos/*`, `activity/feed` (capped list) and `log/<yyyy-mm-dd>` (one doc
  per day). Seed contacts with `write_db` batches, never by hardcoding rows in the page.
- **sample** drafts value-first emails (VALUE / CONVERSATION / LEAD_MAGNET, or SKIP when
  the angle is weak), matches offers and angles, suggests follow-ups from real replies, and
  builds meeting prep. Rules mirror the AI Outreach Engine prompts.
- **mcp** reads recent inbox threads and replies from CRM addresses, reads a thread in
  full for follow-ups, and creates Gmail drafts on approval. Calendar lists upcoming
  events. Every failure is branched by error code; the page degrades per section.
- **Agents tab**: watches the n8n connector (`search_workflows` every 2 min, `search_workflow_executions` every 1 min) and shows each Rockstar agent's state, last run, 24-hour success/failure counts, and the 30 most recent runs. Today carries a compact "Agents running" card. Hosted mode shows a not-linked notice.
- Without `db` the page runs local-only (browser storage); without `mcp` it still drafts
  and approves, and the user copies text into Gmail or LinkedIn.

## Guardrails built into the page
- Daily cap from the ramp table (10 → 15 → 20 → 25 → 40 → 60); held at 5 for the first
  three days when the domain is unauthenticated. Drafting stops at the cap.
- Approval ladder: 1 at a time; offer 5 after five clean approvals; 10 after fifteen.
  Heavy edits reset the streak and drop the batch size.
- Draft checks: under 120 words, one link, no banned phrases, no shouting, subject 3–7
  words without spam triggers, references the contact's angle.
- Follow-ups first: replied contacts always sit at the top of Next moves.

## Re-publishing
Edit the template, then publish the same file path (or pass the artifact `url`). Omit
`capabilities` to keep the stored declaration.

## Web-server build (`site/index.html`)
`site/index.html` is the same page wrapped as a full document for hosting on any static
server (Vercel, Netlify, S3). It detects the missing claude.ai runtime and switches to
hosted mode: data stays in that browser's local storage, drafting uses an Anthropic API
key the user enters in Settings (kept in local storage only, `claude-opus-5` over raw
HTTPS with server-side refusal fallbacks enabled), and Approve opens a prefilled Gmail
compose window instead of creating a draft through the connector. Calendar and reply
watching are not available in hosted mode. Regenerate it from the template when the
fragment changes: doctype + head (charset, viewport, noindex, the font link, the style
block) + body (the rest).
