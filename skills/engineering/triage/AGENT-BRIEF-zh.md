# 编写 Agent Briefs

Agent brief 是一个结构化的 comment，当 issue 或 PR 移动到 `ready-for-agent` 时发布在 GitHub issue 或 PR 上。它是 AFK agent 将依据其工作的权威规范。原始 body 和讨论是 context —— agent brief 是契约。

Brief 陈述**agent 应该做什么**，这延伸到两个面：对于 issue，是从无到有构建变更；对于 PR，是*对现有 diff* 还需要做什么——完成它、填补空白、处理审查意见。相同原则，任一方向适用；下面的 PR 示例显示了差异。

## 原则

### 持久性优于精确性

Issue 可能在 `ready-for-agent` 中停留数天或数周。代码库在此期间会变化。编写 brief 使其即使在文件被重命名、移动或重构时仍然有用。

- **要**描述 interfaces、 types 和 behavioral contracts
- **要**命名 agent 应该查找或修改的特定 types、 function signatures 或 config shapes
- **不要**引用文件路径——它们会过时
- **不要**引用行号
- **不要**假设当前的实现结构将保持不变

### 行为性，而非过程性

描述系统**应该做什么**，而不是**如何实现**。 Agent 将重新探索代码库并做出自己的实现决策。

- **好：** "`SkillConfig` type 应该接受一个可选的 `schedule` 字段，类型为 `CronExpression`"
- **坏：** "打开 src/types/skill.ts 并在第 42 行添加一个 schedule 字段"
- **好：** "当用户无参数运行 `/triage` 时，他们应该看到需要关注的 issues 摘要"
- **坏：** "在主 handler 函数中添加一个 switch 语句"

### 完整的验收标准

Agent 需要知道什么时候完成。每个 agent brief 必须有具体的、可测试的验收标准。每个标准应能独立验证。

- **好：** "运行 `gh issue list --label needs-triage` 返回已经过初始分类的 issues"
- **坏：** "Triage 应该正常工作"

### 明确的范围边界

说明什么是 out of scope。这防止 agent 过度设计或对相邻功能做出假设。

## 模板

```markdown
## Agent Brief

**Category:** bug / enhancement
**Summary:** 需要发生什么的单行描述

**Current behavior:**
描述现在发生了什么。对于 bug，这是损坏的行为。
对于 enhancement，这是该功能构建的现状。

**Desired behavior:**
描述 agent 工作完成后应该发生什么。
关于 edge cases 和错误条件要具体。

**Key interfaces:**
- `TypeName` ——需要改变什么以及为什么
- `functionName()` return type ——它当前返回什么 vs 应该返回什么
- Config shape ——任何需要的新配置选项

**Acceptance criteria:**
- [ ] 具体的、可测试的标准 1
- [ ] 具体的、可测试的标准 2
- [ ] 具体的、可测试的标准 3

**Out of scope:**
- 不应在此 issue 中更改或解决的问题
- 看似相关但实际独立的相邻功能
```

## 示例

### 好的 agent brief (bug)

```markdown
## Agent Brief

**Category:** bug
**Summary:** Skill description 截断在单词中间丢弃，产生损坏的输出

**Current behavior:**
当 skill description 超过 1024 个字符时，它被精确截断在
1024 个字符处，无论单词边界。这产生在单词中间
结束的 description （例如"Use when the user wants to confi"）。

**Desired behavior:**
截断应在 1024 个字符之前的最后一个单词边界处断开
并追加"..."以指示截断。

**Key interfaces:**
- `SkillMetadata` type 的 `description` 字段——无需类型变更，
 但填充它的 validation/processing 逻辑需要尊重
 单词边界
- 任何读取 SKILL.md frontmatter 并提取 description 的函数

**Acceptance criteria:**
- [ ] 1024 字符以下的 descriptions 不变
- [ ] 1024 字符以上的 descriptions 在 1024 字符之前的
 最后一个单词边界处截断
- [ ] 截断的 descriptions 以"..."结尾
- [ ] 包括"..."在内的总长度不超过 1024 字符

**Out of scope:**
- 改变 1024 字符限制本身
- 多行 description 支持
```

