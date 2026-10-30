# Story Forge 故事锻造工具集 Installation

**Toolset ID:** `story-forge`

## Overview

本文档提供 Story Forge 故事锻造工具集的安装与设置指引。Story Forge 从一句话的设定出发，沿「设定解析 → 世界观/人物/主线 → 分章情节 → 完整故事文本」的流水线逐级生成完整故事，解决长篇创作中的设定漂移、人物失焦与结构失控问题。

## AI Guided Installation

若要使用 AI 引导安装，将以下提示词复制粘贴到你的 AI Coding 工具对话窗口：

```shell
Follow instructions in .asdm/toolsets/story-forge/INSTALL.md
```

## Installation Steps

### 1. Create workspace directories

创建工作区基础目录（具体的故事项目目录由 `expand-premise` 在运行期按故事动态创建）：

```bash
mkdir -p .asdm/workspace
```

### 2. Detect the current `Agentic Engine` provider

检测当前 AI coding assistant 提供商（如 Claude Code、GitHub Copilot、Tencent CodeBuddy）。按以下规则检测：

- 若存在 `.claude` 目录，使用 `Claude Code`
- 若存在 `.github` 目录，使用 `GitHub Copilot`
- 若存在 `.codebuddy` 目录，使用 `Tencent CodeBuddy`
- 若当前工作区未发现上述任一目录，提示用户手动选择提供商

### 3. Create shortcuts commands for Story Forge 故事锻造工具集 (toolset ID: `story-forge`) in provider's entry point

根据检测到的提供商，在对应位置创建快捷命令。安装过程在各提供商间保持一致——使用 `cat` 将提供商专属 frontmatter 与指令内容拼接：

#### For Claude Code (`.claude/commands/`):

Claude Code 使用带 Frontmatter 元数据的 Markdown 文件作为 slash command。通过拼接 Claude 专属 frontmatter 与指令内容创建命令：

```bash
mkdir -p .claude/commands/

# expand-premise command
cat > .claude/commands/asdm-expand-premise.md << 'EOF'
---
description: "设定解析与创意孵化：一句话设定 → 创意方向选择 → 故事概念卡"
argument-hint: "[一句话设定]"
---

EOF
cat .asdm/toolsets/story-forge/actions/expand-premise.md >> .claude/commands/asdm-expand-premise.md

# build-world command
cat > .claude/commands/asdm-build-world.md << 'EOF'
---
description: "世界观构建：概念卡 → 世界观圣经（世界规则硬约束）"
argument-hint: "[故事项目]"
---

EOF
cat .asdm/toolsets/story-forge/actions/build-world.md >> .claude/commands/asdm-build-world.md

# design-characters command
cat > .claude/commands/asdm-design-characters.md << 'EOF'
---
description: "人物设定：概念卡+世界观 → 人物圣经与关系"
argument-hint: "[故事项目]"
---

EOF
cat .asdm/toolsets/story-forge/actions/design-characters.md >> .claude/commands/asdm-design-characters.md

# design-plotline command
cat > .claude/commands/asdm-design-plotline.md << 'EOF'
---
description: "主线设计：概念卡+世界观+人物 → 故事蓝图（主线/结构/节点/结局）"
argument-hint: "[故事项目]"
---

EOF
cat .asdm/toolsets/story-forge/actions/design-plotline.md >> .claude/commands/asdm-design-plotline.md

# plan-chapters command
cat > .claude/commands/asdm-plan-chapters.md << 'EOF'
---
description: "分章情节大纲：故事蓝图 → 分卷/章/场景三级大纲（含伏笔登记）"
argument-hint: "[故事项目]"
---

EOF
cat .asdm/toolsets/story-forge/actions/plan-chapters.md >> .claude/commands/asdm-plan-chapters.md

# write-story command
cat > .claude/commands/asdm-write-story.md << 'EOF'
---
description: "完整故事文本写作：分章大纲 → 逐章正文（每章更新故事状态，人工审阅后继续）"
argument-hint: "[章节范围]"
---

EOF
cat .asdm/toolsets/story-forge/actions/write-story.md >> .claude/commands/asdm-write-story.md
```

#### For GitHub Copilot (`.github/prompts/`):

GitHub Copilot 使用带 YAML frontmatter 的 `.prompt.md` 文件。通过拼接 GitHub 专属 frontmatter 与指令内容创建提示文件：

