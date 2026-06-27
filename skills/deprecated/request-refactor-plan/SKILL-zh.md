---
name: request-refactor-plan
description: Create a detailed refactor plan with tiny commits via user interview, then file it as a GitHub issue. Use when user wants to plan a refactor, create a refactoring RFC, or break a refactor into safe incremental steps.
---

当用户想要创建一个重构请求时，将调用此 skill。你应该按照以下步骤进行。如果你认为某些步骤不必要，可以跳过。

1. 向用户索要他们想解决的问题的长而详细的描述，以及任何潜在的解决方案想法。

2. 探索 repo 以验证他们的断言并了解代码库的当前状态。

3. 询问他们是否考虑过其他选项，并向他们呈现其他选项。

4. 就实现细节对用户进行访谈。要极其详细和彻底。

5. 敲定实现的确切范围。确定你计划改变什么和计划不改变什么。

6. 在代码库中检查该区域的测试覆盖率。如果测试覆盖率不足，询问用户他们的测试计划。

7. 将实现分解为微小 commits 的计划。记住 Martin Fowler 的建议"使每个重构步骤尽可能小，以便你总是能看到程序在工作。"

8. 创建带有重构计划的 GitHub issue。对 issue 描述使用以下模板：

<refactor-plan-template>

## 问题陈述

开发者面临的问题，从开发者的角度出发。

## 解决方案

问题的解决方案，从开发者的角度出发。

## Commits

一个**长**而详细的实现计划。用通俗英语编写计划，将实现分解为尽可能小的 commits。每个 commit 应使代码库处于工作状态。

## 决策文档

已做出的实现决策列表。可以包括：

- 将要构建/修改的 modules
- 将要修改的 modules 的 interfaces
- 来自开发者的技术澄清
- 架构决策
- Schema 变更
- API 契约
- 特定的交互

**不要**包括特定的文件路径或代码片段。它们可能很快过时。

## 测试决策

已做出的测试决策列表。包括：

- 什么构成好测试的描述（只测试外部行为，不是 implementation 细节）
- 哪些 modules 将被测试
- 测试的 prior art （即代码库中类似类型的测试）

## Out of Scope

此重构中排除的内容描述。

## 进一步说明（可选）

关于重构的任何进一步说明。

</refactor-plan-template>
