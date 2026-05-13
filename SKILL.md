---
name: renpy-character-skill-builder
description: Use when a user wants project-local character skills for a Ren'Py visual novel, especially from extracted story text, romance routes, gallery/PAX relationship systems, max-affection branches, existing character-skill repositories, or game-update reruns.
---

# Ren'Py Character Skill Builder

## Overview

Build project-local character skills from a Ren'Py visual novel by grounding every skill in source text, route variables, relationship UI evidence, gallery/replay metadata, and the highest-affection branch. The goal is not a lore summary; the goal is a reusable character model that can write new scenes and direct chats with the same voice, memory, boundaries, and choices as the source character.

This workflow skill itself may be installed in an agent skill directory such as `.codex/skills` for reuse across projects. That does not change where it writes character skills: generated character skills are project artifacts and must be placed inside the current game/project folder.

When the user refers to this skill builder, its source repository is the installed `.codex/skills/renpy-character-skill-builder` folder. Only generated character skills belong in the analyzed game/project folder.

Use this on story text extracted by `renpy-story-extraction-skill` whenever possible: `https://github.com/atmostfair/renpy-story-extraction-skill.git`. If raw story text has not already been extracted, extract it first with that skill or the local `extract-renpy-story` workflow.

## Workflow

1. Locate and prepare Ren'Py sources.
   - Prefer `.rpy` files. If only `.rpyc` files exist, decompile first using `extract-renpy-story`.
   - Common Android roots include `assets/x-game/`; desktop projects usually use `game/`.
   - Confirm character definitions, route variables, gallery/replay data, relationship screens, and story scripts are all available before writing character skills.

2. Extract and audit player-visible story text.
   - Prefer story text produced by `renpy-story-extraction-skill`: `https://github.com/atmostfair/renpy-story-extraction-skill.git`.
   - Use `extract-renpy-story` rules for story order, speaker rendering, thought/speech distinction, and protagonist normalization.
   - Keep per-story or per-route outputs when possible; do not rely only on one giant merged file.
   - Audit extracted text for unresolved variables, resource paths, leftover Ren'Py tags, fake speakers, and missing thought markers.
   - Record the extraction source in the generated character-skill repository README.

3. Detect create, update, or no-op mode.
   - Before writing anything, check whether a generated character-skill repository already exists. Look for `character_skills/.git`, `character_skills/README.md`, `character_skills/*/SKILL.md`, or a user-provided repository root containing character folders.
   - If no character skills exist, run in create mode.
   - If character skills already exist, run in update mode. Treat existing skills as valuable artifacts, not disposable drafts.
   - In update mode, inspect `git status --short` in the character-skill repository. Do not overwrite dirty user edits. If dirty files overlap the intended update, either integrate carefully with those edits or stop and report the conflict.
   - Compare current extraction inputs against the last generated state before changing skills. Use available evidence: Git diff, extraction output hashes, source script version markers, file timestamps, README provenance, route variables, gallery/PAX data, and changed character dialogue.
   - Maintain or update a small machine-readable manifest when possible, such as `build-manifest.json`, recording source/extraction paths, relevant file hashes, target character list, generation date, and builder version/commit. Use it only as evidence; still verify changed story content directly when hashes differ.
   - If the extraction output and relevant route/relationship evidence have not changed, stop after validation and report that no character-skill changes are needed. Do not rewrite, rephrase, reorder, normalize, or "improve" existing skills when there is no source change.
   - If only some characters changed, update only those character skills and shared repository files that must change. Leave unaffected character files byte-for-byte unchanged whenever possible.
   - If a game update adds a new character, add a new character skill without regenerating unchanged existing ones.
   - If a game update changes route state, relationship flags, or the max-affection branch for a character, update that character's active branch, decision rules, and relationship state from source evidence.
   - If a game update removes or contradicts old canon, preserve useful prior history only when it remains canon-compatible. Otherwise mark superseded facts explicitly or remove them with evidence.

4. Identify the target character set from systems, not memory.
   - Use relationship and romance systems first: variables such as `<name>_points`, `<name>path`, `<name>u`, heart tables, PAX/phone panels, romance flags, and route gates.
   - Cross-check with gallery filters, replay entries, bios, character definitions, and side-character buckets.
   - Treat grouped labels such as `side`, `all`, or generic NPC roles as groups, not individual character skills, unless the user explicitly asks for side characters.
   - Record evidence in the character skill: speaker keys, route variables, active branch flags, and major story contexts.

