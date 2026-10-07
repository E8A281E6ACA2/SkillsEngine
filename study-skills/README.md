# study-skills — skill 写法学习区

这里存放**优秀 skill 范例**，用来研究「怎么写好一个 skill」。**这些不是生效的 skill**。

## 为什么放在这里就不生效

生效的 skill 必须**直接位于 `skills/`**（junction 分发到 `~/.claude/skills` 等）。Claude Code 经实测**只扫描直接子目录**，`study-skills/<名字>/SKILL.md` 不会被任何工具发现——所以这里可以安全地堆放范例，不干扰实际使用的 skill。

```
SkillsEngine/
├── skills/         # 生效中：被三个工具加载
└── study-skills/   # 学习区：只阅读，不加载
    ├── README.md   ← 本文件
    └── <名字>/SKILL.md   ← 13 个学术学习类范例
```

## 范例清单（13 个，来自 claude-skills-academic）

### 学习与理解
| Skill | 用途 |
|-------|------|
| `resume-university` | 深度、系统地总结任何学习内容（像上一门课那样讲透） |
| `explain-like-im-5` | 三级递进式解释一个概念（零基础 → 专业） |
| `concept-map-builder` | 生成概念地图（文字或 mermaid 图） |
| `jargon-buster` | 提取陌生领域术语并逐一解释 |

### 计划与复习
| Skill | 用途 |
|-------|------|
| `study-sprint` | 课程大纲 / 书单 → 学习日程表 |
| `reading-pace-planner` | 长书 / PDF → 每日阅读计划 |
| `flashcard-forge` | 文本 / PDF / 笔记 → 问答闪卡 |
| `wrong-answer-log` | 错题 / 考试 → 错题日志 |

### 学术写作与研究
| Skill | 用途 |
|-------|------|
| `citation-untangler` | 整理杂乱、不一致的参考文献 |
| `reference-analysis` | 结合正文分析参考文献的质量与缺口 |
| `peer-review-lens` | 审稿人视角给论文 / 综述结构化意见 |
| `lecture-to-outline` | 讲座录音 / 转写稿 → 层级大纲 |
| `science-practice` | 用科学证据支撑策略 / 计划 / 决策 |

## 学写 skill 时重点看什么

1. **frontmatter 极简**：基本只有 `name` + `description`；`name` 必须与目录名一致（opencode 强制校验）。可选字段：`license`、`compatibility`、`metadata`。
2. **`description` 是灵魂**：这是工具判断“何时调用”的唯一依据。范例里常写成「做什么 + 何时触发」，并直接写入用户会说的触发短语。
3. **正文结构**：目标 → 步骤 / 流程 → 输出格式 → 示例，条理清晰。
4. **篇幅**：多数 40～150 行，**一个 skill 只干一件事**。
5. **渐进披露**：长资料拆成同目录的引用文件，正文只留主干。

## 把范例变成生效的 skill

```powershell
git mv study-skills/<名字> skills/<名字>   # 移进生效目录
```

然后（可选）：删掉 frontmatter 里的 `metadata.category`、把 `description` 改成中文贴合自己习惯。`git commit` 后**即时生效**（junction 已在），无需重启。

## 参考

- [Agent Skills 开放标准](https://agentskills.io)
- [Claude Code Skills 文档](https://code.claude.com/docs/en/skills)
- [Codex Skills 文档](https://developers.openai.com/codex/skills)
- [opencode Skills 文档](https://opencode.ai/docs/skills/)
