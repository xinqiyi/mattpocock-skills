---
name: loop-me
description: Grill me about specs for the workflows I want to build, within this workspace.
disable-model-invocation: true
argument-hint: "要设计的工作流，或无参数去发现一个"
---

运行一个有状态的 `/grilling` session，其唯一输出是**workflow** specs。使用 grilling 纪律—— relentless，一次一个问题，每个问题附带推荐答案——针对下面的词汇和目标。随着 grilling 解决问题，创建、编辑和删除 specs。

## Loop 视角

**Loop** 是用户生活中的重复模式：他们的事业、他们的一周、他们的早晨、一个单一的重复活动。将生活想象为 loops 中的 loops 揭示了它的活动实际上有多么可预测——这就是使它们值得**委托**的原因。使用这个视角来找到值得指定的 loops，并提出用户尚未注意到的。

**Workflow** 是一个 loop 的 spec，使之成为现实。你在一个 loop 上运行 workflow —— loop 是它的运行实例化。 Workflows 存在于 `workflows/*.md` 中，是真相的来源。

## 词汇

一种共享语言，仅当一个 workflow 需要时才使用——绝不是清单。**不强制任何结构性内容**：一个 workflow 不需要 AI、不需要 checkpoint、也不需要 schedule，除非 grilling 表明它需要。

- **Trigger** ——什么触发每次运行：一个**事件**（新邮件、新 issue）或一个**计划**（每天早晨）。事件触发通常更高效。
- **Checkpoint** ——一个人工在环中的点，用户被要求验证或决定。有些 workflow 没有这个并自主运行；有些完全不使用 AI。
- **向右推** ——尽可能推迟 checkpoint。在涉及人类之前做最大量的工作，以便他们被一次性询问，晚些时候，带着准备好的一切。
- **Brief** —— checkpoint 呈现的内容：一个紧凑、决策就绪的摘要——生产了什么、为什么、以及指向资产本身的链接——永远不是原始输出。用户阅读 brief，而不是草稿。审查速度至关重要。

## 完成定义

当一个 workflow spec 完成时，一个实现者 agent 可以无需问任何问题就能构建它。在那之前继续 grilling；只要还有一个问题未解决，就没有完成。

## 工作区

- `workflows/*.md` ——每个 workflow 一个 spec。
- `NOTES.md` ——关于用户世界的原始笔记：他们使用的工具、他们处理的信息渠道、以及他们自己对这两者的术语。当它是空的或薄的，在指定任何东西之前先采访他们关于他们的世界。精炼模糊的术语为规范的术语，并将它们记录在这里。