5. Build a per-character canon dossier.
   - Gather dialogue and scenes for the character from extracted story text and source `rg`.
   - If this project already has good character skills, use them as style and quality exemplars. Still rebuild evidence from source text instead of copying assumptions.
   - Separate:
     - voice markers: rhythm, vocabulary, humor, profanity, sentence length, pet names, stammers, formal address.
     - core contradiction: what the character wants versus what scares or limits them.
     - experiences: events that explain current behavior.
     - relationship behavior: what the protagonist did in the successful branch.
     - boundaries: what the character still refuses even in intimacy.
   - Use multiple scene types: daily chat, danger, jealousy, family conflict, vulnerability, romance, and recovery after fear or failure.

6. Lock the active branch to the most intimate route.
   - Use the highest-affection, most intimate protagonist branch as the only active branch unless the user asks for analysis.
   - Assume route path flags are active, relevant `u`/met flags are true, highest useful point thresholds are reached, and late route gates such as love confession, together flags, private trust, or relationship negotiation have resolved in the protagonist's favor.
   - Treat missed choices, low-affection outcomes, failed honesty checks, friendship-only paths, and alliance-only readings as inactive alternate branches.
   - Preserve earlier experiences as memory and explanation only. Do not make the character regress to earlier uncertainty in direct chat.

7. Write one project-local skill per character.
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

8. Use this `SKILL.md` structure for each character.

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

9. Write `agents/openai.yaml`.
   - Keep it small and human-facing.
   - The default prompt must name the skill and the project-local path.

```yaml
interface:
  display_name: "Character"
  short_description: "Distinctive voice cue."
  default_prompt: "Use $character-name from the project-local character_skills/character-name skill to write a conversation or scene in Character's voice."
```

10. Organize generated character skills as a project-local Git repository.
   - Treat the generated `character_skills/` output root as a repository-ready artifact.
   - If `character_skills/` is not already inside the intended Git repository, run `git init` in `character_skills/`.
   - If the user provided a dedicated output repository path, put the character folders at that repository root instead of nesting them twice.
   - Add a bilingual `README.md` to the generated character-skill repository.
   - The README must prominently state that these character skills are built from story text extracted by `renpy-story-extraction-skill`: `https://github.com/atmostfair/renpy-story-extraction-skill.git`.
   - Include source provenance, output structure, use examples, relationship-state policy, max-affection branch policy, and rerun/update behavior.
   - Add and commit generated files when Git is available and actual files changed. Push only when a remote is already configured or the user explicitly gives one.

Use this README shape for generated character-skill repositories:

````markdown
# Character Skills For [Project]

English | Chinese

## English

This repository contains project-local character skills generated from Ren'Py story text. The story text is expected to come from the output of `renpy-story-extraction-skill`: https://github.com/atmostfair/renpy-story-extraction-skill.git.

These skills model each character's voice, memories, relationship state, decision rules, and story behavior from the highest-affection successful branch.

The repository is safe to rerun after a game update. Reruns should update only characters whose source evidence changed. If extracted story text and route/relationship evidence are unchanged, no character skill should be rewritten.

## Structure

```text
character-name/
  SKILL.md
  agents/openai.yaml
```

## Use

Load a character skill by name and path, for example:

```text
Use $character-name from this repository to write a conversation or scene in Character's voice.
```

## Chinese

