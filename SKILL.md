---
name: renpy-character-skill-builder
description: Use when a user wants project-local character skills for a Ren'Py visual novel, especially from source scripts, extracted story text, romance routes, gallery/PAX relationship systems, or max-affection character branches.
---

# Ren'Py Character Skill Builder

## Overview

Build project-local character skills from a Ren'Py visual novel by grounding every skill in source text, route variables, relationship UI evidence, gallery/replay metadata, and the highest-affection branch. The goal is not a lore summary; the goal is a reusable character model that can write new scenes and direct chats with the same voice, memory, boundaries, and choices as the source character.

This workflow skill itself may be installed in an agent skill directory such as `.codex/skills` for reuse across projects. That does not change where it writes character skills: generated character skills are project artifacts and must be placed inside the current game/project folder.

Use this together with `extract-renpy-story` when raw story text has not already been extracted.

## Workflow

1. Locate and prepare Ren'Py sources.
   - Prefer `.rpy` files. If only `.rpyc` files exist, decompile first using `extract-renpy-story`.
   - Common Android roots include `assets/x-game/`; desktop projects usually use `game/`.
   - Confirm character definitions, route variables, gallery/replay data, relationship screens, and story scripts are all available before writing character skills.

2. Extract and audit player-visible story text.
   - Use `extract-renpy-story` rules for story order, speaker rendering, thought/speech distinction, and protagonist normalization.
   - Keep per-story or per-route outputs when possible; do not rely only on one giant merged file.
   - Audit extracted text for unresolved variables, resource paths, leftover Ren'Py tags, fake speakers, and missing thought markers.

3. Identify the target character set from systems, not memory.
   - Use relationship and romance systems first: variables such as `<name>_points`, `<name>path`, `<name>u`, heart tables, PAX/phone panels, romance flags, and route gates.
   - Cross-check with gallery filters, replay entries, bios, character definitions, and side-character buckets.
   - Treat grouped labels such as `side`, `all`, or generic NPC roles as groups, not individual character skills, unless the user explicitly asks for side characters.
   - Record evidence in the character skill: speaker keys, route variables, active branch flags, and major story contexts.

4. Build a per-character canon dossier.
   - Gather dialogue and scenes for the character from extracted story text and source `rg`.
   - If this project already has good character skills, use them as style and quality exemplars. Still rebuild evidence from source text instead of copying assumptions.
   - Separate:
     - voice markers: rhythm, vocabulary, humor, profanity, sentence length, pet names, stammers, formal address.
     - core contradiction: what the character wants versus what scares or limits them.
     - experiences: events that explain current behavior.
     - relationship behavior: what the protagonist did in the successful branch.
     - boundaries: what the character still refuses even in intimacy.
   - Use multiple scene types: daily chat, danger, jealousy, family conflict, vulnerability, romance, and recovery after fear or failure.

5. Lock the active branch to the most intimate route.
   - Use the highest-affection, most intimate protagonist branch as the only active branch unless the user asks for analysis.
   - Assume route path flags are active, relevant `u`/met flags are true, highest useful point thresholds are reached, and late route gates such as love confession, together flags, private trust, or relationship negotiation have resolved in the protagonist's favor.
   - Treat missed choices, low-affection outcomes, failed honesty checks, friendship-only paths, and alliance-only readings as inactive alternate branches.
   - Preserve earlier experiences as memory and explanation only. Do not make the character regress to earlier uncertainty in direct chat.

6. Write one project-local skill per character.
   - Put generated character skills inside the current game/project folder, not in `.codex/skills`, `.agents/skills`, or other auto-discovered agent skill directories.
   - Do not confuse this workflow skill's install location with its output location. Even when this workflow skill is loaded from an agent skill directory, its per-character outputs belong in the project being analyzed.
   - Recommended layout:

```text
character_skills/
  character-name/
    SKILL.md
    agents/openai.yaml
```

   - Use lowercase hyphen-safe folder names. For single-name characters, use the name directly.
   - Keep files ASCII unless the project already requires another character set.

7. Use this `SKILL.md` structure for each character.

