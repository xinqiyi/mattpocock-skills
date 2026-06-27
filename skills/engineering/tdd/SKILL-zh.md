---
name: tdd
description: Test-driven development. Use when the user wants to build features or fix bugs test-first, mentions "red-green-refactor", or wants integration tests.
---

# Test-Driven Development

## 理念

**核心原则**：测试应通过公共 interfaces 验证行为，而不是 implementation 细节。代码可以完全更改；测试不应该。

**好测试**是 integration-style 的：它们通过公共 APIs 执行真实的代码路径。它们描述系统_做什么_，而不是_怎么做_。一个好的测试读起来像一份 specification ——"user can checkout with valid cart" 精确地告诉你存在什么能力。这些测试能在重构中存活，因为它们不关心内部结构。

**坏测试**与 implementation 耦合。它们 mock 内部协作者、测试私有方法，或通过外部手段验证（例如直接查询数据库而不是使用 interface）。警告信号：你的测试在你重构时失败，但行为没有改变。如果你重命名一个内部函数且测试失败，那些测试是在测试 implementation，而不是行为。

参见 [tests-zh.md](tests-zh.md) 获取示例，[mocking-zh.md](mocking-zh.md) 获取 mocking 指南。

## 反模式： Horizontal Slices

**不要先写所有测试，然后所有 implementation。** 这是"水平切片"——将 RED 视为"写所有测试"， GREEN 视为"写所有代码。"

这会产生**糟糕的测试**：

- 批量编写的测试测试的是_想象中_的行为，而不是_实际的_行为
- 你最终测试的是事物的_形态_（数据结构、 function signatures）而不是面向用户的行为
- 测试对真实变更变得不敏感——当行为被破坏时它们通过，当行为正常时它们失败
- 你在理解 implementation 之前就提交了测试结构，超越了你的前照灯

**正确方法**：通过 tracer bullets 进行 vertical slices。一个测试 → 一个 implementation → 重复。每个测试响应你从上一个 cycle 中学到的东西。因为你刚刚写了代码，你确切知道什么行为重要以及如何验证它。

```
错误（水平）：
 RED: test1, test2, test3, test4, test5
 GREEN: impl1, impl2, impl3, impl4, impl5

正确（垂直）：
 RED → GREEN: test1→ impl1
 RED → GREEN: test2→ impl2
 RED → GREEN: test3→ impl3
 ...
```

## 工作流

### 1. 计划

在探索代码库时，阅读 `CONTEXT.md`（如果存在），以便测试名称和 interface 词汇与项目的领域语言匹配，并尊重你正在接触区域的 ADR。

在编写任何代码之前：

- [ ] 与用户确认需要什么 interface 变更
- [ ] 与用户确认要测试哪些行为（优先排序）
- [ ] 识别深层 modules 的机会（小型 interface，深层 implementation）——运行 `/codebase-design` skill 获取词汇和 testability checks
- [ ] 列出要测试的行为（不是实现步骤）
- [ ] 获得用户对计划的批准

问："公共 interface 应该是什么样的？哪些行为最重要需要测试？"

**你无法测试所有东西。** 与用户确认具体哪些行为最重要。将测试工作集中在关键路径和复杂逻辑上，而不是每个可能的 edge case。

### 2. Tracer Bullet

写一个测试确认系统的一个方面：

```
RED: 为第一个行为写测试 → 测试失败
GREEN: 写最少的代码通过 → 测试通过
```

这是你的 tracer bullet ——证明路径端到端有效。

### 3. 增量循环

对于每个剩余的行为：

```
RED: 写下个测试 → 失败
GREEN: 最少的代码通过 → 通过
```

规则：

- 一次一个测试
- 只有足够通过当前测试的代码
- 不要预期未来测试
- 保持测试聚焦于可观察行为

### 4. 重构

所有测试通过后，寻找[重构候选对象](refactoring-zh.md)：

- [ ] 提取重复
- [ ] 深化 modules （将复杂性移到简单的 interfaces 后面）
- [ ] 在自然的地方应用 SOLID 原则
- [ ] 考虑新代码揭示了关于现有代码的什么
- [ ] 在每个重构步骤后运行测试

**在 RED 状态下绝不重构。** 先达到 GREEN。

## 每个循环的清单

```
[ ] 测试描述行为，而不是 implementation
[ ] 测试仅使用公共 interface
[ ] 测试在内部重构中能存活
[ ] 代码对此测试来说是最少的
[ ] 未添加推测性功能
```
