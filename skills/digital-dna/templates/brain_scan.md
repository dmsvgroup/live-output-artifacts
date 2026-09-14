# Skill: brain_scan

<!-- Pasted as TEXT into the Brain Builder co-pilot. Do not upload this file — an upload is
     treated as knowledge and the brain will not execute it. Skills are the HOW and the
     NOW: deterministic, numbered, with a fixed output shape. -->

## Trigger
"scan my digital DNA", "rerun the DNA scan", "update my persona", "deep scan".

## Inputs
- `mode`: `shallow` (default) or `deep`.
- Sources: the conversations, memory, projects, exports, or files available in the session.
- If no sources beyond the current chat are available: say so and ask for an export or a
  folder before continuing.

## Steps
1. Confirm the mode and list the sources that will be scanned.
2. Read the sources. In `deep` mode keep a coverage log (scanned / skipped / why).
3. Extract the Persona: identity, purpose, voice rules, verbatim voice samples, audience,
   offers, hard rules. Mark inferences `[UNVERIFIED]`.
4. Extract the Knowledge: positioning, offers in depth, frameworks, FAQ, reference
   summaries. Informational only — no procedures.
5. Extract the Skills: every repeatable task with a name, trigger, inputs, numbered steps,
   output shape, constraints, and a real example. `shallow` = 1 skill; `deep` = 3–5.
6. `deep` mode: run the contradiction check and record superseded statements.
7. Write `persona.md`, `knowledge.md`, `brain_scan.md` (+ `skill_<slug>.md` per extra
   skill) and zip them as `digital-dna.zip`.
8. Return a 5-line summary: what was found, thin sections to review, and the next step
   (build → connect → validate).

## Output
- `digital-dna.zip` containing the files above, each using the section headings from the
  matching template.

## Constraints
- Never invent facts, clients, numbers, or results.
- Prefer the user's own phrasing over paraphrase.
- Keep persona under ~600 words; knowledge has no limit; each skill under ~400 words.

## Example
Input: `mode: shallow`, sources: this conversation + `meeting-notes/`.
Output: `digital-dna.zip` with a persona for an industrial AI consultant (direct, outcome-led
voice; EPC and utilities audience; two offers), a knowledge file with both offers and an
FAQ, and one skill, `brain_scan`. Summary flags the "Proof" sections as `[UNVERIFIED]`.
