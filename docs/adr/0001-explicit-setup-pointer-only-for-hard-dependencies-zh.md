# 仅对硬依赖使用显式的 `/setup-matt-pocock-skills` 指引

Engineering skills 依赖于由 `/setup-matt-pocock-skills` 播种的 per-repo 配置（issue tracker、 triage label 词汇、 domain doc 布局）。有些 skills 没有这些配置就无法有意义地运作——它们必须发布到特定的 issue tracker 或应用特定的 label 字符串。其他 skills 仅使用它来精炼输出（词汇、 ADR 意识）并在没有它时优雅降级。

我们将这些分为 **hard-dependency** 和 **soft-dependency** skills：

- **Hard dependency**（`to-issues`、`to-prd`、`triage`）—— 包含显式的一行指引：_"… should have been provided to you — run `/setup-matt-pocock-skills` if not."_ 没有映射关系时，输出是错误的，而不仅仅是模糊。
- **Soft dependency**（`diagnose`、`tdd`、`improve-codebase-architecture`）—— 仅在泛化措辞中引用"项目的 domain glossary"和"你正在接触的区域的 ADR"。如果这些文档不存在， skill 仍然可用；只是输出不那么精准。

这种划分使 soft-dependency skills 保持 token 轻量，并避免将 setup 指引盲目复制到不需要它的地方。
