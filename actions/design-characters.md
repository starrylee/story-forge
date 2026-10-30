# Instructions for design-characters action

## Metadata

```json
{
  "guid": "f6a7b8c9-d0e1-4f2a-9b3c-4d5e6f7a8b9c",
  "name": "design-characters",
  "displayName": "人物设定",
  "description": "基于故事概念卡与世界观设计人物圣经（Character Bible），包含人物档案、动机、关系与成长弧",
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

This instruction guides the AI model to 设计小说的灵魂——人物。它基于概念卡与世界观生成人物圣经，为每个核心人物建立欲望、恐惧、缺陷与成长弧，并梳理人物关系，避免人物沦为"只有名字和外貌"的空壳。

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

1. **premise.md / concept.md** (Required)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/`
   - 用途：确定核心人物名单与人物设计方向

2. **worldbuilding.md** (Required)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/worldbuilding.md`
   - 用途：人物的身世、能力、社会归属必须符合世界观规则

若前置产物缺失，提示用户先完成对应的前序 action。

## Steps to Design Characters

### 1. 角色识别

基于概念卡列出角色清单并分类：
- 主角（1-2 名）
- 核心配角
- 反派
- 功能性角色（导师、盟友、信息源等）

角色清单先向用户确认增删。

### 2. 建立人物档案

为每个核心人物建立档案：
- 姓名、年龄、外貌、身份、职业、性格
- **外在目标**：人物想要什么
- **内在需求**：人物真正需要什么（常与外在目标矛盾）
- **最大恐惧**、**性格缺陷**、**秘密**
- 成长经历与关键创伤/记忆
- 语言习惯与表达方式（口头禅、语气特征）

### 3. 设计人物弧光

为主要人物设计在故事中的变化轨迹（例如：从孤立自我 → 学会信任合作）。弧光应与概念卡的核心主题呼应。

### 4. 梳理人物关系

生成人物关系描述：亲情、友情、爱情、敌对、利益关系，以及关系中的内在张力。以列表或 Mermaid 关系图呈现。

### 5. 生成人物圣经

将以上内容整合保存到 `characters.md`，按人物分节，每个人物一个小节。

### 6. 用户确认（人工确认节点）

- 向用户展示人物要点，收集修改意见并修订
- 确认完成后提示：可运行 `/asdm-design-plotline`（若世界观/主线尚未完成则先补齐）

## Execution Guidelines

### When to Use This Action

Use this action when:
- 概念卡与世界观已确认，进入人物设计阶段
- 创作中途发现人物行为失去依据，需要补充/修订人物设定

### 人物设计准则

- 每个核心人物必须有"欲望-恐惧-缺陷"三角，驱动其一切行为
- 反派同样需要合理的动机与立场，不能只是"坏"
- 人物设定不得违反世界观规则（能力体系、社会结构等）
- 功能性角色档案可从简，避免设定膨胀

### 一致性与修订同步

若用户修改已确认的人物设定：
- 更新 `characters.md` 并维护修订记录
- 提醒用户检查主线与分章大纲中该人物的情节是否受影响

## Usage

To use this instruction, the AI model should:
1. 检测响应语言
2. 读取 `concept.md` 与 `worldbuilding.md`
3. 与用户确认角色清单
4. 生成人物档案、弧光与关系
5. 保存 `characters.md`，经用户确认后收尾

## Output Summary

After completing the action, the following artifacts will be generated:
- `.asdm/workspace/<story-id>-<story-name>/characters.md`（人物圣经，含人物关系）

该产物是 `design-plotline`、`plan-chapters`、`write-story` 的事实依据。
