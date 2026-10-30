# Instructions for design-plotline action

## Metadata

```json
{
  "guid": "a7b8c9d0-e1f2-4a3b-8c4d-5e6f7a8b9c0d",
  "name": "design-plotline",
  "displayName": "主线设计",
  "description": "基于概念卡、世界观与人物设定设计故事蓝图（Story Blueprint），确定核心冲突、主线支线、故事结构、情节节点与结局",
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

This instruction guides the AI model to 完成从"有一个创意"到"真正有一个故事"的关键转折。它基于概念卡、世界观与人物圣经，确定核心冲突、故事主线与支线、整体结构、关键情节节点与结局，生成故事蓝图供后续大纲拆解。

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

1. **concept.md** (Required)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/concept.md`
   - 用途：核心冲突与主题必须兑现概念卡中的承诺

2. **worldbuilding.md** (Required)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/worldbuilding.md`
   - 用途：情节走向不得违反世界规则

3. **characters.md** (Required)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/characters.md`
   - 用途：主线须承载人物弧光，情节由人物动机驱动

若前置产物缺失，提示用户先完成对应的前序 action。

## Steps to Design Plotline

### 1. 确定核心冲突

确定故事最主要的矛盾：
- 外部冲突（人物 vs 人物 / 环境 / 社会）
- 内部冲突（人物内心矛盾，对应其"外在目标 vs 内在需求"）
- 两层冲突应交织升级

### 2. 设计故事主线与支线

- 主线：主角追求的目标与行动路径（含关键转折点）
- 支线：爱情、友情、成长等支线，须服务或反衬主线与主题

### 3. 选择故事结构

与用户确认结构模型：
- 三幕式（建立 25% / 对抗 50% / 解决 25%）
- 英雄之旅、起承转合等其它模型

三幕式为默认推荐，但不是唯一选择，由用户拍板。

### 4. 设计情节节点

按所选结构设计关键节点：
- 触发事件、第一幕转折、中点反转、重大挫折、最终对决、高潮、结局
- 每个节点写明：发生什么、谁参与、对主线与人物弧的影响

**若概念卡商业模式为「短剧向」**：读取 `spec/hit-formula-spec.md` 并应用其「黄金公式」：
- 每个节点遵循 **冲突 → 反转 → 打脸 → 钩子** 循环，并标注「爽点类型与结算周期」（该节点兑付什么情绪）
- 核心冲突须绑定降维打击合理性（主角为何必赢：马甲/前世记忆/辈分碾压）
- 主线一根筋，无支线干扰；前 3 章给完所有核心人设——确定性本身就是付费动力

### 5. 设计结局

确定结局形态及其情感效果：
- 结局是否解决核心冲突、如何解决
- 人物弧光是否完成
- 主题如何落点、留给读者的余韵

### 6. 设计悬念与期待

设计跨章节的谜团、反转与读者期待，标注"需在 `plan-chapters` 中安排回收的位置"。

### 7. 生成故事蓝图

将以上内容整合保存到 `plotline.md`，按"核心冲突 → 主线/支线 → 结构 → 情节节点 → 结局 → 悬念"分节。

### 8. 用户确认（人工确认节点）

- 向用户展示蓝图全貌，收集修改意见并修订
- 确认完成后提示：可运行 `/asdm-plan-chapters` 进入分章大纲

## Execution Guidelines

### When to Use This Action

Use this action when:
- 概念卡、世界观、人物设定已确认，需要搭建完整故事架构
- 已有架构经用户要求推翻重构

### 架构设计准则

- 情节由人物动机驱动，不设计"人物为了剧情需要而做出违背设定行为"的节点
- 主线必须完整：目标明确、阻力递增、代价可感、结局可信
- 支线克制，宁少勿滥
- 结构模型是工具而非教条；若用户类型（如悬疑多重反转）需要变体，向用户说明并调整

### 一致性检查

生成蓝图后自检：
- 每个情节节点可追溯到某个人物动机
- 无节点违反世界观规则
- 核心冲突在结局得到回应

### 修订同步

若用户修改已确认的蓝图：
- 更新 `plotline.md` 并维护修订记录
- 若已存在分章大纲，提醒用户后续需运行 `/asdm-plan-chapters` 重新拆解受影响部分

## Usage

To use this instruction, the AI model should:
1. 检测响应语言
2. 读取 `concept.md`、`worldbuilding.md`、`characters.md`
3. 与用户确认结构模型
4. 依次完成冲突、主线、节点、结局、悬念设计
5. 保存 `plotline.md`，经用户确认后收尾

## Output Summary

After completing the action, the following artifacts will be generated:
- `.asdm/workspace/<story-id>-<story-name>/plotline.md`（故事蓝图）

该产物是 `plan-chapters` 的直接输入。
