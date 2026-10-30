# Instructions for write-story action

## Metadata

```json
{
  "guid": "c9d0e1f2-a3b4-4c5d-8e6f-7a8b9c0d1e2f",
  "name": "write-story",
  "displayName": "完整故事文本写作",
  "description": "基于分章大纲逐章创作小说正文，以场景为单位生成初稿并整合为完整章节，写作前后维护故事状态记忆",
  "toolset": {
    "guid": "c3d4e5f6-a7b8-4c9d-8e0f-1a2b3c4d5e6f",
    "id": "story-forge",
    "name": "Story Forge 故事锻造工具集",
    "version": "0.0.1"
  },
  "scenario": "故事创作"
}
```

## Purpose

This instruction guides the AI model to 以章节为单位创作小说正文。它按分章大纲加载上下文、以场景为单位生成初稿、扩写细节与对话、整合为完整章节，并在每章完成后更新故事状态记忆，防止长篇创作中出现人物失忆、物品瞬移、时间冲突等问题。

## Language Setting

默认使用**中文（简体中文）**作为输出语言。所有生成的内容均使用中文，并遵循中文写作规范。

如需切换语言，可在工作区根目录创建 `.asdm/config.json` 配置：
```json
{
  "language": "zh"
}
```
支持的语言：`zh`（中文，默认）、`en`（英文）。

## Context Injection

执行本 action 前，AI model 应读取故事项目的前置产物：

1. **chapters.md** (Required)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/chapters.md`
   - 用途：本章的目标、冲突、转折、钩子与场景安排

2. **characters.md** (Required)
   - 用途：人物性格、语言习惯决定对话与行为

3. **worldbuilding.md** (Required)
   - 用途：场景细节与设定描述不得违反世界规则

4. **concept.md / plotline.md** (Recommended)
   - 用途：把握类型基调、文风方向与情节走向

5. **story-state.md** (Required - 若存在)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/story-state.md`
   - 用途：上一章遗留的人物状态、时间线、伏笔状态

6. **已写就的前文** (Recommended)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/story/`
   - 用途：对齐文风与叙事口吻；至少重读上一章结尾

若 `chapters.md` 缺失，提示用户先运行 `/asdm-plan-chapters`。

## Steps to Write Story

### 1. 确定写作范围

与用户确认本次要写的章节（默认下一章，也可指定多章）。**不要一次性写作全部章节**——每章完成后应经人工审阅再继续。

### 2. 写作准备

加载写作上下文：
- 本章大纲四要素（目标/冲突/转折/钩子）与场景安排
- 本章出场人物档案与关系
- 相关世界规则
- `story-state.md` 中的当前状态（人物位置、掌握信息、时间线、待回收伏笔）

### 3. 场景生成

按大纲逐场景生成初稿：
- 遵循场景的地点、人物与功能设计
- 每个场景达成其功能即止，不注水

### 4. 细节扩写

补充环境描写、动作细节与心理活动，注意：
- 描写须符合世界观与时代氛围
- 避免连续大段静态描写打断叙事节奏

### 5. 对话创作

生成符合人物设定的对话：
- 每个人物的语气、用词、口头禅符合 `characters.md` 中的人物声音
- 对话承载信息或张力，删除不推进任何东西的寒暄

### 6. 情绪与文风控制

- 按本章在整体节奏中的位置营造情绪（铺垫章克制、高潮章浓烈）
- 叙事视角、语言风格与前文保持一致

### 7. 章节整合与自检

将场景整合为完整章节，自检：
- 本章目标是否达成
- 结尾钩子是否有效
- 有无违反世界规则、人物设定、时间线
- 字数是否在目标范围

### 8. 保存章节

保存到 `.asdm/workspace/<story-id>-<story-name>/story/chapter-<NN>.md`（`<NN>` 为两位章号，如 `chapter-01.md`），文件头部保留章名。

### 9. 更新故事状态记忆

每写完一章，更新 `story-state.md`：
- 人物当前状态及位置
- 已发生的重要事件
- 人物掌握的信息（谁知道什么——信息差是悬念的来源）
- 未解决的冲突
- 已埋下/已回收的伏笔
- 当前时间线

### 10. 人工审阅节点

将章节呈现给用户审阅：
- 用户提出修改 → 修订后更新文件与 `story-state.md`
- 用户确认 → 提示继续写下一章（重复步骤 1-10）

## Execution Guidelines

### When to Use This Action

Use this action when:
- 分章大纲已确认，进入正文创作
- 补写或重写某一章

### 写作准则

- **以场景为单位推进，以章节为单位交付**：先保证每个场景功能达成，再谈文笔
- 大纲是契约：正文须兑现本章四要素；若写作中发现大纲不合理，停下来向用户说明并提议修改大纲，而非擅自偏离
- 每章完成后必须更新 `story-state.md`——这是防止长篇失忆的核心机制
- 伏笔按 `chapters.md` 登记的位置埋设与回收

**短剧向节拍约束**（仅当概念卡商业模式为「短剧向」时生效，见 `spec/hit-formula-spec.md`）——只加节拍约束，不干预文风本身：
- 动作戏优先：不写静态心理独白，用动作与对话承载情绪
- 嘴替台词：主角说观众不敢说的话——一针见血、当场掀桌、不内耗
- 每章至少一次情绪兑现（打脸成功/信息差变现/代偿时刻）
- 短句节奏、高换行、强对白（网文阅读习惯），但不牺牲基本文学质量

### 错误处理

- 若上下文过大（超长篇），优先读取：本章大纲 + 本章出场人物 + story-state + 上一章结尾，其余按需加载
- 若用户要求偏离大纲写作，按要求执行但在章节末尾提示可能需要的后续大纲调整

## Usage

To use this instruction, the AI model should:
1. 检测响应语言
2. 与用户确认本次写作的章节范围
3. 加载大纲、人物、世界观与故事状态上下文
4. 按场景生成初稿，扩写细节与对话，控制文风
5. 整合自检后保存章节文件
6. 更新 `story-state.md`
7. 交用户审阅，确认后继续下一章

## Output Summary

After completing the action, the following artifacts will be generated:
- `.asdm/workspace/<story-id>-<story-name>/story/chapter-<NN>.md`（章节正文，每次一章或多章）
- `.asdm/workspace/<story-id>-<story-name>/story-state.md`（故事状态记忆，逐章更新）

全部章节完成后，即得到完整故事文本；后续可扩展审校、定稿与导出能力。
