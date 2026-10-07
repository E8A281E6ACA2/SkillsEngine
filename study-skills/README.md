# study-skills — skill 写法学习区

这里存放**优秀 skill 范例**与**学习资料**，用来研究「怎么写好一个 skill」和「工程实践中怎么用 skill」。**这些不是生效的 skill**。

## 为什么放在这里就不生效

生效的 skill 必须**直接位于 `skills/`**（junction 分发到 `~/.claude/skills`、`~/.agents/skills`）。工具只扫描**直接子目录**，`study-skills/<...>/SKILL.md` 不会被加载——所以这里可以安全堆放范例，不干扰实际使用的 skill。

```
SkillsEngine/
├── skills/              # 生效中（5 个，被三个工具加载）
└── study-skills/        # 学习区（只阅读，不加载）
    ├── README.md                         ← 本文件
    ├── engineering/                       ← 25 个工程类 skill 范例（开发/运维/测试…）
    ├── writing-skills/                    ← 如何写 skill 的方法论 + Anthropic 官方最佳实践
    ├── using-superpowers/                 ← skill 框架的用法说明（writing-skills 的依赖）
    └── <学术类 13 个>/SKILL.md            ← 学术学习类范例
```

> 把某个范例变成**生效**的 skill：`git mv study-skills/<名字> skills/<名字>`，`git commit` 后即时生效（junction 已在，无需重启）。

---

## 一、工程类 skill（25 个，来自 addyosmani/agent-skills）

Google 工程师 Addy Osmani 出品的「生产级工程技能」，覆盖软件全生命周期，是学习**工程场景怎么写 skill** 的上佳范例。

### 定义与规划
| Skill | 用途 |
|-------|------|
| `interview-me` | 一次一个问题地追问，挖出用户真正想要的需求 |
| `idea-refine` | 用发散-收敛思维把粗糙想法打磨成可执行概念 |
| `spec-driven-development` | 写代码前先写规格（spec before code） |
| `planning-and-task-breakdown` | 把规格拆成有序、可执行的原子任务 |
| `constraint-driven-development` | 把质量门槛写成契约，防止 agent 悄悄降低标准 |

### 设计与实现
| Skill | 用途 |
|-------|------|
| `api-and-interface-design` | 设计稳定的 API 与模块边界 |
| `frontend-ui-engineering` | 构建生产级、可访问、响应式的界面 |
| `incremental-implementation` | 以薄而可验证的切片增量交付 |
| `code-simplification` | 在不改变行为的前提下让代码更清晰 |
| `source-driven-development` | 每个实现决策都依据官方文档 |

### 测试与调试
| Skill | 用途 |
|-------|------|
| `test-driven-development` | 红-绿-重构循环驱动开发 |
| `debugging-and-error-recovery` | 系统化的根因调试 |
| `doubt-driven-development` | 对每个非平凡决策做全新上下文的对抗性复核 |
| `browser-testing-with-devtools` | 通过 Chrome DevTools 在真实浏览器中测试 |

### 评审与质量
| Skill | 用途 |
|-------|------|
| `code-review-and-quality` | 多维度代码评审（合并前） |
| `security-and-hardening` | 审计并加固代码，防御漏洞 |
| `performance-optimization` | 优化前后端、查询与数据库性能 |
| `observability-and-instrumentation` | 加日志/指标/追踪/告警，让生产行为可观测 |

### 交付与运维
| Skill | 用途 |
|-------|------|
| `ci-cd-and-automation` | 搭建/修改构建与部署流水线 |
| `git-workflow-and-versioning` | 提交、分支、解决冲突的规范化工作流 |
| `shipping-and-launch` | 准备生产发布 |
| `deprecation-and-migration` | 管理旧系统/API 的弃用与迁移 |
| `documentation-and-adrs` | 记录架构决策（ADR）与文档 |

### 元技能
| Skill | 用途 |
|-------|------|
| `context-engineering` | 优化 agent 上下文设置 |
| `using-agent-skills` | 发现并调用合适的 skill |

---

## 二、如何写 skill（重点，你主要来学这个）

### 1. `writing-skills`（来自 obra/superpowers）
核心观点：**写 skill 就是给"流程文档"做测试驱动开发（TDD）**。你写"压力测试场景"，先看 agent 在没有 skill 时如何失败（基线），再写 skill，再看测试是否通过，最后重构堵漏。
- **核心原则**：如果你没亲眼看 agent 在没有 skill 时失败，你就不知道这个 skill 是否教对了东西。
- 关键参考文件（同目录）：
  - `anthropic-best-practices.md` — **Anthropic 官方 skill 编写最佳实践**（强烈建议先读）
  - `testing-skills-with-subagents.md` — 如何用子代理测试 skill
  - `persuasion-principles.md` — 让指令更有说服力的原则
  - `examples/` — 示例

