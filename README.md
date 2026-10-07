# My Skills

个人 AI 编程助手 Skills 仓库。

## 结构

```
SkillsEngine/
├── skills/         # 生效中的 skills（junction 分发给 Claude Code / Codex / opencode）
└── study-skills/   # 学习区：优秀 skill 范例，用于研究怎么写 skill（不参与加载）
```

### skills/（生效中，5 个）

每个 skill 一个文件夹、**直接位于 `skills/` 下**，只含一个 `SKILL.md`：

| Skill | 用途 |
|-------|------|
| `quick-commit` | Conventional Commits 规范的 commit message |
| `paper-summary` | 论文要点总结 |
| `paper-cite` | APA/MLA/IEEE/BibTeX 引用生成 |
| `auto-test` | 自动生成测试 |
| `auto-doc` | 自动生成文档 |

### study-skills/（学习区，只阅读、不加载）

学习「怎么写 skill」的范例与资料合集，**刻意放在 `skills/` 之外**，因此不会被工具加载：

| 子目录 | 内容 | 来源 |
|--------|------|------|
| `engineering/` | 25 个工程类 skill（开发/测试/评审/CI-CD/可观测性…） | addyosmani/agent-skills (MIT) |
| `writing-skills/` | 如何写 skill 的方法论 + Anthropic 官方最佳实践 | obra/superpowers (MIT) |
| `using-superpowers/` | skill 框架用法（上述方法的依赖） | obra/superpowers (MIT) |
| 学术类 13 个目录 | 学术学习类范例（已译中文） | claude-skills-academic |

完整清单、书籍与学习资源见 [study-skills/README.md](./study-skills/README.md)。

> **为什么生效的 skill 必须扁平放 `skills/`**：实测（`claude -p`）Claude Code **只加载 `~/.claude/skills/` 的直接子目录**，嵌套在分类文件夹里的 skill 不会被发现。Codex 与 opencode 虽支持递归扫描，但为保持三工具一致，`skills/` 下不放分类子目录——要放**不生效**的范例，就放到 `study-skills/`。


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
