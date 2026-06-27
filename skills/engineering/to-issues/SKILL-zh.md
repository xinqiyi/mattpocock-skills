---
name: to-issues
description: Break a plan, spec, or PRD into independently-grabbable issues on the project issue tracker using tracer-bullet vertical slices.
disable-model-invocation: true
---

# To Issues

使用 vertical slices （tracer bullets）将计划分解为可独立抓取的 issues。

Issue tracker 和 triage label 词汇应该已经提供给你——如果没有，运行 `/setup-matt-pocock-skills`。

## 流程

### 1. 收集 context

从对话 context 中已有的内容开始。如果用户传递了一个 issue 引用（issue 编号、 URL 或路径）作为参数，从 issue tracker 获取它并阅读其完整主体和 comments。

### 2. 探索代码库（可选）

如果你还没有探索代码库，请这样做以了解代码的当前状态。 Issue 标题和描述应使用项目的 domain glossary 词汇，并尊重你正在接触区域的 ADR。

寻找预先重构代码以使实现更容易的机会。"Make the change easy, then make the easy change."

### 3. 起草 Vertical Slices

将计划分解为 **tracer bullet** issues。每个 issue 是一个薄的 vertical slice，端到端地贯穿所有集成层，而不是一个层的水平切片。

<vertical-slice-rules>

- 每个 slice 提供一条狭窄但**完整的**路径，贯穿每一层（schema、 API、 UI、 tests）
- 一个完成的 slice 可以独立演示或验证
- 任何预先重构应先完成

</vertical-slice-rules>

### 4. 询问用户

将提议的分解呈现为编号列表。对于每个 slice，展示：

- **标题**：简短的描述性名称
- **被谁阻塞**：哪些其他 slices （如果有）必须先完成
- **覆盖的用户故事**：这解决了哪些用户故事（如果原始材料有的话）

询问用户：

- 粒度感觉合适吗？（太粗 / 太细）
- 依赖关系正确吗？
- 任何 slices 应该合并或进一步拆分吗？

迭代直到用户批准分解。

### 5. 将 Issues 发布到 Issue Tracker

对于每个批准的 slice，向 issue tracker 发布一个新 issue。使用下面的 issue 主体模板。这些 issues 被认为已为 AFK agents 准备就绪，因此除非另有指示，使用正确的 triage label 发布它们。

按依赖顺序发布 issues （blockers 优先），以便你可以在"Blocked by"字段中引用真实的 issue 标识符。

<issue-template>
## Parent

对 issue tracker 上父级 issue 的引用（如果源材料是一个现有的 issue，否则省略此 section）。

## 要构建什么

此 vertical slice 的简洁描述。描述端到端行为，而不是逐层实现。

避免特定的文件路径或代码片段——它们很快会过时。例外：如果一个 prototype 产生了一个比散文更能精确编码决策的 snippet （state machine、 reducer、 schema、 type shape），在此处内联它并简要说明它来自 prototype。修剪到决策丰富的部分——不是一个可工作的 demo，只是重要的部分。

## 验收标准

- [ ] 标准 1
- [ ] 标准 2
- [ ] 标准 3

## 被谁阻塞

- 对阻塞 ticket 的引用（如果有）

如果没有阻塞者，则为"None - can start immediately"。

</issue-template>

不要关闭或修改任何父级 issue。