### 2. `using-superpowers`（依赖项）
说明该 skill 框架如何发现和调用 skill，是理解 `writing-skills` 的背景。

### 3. Anthropic 官方最佳实践要点（摘自 `anthropic-best-practices.md`）
- **简洁是关键**：上下文窗口是公共资源，只补充 agent 不知道的内容
- **设置合适的自由度**：脆弱/需一致性的操作写死步骤；灵活的操作用"为什么"引导
- **给默认值，而非菜单**：多个方案时给出一个默认 + 简短提及备选
- **讲流程，而非给具体答案**：教方法，让 skill 可复用而非只解决当下这一题
- **Gotchas 段落**：最有价值的内容是"违背常识的环境特定事实"，把踩过的坑写进去
- **SKILL.md 控制在 500 行 / 5000 token 内**，长内容拆到 `references/`
- **渐进披露**：明确告诉 agent **何时**去读哪个参考文件

### 4. 官方规范与教程（在线，权威）
- **Agent Skills 开放标准**：https://agentskills.io
  - 快速上手：https://agentskills.io/skill-creation/quickstart
  - 最佳实践：https://agentskills.io/skill-creation/best-practices
  - 规范全文：https://agentskills.io/specification
- **Claude Code Skills 文档**：https://code.claude.com/docs/en/skills
- **Codex (GPT) Skills 文档**：https://developers.openai.com/codex/skills
- **opencode Skills 文档**：https://opencode.ai/docs/skills/

---

## 三、书籍与延伸学习资源

> 说明：Agent Skills 是 2025-2026 才出现的开放标准，目前**尚无专门成书**。上面第二节的官方文档/最佳实践是当前最权威、最新的材料。以下是相关的成体系书籍与仓库，帮你打底层认知：

### 智能体原理与实践
| 资源 | 说明 | 链接 |
|------|------|------|
| 《从零开始构建智能体》 | Datawhale 中文开源书，从原理到手写 Agent（8 万+ star） | https://github.com/datawhalechina/hello-agents |
| Anthropic《Building Effective Agents》 | 官方经典文章，讲 agent 设计模式 | https://www.anthropic.com/engineering/building-effective-agents |

### AI 工程
| 资源 | 说明 | 链接 |
|------|------|------|
| 《AI Engineering》(Chip Huyen, 2025, O'Reilly) | AI 工程师必备，涵盖智能体/评估/检索等 | https://github.com/chiphuyen/aie-book |
| 《Agent Skills 图解》(中文，社区) | 从零构建技能库与实战（笔记类，社区维护） | https://github.com/shawngux/agent-skills-book |

### 可参考的优秀 skill 仓库
| 仓库 | 说明 |
|------|------|
| https://github.com/addyosmani/agent-skills | 本文工程类 skill 的来源（MIT） |
| https://github.com/obra/superpowers | skill 框架 + `writing-skills` 方法论（MIT） |
| https://github.com/anthropics/skills | Anthropic 官方 skill 合集（pdf/docx/pptx 等） |
| https://github.com/vercel-labs/agent-skills | Vercel 官方 skill 合集 |
| https://github.com/vercel-labs/skills | 开源 skills CLI（`npx skills add ...`，装到 70+ 工具） |

### 官方 skill 合集里的工程/文档范例
`anthropics/skills` 内置了 `skill-creator`（教你创建 skill）与 `mcp-builder`、`webapp-testing` 等实用 skill，值得对照学习。

---

## 四、来源与许可

| 目录 | 来源 | 许可 |
|------|------|------|
| `engineering/` | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | MIT（见 `engineering/LICENSE-ADDYOSMANI.txt`） |
| `writing-skills/`、`using-superpowers/` | [obra/superpowers](https://github.com/obra/superpowers) | MIT（见 `writing-skills/source-license/LICENSE-SUPERPOWERS.txt`） |
| 学术类 13 个 | [Viniciusvcgprofssional/claude-skills-academic](https://github.com/Viniciusvcgprofssional/claude-skills-academic) | 已翻译为中文 |

> 这些是**第三方 skill 的副本**，仅用于学习，放在学习区不参与加载。若你日后要把某个据为己用并再分发，请保留对应 LICENSE。

---

## 五、学写法建议

1. **先读**：`writing-skills/anthropic-best-practices.md`（官方）→ `writing-skills/SKILL.md`（TDD 方法）
2. **再对照**：挑 2-3 个工程范例（推荐 `interview-me`、`test-driven-development`、`code-review-and-quality`），看它们的 `description` 怎么写得能精确触发
3. **然后动手**：照 `skills/` 的规范自己写一个（`name` 要等于目录名，`description` 写清"做什么+何时用"）
4. **用真实会话迭代**：用得不好就改措辞，skill 是活文档
