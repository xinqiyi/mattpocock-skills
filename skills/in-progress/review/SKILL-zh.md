---
name: review
description: Review the changes since a fixed point (commit, branch, tag, or merge-base) along two axes — Standards (does the code follow this repo's documented coding standards?) and Spec (does the code match what the originating issue/PRD asked for?). Runs both reviews in parallel sub-agents and reports them side by side. Use when the user wants to review a branch, a PR, work-in-progress changes, or asks to "review since X".
---

对 `HEAD` 与用户提供的固定点之间的 diff 进行双轴审查：

- **Standards** ——代码是否符合此 repo 记录的编码标准？
- **Spec** ——代码是否忠实地实现了源 issue / PRD / spec？

两个轴都作为**并行 sub-agents** 运行，因此它们不会污染彼此的 context，然后此 skill 聚合它们的发现。

Issue tracker 应该已提供给你——如果 `docs/agents/issue-tracker.md` 缺失，运行 `/setup-matt-pocock-skills`。

## 流程

### 1. 确定固定点

用户说的任何固定点—— commit SHA、 branch 名称、 tag、`main`、`HEAD~5` 等。如果他们没指定，询问它。

捕获 diff 命令一次：`git diff <fixed-point>...HEAD`（三点，因此比较是针对 merge-base）。同时通过 `git log <fixed-point>..HEAD --oneline` 记录 commits 列表。

在继续之前，确认固定点可以解析（`git rev-parse <fixed-point>`）且 diff 非空。坏的 ref 或空 diff 应在这里失败——而不是在两个并行 sub-agents 内部。

### 2. 识别 Spec 来源

寻找源 spec，按此顺序：

1. Commit messages 中的 issue 引用（`#123`、`Closes #45`、 GitLab `!67` 等）——通过 `docs/agents/issue-tracker.md` 中的 workflow 获取。
2. 用户作为参数传递的路径。
3. `docs/`、`specs/` 或 `.scratch/` 下匹配 branch 名称或功能的 PRD/spec 文件。
4. 如果什么都没找到，询问用户 spec 在哪里。如果他们说没有，**Spec** sub-agent 将跳过并报告"no spec available"。

### 3. 识别 Standards 来源

repo 中任何记录代码应如何编写的内容，如 `CODING_STANDARDS.md` 或 `CONTRIBUTING.md`。

### 4. 并行启动两个 Sub-Agents

使用单个消息发送两个 `Agent` tool 调用。两者都使用 `general-purpose` subagent。

**Standards sub-agent prompt** ——包括：

- 完整的 diff 命令和 commits 列表。
- 你在步骤 3 中找到的 standards-source 文件列表。
- Brief："报告——每个相关文件/hunk —— diff 违反已记录标准的每个地方。引用标准（文件 + 规则）。区分硬违规和判断性调用。跳过任何工具强制实施的内容。400 词以内。"

**Spec sub-agent prompt** ——包括：

- diff 命令和 commits 列表。
- spec 的路径或获取的内容。
- Brief："报告：(a) spec 要求但缺失或不完整的需求；(b) diff 中未被要求的行为（范围蔓延）；(c) 看似已实现但实现方式看起来错误的需求。对于每个发现引用 spec 行。400 词以内。"

如果 spec 缺失，跳过 Spec sub-agent 并在最终报告中注明。

### 5. 聚合

在 `## Standards` 和 `## Spec` 标题下呈现两个报告，逐字或轻度清理。**不要**合并或重新排序发现——两个轴是故意分开的（参见_为什么两个轴_）。

以一行摘要结束：每个轴的总发现数，以及_每个轴内_最严重的问题（如果有）。不要跨轴选择一个胜者——这就是分离要防止的重新排序。

## 为什么两个轴

一个变更可能通过一个轴而失败另一个：

- 遵循每个标准但实现了错误内容的代码 → **Standards pass, Spec fail。**
- 完全按照 issue 要求但破坏了项目约定的代码 → **Spec pass, Standards fail。**

分开报告它们防止一个轴掩盖另一个。
