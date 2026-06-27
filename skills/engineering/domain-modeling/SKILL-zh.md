---
name: domain-modeling
description: Build and sharpen a project's domain model. Use when the user wants to pin down domain terminology or a ubiquitous language, record an architectural decision, or when another skill needs to maintain the domain model.
---

# Domain Modeling

在设计过程中主动构建和精炼项目的 domain model。这是*主动的*纪律——挑战术语、发明 edge-case 场景、在术语和决策凝固的那一刻就写下来。（仅仅是*阅读* `CONTEXT.md` 获取词汇不是这个 skill ——那是任何 skill 都可以做的一行习惯。这个 skill 用于你在**改变**模型，而不仅仅是消费它。）

## 文件结构

大多数 repo 有一个单一的 context：

```
/
├── CONTEXT.md
├── docs/
│ └── adr/
│ ├── 0001-event-sourced-orders.md
│ └── 0002-postgres-for-write-model.md
└── src/
```

如果根目录存在 `CONTEXT-MAP.md`， repo 有多个 contexts。该 map 指向每个 context 所在的位置：

```
/
├── CONTEXT-MAP.md
├── docs/
│ └── adr/ ← 系统级决策
├── src/
│ ├── ordering/
│ │ ├── CONTEXT.md
│ │ └── docs/adr/ ← context 特定决策
│ └── billing/
│ ├── CONTEXT.md
│ └── docs/adr/
```

懒加载创建文件——只有当你有什么要写的时候才创建。如果不存在 `CONTEXT.md`，在第一个术语被解析时创建。如果不存在 `docs/adr/`，在第一个 ADR 需要时创建。

## 在 Session 期间

### 对照 Glossary 进行挑战

当用户使用的术语与 `CONTEXT.md` 中的现有语言冲突时，立即指出来。"你的 glossary 将 'cancellation' 定义为 X，但你似乎说的是 Y ——到底是哪个？"

### 精炼模糊的语言

当用户使用模糊或过载的术语时，提出一个精确的规范术语。"你在说 'account'——你是指 Customer 还是 User？它们是不同的东西。"

### 讨论具体场景

在讨论领域关系时，用具体的场景进行压力测试。发明探索 edge cases 的场景，迫使用户在概念之间的边界上精确。

### 与代码交叉引用

当用户陈述某事物如何工作时，检查代码是否同意。如果发现矛盾，指出来："你的代码取消了整个 Orders，但你刚才说部分取消是可能的——哪个是对的？"

### 内联更新 CONTEXT.md

当一个术语被解析时，立即更新 `CONTEXT.md`。不要积攒它们——在发生时捕获。使用 [CONTEXT-FORMAT-zh.md](./CONTEXT-FORMAT-zh.md) 中的格式。

`CONTEXT.md` 应该完全不含 implementation 细节。不要将 `CONTEXT.md` 视为 spec、草稿本或 implementation 决策的存储库。它是一个 glossary，仅此而已。

### 审慎提供 ADR

仅当以下三者都为真时才提议创建 ADR：

1. **难以逆转**——事后改变想法的成本是显著的
2. **没有 context 会令人惊讶**——未来的读者会想知道"他们为什么这样做？"
3. **是真正权衡的结果**——有真正的替代方案，你出于特定原因选择了其中一个

如果三者中缺少任何一个，跳过 ADR。使用 [ADR-FORMAT-zh.md](./ADR-FORMAT-zh.md) 中的格式。
