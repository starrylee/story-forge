# ASDM Toolset - Story Forge 故事锻造工具集

toolset-id: story-forge
toolset-name: Story Forge 故事锻造工具集
version: 0.0.1
updated-date: 2026-10-30
toolset-description: 从一句话的设定出发，沿「设定解析 → 世界观/人物/主线 → 分章情节 → 完整故事文本」的流水线逐级生成完整故事。

## Overview

Story Forge 把"从一个灵感火花到一个完整故事"的创作过程沉淀为一组可重复执行的 AI 动作。用户只需提供一句话设定（如"一个失去记忆的灯塔守夜人发现塔顶的灯每晚自己亮起"），工具集即沿「创意孵化 → 世界观构建 → 人物设定 → 主线设计 → 分章情节大纲 → 逐章正文创作」的层级逐级产出创作资产。

工具集面向任何想完成一部完整小说的创作者。它解决的核心问题是：长篇叙事中常见的设定漂移、人物失焦与结构失控。为此，Story Forge 在机制上做了三件事：

1. **逐级确认的流水线**：每一级产物（概念卡、世界观、人物圣经、故事蓝图、分章大纲、章节正文）都保存为独立 Markdown 文件，经用户确认后才进入下一级，且各级都可回改并向下游同步。
2. **故事状态记忆**：每写完一章自动更新故事状态（人物位置、已知信息、时间线、伏笔状态），防止长篇创作中的"人物失忆、物品瞬移、时间冲突"。
3. **事实源头约束**：概念卡与世界规则是后续所有创作的硬约束，任何偏离都须显式向用户说明，避免 AI 擅自改写设定。

## Features

### Common features

- **逐级人工确认**：每个 action 的关键产物在保存前均设人工确认节点（如创意方向选择、世界规则确认、大纲四要素确认、章节审阅），AI 提供选项与建议，不替用户拍板。
- **每级产物可独立验收**：每级产物对应独立文件（premise/concept/worldbuilding/characters/plotline/chapters/story），可对照 spec 单独评审。
- **回改向下游同步**：修改任一级设定（如世界规则、人物设定）时，action 内置"修订同步"准则，提示用户检查并更新受影响的下游产物。
- **产物纯 Markdown**：所有创作资产均为结构化 Markdown 文件，天然可纳入版本管理。

### Feature 1: 设定解析与创意孵化（expand-premise）

将用户的一句话设定解析为核心人物、事件、背景与特殊设定，扩展出 2-3 个差异化创意方向供用户选择，最终生成结构化的故事概念卡（类型、核心冲突、主题、卖点、Logline）。

**Input**: 用户的一句话设定（可选澄清问答）
**Output**: `premise.md`（设定原文）、`concept.md`（故事概念卡）
**Use Case**: 新故事项目的起点；把模糊灵感发展为明确的故事方向

### Feature 2: 世界观构建（build-world）

基于故事概念卡构建世界观圣经（World Bible）：时代背景、世界规则及其代价限制、历史、地理、社会文化、势力关系与特殊体系。

**Input**: `concept.md`、`premise.md`
**Output**: `worldbuilding.md`（世界观圣经，世界规则为后续写作硬约束）
**Use Case**: 科幻、奇幻、玄幻、历史等强设定类型必做；现实主义类型可按范围裁剪；创作中修订世界设定时复用

### Feature 3: 人物设定（design-characters）

基于概念卡与世界观设计人物圣经：主角/配角/反派的档案（欲望、恐惧、缺陷、秘密）、成长弧光与人物关系。

**Input**: `concept.md`、`worldbuilding.md`
**Output**: `characters.md`（人物圣经，含人物关系）
**Use Case**: 世界观确认后进入人物设计；人物行为失去依据时补充/修订设定

### Feature 4: 主线设计（design-plotline）

基于概念卡、世界观与人物设定生成故事蓝图：核心冲突（外部+内部）、主线与支线、故事结构（三幕式/英雄之旅等，用户选择）、情节节点、结局与悬念设计。

**Input**: `concept.md`、`worldbuilding.md`、`characters.md`
**Output**: `plotline.md`（故事蓝图）
**Use Case**: 完成"从一个创意"到"一个完整故事"的架构转折；架构重构时复用

### Feature 5: 分章情节大纲（plan-chapters）

将故事蓝图拆解为「卷 → 章 → 场景」三级大纲：每章强制四要素（目标/冲突/转折/钩子）、场景功能标注、伏笔埋设与回收登记、节奏检查。

**Input**: `plotline.md`、`characters.md`、`worldbuilding.md`；与用户确认篇幅规划
**Output**: `chapters.md`（三级大纲 + 伏笔登记表）
**Use Case**: 正文创作前的最后准备；调整后续章节安排时复用

### Feature 6: 完整故事文本写作（write-story）

按分章大纲逐章创作正文：以场景为单位生成初稿、扩写细节与对话、控制情绪与文风、章节自检与整合；每章完成后更新故事状态记忆（story-state.md），交用户审阅确认后继续。

