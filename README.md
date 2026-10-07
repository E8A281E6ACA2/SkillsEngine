# My Skills

个人 AI 编程助手 Skills 仓库。

## 结构

每个 skill 一个文件夹、**直接位于 `skills/` 下**，只含一个 `SKILL.md`。分类通过 frontmatter 的 `metadata.category` 标记，**不建物理子目录**：

| 分类 | skills |
|------|--------|
| commit | `quick-commit` |
| paper | `paper-summary`、`paper-cite` |
| automation | `auto-test`、`auto-doc` |
| **study-skills**（学术学习） | `resume-university`、`flashcard-forge`、`explain-like-im-5`、`concept-map-builder`、`study-sprint`、`reading-pace-planner`、`citation-untangler`、`reference-analysis`、`peer-review-lens`、`lecture-to-outline`、`jargon-buster`、`wrong-answer-log`、`science-practice` |

```markdown
---
name: flashcard-forge
description: ...
metadata:
  category: study-skills
---
```

> **为什么不建 `study-skills/` 物理子目录**：实测（`claude -p "/skills"`）Claude Code **只加载 `~/.claude/skills/` 的直接子目录**，嵌套在分类文件夹里的 skill 不会被发现。Codex 与 opencode 支持递归扫描，但为了三个工具一致可用，统一采用扁平结构 + `metadata.category` 标签做分类。


## 加载方式

`skills/` 目录通过两条 junction 链接分发，仓库里的改动**即时生效**——各工具都会在会话中实时监测 skills 目录：

| 工具 | 读取路径 | 链接 |
|------|---------|------|
| Claude Code | `~/.claude/skills`（原生） | junction → 本仓库 |
| Codex (GPT) | `~/.agents/skills`（USER 级，官方文档） | junction → 本仓库 |
| opencode | 同时读 `~/.claude/skills` 和 `~/.agents/skills` | 复用上面两条 |

说明：
- opencode 扫描 `~/.agents` 优先级高于 `~/.claude`，同名 skill 静默覆盖（已读源码确认），不会重复加载
- `SKILL.md` 遵循 Agent Skills 开放标准（agentskills.io），Claude Code、Codex、opencode、Cursor、Gemini CLI、GitHub Copilot、Amp、Goose 等均支持此格式
- 各工具调用方式：Claude Code 输入 `/quick-commit`；Codex 输入 `$` 或 `/skills`；opencode 由 agent 自动调用

换机器时重建链接（不需要管理员权限，junction 即可）：

```powershell
New-Item -ItemType Junction -Path "$HOME\.claude\skills" -Target "<本仓库路径>\skills"
New-Item -ItemType Junction -Path "$HOME\.agents\skills" -Target "<本仓库路径>\skills"
```

## 新增 Skill

1. 新建 `skills/<名字>/SKILL.md`（名字用小写字母和连字符）
2. 写入：

   ```markdown
   ---
   name: <名字>
   description: 做什么 + 什么时候用（工具靠它判断是否调用）
   ---

   给 AI 的指令正文...
   ```

3. 可选：用 `metadata.category` 给 skill 打分类标签（如 `study-skills`、`paper`）
4. `name` 必须与目录名一致（opencode 会强制校验），`description` 为必填
5. `git add . && git commit` 即可，无需任何注册/索引步骤。

## 写作要点

- **description 决定触发时机**：写清楚"做什么 + 何时用"，这比正文更重要
- **frontmatter 只用标准字段**：`name`、`description`、`license`、`compatibility`、`metadata`、`allowed-tools`（外加 Claude Code 扩展字段）；未知字段会被静默忽略，但上传 claude.ai 时会直接报错
- **正文保持精简**：加载后常驻上下文，逐行都是持续的 token 成本；长内容（规范、模板、示例）拆成同目录文件按需引用
- **从重复劳动提炼**：每当你发现自己在重复教 AI 同一件事，就把它固化成 skill
- **用真实会话迭代**：效果不好就改措辞，skill 是活文档

## 参考

- Claude Code Skills 官方文档：https://code.claude.com/docs/en/skills
- Codex (GPT) Skills 官方文档：https://developers.openai.com/codex/skills
- opencode Agent Skills 官方文档：https://opencode.ai/docs/skills/
- Agent Skills 开放标准（跨工具通用）：https://agentskills.io
- Conventional Commits：https://www.conventionalcommits.org/