```bash
mkdir -p .github/prompts/

# expand-premise prompt
cat > .github/prompts/asdm-expand-premise.prompt.md << 'EOF'
---
agent: 'agent'
description: '设定解析与创意孵化：一句话设定 → 创意方向选择 → 故事概念卡'
argument-hint: '[一句话设定]'
---

EOF
cat .asdm/toolsets/story-forge/actions/expand-premise.md >> .github/prompts/asdm-expand-premise.prompt.md

# build-world prompt
cat > .github/prompts/asdm-build-world.prompt.md << 'EOF'
---
agent: 'agent'
description: '世界观构建：概念卡 → 世界观圣经（世界规则硬约束）'
argument-hint: '[故事项目]'
---

EOF
cat .asdm/toolsets/story-forge/actions/build-world.md >> .github/prompts/asdm-build-world.prompt.md

# design-characters prompt
cat > .github/prompts/asdm-design-characters.prompt.md << 'EOF'
---
agent: 'agent'
description: '人物设定：概念卡+世界观 → 人物圣经与关系'
argument-hint: '[故事项目]'
---

EOF
cat .asdm/toolsets/story-forge/actions/design-characters.md >> .github/prompts/asdm-design-characters.prompt.md

# design-plotline prompt
cat > .github/prompts/asdm-design-plotline.prompt.md << 'EOF'
---
agent: 'agent'
description: '主线设计：概念卡+世界观+人物 → 故事蓝图（主线/结构/节点/结局）'
argument-hint: '[故事项目]'
---

EOF
cat .asdm/toolsets/story-forge/actions/design-plotline.md >> .github/prompts/asdm-design-plotline.prompt.md

# plan-chapters prompt
cat > .github/prompts/asdm-plan-chapters.prompt.md << 'EOF'
---
agent: 'agent'
description: '分章情节大纲：故事蓝图 → 分卷/章/场景三级大纲（含伏笔登记）'
argument-hint: '[故事项目]'
---

EOF
cat .asdm/toolsets/story-forge/actions/plan-chapters.md >> .github/prompts/asdm-plan-chapters.prompt.md

# write-story prompt
cat > .github/prompts/asdm-write-story.prompt.md << 'EOF'
---
agent: 'agent'
description: '完整故事文本写作：分章大纲 → 逐章正文（每章更新故事状态，人工审阅后继续）'
argument-hint: '[章节范围]'
---

EOF
cat .asdm/toolsets/story-forge/actions/write-story.md >> .github/prompts/asdm-write-story.prompt.md
```

#### For Tencent CodeBuddy (`.codebuddy/commands/`):

CodeBuddy 不支持 frontmatter，直接复制指令文件即可：

```bash
mkdir -p .codebuddy/commands/

# 直接复制指令文件（无需 frontmatter）
cp .asdm/toolsets/story-forge/actions/expand-premise.md .codebuddy/commands/
cp .asdm/toolsets/story-forge/actions/build-world.md .codebuddy/commands/
cp .asdm/toolsets/story-forge/actions/design-characters.md .codebuddy/commands/
cp .asdm/toolsets/story-forge/actions/design-plotline.md .codebuddy/commands/
cp .asdm/toolsets/story-forge/actions/plan-chapters.md .codebuddy/commands/
cp .asdm/toolsets/story-forge/actions/write-story.md .codebuddy/commands/
```

### 4. Manual Usage for Other Providers

若你的 AI coding assistant 提供商未被自动检测逻辑覆盖（Claude Code、GitHub Copilot 或 Tencent CodeBuddy 之外），仍可手动使用 Story Forge。按以下步骤操作：

#### 直接使用指令文件

你可以直接复制指令文件的相对路径并粘贴到 AI coding assistant 的对话窗口：

1. **导航到指令文件目录**：
   ```bash
   cd .asdm/toolsets/story-forge/actions/
   ```

2. **右键点击目标指令文件** 并复制其相对路径：
   - 设定解析与创意孵化：`expand-premise.md`
   - 世界观构建：`build-world.md`
   - 人物设定：`design-characters.md`
   - 主线设计：`design-plotline.md`
   - 分章情节大纲：`plan-chapters.md`
   - 完整故事文本写作：`write-story.md`

3. **在 AI coding assistant 中输入提示**：
   ```
   Follow the instructions in {instruction 文件的相对路径}
   ```

## Initializing Story Forge 故事锻造工具集

### 设定解析与创意孵化（expand-premise）

安装完成后，从第一个 action 开始：

```shell
Follow the instructions in .asdm/toolsets/story-forge/actions/expand-premise.md
```

