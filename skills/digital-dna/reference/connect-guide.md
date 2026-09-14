# Connect a MyGentic brain to your LLM (MCP)

The brain does nothing until the LLM can call it, and the LLM cannot call it until you
have **authenticated** the connection. Every step below matters.

## 1. Get the MCP link
1. Open the brain in the MyGentic Brain Builder.
2. Go to the **Publish** section.
3. Copy the brain's **MCP link** (an `https://…` URL).

## 2a. Claude (web / desktop)
1. **Settings → Connectors** (organization admins: the org connector settings).
2. **Add custom connector**, paste the MCP link as the remote server URL, save.
3. Click **Connect** on the new connector and authorize in the MyGentic window that opens.
4. In a new chat, open the tools menu and confirm the brain is enabled. The tool appears as
   `brain_<slug>` under the connector's name.

## 2b. ChatGPT
1. **Settings → Apps & connectors** (developer mode / plugins, depending on your plan).
2. Add the MCP server with the MCP link, and enable **Streamable** (streamable HTTP
   transport). Without it the connection will time out or return nothing.
3. Click **Authenticate** and authorize via MyGentic.
4. In a new chat, enable the connector for that conversation.

## 2c. Claude Code / Cowork
Add the server to `.mcp.json` (or `claude mcp add --transport http <name> <mcp-link>`),
then run `/mcp` in an interactive session and complete the OAuth login. In Cowork live
artifacts the tool routes by the fully-qualified name `mcp__<serverId>__brain_<slug>`, where
`<serverId>` is a UUID that changes per install — read it live, never hardcode it.

## 3. Verify
Ask the LLM: *"Using my brain, summarize my persona in 5 lines."* If the answer names your
real business, audience, and offers, the connection works. If it answers generically, the
connector is not authorized or not enabled for that chat.

## Common errors
| Symptom | Fix |
|---|---|
| "Brain name already exists" when creating the brain | Names are unique per account — add a version or date to the name. |
| Connector added but the LLM ignores the brain | You skipped **Connect / Authenticate**, or the connector is not enabled for this chat. |
| ChatGPT connection hangs | **Streamable** transport is off. |
| Skill is answered like a document instead of executed | It was uploaded as an attachment (knowledge). Paste the skill text into the co-pilot instead. |
| Output still sounds generic | The persona is thin. Rerun the scan in `deep` mode and edit the files before ingestion. |
