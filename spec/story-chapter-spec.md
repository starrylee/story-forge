# 章节正文（Story Chapter）Specification

## Language Guidelines

本文档类产物使用环境检测到的响应语言生成。确保文档内容遵循：

1. **语言一致性**：全文使用同一种语言
2. **写作规范**：遵循该语言的写作风格与格式规范
3. **清晰性**：内容清晰易懂

**支持语言**：中文（zh，默认）、英文（en）

---

## Overview

本规范定义了**章节正文（story/chapter-<NN>.md）**的结构和格式要求。章节正文以场景为单位生成初稿、扩写细节与对话、整合为完整章节，须兑现分章大纲中本章的四要素契约。

**主要用途**:
- 按 `chapters.md` 逐章产出小说正文，逐章交付与审阅
- 补写或重写单章时的格式基准
- 全部章节完成后汇成完整故事文本

---

## Document Structure

使用以下模板结构生成章节正文：

```markdown
# 第 <N> 章：<章名>

<正文：以场景推进，含动作、环境、心理与对话>
```

**Key Elements**:
- 文件头：一级标题保留章号与章名，与 `chapters.md` 一致
- 场景衔接：正文按大纲场景顺序推进，场景间自然过渡
- 章末钩子：结尾落在大纲规定的本章钩子上
- 字数：在 `chapters.md` 本章预计字数的目标范围内

---

## Section Guidelines

### 正文写作 Guidelines

**Important**:
- **以场景为单位推进，以章节为单位交付**：先保证每个场景功能达成，再谈文笔
- 大纲是契约：正文须兑现本章四要素；若写作中发现大纲不合理，停下来向用户说明并提议修改大纲，而非擅自偏离
- 描写须符合世界观与时代氛围；避免连续大段静态描写打断叙事节奏
- 对话承载信息或张力，删除不推进任何东西的寒暄
- 叙事视角、语言风格与前文保持一致（至少重读上一章结尾对齐口吻）

**Example**:
```markdown
# 第 3 章：注释

凌晨两点，屏幕的光落在林川的手背上……

“别再往下查了。”

那行注释还在。他没写。也没人碰过他的键盘。
```

### 章节自检 Guidelines

整合后自检：本章目标是否达成；结尾钩子是否有效；有无违反世界规则、人物设定、时间线；字数是否在目标范围。自检不过则修订后再交付审阅。

---

## Usage Guidelines

生成章节正文时：

1. **Step 1**: 与用户确认本次写作的章节范围（默认下一章；**不要一次性写作全部章节**）
2. **Step 2**: 加载上下文：本章大纲四要素与场景安排、出场人物档案、相关世界规则、`story-state.md` 当前状态、上一章结尾
3. **Step 3**: 逐场景生成初稿，扩写细节与对话，控制情绪与文风
4. **Step 4**: 整合自检后保存章节文件
5. **Step 5**: 更新 `story-state.md`，交用户审阅，确认后继续下一章

**Important**:
- 每个人物的语气、用词、口头禅符合 `characters.md` 中的人物声音
- 伏笔按 `chapters.md` 登记的位置埋设与回收
- 上下文过大时优先读取：本章大纲 + 本章出场人物 + story-state + 上一章结尾
- 用户要求偏离大纲写作时按要求执行，但在章节末尾提示可能需要的后续大纲调整

---

## Output Format

章节正文输出为：

**Format**: Markdown（正文体裁，非结构化字段）
**Location**: `.asdm/workspace/<story-id>-<story-name>/story/chapter-<NN>.md`
**Naming**: `chapter-<NN>.md`，`<NN>` 为两位章号（`chapter-01.md`、`chapter-02.md`）

**Example**:
- Example output path: `.asdm/workspace/code-reality/code-reality/story/chapter-03.md`

---

## Best Practices

1. **先功能后文笔**：场景功能未达成的文字再美也应重写
2. **对话可辨人**：遮住名字能认出谁在说话
3. **情绪按位投放**：铺垫章克制、高潮章浓烈，按本章在整体节奏中的位置定调

### Common Pitfalls to Avoid

- 为凑字数注水：场景达成功能即止
- 对话成为设定说明的传声筒

---

## Related Documents

- **chapters-outline-spec.md**：本章四要素与场景安排的契约来源
- **character-bible-spec.md / worldbuilding-spec.md**：对话与描写的事实依据
- **story-state-spec.md**：每章完成后必须同步更新

This spec is used by:

- **Action: write-story**: 逐章生成并保存章节正文

---

## Checklist

Before finalizing 章节正文, check:

- [ ] 本章四要素（目标/冲突/转折/钩子）全部兑现
- [ ] 场景按大纲顺序且各达其功能
- [ ] 无违反世界规则、人物设定、时间线
- [ ] 文风与叙事视角与前文一致
- [ ] 字数在目标范围
- [ ] 已保存至 `story/chapter-<NN>.md` 并同步更新 `story-state.md`
- [ ] 已交用户审阅
