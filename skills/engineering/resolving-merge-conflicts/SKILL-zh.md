---
name: resolving-merge-conflicts
description: "Use when you need to resolve an in-progress git merge/rebase conflict."
---

1. **查看当前状态** 的 merge/rebase。检查 git history 和冲突文件。

2. **找到每个冲突的主要来源**。深入理解每个变更为什么做出，以及原始意图是什么。阅读 commit messages，检查 PRs，检查原始的 issues/tickets。

3. **解决每个 hunk。** 尽可能保留双方的意图。在不兼容时，选择符合 merge 声明目标的那一方，并记录权衡。**不要**发明新的行为。始终解决；永远不要 `--abort`。

4. 发现项目的**自动化检查**并运行它们——通常是 typecheck，然后是 tests，然后是 format。修复 merge 破坏的任何东西。

5. **完成 merge/rebase。** Stage 所有内容并 commit。如果是 rebasing，继续 rebase 过程直到所有 commits 都被 rebase。
