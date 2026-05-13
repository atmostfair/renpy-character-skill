# RenPy Character Skill Builder

English | 中文

## English

`renpy-character-skill-builder` is a Codex skill for turning Ren'Py visual novel story text into project-local character skills. It is designed to work from story text extracted by `renpy-story-extraction-skill`: https://github.com/atmostfair/renpy-story-extraction-skill.git.

The builder focuses on romanceable or relationship-driven female characters. It reads route variables, relationship systems, gallery/replay metadata, PAX/phone panels, extracted dialogue, and source branch flags, then writes reusable character skills that preserve voice, memory, relationship state, final-branch decision logic, and direct-chat behavior.

Generated character skills are project artifacts. They should be written to the analyzed game/project folder, usually under `character_skills/`, not to `.codex/skills`.

## What It Creates

- One rich `SKILL.md` and `agents/openai.yaml` per target character.
- A project-local character-skill repository, usually under `character_skills/`.
- A bilingual README for the generated character-skill repository.
- Git organization for generated character skills, with commits when Git is available.
- Character models locked to the highest-affection / most intimate successful branch.
- Safe reruns for existing character-skill repositories after game updates.

## Quality Policy

Character skills should not be overcompressed. Because a user usually loads one character skill for one writing or chat task, each skill should include enough source-grounded material for the model to reproduce the character without rereading the full story extraction.

Good character skills include expanded canon memory, dialogue material, max-state detail, relationship state, decision rules, story use, direct chat rules, and avoid rules. They summarize source evidence and speech patterns without copying long canon passages.

## Reruns And Updates

The builder can be run again on an existing `character_skills/` repository when a game updates. It should compare the new extracted story text and route/relationship evidence against the previous generated state, update only characters whose source evidence changed, and leave unchanged character skills untouched. If no relevant source evidence changed, the correct result is a no-op report instead of rewriting files.

## Install

Clone this repository into your Codex skills directory using the skill folder name:

```powershell
git clone https://github.com/atmostfair/renpy-character-skill.git "$env:USERPROFILE\.codex\skills\renpy-character-skill-builder"
```

On Unix-like systems:

```bash
git clone https://github.com/atmostfair/renpy-character-skill.git ~/.codex/skills/renpy-character-skill-builder
```

Restart or reload Codex so the skill list can be refreshed.

## Use

First extract story text with:

```text
https://github.com/atmostfair/renpy-story-extraction-skill.git
```

Then, from the extracted story-text project or game folder, ask Codex:

```text
Use $renpy-character-skill-builder to create project-local character skills from this RenPy visual novel.
```

Expected generated layout:

```text
character_skills/
  README.md
  character-name/
    SKILL.md
    agents/openai.yaml
```

## 中文

`renpy-character-skill-builder` 是一个 Codex skill，用于把 Ren'Py 视觉小说剧情文本转化为项目本地的人物角色 skill。它设计为工作在 `renpy-story-extraction-skill` 的剧情提取结果之上：https://github.com/atmostfair/renpy-story-extraction-skill.git。

这个 builder 重点处理可攻略或关系驱动的女性角色。它会读取路线变量、关系系统、gallery/replay 元数据、PAX/手机关系面板、已提取对白和源码分支标志，然后生成可复用的人物 skill，用来保留角色语气、记忆、关系状态、最终分支选择逻辑和直接聊天感。

生成的人物 skill 是项目产物，应写入被分析的游戏/项目目录，通常是 `character_skills/`，不要写入 `.codex/skills`。

## 生成内容

- 每个目标角色一个内容充实的 `SKILL.md` 和 `agents/openai.yaml`。
- 一个项目本地的人物 skill 仓库，通常位于 `character_skills/`。
- 为生成的人物 skill 仓库创建中英文 README。
- 在 Git 可用时，将生成结果组织为 Git 仓库并提交。
- 将角色模型锁定到最高好感 / 最亲密 / 成功分支。
- 支持游戏更新后对已有角色 skill 仓库重复运行。

## 质量原则

人物 skill 不应该过度压缩。因为用户通常一次任务只加载一个角色 skill，所以每个 skill 应包含足够的原文证据、经历记忆、说话素材和最终状态细节，使模型不必重新阅读完整剧情提取文本，也能复现角色的选择和聊天感觉。

好的角色 skill 应包括 expanded canon memory、dialogue material、max-state detail、relationship state、decision rules、story use、direct chat rules 和 avoid rules。它应总结来源证据和说话模式，不应复制长段原文。

## 重复运行与更新

游戏更新后，可以在已有的 `character_skills/` 仓库上再次运行 builder。builder 应比较新的剧情提取文本、路线证据和关系证据，只更新源证据发生变化的角色，并保持未变化角色的 skill 不动。如果没有相关源证据变化，正确结果是报告无需更新，而不是重写文件。

## 安装

将本仓库 clone 到 Codex skills 目录，并使用固定 skill 文件夹名：

```powershell
git clone https://github.com/atmostfair/renpy-character-skill.git "$env:USERPROFILE\.codex\skills\renpy-character-skill-builder"
```

Unix-like 系统：

```bash
git clone https://github.com/atmostfair/renpy-character-skill.git ~/.codex/skills/renpy-character-skill-builder
```

然后重启或刷新 Codex，使 skill 列表重新加载。

## 使用

先使用以下 skill 提取剧情文本：

```text
https://github.com/atmostfair/renpy-story-extraction-skill.git
```

然后在已提取剧情文本的项目或游戏目录中，对 Codex 说：

```text
Use $renpy-character-skill-builder to create project-local character skills from this RenPy visual novel.
```

预期生成结构：

```text
character_skills/
  README.md
  character-name/
    SKILL.md
    agents/openai.yaml
```