**Input**: `chapters.md`、`characters.md`、`worldbuilding.md`、`story-state.md`（若存在）；与用户确认本次写作范围
**Output**: `story/chapter-<NN>.md`（逐章正文）、`story-state.md`（故事状态记忆）
**Use Case**: 大纲确认后的正文创作，逐章推进；也可用于补写或重写单章

## Toolset Installation Process

`INSTALL.md` will setup the toolset with the following steps:

创建 `.asdm/workspace/` 下的故事项目目录模板（运行期由 `expand-premise` 按故事动态创建）、部署 6 个 actions 与对应 spec、注册 `/asdm-<action>` 快捷命令、校验安装结果。refer to INSTALL.md for details.

## Toolset Workflow

Once Story Forge 故事锻造工具集 is installed, user can use the following commands:

- `/asdm-expand-premise`: 一句话设定 → 创意方向选择 → 故事概念卡
- `/asdm-build-world`: 概念卡 → 世界观圣经（世界规则硬约束）
- `/asdm-design-characters`: 概念卡+世界观 → 人物圣经与关系
- `/asdm-design-plotline`: 概念卡+世界观+人物 → 故事蓝图（主线/结构/节点/结局）
- `/asdm-plan-chapters`: 故事蓝图 → 分卷/章/场景三级大纲（含伏笔登记）
- `/asdm-write-story`: 分章大纲 → 逐章正文（每章更新故事状态，人工审阅后继续）

典型使用顺序：一句话设定 →（用户确认方向）→ 世界观/人物/主线 →（用户确认）→ 分章情节 →（用户确认）→ 逐章写作 ↔ 人工审阅与修改。每步产出后由用户确认再进入下一步，任何一步都可回改并向下游同步。

## Toolset Structure

The Story Forge 故事锻造工具集 toolset has the following structure:

```
.asdm/
└── toolsets/
    └── story-forge/
        ├── INSTALL.md
        ├── README.md
        ├── manifest.json
        ├── actions/
        │   ├── expand-premise.md
        │   ├── build-world.md
        │   ├── design-characters.md
        │   ├── design-plotline.md
        │   ├── plan-chapters.md
        │   └── write-story.md
        └── spec/
            ├── concept-card-spec.md
            ├── worldbuilding-spec.md
            ├── character-bible-spec.md
            ├── plotline-spec.md
            ├── chapters-outline-spec.md
            ├── story-chapter-spec.md
            └── story-state-spec.md
```

### Spec Documents

The toolset uses the following spec documents as templates:

- **concept-card-spec.md**: 故事概念卡的模板——九字段（类型/冲突/主题/卖点/Logline 等）口径、Logline 三要素与验收清单
- **worldbuilding-spec.md**: 世界观圣经的模板——七大模块（时代/规则/历史/地理/社会/势力/特殊体系）、世界规则硬约束与代价限制写法、修订记录结构
- **character-bible-spec.md**: 人物圣经的模板——欲望-恐惧-缺陷三角、人物弧光、人物关系（含内在张力）与功能性角色从简原则
- **plotline-spec.md**: 故事蓝图的模板——双层冲突交织、主线支线、结构模型、情节节点（可追溯人物动机）、结局与悬念回收标注
- **chapters-outline-spec.md**: 分章大纲的模板——卷/章/场景三级结构、每章强制四要素（目标/冲突/转折/钩子）、伏笔登记表与节奏检查
- **story-chapter-spec.md**: 章节正文的模板——场景推进写作准则、人物声音与文风一致性、四要素兑现自检、`chapter-<NN>.md` 命名约定
- **story-state-spec.md**: 故事状态记忆的模板——人物位置、信息差（谁知道什么）、时间线、伏笔状态与 `chapters.md` 对账机制

## Toolset Workspace

The Story Forge 故事锻造工具集 toolset has the following workspace structure:

> 路径约定：feature 相关产物第一级路径为 `.asdm/workspace/<feature-id>-<feature-name>/`（特性目录，其下再按用途分子目录）；`.asdm/workspace/<workspace-type>/` 仅用于跨 feature 的项目级产物。

Story Forge 以**单个故事**为项目粒度，所有产物集中在该故事的项目目录中：

```
.asdm/workspace/<story-id>-<story-name>/
├── premise.md          # 一句话设定（源头输入）
├── concept.md          # 故事概念卡（类型/冲突/主题/卖点/Logline）
├── worldbuilding.md    # 世界观设定（世界圣经，硬约束）
├── characters.md       # 人物设定（人物圣经，含关系）
├── plotline.md         # 主线设计（故事蓝图）
├── chapters.md         # 分卷/章/场景三级大纲（含伏笔登记）
├── story-state.md      # 故事状态记忆（逐章更新）
└── story/              # 完整故事文本（长篇按章分文件）
    ├── chapter-01.md
    ├── chapter-02.md
    └── ...
```

## Copyright & License

Copyright (c) 2026 LeansoftX.com & iSoftStone. All rights reserved.

Licensed under the PROPRIETARY SOFTWARE LICENSE. See [LICENSE](LICENSE) in the project root for license information.