这会：
- 接收你的一句话设定，创建故事项目目录并保存 `premise.md`
- 解析设定的核心人物、事件、背景与特殊设定
- 扩展 2-3 个差异化创意方向，由你选择或融合
- 生成并保存故事概念卡 `concept.md`（经你确认后收尾）

### 世界观构建（build-world）

完成第一个 action 后，运行后续 action：

```shell
Follow the instructions in .asdm/toolsets/story-forge/actions/build-world.md
```

这会：
- 读取 `premise.md` 与 `concept.md`，与你确认构建范围
- 按时代背景、世界规则（含代价与限制）、历史、地理、社会文化、势力、特殊体系七大模块生成世界观
- 保存 `worldbuilding.md`（世界规则为后续写作硬约束），经你确认后收尾

### 人物设定（design-characters）

```shell
Follow the instructions in .asdm/toolsets/story-forge/actions/design-characters.md
```

这会：
- 读取概念卡与世界观，与你确认角色清单
- 为每个核心人物建立档案（外在目标、内在需求、恐惧、缺陷、秘密）、成长弧光与人物关系
- 保存 `characters.md`（人物圣经，含关系），经你确认后收尾

### 主线设计（design-plotline）

```shell
Follow the instructions in .asdm/toolsets/story-forge/actions/design-plotline.md
```

这会：
- 读取概念卡、世界观与人物圣经，与你确认结构模型（三幕式/英雄之旅等）
- 依次完成核心冲突（外部+内部）、主线与支线、情节节点、结局与悬念设计
- 保存 `plotline.md`（故事蓝图），经你确认后收尾

### 分章情节大纲（plan-chapters）

```shell
Follow the instructions in .asdm/toolsets/story-forge/actions/plan-chapters.md
```

这会：
- 读取故事蓝图、人物与世界观，与你确认篇幅规划
- 拆解「卷 → 章 → 场景」三级大纲，每章含目标/冲突/转折/钩子四要素
- 登记伏笔埋设与回收位置，完成节奏检查后保存 `chapters.md`

### 完整故事文本写作（write-story）

```shell
Follow the instructions in .asdm/toolsets/story-forge/actions/write-story.md
```

这会：
- 与你确认本次写作的章节范围（不一次性写完全部章节）
- 按场景生成初稿、扩写细节与对话、控制情绪与文风，整合自检后保存 `story/chapter-<NN>.md`
- 每章完成后更新 `story-state.md`（人物位置、信息差、时间线、伏笔状态），交你审阅确认后继续

### Available Commands

安装完成后，可使用以下命令：

1. **`/asdm-expand-premise`** - 一句话设定 → 创意方向选择 → 故事概念卡
2. **`/asdm-build-world`** - 概念卡 → 世界观圣经（世界规则硬约束）
3. **`/asdm-design-characters`** - 概念卡+世界观 → 人物圣经与关系
4. **`/asdm-design-plotline`** - 概念卡+世界观+人物 → 故事蓝图（主线/结构/节点/结局）
5. **`/asdm-plan-chapters`** - 故事蓝图 → 分卷/章/场景三级大纲（含伏笔登记）
6. **`/asdm-write-story`** - 分章大纲 → 逐章正文（每章更新故事状态，人工审阅后继续）

## Toolset Structure

本工具集以**单个故事**为项目粒度，运行期在 `.asdm/workspace/<story-id>-<story-name>/`（特性目录）下集中产出：

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

## Spec Documents

本工具集使用以下 spec 文档作为模板：

1. **`concept-card-spec.md`** - 故事概念卡的模板——九字段口径、Logline 三要素与验收清单
2. **`worldbuilding-spec.md`** - 世界观圣经的模板——七大模块、世界规则硬约束与代价限制写法
3. **`character-bible-spec.md`** - 人物圣经的模板——欲望-恐惧-缺陷三角、人物弧光与人物关系
4. **`plotline-spec.md`** - 故事蓝图的模板——双层冲突交织、结构模型、情节节点与悬念回收标注
5. **`chapters-outline-spec.md`** - 分章大纲的模板——卷/章/场景三级结构、每章四要素与伏笔登记表
6. **`story-chapter-spec.md`** - 章节正文的模板——场景推进写作准则与 `chapter-<NN>.md` 命名约定
7. **`story-state-spec.md`** - 故事状态记忆的模板——人物位置、信息差、时间线与伏笔状态对账机制

## Verification

