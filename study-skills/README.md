# study-skills — 学术学习 Skills 索引

本目录**不是** skill 本身，而是学术学习类 skill 的**分组索引**，便于浏览和学习。

> 为什么 skill 不放在这里：实测 Claude Code 只加载 `~/.claude/skills/` 的**直接子目录**，嵌套在分类子目录里的 skill 不会被发现（详见 [README 结构说明](../README.md)）。因此所有 skill 物理上都在顶层 `skills/`，分类仅通过 `metadata.category: study-skills` 标记。

## 学术学习类 Skills（13 个）

来源：[claude-skills-academic](https://github.com/Viniciusvcgprofssional/claude-skills-academic)（Agent Skills 标准格式）

### 学习与理解

| Skill | 用途 |
|-------|------|
| `resume-university` | 深度、系统地总结任何学习内容（像上一门课那样讲透） |
| `explain-like-im-5` | 用三级递进的复杂度解释一个技术概念（从零基础到专业） |
| `concept-map-builder` | 生成概念地图，展示知识点之间的关系（文字或 mermaid 图） |
| `jargon-buster` | 从陌生领域的文本中提取专业术语并逐一解释 |

### 计划与复习

| Skill | 用途 |
|-------|------|
| `study-sprint` | 把课程大纲 / 书单 / 学科计划转成学习日程表 |
| `reading-pace-planner` | 把长书 / PDF 按目标日期拆成每日阅读计划 |
| `flashcard-forge` | 把任意文本 / PDF / 笔记转成问答闪卡（间隔复习） |
| `wrong-answer-log` | 从做错的习题 / 考试中生成错题日志，定位薄弱点 |

### 学术写作与研究

| Skill | 用途 |
|-------|------|
| `citation-untangler` | 把杂乱、不一致的参考文献整理成统一引用格式 |
| `reference-analysis` | 结合正文，分析参考文献的质量与作用、找出缺口 |
| `peer-review-lens` | 以审稿人视角，对论文 / 综述 / 毕设给出结构化意见 |
| `lecture-to-outline` | 把讲座录音 / 转写稿整理成层级大纲 |
| `science-practice` | 用科学证据支撑某个策略、计划或决策 |

## 调用方式

| 工具 | 用法 |
|------|------|
| Claude Code | `/resume-university`、`/flashcard-forge` … |
| Codex (GPT) | `$resume-university` 或 `/skills` |
| opencode | 直接用自然语言描述任务，由 agent 自动调用 |

## 新增学术 skill

1. 在 `skills/<名字>/SKILL.md` 新建（顶层，不放进本目录）
2. frontmatter 加分类标签：

   ```markdown
   ---
   name: <名字>
   description: 做什么 + 什么时候用
   metadata:
     category: study-skills
   ---
   ```

3. 在上面的表格里补一行索引

## 参考

- [Agent Skills 开放标准](https://agentskills.io)
- [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)
- [Codex Skills 文档](https://developers.openai.com/codex/skills)
- [opencode Skills 文档](https://opencode.ai/docs/skills/)
