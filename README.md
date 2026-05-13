# RenPy Character Skill Builder

English | 中文

## English

`renpy-character-skill-builder` is a Codex skill for turning Ren'Py visual novel story text into project-local character skills. It is designed to work on the output of `renpy-story-extraction-skill`: https://github.com/atmostfair/renpy-story-extraction-skill.git.

The builder focuses on romanceable or relationship-driven female characters. It reads route variables, relationship systems, gallery/replay metadata, and extracted dialogue, then writes reusable character skills that preserve voice, memory, relationship state, decision logic, and direct-chat behavior.

## What It Creates

- One `SKILL.md` and `agents/openai.yaml` per target character.
- A project-local character-skill repository, usually under `character_skills/`.
- A bilingual README for the generated character-skill repository.
- Git organization for generated character skills, with commits when Git is available.
- Character models locked to the highest-affection / most intimate successful branch.
- Safe reruns for existing character-skill repositories after game updates.

Generated character skills are project artifacts. They should be written to the analyzed game/project folder, not to `.codex/skills`.

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

`renpy-character-skill-builder` 是一个 Codex skill，用于把 Ren'Py 视觉小说剧情文本转化为项目本地的人物角色 skill。它设计为工作在 `renpy-story-extraction-skill` 的提取结果之上：https://github.com/atmostfair/renpy-story-extraction-skill.git。

这个 builder 重点处理可攻略或关系驱动的女角色。它会读取路线变量、关系系统、gallery/replay 元数据和已提取对白，然后生成可复用的人物 skill，保留角色语气、记忆、关系状态、选择逻辑和直接聊天感觉。

## 生成内容

- 每个目标角色一个 `SKILL.md` 和 `agents/openai.yaml`。
- 一个项目本地的人物 skill 仓库，通常位于 `character_skills/`。
- 为生成的人物 skill 仓库创建中英文 README。
- 将生成的人物 skill 自动组织为 Git 仓库，并在 Git 可用时提交。
- 角色模型锁定到最高好感 / 最亲密 / 成功分支。
- 支持在游戏更新后对已有的人物 skill 仓库安全重复运行。

生成的人物 skill 是项目产物，应写入被分析的游戏/项目目录，而不是 `.codex/skills`。

## 重复运行与更新

当游戏更新后，可以在已有的 `character_skills/` 仓库上再次运行 builder。它应比较新的剧情提取文本、路线证据和关系证据，只更新源证据发生变化的角色，并保持未变化角色的 skill 不动。如果没有相关源证据变化，正确结果是报告无需更新，而不是重写文件。

## 安装

把本仓库 clone 到 Codex 的 skills 目录，并使用 skill 文件夹名：

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