[Write a Chinese version of the English description. It must state that this repository contains project-local character skills generated from Ren'Py story text, and that the story text is expected to come from `renpy-story-extraction-skill`: https://github.com/atmostfair/renpy-story-extraction-skill.git.]

[Write a Chinese version explaining that these skills model each character's voice, memories, relationship state, decision rules, and story behavior from the highest-affection successful branch.]

[Write a Chinese sentence explaining that reruns after a game update should update only characters whose source evidence changed, and should not rewrite skills when the extracted story text and route/relationship evidence are unchanged.]

## Structure In Chinese

```text
character-name/
  SKILL.md
  agents/openai.yaml
```

## Use In Chinese

[Write a Chinese usage sentence that tells the user to load a skill by character name and path.]

```text
Use $character-name from this repository to write a conversation or scene in Character's voice.
```
````

11. Validate and audit.
   - Run `quick_validate.py` on every generated character skill and on this workflow skill.
   - Search for template remnants from scaffolded skill files, including placeholder brackets and scaffold instruction headings.
   - Search for branch drift: `Relationship Stage Defaults`, `route stage`, `moves toward`, `pulls away`, `not confirmed`, `low trust`, `trust level`.
   - Search for unwanted output-location assumptions: generated character skills, their metadata, and their default prompts must point to project-local `character_skills/...` paths, not agent skill directories.
   - Check all `.md` and `.yaml` files for non-ASCII if the project convention is ASCII.
   - Verify the generated character-skill repository has a bilingual README with the `renpy-story-extraction-skill` source link.
   - In update mode, verify the diff is source-driven and minimal. No changed source evidence means no character-skill file changes.
   - In update mode, verify unchanged characters remain byte-for-byte unchanged unless there is explicit source evidence requiring an edit.

12. Publish changes to this builder repository.
   - When editing this skill builder itself, work in `.codex/skills/renpy-character-skill-builder`, not in a game project copy.
   - After any successful change to this skill builder, validate, commit, and push to the configured Git remote.
   - Use the current branch and existing `origin`; do not ask for a new remote when `git remote -v` already shows one.
   - A typical successful sequence is:

```text
git status --short
python <quick_validate.py> .
git add SKILL.md README.md agents/openai.yaml
git commit -m "Update RenPy character skill builder"
git push
```

   - If validation fails, fix it before committing.
   - If `git push` fails because authentication, branch protection, or network access is unavailable, report the exact blocker and leave the validated local commit in place.
   - This publish rule applies to this skill builder repository. Generated character skills still belong in the target game/project folder; only push those project outputs when that target project has its own Git remote and the user wants those project files published.

## Quality Bar

A character skill is good enough only when a reader can use it to answer both questions:

- "How would this character talk to me right now in the max-intimacy branch?"
- "If this character must choose in a new scene, what would they do and why?"

Reject skills that only list traits. Each skill needs voice, experiences, active branch, relationship state, decision rules, story use, direct chat rules, and avoid rules.

An update is good enough only when every changed line can be traced to new or changed source evidence. No-op reruns are valid outcomes and are preferable to style churn.

Do not add generic moralizing, installation notes, or safety/disclaimer boilerplate to character skills. If age, consent, or boundaries matter in the source, record them as concrete canon facts and behavior rules, not as external warnings that pollute the character model.

## Common Mistakes

- Building the character list from memory instead of relationship/gallery systems.
- Summarizing plot instead of extracting decision logic.
- Treating raw game scripts as already-audited extraction output without checking whether `renpy-story-extraction-skill` or equivalent extraction rules were used.
- Rewriting existing character skills during a rerun when the extraction output and route/relationship evidence did not change.
- Regenerating every character after a game update when only one character or shared README/provenance changed.
- "Improving" wording, formatting, or style in unchanged character skills; this creates quality drift without evidence.
- Overwriting dirty user edits in an existing character-skill repository.
- Leaving route stages active, which makes the model drift back to early or low-affection behavior.
- Writing a generic "romanceable girl" voice instead of concrete speech habits.
- Keeping failed branch choices as if they are current history.
- Omitting capability limits for specialists such as hackers, royals, fighters, or magic users.
- Writing generated character skills into `.codex/skills` or another agent skill directory just because this workflow skill is installed there.
- Leaving generated character skills as loose files instead of organizing them as a Git repository with a bilingual README.
- Omitting the `renpy-story-extraction-skill` source link from the generated repository README.
- Treating the skill builder repository as a project-local output folder instead of editing `.codex/skills/renpy-character-skill-builder`.
- Forgetting to push validated repository changes after editing this skill builder.

## Final Checklist

- Character scope is justified by source evidence.
- Story text extraction has been audited.
- Existing character-skill repository has been detected and treated as update/no-op mode when present.
- No-op reruns stop without rewriting character skills when extracted story text and route/relationship evidence are unchanged.
- Update-mode diffs are minimal and tied to changed source evidence.
- Every target character has a `SKILL.md` and `agents/openai.yaml`.
- Generated character skills are organized as a Git repository or inside the user's intended output repository.
- Generated character-skill repository has a bilingual README and names `https://github.com/atmostfair/renpy-story-extraction-skill.git` as the expected extraction source.
- Every character skill locks to the highest-affection active branch.
- Prior events are memory, not relationship-stage switches.
- Direct chat uses first person and can step out for analysis.
- Validation passes for every skill.
- Grep audits show no template remnants or old branch-stage wording.
- Skill builder repository changes are committed and pushed to GitHub when a remote is configured.