```markdown
---
name: character-name
description: Use when generating in-character dialogue, original scenes, or roleplay/chat responses as Character from Project, especially for [distinctive contexts].
---

# Character

## Purpose
[One paragraph: what this skill writes and what must feel true.]

[Direct chat rule: speak in first person as Character; step out for analysis.]

## Character Model
[Identity, role in story, core contradiction, not-a-stereotype guardrails.]

## Canon Anchors
- Main speaker keys: `...`.
- Route variables: `...`.
- Active branch: assume the highest-affection branch with route path active and deepest relationship flags resolved in the protagonist's favor.
- Major contexts: ...

## Relationship State
Use the highest-affection, most intimate protagonist branch as the only active branch. Treat relationship as a stable current state, not a route timeline or a choice tree.

Default current state for open-ended chat: ...

State handling:
- Keep accumulated trust, shared danger, romantic clarity, and private history active.
- Treat missed choices and low-affection outcomes as inactive alternate branches.
- If older events are mentioned, treat them as remembered history from the current relationship state.
- Do not simulate lower-intimacy branches unless the user explicitly asks for analysis outside role.

## Voice
[Concrete speech rhythm, vocabulary, tells, and direct-chat feel.]

## Experiences And Growth
[Process history that explains current behavior. Keep it memory, not active stage switching.]

## Decision Rules
In the locked max-intimacy branch, assume the protagonist:
- [successful choices and patterns]

Do not base this branch on choices where he:
- [failed choices and hard violations]

If a scene asks, "What would Character choose?", use this priority order:
1. ...

## Story Use
[Scene construction rules, good conflicts, bad conflicts.]

## Direct Chat Rules
[How the character starts, responds to praise, fear, flirting, lies, sadness, and conflict.]

## Avoid
[Long canon copying, stereotypes, flattened voice, broken boundaries.]
```

8. Write `agents/openai.yaml`.
   - Keep it small and human-facing.
   - The default prompt must name the skill and the project-local path.

```yaml
interface:
  display_name: "Character"
  short_description: "Distinctive voice cue."
  default_prompt: "Use $character-name from the project-local character_skills/character-name skill to write a conversation or scene in Character's voice."
```

9. Validate and audit.
   - Run `quick_validate.py` on every generated character skill and on this workflow skill.
   - Search for template remnants from scaffolded skill files, including placeholder brackets and scaffold instruction headings.
   - Search for branch drift: `Relationship Stage Defaults`, `route stage`, `moves toward`, `pulls away`, `not confirmed`, `low trust`, `trust level`.
   - Search for unwanted output-location assumptions: generated character skills, their metadata, and their default prompts must point to project-local `character_skills/...` paths, not agent skill directories.
   - Check all `.md` and `.yaml` files for non-ASCII if the project convention is ASCII.

## Quality Bar

A character skill is good enough only when a reader can use it to answer both questions:

- "How would this character talk to me right now in the max-intimacy branch?"
- "If this character must choose in a new scene, what would they do and why?"

Reject skills that only list traits. Each skill needs voice, experiences, active branch, relationship state, decision rules, story use, direct chat rules, and avoid rules.

Do not add generic moralizing, installation notes, or safety/disclaimer boilerplate to character skills. If age, consent, or boundaries matter in the source, record them as concrete canon facts and behavior rules, not as external warnings that pollute the character model.

## Common Mistakes

- Building the character list from memory instead of relationship/gallery systems.
- Summarizing plot instead of extracting decision logic.
- Leaving route stages active, which makes the model drift back to early or low-affection behavior.
- Writing a generic "romanceable girl" voice instead of concrete speech habits.
- Keeping failed branch choices as if they are current history.
- Omitting capability limits for specialists such as hackers, royals, fighters, or magic users.
- Writing generated character skills into `.codex/skills` or another agent skill directory just because this workflow skill is installed there.

## Final Checklist

- Character scope is justified by source evidence.
- Story text extraction has been audited.
- Every target character has a `SKILL.md` and `agents/openai.yaml`.
- Every character skill locks to the highest-affection active branch.
- Prior events are memory, not relationship-stage switches.
- Direct chat uses first person and can step out for analysis.
- Validation passes for every skill.
- Grep audits show no template remnants or old branch-stage wording.
