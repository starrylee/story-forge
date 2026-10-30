# Instructions for plan-chapters action

## Metadata

```json
{
  "guid": "b8c9d0e1-f2a3-4b4c-9d5e-6f7a8b9c0d1e",
  "name": "plan-chapters",
  "displayName": "分章情节大纲",
  "description": "将故事蓝图拆解为分卷、章节与场景三级大纲，为每章设定叙事目标、冲突、转折与钩子，并规划伏笔埋设与回收",
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

This instruction guides the AI model to 把宏观故事拆解到可实际写作的颗粒度。它将故事蓝图转化为「卷 → 章 → 场景」三级大纲，为每章明确叙事目标、冲突、转折与钩子，并登记伏笔的埋设与回收位置，作为正文创作的直接依据。

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

1. **plotline.md** (Required)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/plotline.md`
   - 用途：章节拆解必须完整覆盖蓝图的情节节点

2. **characters.md** (Required)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/characters.md`
   - 用途：各章出场人物及其行为需符合设定

3. **worldbuilding.md** (Required)
   - 路径：`.asdm/workspace/<story-id>-<story-name>/worldbuilding.md`
   - 用途：场景设计需符合世界规则与地理设定

4. **concept.md** (Recommended)
   - 用途：把握类型与卖点，控制大纲的题材表达

若 `plotline.md` 缺失，提示用户先运行 `/asdm-design-plotline`。

## Steps to Plan Chapters

### 1. 篇幅规划

与用户确认：
- 总字数目标（或篇幅档位：短篇 / 中篇 / 长篇）
- 卷数与章节数量级
- 单章目标字数

### 2. 分卷设计

将故事按蓝图结构划分为卷（短篇可只有一卷），明确每卷的：
- 卷名与卷目标（本卷结束时故事推进到哪里）
- 本卷核心冲突与结局

### 3. 章节拆分与章节大纲

将每卷拆分为具体章节。每章必须包含以下四要素（缺一不可）：

- **本章目标**：本章要让读者/故事获得什么（如"让主角发现代码与现实之间存在联系"）
- **本章冲突**：矛盾如何在本章展开
- **本章转折**：本章结尾处情节/信息如何变化
- **本章钩子**：吸引读者读下一章的抓手

同时写明：出场人物、本章关键信息（读者与人物各获得什么信息）、预计字数。

### 4. 场景拆分

将每章拆分为 2-5 个场景，每个场景一句话说明：地点、参与人物、场景功能（推进/揭示/铺垫/情绪）。

### 5. 伏笔登记

汇总蓝图中的悬念设计，在大纲中登记：
- 伏笔内容、埋设章节、预计回收章节、回收方式

### 6. 节奏检查

通读大纲检查节奏：
- 冲突强度是否持续递进、有无多章平铺直叙
- 信息密度是否失衡（连_dumping 多章设定 / 连续多章无新信息）
- 高潮前的低谷（重大挫折）位置是否合理

发现问题则调整章节安排，并向用户说明改动。

### 7. 生成分章大纲

保存到 `chapters.md`，采用层级结构：

```markdown
# 小说大纲

## 第一卷：觉醒
### 第 1 章：异常代码
- 本章目标：
- 本章冲突：
- 本章转折：
- 本章钩子：
- 出场人物：
- 场景：
  1. 场景 1：深夜办公室——（一句话说明）
  2. 场景 2：神秘代码出现——（一句话说明）
- 预计字数：
```

### 8. 用户确认（人工确认节点）

- 向用户展示大纲（至少逐章列出四要素），收集修改意见并修订
- 确认完成后提示：可运行 `/asdm-write-story` 逐章创作正文

## Execution Guidelines

### When to Use This Action

Use this action when:
- 故事蓝图已确认，需要拆解为可写作的章节大纲
- 正文创作中途需要调整后续章节安排

### 大纲设计准则

- 每章必须有明确的叙事目标，而不是仅罗列"发生了什么"
- 章与章之间靠转折与钩子咬合，不靠流水账堆叠
- 伏笔必须登记回收位置；"只埋不收"的伏笔在检查时标记出来与用户决策（删除或安排回收）
- 大纲颗粒度到"场景一句话"，不过度展开细节，为正文创作保留空间

### 修订同步

若用户修改已确认的大纲：
- 更新 `chapters.md` 并维护修订记录
- 若某些章节正文已写就，提醒用户哪些已写章节可能受影响

## Usage

To use this instruction, the AI model should:
1. 检测响应语言
2. 读取 `plotline.md`、`characters.md`、`worldbuilding.md`
3. 与用户确认篇幅规划
4. 依次完成分卷、章节、场景拆分与伏笔登记
5. 节奏检查后保存 `chapters.md`，经用户确认后收尾

## Output Summary

After completing the action, the following artifacts will be generated:
- `.asdm/workspace/<story-id>-<story-name>/chapters.md`（分卷/章/场景三级大纲，含伏笔登记）

该产物是 `write-story` 的直接输入。
