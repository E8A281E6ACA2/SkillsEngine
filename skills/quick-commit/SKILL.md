---
name: quick-commit
description: 快速生成符合规范的 Git commit message
---

# /qc - Quick Commit

快速分析当前的 Git 变更，生成符合 Conventional Commits 规范的 commit message，并可选择直接提交。

## 功能特性

- 🔍 自动分析 `git diff` 和 `git status`
- 📝 生成符合 Conventional Commits 规范的 commit message
- 🎯 智能识别变更类型（feat, fix, docs, refactor 等）
- ✅ 可选择直接提交或仅生成 message
- 🌍 支持中英文 commit message

## Conventional Commits 规范

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Type 类型：**
- `feat`: 新功能
- `fix`: Bug 修复
- `docs`: 文档变更
- `style`: 代码格式（不影响代码运行）
- `refactor`: 重构（既不是新功能也不是修复）
- `perf`: 性能优化
- `test`: 测试相关
- `chore`: 构建过程或辅助工具的变动
- `ci`: CI 配置文件和脚本的变动
- `build`: 影响构建系统或外部依赖的变更

## 使用方法

```bash
# 基本用法 - 分析变更并生成 commit message
/qc

# 生成后直接提交
/qc --commit

# 生成中文 commit message
/qc --lang zh

# 生成英文 commit message（默认）
/qc --lang en
```

## What You Must Do When Invoked

当用户调用 `/qc` 时，按以下步骤执行：

### Step 1 - 检查 Git 仓库状态

```bash
# 检查是否在 Git 仓库中
git rev-parse --is-inside-work-tree 2>/dev/null

# 检查是否有变更
git status --porcelain
```

如果不在 Git 仓库中或没有变更，提示用户并退出。

### Step 2 - 分析变更内容

并行运行以下命令获取变更信息：

```bash
# 获取已暂存的变更
git diff --cached --stat

# 获取已暂存的详细变更
git diff --cached

# 获取未暂存的变更
git diff --stat

# 获取文件状态
git status --short
```

### Step 3 - 分析并生成 Commit Message

基于变更内容，分析：

1. **变更类型**：
   - 新增文件/功能 → `feat`
   - 修复问题 → `fix`
   - 文档变更 → `docs`
   - 代码重构 → `refactor`
   - 性能优化 → `perf`
   - 测试相关 → `test`
   - 配置/构建 → `chore` 或 `ci`

2. **影响范围 (scope)**：
 - 根据变更的文件路径确定（如 `api`, `ui`, `auth`, `database`）
   - 如果变更跨多个模块，可省略 scope

3. **主题 (subject)**：
   - 简洁描述变更内容（50 字符以内）
   - 使用祈使句（如 "add" 而不是 "added"）
   - 不要以句号结尾

4. **正文 (body)** - 可选：
   - 详细说明变更的原因和内容
   - 每行不超过 72 字符

5. **页脚 (footer)** - 可选：
   - 关联的 issue（如 `Closes #123`）
   - Breaking changes（如 `BREAKING CHANGE: ...`）

### Step 4 - 展示生成的 Commit Message

以清晰的格式展示生成的 commit message：

```
📝 Generated Commit Message:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
feat(api): add user authentication endpoint

Implement JWT-based authentication for user login.
Added middleware for token validation and refresh.

Closes #42
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 5 - 询问用户操作

如果用户没有使用 `--commit` 参数，询问：

```
What would you like to do?
1. Commit with this message
2. Edit the message
3. Cancel
```

根据用户选择：
- **选项 1**：执行 `git commit -m "message"`
- **选项 2**：让用户提供修改后的 message，然后提交
- **选项 3**：退出，不提交

如果用户使用了 `--commit` 参数，直接执行提交。

### Step 6 - 执行提交（如果需要）

```bash
# 如果有未暂存的变更，先暂存
git add -A

# 提交
git commit -m "generated commit message here"

# 显示提交结果
git log -1 --oneline
```

## 示例

### 示例 1：新功能

**变更：**
- 新增 `src/auth/login.ts`
- 修改 `src/routes/api.ts`

**生成的 commit message：**
```
feat(auth): add user login functionality

Implement login endpoint with JWT token generation.
Added password validation and rate limiting.
```

### 示例 2：Bug 修复

**变更：**
- 修改 `src/utils/validation.ts`

**生成的 commit message：**
```
fix(validation): correct email regex pattern

Previous regex failed to validate emails with plus signs.
Updated to RFC 5322 compliant pattern.

Fixes #156
```

### 示例 3：文档更新

**变更：**
- 修改 `README.md`
- 新增 `docs/api.md`

**生成的 commit message：**
```
docs: update API documentation

Add comprehensive API endpoint documentation.
Update README with new installation instructions.
```

## 注意事项

- 如果变更过大（超过 20 个文件），建议用户拆分成多个 commit
- 如果检测到敏感信息（如密码、token），警告用户不要提交
- 如果是 Breaking Change，确保在 commit message 中明确标注
- 生成的 message 应该清晰、准确，避免模糊的描述

## 配置选项（可选）

用户可以在项目根目录创建 `.qc-config.json` 来自定义行为：

```json
{
  "language": "en",
  "maxSubjectLength": 50,
  "includeBody": true,
  "autoStage": true,
  "scopes": ["api", "ui", "auth", "database", "docs"]
}
```

## 相关资源

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Angular Commit Guidelines](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#commit)
