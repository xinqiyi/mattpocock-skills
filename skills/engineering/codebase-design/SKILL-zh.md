---
name: codebase-design
description: Shared vocabulary for designing deep modules. Use when the user wants to design or improve a module's interface, find deepening opportunities, decide where a seam goes, make code more testable or AI-navigable, or when another skill needs the deep-module vocabulary.
---

# Codebase Design

设计**深层 modules**：通过小型 interface 暴露大量行为，放置在清晰的 seam 处，通过该 interface 可测试。无论代码正在被设计还是重构，都使用这种语言和这些原则。目标是调用者的 leverage、维护者的 locality，以及所有人的 testability。

## 词汇表

准确使用这些术语——不要替换为"component"、"service"、"API"或"boundary"。一致的语言是整个关键。

**Module** —— 任何有 interface 和 implementation 的东西。有意地规模无关：一个 function、 class、 package 或跨层 slice。_避免_： unit、 component、 service。

**Interface** —— 调用者正确使用 module 所需知道的一切： type signature，也包括 invariants、 ordering constraints、 error modes、 required configuration 和 performance characteristics。_避免_： API、 signature （太窄——它们仅指 type-level 的表面）。

**Implementation** —— module 内部的东西，它的代码体。与 **Adapter** 的区别：一个东西可以是小的 adapter 但大的 implementation （一个 Postgres repo），或者大的 adapter 但小的 implementation （一个 in-memory fake）。当主题是 seam 时说"adapter"；否则说"implementation"。

**Depth** —— interface 上的 leverage：调用者（或测试）每单位需要学习的 interface 可以执行的行为量。当大量行为位于小型 interface 后面时， module 是**深层的**；当 interface 几乎和 implementation 一样复杂时，它是**浅层的**。

**Seam** _(Michael Feathers)_ —— 一个可以在不编辑该位置的情况下改变行为的地方； module 的 interface 所在的*位置*。把 seam 放在哪里本身就是一个设计决策，与它后面放什么不同。_避免_： boundary （与 DDD 的 bounded context 过载）。

**Adapter** —— 在 seam 处满足 interface 的具体内容。描述*角色*（它填充什么插槽），而不是实质（里面有什么）。

**Leverage** —— 调用者从 depth 中得到的东西：每单位他们学习的 interface 获得更多能力。一个 implementation 在 N 个调用点和 M 个测试中偿还。

**Locality** —— 维护者从 depth 中得到的东西：变更、 bug、知识和验证集中在一个地方，而不是分散在调用者之间。修复一次，处处修复。

## Deep vs Shallow

**深层 module** = 小型 interface + 大量 implementation：

```
┌─────────────────────┐
│ 小型 Interface │ ← 少数方法，简单参数
├─────────────────────┤
│ │
│ 深层 Implementation │ ← 复杂逻辑隐藏在内
│ │
└─────────────────────┘
```

**浅层 module** = 大型 interface + 少量 implementation （避免）：

```
┌─────────────────────────────────┐
│ 大型 Interface │ ← 许多方法，复杂参数
├─────────────────────────────────┤
│ 薄 Implementation │ ← 仅仅是透传
└─────────────────────────────────┘
```

设计 interface 时，问：

- 我能减少方法的数量吗？
- 我能简化参数吗？
- 我能在内部隐藏更多复杂性吗？

## 原则

- **Depth 是 interface 的属性，而不是 implementation 的属性。** 一个深层 module 可以在内部由小型、可 mock、可替换的部件组成——它们只是不是 interface 的一部分。一个 module 可以有**内部 seams**（对其 implementation 私有，由其自己的测试使用）以及其 interface 处的**外部 seam**。
- **删除测试。** 想象删除这个 module。如果复杂性消失了，它就是一个透传。如果复杂性在 N 个调用者之间重新出现，它就在发挥价值。
- **Interface 就是测试面。** 调用者和测试跨越同一个 seam。如果你想*越过* interface 进行测试， module 可能形状不对。
- **一个 adapter 意味着假设性的 seam。两个 adapter 意味着真正的 seam。** 除非有东西确实在 seam 上变化，否则不要引入 seam。

## 为 Testability 设计

好的 interface 使测试自然：

1. **接受依赖，不要创建它们。**

 ```typescript
 // 可测试
 function processOrder(order, paymentGateway) {}

 // 难以测试
 function processOrder(order) {
 const gateway = new StripeGateway();
 }
 ```

2. **返回结果，不要产生副作用。**

 ```typescript
 // 可测试
 function calculateDiscount(cart): Discount {}

 // 难以测试
 function applyDiscount(cart): void {
 cart.total -= discount;
 }
 ```

3. **小的表面积。** 更少的方法 = 更少的测试需求。更少的参数 = 更简单的测试设置。

## 关系

- 一个 **Module** 恰好有一个 **Interface**（它呈现给调用者和测试的表面）。
- **Depth** 是一个 **Module** 的属性，相对于其 **Interface** 衡量。
- **Seam** 是 **Module** 的 **Interface** 所在的地方。
- **Adapter** 位于 **Seam** 处并满足 **Interface**。
- **Depth** 为调用者产生 **Leverage**，为维护者产生 **Locality**。

## 被拒绝的框架

- **Depth 作为 implementation 行数与 interface 行数的比率**（Ousterhout）：奖励填充 implementation。我们使用 depth-as-leverage 代替。
- **"Interface" 作为 TypeScript 的 `interface` 关键字或 class 的 public 方法**：太窄了——这里的 interface 包括调用者必须知道的每一个事实。
- **"Boundary"**：与 DDD 的 bounded context 过载。说 **seam** 或 **interface**。

## 深入阅读

- **在给定依赖的情况下深化一个 cluster**——参见 [DEEPENING-zh.md](DEEPENING-zh.md)：依赖分类、 seam 纪律和 replace-don't-layer 测试。
- **探索替代 interfaces**——参见 [DESIGN-IT-TWICE-zh.md](DESIGN-IT-TWICE-zh.md)：启动并行 sub-agents 以几种截然不同的方式设计 interface，然后在 depth、 locality 和 seam placement 上进行比较。
