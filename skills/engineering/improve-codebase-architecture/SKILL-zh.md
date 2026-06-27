---
name: improve-codebase-architecture
description: Scan a codebase for deepening opportunities, present them as a visual HTML report, then grill through whichever one you pick.
disable-model-invocation: true
---

# Improve Codebase Architecture

暴露架构摩擦并提出**深化机会**——将浅层 modules 变为深层 modules 的重构。目标是 testability 和 AI-navigability。

这个命令_受_项目的 domain model 启发，并构建在共享的设计词汇之上：

- 运行 `/codebase-design` skill 获取架构词汇（**module**、**interface**、**depth**、**seam**、**adapter**、**leverage**、**locality**）及其原则（删除测试、"interface is the test surface"、"one adapter = hypothetical seam, two = real"）。在每个建议中精确使用这些术语——不要漂移到"component"、"service"、"API"或"boundary"。
- `CONTEXT.md` 中的领域语言为好的 seams 提供命名；`docs/adr/` 中的 ADR 记录了这个命令不应重新争论的决策。

## 流程

### 1. 探索

首先阅读项目的 domain glossary （`CONTEXT.md`）和你正在接触区域的任何 ADR。

然后使用带有 `subagent_type=Explore` 的 Agent tool 来遍历代码库。不要遵循僵化的启发式规则——有机地探索，记录你在哪里遇到摩擦：

- 在哪里理解一个概念需要在许多小 modules 之间跳转？
- 哪里 modules 是**浅层的**—— interface 几乎和 implementation 一样复杂？
- 哪里只是为了 testability 而提取了纯函数，但真正的 bug 隐藏在它们被调用的方式中（没有 **locality**）？
- 哪里紧密耦合的 modules 跨越它们的 seams 泄漏？
- 代码库的哪些部分没有测试，或者通过它们当前的 interface 难以测试？

对你怀疑是浅层的任何东西应用**删除测试**：删除它会集中复杂性，还是只是移动它？"是，集中"就是你想要的信号。

### 2. 将候选对象呈现为 HTML 报告

将自包含的 HTML 文件写入 OS 临时目录，以便没有任何东西留在 repo 中。从 `$TMPDIR` 解析临时目录，回退到 `/tmp`（或 Windows 上的 `%TEMP%`），并写入 `<tmpdir>/architecture-review-<timestamp>.html`，以便每次运行获得一个新文件。为用户打开它—— Linux 上 `xdg-open <path>`， macOS 上 `open <path>`， Windows 上 `start <path>`——并告诉他们绝对路径。

报告使用**通过 CDN 的 Tailwind** 进行布局和样式设计，以及**通过 CDN 的 Mermaid**，在图表/流程/序列能可靠传达结构时使用。将 Mermaid 与手工制作的 CSS/SVG 视觉元素混合使用——当关系是图状时（调用图、依赖关系、序列）使用 Mermaid，当你想要更具编辑性的东西（mass diagrams、 cross-sections、 collapse animations）时使用手写的 divs/SVG。每个候选对象获得一个**before/after 可视化**。要有视觉性。

对于每个候选对象，渲染一个带有以下内容的卡片：

- **文件**——涉及哪些文件/modules
- **问题**——为什么当前架构引起摩擦
- **解决方案**——用简单的英语描述什么会改变
- **收益**——用 locality 和 leverage 来解释，以及测试会如何改进
- **之前 / 之后图表**——并排显示，自定义绘制，说明浅层性和深化
- **推荐强度**——`Strong`、`Worth exploring`、`Speculative` 之一，渲染为 badge

以**Top recommendation** section 结束报告：你会先处理哪个候选对象以及为什么。

**使用 CONTEXT.md 词汇用于领域，使用 `/codebase-design` 词汇用于架构。** 如果 `CONTEXT.md` 定义了"Order"，就说"Order intake module"——而不是"FooBarHandler"，也不是"Order service"。

**ADR 冲突**：如果一个候选对象与现有 ADR 矛盾，只有当摩擦足够真实到需要重新审视 ADR 时才暴露它。在卡片中清晰标记（例如警告标注：_"contradicts ADR-0007 — but worth reopening because …"_）。不要列出每个 ADR 禁止的理论重构。

参见 [HTML-REPORT-zh.md](HTML-REPORT-zh.md) 获取完整的 HTML scaffold、图表模式和样式指导。

**现在还不要提出 interfaces。** 文件写入后，询问用户："你想探索哪一个？"

### 3. Grilling 循环

一旦用户选择了一个候选对象，运行 `/grilling` skill 与他们一起遍历设计树——约束、依赖关系、深化后的 module 的形态、 seam 后面是什么、哪些测试保留。

副作用在决策结晶时内联发生——在你进行的过程中运行 `/domain-modeling` skill 以保持 domain model 最新：

- **将一个深化后的 module 命名为 `CONTEXT.md` 中不存在的概念？** 将术语添加到 `CONTEXT.md`。如果文件不存在，懒加载创建。
- **在对话中精炼了一个模糊的术语？** 立即更新 `CONTEXT.md`。
- **用户因为一个起作用的理由拒绝了候选对象？** 提供一个 ADR，表述为：_"Want me to record this as an ADR so future architecture reviews don't re-suggest it?"_ 只有当该理由确实是未来的探索者避免再次建议相同事物所需要的时才提供——跳过短暂的理由（"现在不值得"）和不言自明的原因。
- **想要探索深化后 module 的替代 interfaces？** 运行 `/codebase-design` skill 并使用其 design-it-twice 并行 sub-agent 模式。
