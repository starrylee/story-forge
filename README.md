# ASDM Toolset - Story Forge 故事锻造工具集

toolset-id: story-forge
toolset-name: Story Forge 故事锻造工具集
version: 0.0.1
updated-date: 2026-10-30
toolset-description: 从一句话的设定出发，沿「设定解析 → 世界观/人物/主线 → 分章情节 → 完整故事文本」的流水线逐级生成完整故事。

## Overview

[Developer completes this section - 2-3 paragraphs describing what the toolset does, who it's for, and what problem it solves]

建议围绕以下要点撰写：Story Forge 把"从一个灵感火花到一个完整故事"的创作过程沉淀为一组可重复执行的 AI 动作；用户只需提供一句话设定（如"一个失去记忆的灯塔守夜人发现塔顶的灯每晚自己亮起"），工具集即沿世界观/人物/主线 → 分章情节 → 完整故事文本的层级逐级产出；每一级产物经用户确认后才进入下一级，避免长篇叙事中常见的设定漂移、人物失焦与结构失控。

## Features

### Common features

[Developer completes this section - 通用特性，如：逐级确认的流水线机制、每级产物可独立验收（对应 spec）、支持回改并向下游同步、产物纯 Markdown 可版本管理等]

### Feature 1: <Feature Name>
[Developer completes this section - 建议按流水线层级组织 Feature，例如：设定解析、世界观构建、人物设定、主线设计、分章情节大纲、完整故事文本写作等；每个 Feature 附输入/输出/使用场景]

### Feature 2: <Feature Name>
[Developer completes this section]

[Developer adds more features as needed]

## Toolset Installation Process

`INSTALL.md` will setup the toolset with the following steps:

[Developer briefly describes what installation will do - 建议包含：创建 `.asdm/workspace/` 下的故事项目目录模板、检测 AI provider、部署 actions 与 spec、注册快捷命令、校验安装结果。refer to INSTALL.md for details]

## Toolset Workflow

Once Story Forge 故事锻造工具集 is installed, user can use the following commands:

[Developer lists available commands/workflows - 建议形如：]

- `/asdm-<action-one>`: Description
- `/asdm-<action-two>`: Description
- `/asdm-<action-three>`: Description

[并说明典型使用顺序：一句话设定 → 世界观/人物/主线 → 分章情节 → 完整故事文本，每步产出后由用户确认再进入下一步]

## Toolset Structure

The Story Forge 故事锻造工具集 toolset has the following structure:

```
.asdm/
└── toolsets/
    └── story-forge/
        ├── INSTALL.md
        ├── README.md
        ├── actions/
        │   ├── <action-name>.md
        │   └── ...
        └── spec/
            ├── <spec-name>.md
            └── ...
```

## Toolset Workspace

The Story Forge 故事锻造工具集 toolset has the following workspace structure:

> 路径约定：feature 相关产物第一级路径为 `.asdm/workspace/<feature-id>-<feature-name>/`（特性目录，其下再按用途分子目录）；`.asdm/workspace/<workspace-type>/` 仅用于跨 feature 的项目级产物。

```
.asdm/workspace/<workspace-type>/
├── <files>
└── <directories>
```

[Developer completes this section based on the toolset's needs - 建议按单个故事的粒度设计，形如：]

```
.asdm/workspace/<story-id>-<story-name>/
├── premise.md          # 一句话设定（源头输入）
├── worldbuilding.md    # 世界观设定
├── characters.md       # 人物设定
├── plotline.md         # 主线设计
├── chapters.md         # 分章情节大纲
└── story/              # 完整故事文本（长篇可按章分文件）
```

## Copyright & License

Copyright (c) 2026 LeansoftX.com & iSoftStone. All rights reserved.

Licensed under the PROPRIETARY SOFTWARE LICENSE. See [LICENSE](LICENSE) in the project root for license information.