### 好的 agent brief (enhancement)

```markdown
## Agent Brief

**Category:** enhancement
**Summary:** 添加 `.out-of-scope/` 目录支持以追踪被拒绝的 feature requests

**Current behavior:**
当一个 feature request 被拒绝时， issue 用 `wontfix` label
和 comment 关闭。没有决策或理由的持久记录。
未来类似的请求需要维护者回忆或搜索
先前的讨论。

**Desired behavior:**
被拒绝的 feature requests 应记录在 `.out-of-scope/<concept>.md`
文件中，捕获决策、理由和所有请求该功能的 issues
的链接。在 triage 新 issues 时，应检查这些文件
以查找匹配。

**Key interfaces:**
- `.out-of-scope/` 中的 Markdown 文件格式——每个文件应有
 `# Concept Name` 标题、`**Decision:**` 行、`**Reason:**` 行
 和带 issue 链接的 `**Prior requests:**` 列表
- Triage workflow 应在早期读取所有 `.out-of-scope/*.md` 文件
 并按概念相似性将传入 issues 与它们匹配

**Acceptance criteria:**
- [ ] 将功能作为 wontfix 关闭会在 `.out-of-scope/` 中创建/更新一个文件
- [ ] 该文件包括决策、理由和已关闭 issue 的链接
- [ ] 如果匹配的 `.out-of-scope/` 文件已存在，新 issue 将被追加到
 其"Prior requests"列表而不是创建重复
- [ ] 在 triage 期间，检查现有的 `.out-of-scope/` 文件并在
 新 issue 匹配先前的拒绝时暴露它们

**Out of scope:**
- 自动匹配（人类确认匹配）
- 重新开启先前被拒绝的功能
- Bug reports （只有 enhancement 拒绝进入 `.out-of-scope/`）
```

### 好的 agent brief (PR)

对于 PR，"Current behavior"描述 diff 的状态， brief 要求 agent 完成或修复它，而不是从头构建。

```markdown
## Agent Brief

**Category:** enhancement
**Summary:** 完成贡献者的 `triage list` 的 `--json` 输出标志

**Current behavior:**
PR 添加了一个 `--json` 标志，将 issue 列表序列化为 JSON。
Happy path 工作且 diff 匹配项目的命令结构。两个缺口
仍然存在：错误仍以人类文本（非 JSON）打印，新标志没有
测试覆盖。

**Desired behavior:**
使用 `--json`，所有输出——包括错误——在 stdout 上是格式良好的 JSON，
并且命令的退出码不变。当标志不存在时，现有的
人类可读输出不受影响。

**Key interfaces:**
- 命令的错误路径应在 `--json` 下发出 `{ "error": string }`
 而不是纯文本错误
- 复用 PR 已添加的现有 serializer；不要引入第二个

**Acceptance criteria:**
- [ ] `triage list --json` 为成功和错误情况都发出有效的 JSON
- [ ] 退出码与非 JSON 命令匹配
- [ ] 一个测试覆盖 `--json` 成功输出和一个错误情况
- [ ] 默认（非 JSON）输出按字节不变

**Out of scope:**
- 向任何其他命令添加 `--json`
- 改变 PR 已定义的成功 payload 的 JSON 形状
```

### 坏的 agent brief

```markdown
## Agent Brief

**Summary:** Fix the triage bug

**What to do:**
The triage thing is broken. Look at the main file and fix it.
The function around line 150 has the issue.

**Files to change:**
- src/triage/handler.ts (line 150)
- src/types.ts (line 42)
```

这很糟糕因为：
- 没有 category
- 模糊的描述（"the triage thing is broken"）
- 引用了会过时的文件路径和行号
- 没有验收标准
- 没有范围边界
- 没有描述当前 vs 期望行为
