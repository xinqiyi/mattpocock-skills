---
name: qa
description: Interactive QA session where user reports bugs or issues conversationally, and the agent files GitHub issues. Explores the codebase in the background for context and domain language. Use when user wants to report bugs, do QA, file issues conversationally, or mentions "QA session".
---

# QA Session

运行一个交互式 QA session。用户描述他们遇到的问题。你来澄清、在后台探索代码库以获取 context，然后提交持久的、以用户为中心的、使用项目领域语言的 GitHub issues。

## 对于用户提出的每个 issue

### 1. 倾听和轻度澄清

让用户用自己的话描述问题。最多问 **2-3 个简短澄清问题**，聚焦于：

- 他们期望什么 vs 实际发生了什么
- 复现步骤（如果不明显）
- 是持续性的还是偶发性的

不要过度访谈。如果描述足够清晰，继续。

### 2. 在后台探索代码库

在与用户交谈的同时，启动一个 Agent （subagent_type=Explore）在后台了解相关区域。目标不是找到修复——而是：

- 学习该区域使用的领域语言（检查 UBIQUITOUS_LANGUAGE.md）
- 了解该功能应该做什么
- 识别面向用户的行为边界

这些 context 帮助你编写更好的 issue ——但 issue 本身不应该引用特定文件、行号或内部实现细节。

### 3. 评估范围：单一 issue 还是分解？

在提交之前，决定这是一个**单一 issue** 还是需要**分解**为多个 issues。

当以下情况时分解：

- 修复跨越多个独立区域（例如"表单验证错误 AND 成功消息缺失 AND 重定向损坏"）
- 有明显可分离的关注点，不同人可以并行处理
- 用户描述的内容有多个不同的失败模式或症状

当以下情况时保持单一 issue：

- 是一个地方的一个行为错误
- 症状都由相同的根行为导致

### 4. 提交 GitHub issue(s)

使用 `gh issue create` 创建 issues。**不要**要求用户先审查——直接提交并分享 URL。

Issues 必须是**持久的**——它们应该在大重构后仍然有意义。从用户的角度编写。

#### 对于单一 issue

使用此模板：

```
## What happened

[用通俗语言描述用户实际体验到的行为]

## What I expected

[描述预期行为]

## Steps to reproduce

1. [开发者可以遵循的具体、编号的步骤]
2. [使用代码库中的领域术语，而不是内部 module 名称]
3. [包括相关输入、标志或配置]

## Additional context

[用户或代码库探索提供的任何额外观察，帮助框定问题——例如"这只在使用 Docker layer 时发生，而不是 filesystem layer"——使用领域语言但不引用文件]
```

#### 对于分解（多个 issues）

按依赖顺序创建 issues （blockers 优先），以便你可以引用真实的 issue 编号。

对每个子 issue 使用此模板：

```
## Parent issue

#<parent-issue-number>（如果你创建了追踪 issue）或 "Reported during QA session"

## What's wrong

[描述这个特定的行为问题——只是这个 slice，不是整个报告]

## What I expected

[这个特定 slice 的预期行为]

## Steps to reproduce

1. [特定于此 issue 的步骤]

## Blocked by

- #<issue-number>（如果这个 issue 需要在另一个解决后才能修复）

如果没有 blocker，则为 "None — can start immediately"。

## Additional context

[与此 slice 相关的任何额外观察]
```

在创建分解时：

- **宁愿多个薄 issue 而不是少数厚 issue**——每个应该可以独立修复和验证
- **诚实标记阻塞关系**——如果 issue B 真正需要 issue A 修复后才能测试，说出来。如果它们独立，两者都标记为"None — can start immediately"
- **按依赖顺序创建 issues**——以便你可以在"Blocked by"中引用真实的 issue 编号
- **最大化并行性**——目标是多人（或 agents）可以同时抓取不同的 issues

#### 所有 issue body 的规则

- **没有文件路径或行号**——这些会过时
- **使用项目的领域语言**（检查 UBIQUITOUS_LANGUAGE.md 如果存在）
- **描述行为，而不是代码**——"sync service 未能应用 patch"而不是"applyPatch() 在第 42 行抛异常"
- **复现步骤是强制性的**——如果你无法确定，询问用户
- **保持简洁**——开发者应在 30 秒内读完 issue

提交后，打印所有 issue URL （总结阻塞关系）并问："下一个 issue，还是我们结束了？"

### 5. 继续 session

继续直到用户说结束。每个 issue 是独立的——不要批量处理。
