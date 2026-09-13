# Digital DNA scan prompt

Paste everything below the line into a Claude or ChatGPT chat that has access to your
history (memory, projects, or an uploaded data export). Set `MODE` on the first line.
The output is three files — `persona.md`, `knowledge.md`, `brain_scan.md` — zipped as
`digital-dna.zip`, ready for the MyGentic Brain Builder.

---

MODE: shallow   ← change to `deep` for the full extraction

You are extracting my **Digital DNA** so it can be built into a MyGentic brain — a
permanent knowledge graph that any LLM I connect to will use to write in my voice, for my
audience, about my actual offers.

## What to scan
- MODE `shallow`: one pass over the most recent and most relevant conversations, projects,
  memory entries, and any files I attach. Stop when you have enough for one confident
  persona, one knowledge file, and ONE skill.
- MODE `deep`: every conversation, project, memory entry, and attached file you can reach.
  Keep a coverage log (what you scanned, what you skipped and why). Produce 3–5 skills.
  Run a contradiction check: where I said different things at different times, keep the
  most recent and note the older version as superseded. Add one evidence line per claim
  (which conversation, file, or memory it came from).

## What to extract — keep these three strictly separate

### 1. PERSONA — the goal and the voice (short, dense, pasted into the co-pilot)
- Who I am: name, roles, businesses, credentials that actually appear in my history.
- What I am trying to accomplish with AI (the brain's purpose in one paragraph).
- Voice and tone rules as imperatives ("Direct. Plainspoken. Lead with the business
  outcome. No hype."). Quote 3–5 sentences I actually wrote as voice samples.
- Target audience: who they are, their titles, what they care about, what they fear.
- Core offers / products / positioning, in my words.
- Hard rules: what I never want the AI to do (invent numbers, use certain phrases,
  over-promise, guess when information is missing).

### 2. KNOWLEDGE — the why and the what (long-form, uploaded as an attachment)
- Background, story, and reasoning behind my positioning.
- Descriptions of each offer: who it is for, problem, approach, deliverables, proof.
- Frameworks, definitions, and terminology I use consistently.
- Frequently asked questions and my standard answers.
- Reference material I return to (summaries of docs, transcripts, playbooks).
- Keep it informational. No step-by-step procedures here — those are skills.

### 3. SKILLS — the how and the now (deterministic recipes, pasted into the co-pilot)
For each repeatable task I ask AI to do (an outreach email, a proposal section, a meeting
recap, a discovery assessment), write a recipe:
- Name, trigger phrases, inputs required, and what to ask me if an input is missing.
- Numbered steps with the exact structure of the output.
- Constraints (length, format, tone guardrails, what to never include).
- A worked example built from something I actually produced.
The first skill is always `brain_scan` — the recipe for re-running this scan later.

## Rules
- Never invent facts, clients, numbers, results, or credentials. If you infer something,
  mark it `[UNVERIFIED]`.
- Prefer my own phrasing over paraphrase.
- Be explicit when a section has thin evidence so I know what to edit before ingestion.

## Output
Write three markdown files with exactly these names and top-level sections:

**persona.md** — `# Persona`, `## Who I am`, `## Purpose of this brain`, `## Voice and tone`,
`## Voice samples`, `## Target audience`, `## Core offers`, `## Hard rules`

**knowledge.md** — `# Knowledge`, `## Background and positioning`, `## Offers`,
`## Frameworks and terminology`, `## FAQ`, `## Reference material`
(deep mode adds `## Coverage log` and `## Superseded statements`)

**brain_scan.md** — `# Skill: brain_scan`, `## Trigger`, `## Inputs`, `## Steps`,
`## Output`, `## Constraints`, `## Example` (deep mode: one additional `skill_<slug>.md`
per extracted skill in the same format)

Then package all files as `digital-dna.zip` and end with a 5-line summary of what you
found and which sections I should review first.