安装完成后，验证以下内容：

1. `.asdm/workspace/` 基础目录已存在（Story Forge 故事锻造工具集的工作区根目录）
2. Story Forge 故事锻造工具集（toolset ID: `story-forge`）的快捷命令已创建在对应提供商目录（若使用 Claude Code、GitHub Copilot 或 Tencent CodeBuddy）
3. 工具集文件位于 `.asdm/toolsets/story-forge`（toolset ID: `story-forge`）

**其它提供商**：验证你能访问以下指令文件：
- `.asdm/toolsets/story-forge/actions/expand-premise.md`
- `.asdm/toolsets/story-forge/actions/build-world.md`
- `.asdm/toolsets/story-forge/actions/design-characters.md`
- `.asdm/toolsets/story-forge/actions/design-plotline.md`
- `.asdm/toolsets/story-forge/actions/plan-chapters.md`
- `.asdm/toolsets/story-forge/actions/write-story.md`

## Usage Examples

### Example 1: 从一句话设定开始一个新故事

```shell
# 首先，使用 AI Guided Installation 安装工具集
Follow instructions in .asdm/toolsets/story-forge/INSTALL.md

# 然后运行第一个 action
Follow the instructions in .asdm/toolsets/story-forge/actions/expand-premise.md

# 使用 slash command 时的示例提示：
/asdm-expand-premise 一个失去记忆的灯塔守夜人发现塔顶的灯每晚自己亮起
```

### Example 2: 大纲确认后逐章写作

```shell
# 运行正文写作 action
Follow the instructions in .asdm/toolsets/story-forge/actions/write-story.md

# 使用 slash command 时的示例提示（默认写下一章）：
/asdm-write-story 第 3 章
```

## Usage

### For Supported Providers (Claude Code, GitHub Copilot, Tencent CodeBuddy)

安装完成后，可使用以下命令：

- `/asdm-expand-premise`: 一句话设定 → 创意方向选择 → 故事概念卡
- `/asdm-build-world`: 概念卡 → 世界观圣经（世界规则硬约束）
- `/asdm-design-characters`: 概念卡+世界观 → 人物圣经与关系
- `/asdm-design-plotline`: 概念卡+世界观+人物 → 故事蓝图（主线/结构/节点/结局）
- `/asdm-plan-chapters`: 故事蓝图 → 分卷/章/场景三级大纲（含伏笔登记）
- `/asdm-write-story`: 分章大纲 → 逐章正文（每章更新故事状态，人工审阅后继续）

### For Other Providers (Manual Usage)

若你的提供商未被自动检测，可按上文 "Manual Usage for Other Providers" 章节的步骤手动使用指令。

## Notes

- 安装过程假定你具备创建目录与文件的必要权限
- 命令的实际执行由 AI 模型基于 Story Forge 故事锻造工具集（toolset ID: `story-forge`）提供的模板与指令完成
- 请根据你实际使用的 AI coding assistant 自定义提供商专属设置
- 在命令与文档中引用 Story Forge 时，应一致使用工具集 ID `story-forge`
- **未覆盖的提供商**：用户可复制指令文件相对路径并输入提示（如 "follow the instructions in .asdm/toolsets/story-forge/actions/expand-premise.md"）手动使用
- **工具集特有说明**：故事项目目录（`.asdm/workspace/<story-id>-<story-name>/`）由 `expand-premise` 在运行期按故事动态创建，安装时无需预建；每级产物经用户确认后才进入下一级，任何一级都可回改并向下游同步

## Integration with Other Toolsets

Story Forge 故事锻造工具集可与其他 ASDM 工具集及上下文文件集成。Context Builder 生成的上下文文件可被引用，以将生成的创作文档与实际项目对齐。例如，后续可扩展审校、定稿与导出等工具集与本工具集的产物对接。

### Getting Help

若遇 Story Forge 故事锻造工具集相关问题，请参考：
- [ASDM Documentation](https://asdm.ai/docs)
- 工具集 README：`.asdm/toolsets/story-forge/README.md`
- spec 文档：`.asdm/toolsets/story-forge/spec/`

## License

Copyright (c) 2026 LeansoftX.com & iSoftStone. All rights reserved.

Licensed under the PROPRIETARY SOFTWARE LICENSE. See [LICENSE](LICENSE) in the project root for license information.

---

*This installation document is part of the Story Forge 故事锻造工具集 toolset. 从一句话设定到完整故事，逐级确认、逐章交付。*
