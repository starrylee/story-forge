# Instructions for expand-premise action

## Metadata

```json
{
  "guid": "d4e5f6a7-b8c9-4d0e-9f1a-2b3c4d5e6f7a",
  "name": "expand-premise",
  "displayName": "设定解析与创意孵化",
  "description": "将用户的一句话设定解析并扩展为结构化的故事概念方案，输出故事概念卡供用户选择确认",
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

This instruction guides the AI model to 将用户的一句话设定（premise）发展为值得讲述的故事概念。它解析设定中的核心要素、提供多个创意方向供用户选择，并生成结构化的故事概念卡，作为后续所有创作阶段的事实源头。

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

执行本 action 前无需读取上游故事产物（本 action 是流水线起点），但应：

1. **确认故事项目目录**
   - 若 `.asdm/workspace/` 下已有多个故事项目目录，询问用户本次针对哪个故事
   - 若是新故事，与用户确定 story-id 与故事名称（用于创建项目目录）

## Steps to Expand Premise

### 1. 接收一句话设定

接收用户的一句话设定（例如："一个程序员发现自己写的代码，正在改变现实世界"）。

若用户未提供设定：
- 引导用户提供一句话设定，可给出示例帮助理解
- 不要代替用户虚构设定

### 2. 创建故事项目目录

- 路径：`.asdm/workspace/<story-id>-<story-name>/`
- story-id 使用小写连字符短标识；story-name 使用故事名 slug
- 将一句话设定原文保存到 `premise.md`（含设定原文与记录日期）

### 3. 创意解析

从设定中提取：
- 核心人物（是谁）
- 核心事件（发生了什么）
- 背景（在哪里、什么时代）
- 特殊设定（超常规的设定元素）

若设定过于模糊，向用户提出 2-3 个澄清问题后再继续。

### 4. 创意扩展

从不同方向扩展故事可能性，**提供 2-3 个创意方向**，每个方向包含：
- 故事类型（科幻、悬疑、奇幻、爱情、现实主义等，可组合）
- 核心冲突的不同可能形态
- 主题的不同侧重
- 独特卖点（这个故事最吸引人的地方）

### 5. 用户选择方向（人工确认节点）

向用户展示各创意方向，**由用户选择或融合**，不要代替用户决定。用户确认后进入下一步。

### 6. 生成故事概念卡

基于用户选定的方向，生成故事概念卡并保存到 `concept.md`，包含：

```markdown
# 故事概念卡（Story Concept）

- 故事名称（暂定）：
- 故事类型：
- 故事背景：
- 核心人物：
- 核心设定：
- 核心冲突：
- 故事主题：
- 故事卖点：
- 一句话故事概述（Logline）：
```

### 7. 确认与收尾

- 向用户展示概念卡，接受微调意见并修订
- 确认完成后提示用户：可运行 `/asdm-build-world` 进入世界观构建

## Execution Guidelines

### When to Use This Action

Use this action when:
- 用户带着一句话设定（或模糊灵感）开始一个新故事项目
- 用户希望把模糊想法发展为完整的故事概念

### 创意扩展准则

- 扩展方向之间要有实质差异（类型、冲突形态、主题侧重至少一项显著不同）
- Logline 应包含：主角 + 核心冲突 + 风险/代价，一句话讲清"这是一个什么故事"
- 概念卡是后续所有阶段的事实源头，字段必须明确、无歧义

### 人工确认原则

- 创意方向的选择权在用户；AI 提供选项与推荐理由，不替用户拍板
- 概念卡保存前必须经过用户确认

## Usage

To use this instruction, the AI model should:
1. 检测响应语言
2. 接收一句话设定，创建故事项目目录并保存 `premise.md`
3. 解析设定要素，扩展出 2-3 个创意方向
4. 等待用户选择方向
5. 生成并保存 `concept.md`，经用户确认后收尾

## Output Summary

After completing the action, the following artifacts will be generated:
- `.asdm/workspace/<story-id>-<story-name>/premise.md`（一句话设定原文）
- `.asdm/workspace/<story-id>-<story-name>/concept.md`（故事概念卡）

这些产物是 `build-world`、`design-characters`、`design-plotline` 等后续 action 的前置输入。
