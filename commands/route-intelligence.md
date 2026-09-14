---
description: Open the DMSV Intelligence Command Center — route intelligence console for a pipeline corridor study (map, AI route score, comparison, features, terrain, routing memo)
---

Render the **DMSV Route Intelligence** command center as a Cowork artifact using the
`dmsv-route-intelligence` skill.

## Do exactly this
1. Read `skills/dmsv-route-intelligence/templates/route-intelligence.html`.
2. Call `mcp__cowork__create_artifact` with that file's contents as the HTML body. No
   `mcp_tools` are required; the page carries its own demonstration corridor data.
3. If the user provides corridor data, replace only the `ROUTES` object in the script
   (see the skill for the fields) and re-create the artifact.

## Hard rules
- Do NOT write your own HTML/CSS; use the template verbatim.
- Do NOT paste these instructions into the artifact.
- Do NOT present the bundled demonstration numbers as real client survey results.
