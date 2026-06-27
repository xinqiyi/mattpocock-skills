---
name: to-prd
description: Turn the current conversation into a PRD and publish it to the project issue tracker — no interview, just synthesis of what you've already discussed.
disable-model-invocation: true
---

此 skill 获取当前对话 context 和代码库理解，并生成一个 PRD。**不要**访谈用户——只需综合你已经知道的内容。

Issue tracker 和 triage label 词汇应该已经提供给你——如果没有，运行 `/setup-matt-pocock-skills`。

## 流程

1. 探索 repo 以了解代码库的当前状态，如果你还没有这样做。在整个 PRD 中使用项目的 domain glossary 词汇，并尊重你正在接触区域的任何 ADR。

2. 草拟你将要在其上测试功能的 seams。现有 seams 应优先于新 seams。使用尽可能高的 seam。如果需要新 seams，在你能达到的最高点提出它们。代码库中的 seams 越少越好——理想数量是一个。

 与用户确认这些 seams 符合他们的期望。

3. 使用下面的模板编写 PRD，然后将其发布到项目 issue tracker。应用 `ready-for-agent` triage label ——无需额外 triage。

<prd-template>

## 问题陈述

用户面临的问题，从用户的角度出发。

## 解决方案

问题的解决方案，从用户的角度出发。

## 用户故事

一个**长**的、编号的用户故事列表。每个用户故事的格式应为：

1. 作为 <actor>，我想要 <feature>，以便 <benefit>

<user-story-example>
1. 作为移动银行客户，我想查看我的账户余额，以便我能做出更明智的消费决策
</user-story-example>

这个用户故事列表应该非常全面且覆盖功能的所有方面。

## 实现决策

已做出的实现决策列表。可以包括：

- 将要构建/修改的 modules
- 将要修改的 modules 的 interfaces
- 来自开发者的技术澄清
- 架构决策
- Schema 变更
- API 契约
- 特定的交互

**不要**包括特定的文件路径或代码片段。它们可能很快过时。

例外：如果一个 prototype 产生了一个比散文更能精确编码决策的 snippet （state machine、 reducer、 schema、 type shape），在相关的决策中内联它并简要说明它来自 prototype。修剪到决策丰富的部分——不是一个可工作的 demo，只是重要的部分。

## 测试决策

已做出的测试决策列表。包括：

- 什么构成好测试的描述（只测试外部行为，不是 implementation 细节）
- 哪些 modules 将被测试
- 测试的 prior art （即代码库中类似类型的测试）

## Out of Scope

此 PRD 中排除的内容描述。

## 进一步说明

关于该功能的任何进一步说明。

</prd-template>
