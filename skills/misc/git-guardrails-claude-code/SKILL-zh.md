---
name: git-guardrails-claude-code
description: Set up Claude Code hooks to block dangerous git commands (push, reset --hard, clean, branch -D, etc.) before they execute. Use when user wants to prevent destructive git operations, add git safety hooks, or block git push/reset in Claude Code.
---

# 设置 Git Guardrails

设置一个 PreToolUse hook，在 Claude 执行之前拦截并阻止危险的 git 命令。

## 什么被阻止

- `git push`（所有变体包括 `--force`）
- `git reset --hard`
- `git clean -f` / `git clean -fd`
- `git branch -D`
- `git checkout .` / `git restore .`

当被阻止时， Claude 会看到一条消息，告诉它没有权限访问这些命令。

## 步骤

### 1. 询问范围

询问用户：为**此项目**安装（`.claude/settings.json`）还是**所有项目**（`~/.claude/settings.json`）？

### 2. 复制 Hook 脚本

打包的脚本位于：[scripts/block-dangerous-git.sh](scripts/block-dangerous-git.sh)

根据范围将其复制到目标位置：

- **项目**：`.claude/hooks/block-dangerous-git.sh`
- **全局**：`~/.claude/hooks/block-dangerous-git.sh`

使用 `chmod +x` 使其可执行。

### 3. 将 Hook 添加到 Settings

添加到适当的 settings 文件：

**项目**（`.claude/settings.json`）：

```json
{
 "hooks": {
 "PreToolUse": [
 {
 "matcher": "Bash",
 "hooks": [
 {
 "type": "command",
 "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/block-dangerous-git.sh"
 }
 ]
 }
 ]
 }
}
```

**全局**（`~/.claude/settings.json`）：

```json
{
 "hooks": {
 "PreToolUse": [
 {
 "matcher": "Bash",
 "hooks": [
 {
 "type": "command",
 "command": "~/.claude/hooks/block-dangerous-git.sh"
 }
 ]
 }
 ]
 }
}
```

如果 settings 文件已存在，将 hook 合并到现有的 `hooks.PreToolUse` 数组中——不要覆盖其他设置。

### 4. 询问自定义

询问用户是否想从已阻止列表中添加或移除任何模式。相应编辑复制后的脚本。

### 5. 验证

运行快速测试：

```bash
echo '{"tool_input":{"command":"git push origin main"}}' | <path-to-script>
```

应退出代码 2 并向 stderr 打印 BLOCKED 消息。
