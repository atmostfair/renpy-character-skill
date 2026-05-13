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

Generated character skills are project artifacts. They should be written to the analyzed game/project folder, not to `.codex/skills`.

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

生成的人物 skill 是项目产物，应写入被分析的游戏/项目目录，而不是 `.codex/skills`。

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
