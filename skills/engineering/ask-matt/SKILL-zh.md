---
name: ask-matt
description: Ask which skill or flow fits your situation. A router over the user-invoked skills in this repo.
disable-model-invocation: true
---

# Ask Matt

你不记得每个 skill，所以来问。

**Flow** 是贯穿 skills 的一条路径。大多数路径沿着一个**主流程**走，两个**入口**汇入其中。其他都是独立的。

## 主流程： idea → ship

大多数工作走的路线。你有一个想法并想把它构建出来。

1. **`/grill-with-docs`** — 通过访谈来精炼想法。当你**有代码库**时从这里开始：它是有状态的，将学到的内容保留在 `CONTEXT.md` 和 ADR 中。（没有代码库？使用 `/grill-me`——参见独立内容。）
2. **分支——你能在对话中解决所有问题吗？** 如果一个问题需要可运行的答案（状态、业务逻辑、你必须看到的 UI），绕道通过一个 prototype，并由 **`/handoff`** 架起双向桥梁（参见跨 sessions）：
 - **`/handoff`** 出去，然后打开一个新的 session 针对该文件，
 - **`/prototype`** 用可丢弃的代码来回答问题，
 - **`/handoff`** 回你所学到的内容，并从原始想法线程中引用它。
3. **分支——这是多 session 构建吗？**
 - **是** → **`/to-prd`**（将线程转换为 PRD）→ **`/to-issues`**（将 PRD 拆分为可独立抓取的 issues）。因为 issues 是独立的，**在每个 issue 之间清理 context**：每个 issue 开启一个全新的 session，并通过传递 PRD 和要处理的单个 issue 来启动 **`/implement`**。
 - **否** → 直接在这里使用 **`/implement`**，在同一个 context window 中。

### Context 卫生

将步骤 1-3 保持在一个**不中断的 context window** 中——不要压缩或清理，直到 `/to-issues` 之后——这样 grilling、 PRD 和 issues 都建立在相同的思考基础上。然后每个 `/implement` 从 issue 开始重新开始。

这个的限制是 **[smart zone](https://www.aihero.dev/ai-coding-dictionary/smart-zone)**： model 仍能清晰推理的窗口（约 120k token，在最新 models 上）。如果一个 session 在 `/to-issues` 之前接近这个限制，不要用降级的方式硬推——使用 `/handoff` 并在新线程中继续。

## 入口

生成工作然后汇入主流程的起始情况。

- **Bugs 和请求堆积** → **`/triage`**。它通过 triage roles 推动 issues，生成 agent-ready 的 issues，稍后由 **`/implement`** 拾取。

 Triage 只适用于**不是你创建的** issues —— bug reports、传入的 feature requests、任何原始到达的。由 `/to-issues` 生成的 issues 已经是 agent-ready 的，所以**不要对它们进行 triage**。

## 代码库健康

不是功能工作——而是维护。

- **`/improve-codebase-architecture`** —— 每当你有空的时候运行，保持代码库让 agent 易于操作。它暴露深化的机会；选择一个_产生一个想法_，你可以将其带入主流程的 `/grill-with-docs`。

## 跨 Sessions

- **`/handoff`** —— 当一个线程已满或你需要分支出去时（例如进入一个 `/prototype` session），它将对话压缩为一个 markdown 文件。你不会在原地继续——你**打开一个新 session 并引用该文件**来携带 context。它是 context windows 之间的桥梁，双向通用。当你想要一个**全新的 session**但需要**保留当前对话**时使用它。
- **`/compact`**（内置）—— 停留在**同一对话**中，让前面的轮次被总结。在阶段之间**有意的中断**时使用它，当你不介意丢失逐字历史时。不要在阶段中途 compact —— agent 可能会迷失方向。`/handoff` 分叉；`/compact` 继续。

## 独立

完全脱离主流程。

- **`/grill-me`** —— 与 `/grill-with-docs` 相同的 relentless 访谈，但用于你**没有代码库**的时候。无状态：它不在本地保存任何东西，不构建 `CONTEXT.md`。用它来精炼任何不驻留在 repo 中的计划或设计。
- **`/teach`** —— 在多个 session 中学习一个概念，使用当前目录作为有状态的 workspace。
- **`/writing-great-skills`** —— 编写和编辑 skills 的参考。

## 前置条件

**`/setup-matt-pocock-skills`** —— 在你的第一个 engineering flow 之前运行，以配置其他 skills 所依赖的 issue tracker、 triage labels 和 doc 布局。自定义 issue trackers 也可以工作。
