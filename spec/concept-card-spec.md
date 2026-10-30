# 故事概念卡（Story Concept）Specification

## Language Guidelines

本文档类产物使用环境检测到的响应语言生成。确保文档内容遵循：

1. **语言一致性**：全文使用同一种语言
2. **写作规范**：遵循该语言的写作风格与格式规范
3. **清晰性**：内容清晰易懂

**支持语言**：中文（zh，默认）、英文（en）

---

## Overview

本规范定义了**故事概念卡（concept.md）**的结构和格式要求。故事概念卡是 Story Forge 流水线的第一级正式产物，将一句话设定扩展为结构化的故事方案，是后续世界观、人物、主线等所有创作阶段的事实源头。

**主要用途**:
- 把模糊灵感固化为明确、无歧义的故事方向
- 作为后续所有创作 action 的前置输入与一致性基准
- 立项评估：快速判断故事类型、冲突、卖点是否成立

---

## Document Structure

使用以下模板结构生成故事概念卡：

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

## 创意方向备选记录（可选）

## 修订记录（可选）
```

**Key Elements**:
- 故事名称：暂定名即可，后续可改
- 故事类型：科幻/悬疑/奇幻/爱情/现实主义等，可组合
- 核心设定：一句话设定中提炼出的超常规设定元素
- 核心冲突：主角 + 对抗力量，须可感知、可升级
- Logline：主角 + 核心冲突 + 风险/代价，一句话讲清故事

---

## Section Guidelines

### 基本信息区 Guidelines

九个字段一行一项，全部必填（名称可为暂定）。

**Important**:
- 每个字段必须明确、无歧义，概念卡是事实源头
- Logline 不得只描述设定，必须包含冲突与代价
- 故事背景与核心设定须与 `premise.md` 原文一致，扩展不得背离

**Example**:
```markdown
- 一句话故事概述（Logline）：一个程序员发现自己写的代码正在改变现实，但每次改写都要付出记忆为代价，而他想改掉的正是自己最不愿失去的一段过去。
```

### 创意方向备选记录 Guidelines（可选）

记录用户未选择的 2-3 个创意方向的一句话摘要，便于日后回溯或转向。

---

## Usage Guidelines

生成故事概念卡时：

1. **Step 1**: 从 `premise.md` 解析核心人物、核心事件、背景、特殊设定
2. **Step 2**: 扩展 2-3 个差异化创意方向（类型、冲突形态、主题侧重至少一项显著不同）
3. **Step 3**: 由用户选择或融合方向后，填写概念卡并保存

**Important**:
- 创意方向的选择权在用户，AI 提供选项与推荐理由，不替用户拍板
- 概念卡保存前必须经过用户确认
- 设定过于模糊时，先向用户提出 2-3 个澄清问题

---

## Output Format

故事概念卡输出为：

**Format**: Markdown
**Location**: `.asdm/workspace/<story-id>-<story-name>/concept.md`
**Naming**: 固定文件名 `concept.md`

**Example**:
- Example output path: `.asdm/workspace/lost-lighthouse-lost-lighthouse-keeper/concept.md`

---

## Best Practices

1. **字段无歧义**：避免" TBD""待定"长期滞留，确认前补全
2. **Logline 自检**：一句话能否让陌生读者明白"这是什么故事"
3. **卖点差异化**：独特卖点应指向这个故事区别于同类作品的地方

### Common Pitfalls to Avoid

- 概念卡写成故事梗概——概念卡是"方向"，不是"情节"
- 核心冲突写成抽象主题词（如"爱与救赎"），缺少具体对抗形态

---

## Related Documents

- **premise.md**：概念卡的源头输入
- **worldbuilding-spec.md / character-bible-spec.md / plotline-spec.md**：下游产物须与概念卡一致

This spec is used by:

- **Action: expand-premise**: 生成并保存故事概念卡

---

## Checklist

Before finalizing 故事概念卡, check:

- [ ] 九个基本字段全部填写且无歧义
- [ ] Logline 包含主角、核心冲突、风险/代价
- [ ] 核心设定与 `premise.md` 原文一致
- [ ] 创意方向已经用户选择/融合确认
- [ ] 概念卡已经用户确认后保存
