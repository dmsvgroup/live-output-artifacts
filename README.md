# live-output-artifacts

A Claude plugin that bundles your MCP server with a skill so that, whenever your
output-generating tool runs, Claude renders the result as a **live, re-fetching
Cowork artifact** instead of plain chat text.

## What's inside

```
live-output-artifacts/
├── .claude-plugin/
│   ├── plugin.json          # plugin manifest
│   └── marketplace.json     # marketplace listing (for publishing)
├── .mcp.json                # ir-mcp server (remote HTTP)
├── commands/
│   ├── agents-home.md       # /agents-home
│   ├── rockstar-chat.md     # /rockstar-chat
│   ├── digital-dna.md       # /digital-dna [shallow|deep] [scan|build|connect|validate]
│   └── route-intelligence.md  # /route-intelligence — DMSV routing command center
├── skills/
│   ├── render-live-artifact/
│   │   ├── SKILL.md         # drives the auto-artifact behavior
│   │   └── templates/live-artifact.html
│   ├── rockstar-agents-home/  # command center + Outputs tab
│   ├── rockstar-chat/         # chat panel wired to the rockstar orchestrator
│   ├── dmsv-route-intelligence/  # executive routing console (map, AI score, memo)
│   └── digital-dna/
│       ├── SKILL.md         # SCAN → BUILD → CONNECT → VALIDATE
│       ├── reference/
│       │   ├── dna-scan-prompt.md   # paste into Claude/ChatGPT; MODE: shallow|deep
│       │   └── connect-guide.md     # MCP link → Claude connector / ChatGPT (Streamable) → Authenticate
│       └── templates/
│           ├── persona.md   # pasted into the Brain Builder co-pilot
│           ├── knowledge.md # uploaded via Attachments
│           ├── brain_scan.md# the first skill — pasted, never uploaded
│           └── dna-scorecard.html   # live with-brain vs without-brain scorecard
└── README.md
```

The **skill** is what makes the behavior automatic; the **plugin** packages the
skill + server together so it installs in one step. The plugin adds no new
capability beyond Cowork's existing `create_artifact` — it just makes the setup
repeatable and shippable.

## Already configured

- Server: `ir-mcp` at `https://paytest.testir.xyz/mcp` (`.mcp.json`).
- The skill triggers on **any** tool from the `ir-mcp` server, so new
  orchestrators you expose later are covered automatically — no per-tool edits.

## Ready to publish

Repo: `https://github.com/Sunil3696/live-output-artifacts`. Nothing left to fill
in. (In the artifact template, `TOOL` is a placeholder Claude substitutes with the
actual orchestrator name at artifact-creation time — you don't hardcode it.)

If your server uses SSE rather than streamable HTTP, change `"type": "http"` to
`"type": "sse"` in `.mcp.json`. (HTTP is recommended where supported.)

## Publish it

1. Push this folder to a GitHub repo (it can be both the plugin and the
   marketplace, since `marketplace.json` lists itself).
2. Validate locally: `claude plugin validate --plugin-dir ./live-output-artifacts`

## Install it (what your users run)

```
/plugin marketplace add Sunil3696/live-output-artifacts
/plugin install live-output-artifacts
/reload-plugins
```

After install, the MCP server connects and the skill activates automatically —
when any `ir-mcp` orchestrator produces output, Claude renders it as a live artifact.

## Digital DNA (personalized output via a MyGentic brain)

LLMs lose context in long chats and drift into generic, "AI-sounding" text. A MyGentic
brain is a permanent, token-efficient knowledge graph the LLM queries over MCP instead of
re-reading your history. `/digital-dna` runs the four stages:

1. **Scan** your AI history (`shallow` first, `deep` on the rerun) into three files —
   `persona.md` (goal + voice), `knowledge.md` (why + what), `brain_scan.md` (how + now, a
   skill). Edit them for accuracy before ingestion.
2. **Build** the brain in the MyGentic Brain Builder: persona pasted into the co-pilot,
   knowledge uploaded via Attachments, skills pasted as text (uploading makes them knowledge).
3. **Connect** the brain's MCP link (Publish section) as a Claude custom connector or a
   ChatGPT connector with Streamable on, then click **Connect / Authenticate**.
4. **Validate** with the live scorecard artifact: one prompt, generated with and without the
   brain, blind-scored 0–100% on voice, audience, offers, AI-isms, and actionability. The
   reference demo scored 70% with the brain vs 24% without.

## A note on "live" + LLM output

Live artifacts re-run your tool every time the page is opened. If your tool
regenerates LLM output on each call, the page will produce different results on
each reload. The skill handles this: if the output isn't meaningfully refreshable,
it falls back to a one-time static artifact. Decide which fits your tool.
