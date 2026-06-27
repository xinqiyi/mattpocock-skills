---
name: decision-mapping
description: Turn a loose idea into a sequenced map of investigation tickets, then drive them to resolution one at a time.
disable-model-invocation: true
---

当一个松散的想法需要超过一个 agent session 才能转化为计划时，调用此 skill。它在 markdown 文件中创建一个有状态的决策图，并驱动用户通过一系列 tickets 来解决未解决的问题——可能需要 prototyping、 research 或 discussion。

## 决策图

决策图是一个单一的紧凑 Markdown 文件，每个规划工作一个，与项目一起进行 git 追踪。它是规范 artifact ——**整个图作为 context 加载到每个 session 中**，因此它必须保持紧凑。

在 tickets 期间创建的资产应从图中链接，而不是在其中重复。

### 结构

编号条目（"tickets"），每个都有自己的 section，以其编号为键：

```markdown
## #1: 关系型还是非关系型数据库？

Blocked by: #<ticket-number>, #<ticket-number>
Type: Research | Prototype | Grilling

### 问题

<question-here>

### 答案

<answer-here>
```

每个 ticket 必须适配到一个 100K token 的 agent session 中。

## Ticket 类型

有三种类型的 tickets：

- **Research**：阅读文档、第三方 API 或本地资源如知识库。创建一个 markdown 摘要作为资产。当需要当前工作目录之外的知识时使用。
- **Prototype**：编写 UI 或逻辑代码以测试假设，或探索设计空间。使用 /prototype skill。创建一个 prototype 作为资产。当"它应该看起来如何"或"它应该表现如何"是关键问题时使用。
- **Grilling**：与 agent 对话。使用 /grilling 和 /domain-modeling skills。一次问一个问题。默认情况。

## 战争的迷雾

该图在前沿之外是_故意_不完整的。你的工作是调查前沿，并按顺序解决 tickets 以推动前沿前进。一次一个节点地推开战争的迷雾。

在某些时候，战争的迷雾应该已经被推回到足够远，使得通往终点的路径变得清晰。到那时，不再需要更多的 tickets，决策图可以被视为"完成"。

## 调用

有两种方式可以调用此 skill：**bootstrap** 和 **resume**。

### Bootstrap

用户用一个松散的想法调用。

1. 运行 /grilling + /domain-modeling session 以暴露未解决的决策。一次问一个问题。
2. 写入一个新的决策图——大部分是迷雾，前沿已识别，可以简单决定的 entry 内联解决。
3. 停止。地图构建是一个 session 的工作；不要同时解决 tickets。

### Resume

用户通过现有图的路径和一个 ticket 编号调用。

1. 将**整个图**加载为 context。
2. 运行 session 以解决 ticket，根据需要调用 skills。如有疑问，使用 `/grilling` 和 `/domain-modeling`。
3. 在 ticket 的 body 中记录 session 解决的内容。
4. 添加新发现的 tickets （带有正确的 `blocked_by` 边）。
5. 停止。

如果做出的决定使图的其他部分无效，更新或删除那些节点。

## 并行性

用户可能选择并行处理 tickets，因此期待其他 agents 对图做出更改。

## 跳过决策图

很多时候，初始 grilling 将导致没有战争的迷雾。没有未解决的 tickets。除了实现，没什么可做的。

在这些情况下，你应该提供用户跳过决策图的机会——因为只有在需要多 session 决策时才需要决策图。

如果他们跳过，你应该建议直接实现或使用 `/to-prd` 来安排多 session 实现。
