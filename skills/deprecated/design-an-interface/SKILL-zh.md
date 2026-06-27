---
name: design-an-interface
description: Generate multiple radically different interface designs for a module using parallel sub-agents. Use when user wants to design an API, explore interface options, compare module shapes, or mentions "design it twice".
---

# Design an Interface

基于《 A Philosophy of Software Design 》中的"Design It Twice"：你的第一个想法不太可能是最好的。生成多种截然不同的设计，然后进行比较。

## 工作流

### 1. 收集需求

在设计之前，了解：

- [ ] 这个 module 解决什么问题？
- [ ] 调用者是谁？（其他 modules、外部用户、 tests）
- [ ] 关键操作是什么？
- [ ] 有任何约束吗？（性能、兼容性、现有模式）
- [ ] 哪些应该在内部隐藏 vs 暴露？

问："这个 module 需要做什么？谁会使用它？"

### 2. 生成设计（并行 Sub-Agents）

使用 Task tool 同时启动 3+ 个 sub-agents。每个必须产生一种**截然不同的**方法。

```
每个 sub-agent 的 prompt 模板：

为以下内容设计 interface：[module description]

需求：[collected requirements]

此设计的约束：[为每个 agent 分配不同的约束]
- Agent 1："最小化方法数量——目标最多 1-3 个方法"
- Agent 2："最大化灵活性——支持多种用例"
- Agent 3："针对最常见情况进行优化"
- Agent 4："从[特定范式/库]中获取灵感"

输出格式：
1. Interface signature （types/methods）
2. 使用示例（调用者如何使用它）
3. 此设计在内部隐藏了什么
4. 此方法的权衡
```

### 3. 展示设计

展示每个设计，附带：

1. **Interface signature** —— types、 methods、 params
2. **使用示例** —— 调用者实际如何使用
3. **它隐藏了什么** —— 保持在内部的复杂性

按顺序展示设计，以便用户在比较之前吸收每种方法。

### 4. 比较设计

展示所有设计后，在以下方面进行比较：

- **Interface 简洁性**：更少的方法、更简单的 params
- **通用 vs 专用**：灵活性 vs 专注度
- **实现效率**：形态是否允许高效的内部结构？
- **Depth**：小型 interface 隐藏大量复杂性（好） vs 大型 interface 薄 implementation （坏）
- **正确使用的容易度** vs **误用的容易度**

用散文讨论权衡，而不是表格。突出设计差异最大的地方。

### 5. 综合

通常最好的设计结合了多个选项的洞察。问：

- "哪个设计最适合你的主要用例？"
- "其他设计中有值得纳入的元素吗？"

## 评估标准

来自《 A Philosophy of Software Design 》：

**Interface 简洁性**：更少的方法、更简单的 params = 更容易学习和正确使用。

**通用性**：无需更改即可处理未来用例。但注意过度泛化。

**实现效率**： Interface 形态是否允许高效的实现？或者强制使用笨拙的内部结构？

**Depth**：小型 interface 隐藏大量复杂性 = 深层 module （好）。大型 interface 薄 implementation = 浅层 module （避免）。

## 反模式

- 不要让 sub-agents 生成相似的设计——强制根本性差异
- 不要跳过比较——价值在于对比
- 不要实现——这纯粹关于 interface 形态
- 不要根据实现工作量评估
