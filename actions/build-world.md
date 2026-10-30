# Instructions for build-world action

## Metadata

```json
{
  "guid": "e5f6a7b8-c9d0-4e1f-8a2b-3c4d5e6f7a8b",
  "name": "build-world",
  "displayName": "世界观构建",
  "description": "基于故事概念卡构建世界观设定集（World Bible），确立世界规则、历史、地理、社会与势力体系",
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

This instruction guides the AI model to 基于已确认的故事概念卡，构建世界观圣经（World Bible）。它确立世界规则、历史、地理、社会文化与势力关系，作为后续写作中不可违背的事实依据，避免长篇创作中出现世界规则自相矛盾。

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

1. **premise.md** (Required)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/premise.md`
   - 用途：理解一句话设定源头，防止世界观偏离原始创意

2. **concept.md** (Required)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/concept.md`
   - 用途：世界观的类型、背景、核心设定必须与概念卡保持一致

若任一文件不存在，提示用户先运行 `/asdm-expand-premise`。

## Steps to Build World

### 1. 定位故事项目

若工作区存在多个故事项目，先与用户确认目标项目；校验 `premise.md` 与 `concept.md` 存在。

### 2. 确定世界观构建范围

根据故事类型决定各模块的详略：
- 科幻/奇幻/玄幻/历史类：完整构建（规则、历史、地理、社会、势力、特殊体系）
- 现实主义/爱情类：以现实世界为基线，仅补充故事涉及的特殊背景设定

范围需向用户简要说明并确认。

**若概念卡商业模式为「短剧向」**：读取 `spec/hit-formula-spec.md` 并应用其「封闭小世界原则」——世界观半径收窄到一个熟人社会权力结构（家族/公司/村庄/宅院），优先设计封闭空间内的人物阵营与权力关系，不为剧情之外的宏大设定着墨。

### 3. 构建世界设定

按以下模块生成世界观内容：

#### 3.1 时代与背景
故事发生的时间、地点、时代氛围。

#### 3.2 世界规则
物理规则、魔法/科技/超自然规则及其**代价与限制**（规则一旦确立，后续写作必须遵守）。

#### 3.3 历史设定
世界历史脉络与影响当前故事的关键历史事件。

#### 3.4 地理环境
故事涉及的国家、城市、区域和重要地点（仅详细展开剧情会到达的地点）。

#### 3.5 社会文化
社会制度、文化、宗教、经济等与剧情相关的设定。

#### 3.6 势力组织
国家、组织、家族、阵营及其相互关系。

#### 3.7 特殊体系
能力等级、科技体系、魔法体系等（仅适用于有体系设定的类型）。

### 4. 生成世界观圣经

将设定整合保存到 `worldbuilding.md`，结构化分节，每条世界规则以明确的陈述句列出。

### 5. 用户确认（人工确认节点）

- 向用户展示世界观要点，收集修改意见并修订
- 特别提醒用户：世界规则一经确认即为后续写作的硬约束
- 确认完成后提示：可运行 `/asdm-design-characters` 或 `/asdm-design-plotline`

## Execution Guidelines

### When to Use This Action

Use this action when:
- 故事概念卡已确认，进入世界观构建阶段
- 后续创作中发现世界观缺失或矛盾，需要补充/修订设定

### 一致性准则

- 世界观不得与概念卡的核心设定冲突；若创作上确需偏离，须向用户显式说明并更新概念卡
- 规则要写"限制与代价"，不只是"能做什么"——无代价的规则是情节漏洞的温床
- 详略取舍：为剧情服务的设定详细展开，与剧情无关的设定点到为止

### 修订同步

若用户要求修改已确认的世界规则：
- 更新 `worldbuilding.md` 并在文件头部维护修订记录
- 提醒用户检查下游产物（人物、主线、分章大纲）是否受影响

## Usage

To use this instruction, the AI model should:
1. 检测响应语言
2. 读取 `premise.md` 与 `concept.md`
3. 与用户确认构建范围
4. 按模块生成世界观设定
5. 保存 `worldbuilding.md`，经用户确认后收尾

## Output Summary

After completing the action, the following artifacts will be generated:
- `.asdm/workspace/<story-id>-<story-name>/worldbuilding.md`（世界观圣经）

该产物是 `design-characters`、`design-plotline`、`plan-chapters`、`write-story` 的事实依据。
