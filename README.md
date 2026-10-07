# My Skills

个人 AI 编程助手 Skills 仓库。

## 结构

每个 skill 一个文件夹，只含一个 `SKILL.md`（frontmatter 声明名称和描述，正文是给 AI 的指令）：

```
skills/
├── 快速提交
│   └── quick-commit/  # Conventional Commits 规范的 commit message
├── 论文写作（原有）
│   ├── paper-summary/ # 论文要点总结
│   └── paper-cite/    # APA/MLA/IEEE/BibTeX 引用生成
├── 代码自动化（原有）
│   ├── auto-test/     # 自动生成测试
│   └── auto-doc/      # 自动生成文档
└── 学术学习（新增，来自 claude-skills-academic）
    ├── resume-university/     # 深度、系统地总结任何学习内容
    ├── flashcard-forge/       # 任意文本/笔记转问答闪卡
    ├── explain-like-im-5/     # 三级递进解释复杂概念
    ├── concept-map-builder/   # 生成概念地图（mermaid）
    ├── study-sprint/          # 课程大纲/书单 → 学习日程表
    ├── reading-pace-planner/  # 长书/PDF → 每日阅读计划
    ├── citation-untangler/    # 整理混乱的参考文献
    ├── reference-analysis/    # 结合正文分析参考文献质量
    ├── peer-review-lens/     # 以审稿人视角给学术写作提意见
    ├── lecture-to-outline/   # 讲座录音/转写 → 层级大纲
    ├── jargon-buster/        # 提取并解释陌生领域术语
    ├── wrong-answer-log/      # 从错题中生成错误日志
    └── science-practice/      # 用科学证据支撑决策与计划
```

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

3. `name` 必须与目录名一致（opencode 会强制校验），`description` 为必填
4. `git add . && git commit` 即可，无需任何注册/索引步骤。

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
