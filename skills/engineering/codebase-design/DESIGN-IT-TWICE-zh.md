# Design It Twice

当用户想要为选定的深化候选对象探索替代 interfaces 时，使用这个并行 sub-agent 模式。基于"Design It Twice"（Ousterhout）——你的第一个想法不太可能是最好的。

使用 [SKILL-zh.md](SKILL-zh.md) 中的词汇——**module**、**interface**、**seam**、**adapter**、**leverage**。

## 流程

### 1. 界定问题空间

在启动 sub-agents 之前，为选定的候选对象编写一个面向用户的问题空间解释：

- 任何新 interface 需要满足的约束条件
- 它将依赖的依赖关系，以及它们属于哪个类别（参见 [DEEPENING-zh.md](DEEPENING-zh.md)）
- 一个粗略的说明性代码草图来具体化约束条件——不是提案，只是让约束条件具体化的一种方式

向用户展示这些，然后立即进入步骤 2。用户在 sub-agents 并行工作时阅读和思考。

### 2. 启动 Sub-agents

使用 Agent tool 并行启动 3 个以上的 sub-agents。每个必须为深化后的 module 生成一个**截然不同的** interface。

为每个 sub-agent 提供一个单独的技术简报（文件路径、耦合细节、[DEEPENING-zh.md](DEEPENING-zh.md) 中的依赖类别、 seam 后面是什么）。简报独立于步骤 1 中面向用户的问题空间解释。给每个 agent 一个不同的设计约束：

- Agent 1："最小化 interface ——目标是最多 1-3 个入口点。最大化每个入口点的 leverage。"
- Agent 2："最大化灵活性——支持许多用例和扩展。"
- Agent 3："针对最常见的调用者进行优化——使默认情况变得简单。"
- Agent 4（如果适用）："围绕跨 seam 依赖的 ports & adapters 进行设计。"

在简报中包含 [SKILL-zh.md](SKILL-zh.md) 词汇和 CONTEXT.md 词汇，以便每个 sub-agent 与架构语言和项目的领域语言一致地进行命名。

每个 sub-agent 输出：

1. Interface （types、 methods、 params ——加上 invariants、 ordering、 error modes）
2. 使用示例，展示调用者如何使用它
3. Implementation 在 seam 后面隐藏了什么
4. 依赖策略和 adapters （参见 [DEEPENING-zh.md](DEEPENING-zh.md)）
5. 权衡——哪里 leverage 高，哪里薄弱

### 3. 展示和比较

按顺序展示设计，以便用户可以逐个吸收，然后以散文形式进行比较。通过 **depth**（interface 上的 leverage）、**locality**（变更集中的地方）和 **seam placement** 进行对比。

比较之后，给出你自己的推荐：你认为哪个设计最强以及为什么。如果不同设计中的元素可以很好地组合，提出一个 hybrid。要有见解——用户想要一个有力的观点，而不是一个菜单。
