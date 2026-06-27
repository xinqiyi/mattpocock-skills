# Glossary — Building Great Skills

构成优秀 skill 的领域模型。一个 skill 的存在是为了从随机系统中 wrangle 确定性；根本美德是 **Predictability**，下面的每个术语都是其上的杠杆。这是 [`writing-great-skills`](SKILL-zh.md) 的已披露 reference。

术语按轴分组：**Invocation**（skill 如何被访问）、**Information Hierarchy**（其内容如何排列）、**Steering**（agent 的运行行为如何塑造）和 **Pruning**（如何保持精简）。每个 **failure mode** 位于治愈它的杠杆旁边，标记为 _failure mode_。

任何定义中的**粗体术语**本身在这个 glossary 中定义；通过其标题查找。

## Predictability

Skill 使 agent 在每次运行时以相同_方式_行为的程度——相同的过程，而不是相同的输出（一个 brainstorming skill 应_可预测地_发散；其 tokens 变化，行为不变）。其他每个术语服务的根本美德——成本和可维护性只是它的症状，而不是竞争对手。

_避免_： consistency, reliability, robustness, output-determinism

## Invocation

Skill 如何被访问——以及你为选择支付的两种负载。

### Model-Invoked

保留其 **description** 字段的 skill，因此 agent 可以看到它并自主触发它——而人类仍然可以输入其名称，因此 model-invocation 始终_包含_用户可达。没有仅 model 的状态： description 只_添加_ agent 发现，从不移除人类的。在每个回合支付永久的 **context load** 以换取这种可发现性。可被其他 skills 访问，因为使其 agent 可发现的 description 也使其可调用。一个 model-invoked 的 skill，其内容全部是 **reference**，也是共享 reference 的一个家：其他 skill 可以调用它，因此多个 skills 所需的 reference 生活在一个地方。仅在 agent 必须自己访问该 skill 时选择 model-invocation；如果它从不手动调用以外的方式触发，去掉 description 且不支付 context load。

_避免_： ability, tool, capability

### User-Invoked

**description** 被移除的 skill ——对 agent 不可见，仅能通过人类输入其名称来访问（仅用户，而 **model-invoked** 是用户和 agent）。以零 **context load** 换取 agent-可发现性。因为它没有 description，除了人类没有其他东西可以访问它：没有其他 skill 可以触发它。

_避免_： procedure, workflow, command

### Description

Skill 的机器可读触发器，以及 **model-invoked** skill 被迫始终加载的唯一 **context pointer**。它的存在本身就是 invocation 轴：保留它， skill 是 model-invoked （并可被其他 skills 访问）；删除它， skill 是 **user-invoked**，仅能由人类访问。 Model-invoked skill 的 **context load** 的来源。

_避免_： frontmatter, summary

### Context Pointer

保存在 agent context 中的引用，命名一些上下文外的材料并编码了访问它的条件。**Description** 是顶级的 context pointer （context window → skill）；指向已披露文件的指针是同一个对象在下一级。其措辞，而不是其目标，决定了 agent _何时_访问——以及_多可靠_。一个必须访问的目标放在一个措辞微弱的指针后面是一个方差 bug：先修复措辞，只有当锐化失败时才内联该材料。

_避免_： link, reference, import

### Context Load

**Model-invoked** skill 对 agent 的 context window 施加的成本——其 **description**，始终加载，花费 tokens 和注意力。**User-invoked** skills 通过没有 description 所避免的，以及拆分出更多 model-invoked skills 的制动器。

_避免_： token cost, context bloat

### Cognitive Load

**User-invoked** skill 施加给人类的成本——他们必须记在脑海里的东西：哪些 skills 存在以及何时使用每个（人类是指数）。**Model-invocation** 通过 agent 可发现性所移除的，以及拆分出更多 user-invoked skills 的制动器。不是要最小化的成本：它是人类能动性的价格，是一些 skills 保持 user-invoked 的原因。在人类判断重要的地方花费它；在不需要的地方移除它。

_避免_： human index, burden, overhead

### Router Skill

一个 **user-invoked** skill，其工作是指向你的其他 user-invoked skills ——命名每个以及何时使用它——这样人类只需要记住一个 skill 而不是许多。它只能提示，绝不能触发它们： user-invoked skills 没有 **description**，所以除了人类没有东西可以访问它们。当 user-invoked skills 增长时，**cognitive load** 的解药。

_避免_： dispatcher, menu, registry, index, router procedure

### Granularity

你多精细地划分 skills。更精细的划分花费两种负载之一：更多 **model-invoked** skills 花费 **context load**（更多的 descriptions 挤在窗口中并争夺注意力）；更多 **user-invoked** skills 花费 **cognitive load**（人类需要记住和访问的更多）。两种划分指导分割。按 **invocation**，当你有一个不同的 **leading word** 来触发它时拆分出一个 model-invoked skill ——你在 prompts 中实际使用的触发词。按 **sequence**，当一个步骤的 **post-completion steps** 需要隐藏时拆分 **steps** 的运行，因为将其隔离在自己的 context 中清除了随后的事情。当心反之亦然：合并 sequences 暴露每个步骤的 post-completion steps 到后面，招致 premature completion。

_避免_： chunking, modularity

## Information Hierarchy

Skill 的内容如何排列，以及每块在梯子上的深度。

### Information Hierarchy

Skill 的内容按 agent 需要它的紧急程度排序——一个单一的梯子，由两个切分产生：在文件内还是在指针后面，以及 step 还是 reference。梯级：

- **Steps** ——在文件内，主要
- **Reference**，在文件内——次要
- **Reference**，已披露——在 **context pointer** 后面

没有 **steps** 的 skill 只使用底部两个梯级——通常是一个合法的扁平同级集（例如审查的所有规则在一个梯级上），这是很好的安排，不是异味。层级独立于 invocation：一个 skill 可以是 model- 或 user-invoked，无论它是全 steps、全 reference 还是两者兼有。当一个 skill 有 steps 时，应被披露的在文件内 reference 会埋没它们，并使关注于它们变成抛硬币——不仅是可读性问题，还是一个方差杠杆。保持梯子的顶层可读；将你能做到的任何东西推下它。

_避免_： structure, organization, layout

### Steps

Agent 执行的有序动作——当一个 skill 有它们时，其内容的主要层，以及在 SKILL.md 中赢得其位置的部分。不是每个 skill 都有 steps：一个 skill 可以是全 steps （`tdd`）、全 **reference**（一个 review）、或两者兼有，独立于 invocation。每个步骤以一个 **completion criterion** 结束，清晰或模糊。

_避免_： workflow, instructions, choreography

### Reference

Agent 按需参考的材料——定义、事实、参数、示例、条件指令。当一个 skill 有 **steps** 时，它是次要于 steps；当一个 skill 没有 steps 时，它是全部内容；或者它完全存在于任何 skill 之外——参见 **External Reference**。通过 **context pointers** 访问，并且是 **progressive disclosure** 的主要候选。

_避免_： supporting material, docs, background

### External Reference

存在于 skill 系统之外的 **Reference**——一个普通文件，没有 **description**，没有 **steps**，不可调用——任何 skill 都可以指向。共享 reference 的家，它不需要自己触发，也是两个 **user-invoked** skills 可以使用的唯一共享家，因为两者都没有 description，因此都不能触发另一个。

_避免_： doc, resource, knowledge base

### Progressive Disclosure

将 **reference** 沿梯子向下移动——从 SKILL.md 中出来到 **context pointer** 后面——以便顶层保持可读。主要不是 token 优化；它是 **information hierarchy** 如何被保护。受 **branching** 授权：披露只有某些 branches 需要的内容，内联每条路径都需要的内容，如果一个指针在必须拥有的材料上触发不可靠，锐化其措辞，只有在那失败时才将它拉回内联。

_避免_： lazy loading, chunking

### Co-location

将 agent 一次需要的材料保持在一个地方——概念的定义、规则和注意事项在单个标题下，而不是分散在文件中——这样阅读一个部分会带着相邻部分一起。**Information Hierarchy** 在文件内的伴生：层级决定一段放在_多深_； co-location 决定一旦放在那里_什么与它并列_。对于 **reference** 主体的正确格式没有公式；测试是一个 skill 应读起来像为 agent 编写的文档，而分组的材料读起来就是那样，分散的材料不是。与 **Duplication** 不同：那是在两个地方重复一个含义，而分散是将一个含义拆散到许多地方。

_避免_： grouping, clustering, cohesion

### Sprawl

_Failure mode._ 一个 skill 就是太长—— SKILL.md 中的行太多——无论它们是否陈旧或重复。即使全活跃、全唯一的 skill 也会 sprawl。它损害可读性（agent 在行动之前要涉足更多内容，注意力在多余内容上变薄）、可维护性（每多一行都是需要保持 **relevant** 的更多行）和 tokens。解药是 **information hierarchy**：将 **reference** 推到 **context pointers** 后面，通过 **branch** 或 sequence 拆分，以便每条路径只携带它需要的内容。与 **sediment**（来自陈旧积累的长度）和 **duplication**（来自重复含义的长度）不同—— sprawl 是长度本身，无论其原因是什么。

_避免_： bloat, length, size, verbosity

## Steering

将 agent 的运行行为塑向 **Predictability** 的杠杆。

### Branch

Skill 可以被调用的不同方式—— skill 处理的一个 case ——因此不同的运行采取不同的路径穿过它。一个有很多 steps 的 skill 可能携带许多 branches；一个线性的没有。

_避免_： path, case, fork

### Leading Word

一个紧凑的概念——也称为 _Leitwort_——已经存在于 model 的 pretraining 中， agent 在运行 skill 时用它来思考。它通过调用 model 已有的先验知识，以尽可能少的 token 编码一个行为原则（例如 _lesson_、_proximal zone of development_、_fog of war_、_tracer bullets_）。作为一个 token 重复，从不是一个句子，它在整个 skill 中累积一个分布式定义并锚定整个行为区域。如果你清晰定义它，自己造词也可以工作，但一个编造的词不招募任何先验——你用定义 tokens 支付预训练词免费提供的东西。首先找一个已有的词。

Leading word 两次服务于 **predictability**。在 body 中它锚定 _execution_——每次概念出现时 agent 采取相同的行为，而在 flat reference 内部，它集中注意力于一类要查找的东西，每次运行时招募正确的检查。在 **description** 中它锚定 _invocation_——而且不仅在 skill 内部：当同一个词存在于你的 prompts、你的 docs 和你的 codebase 中时， agent 将共享语言与 skill 联系起来并更可靠地触发它。用你在想要该 skill 时实际使用的 leading words 来措辞 description。

_避免_： keyword, term, motif

### Completion Criterion

告诉 agent 一个工作单元已完成的条件——它用来判断的目标。两个属性使其成为杠杆，而不仅仅是质量。其**清晰度**（agent 能区分完成与未完成？）抵抗 **premature completion**——一个模糊的界限（"understanding reached"）让 agent 声明完成并滑入下一步；这个轴需要 _steps_ 才能生效，因为 premature completion 是一个步骤间的失败。其**要求**（要求多少）设置 **legwork**——"every modified model accounted for" 强制彻底的工作，而"produce a change list"则没有——这个轴_不_限于 steps：它也可以绑定一组 flat reference，这就是一个没有 steps 的 skill 仍然携带穷尽性标准的方式（"every rule applied"）。最强的标准既可检查又穷尽。

_避免_： done condition, exit condition, stopping rule

### Legwork

Agent 在单个步骤内幕后所做的工作——读取文件、探索代码库、做出更改、挖掘它需要的东西而不是转嫁给用户。它存在于步骤结构之下：从未写成自己的步骤，隐藏在措辞中，由 agent 而非 skill 控制。与 **post-completion steps** 的跨步骤拉力相对的步骤内对应。由 **leading word**（_comprehensive_、_thorough_）或要求工作必须穷尽的 **completion criterion** 提升——包括应用于 flat reference 的要求轴，这正是驱动一个 flat reference 的 skill 覆盖其所有梯级的原因。当该要求缺失或 **premature completion** 缩短了步骤时，就变薄了。

_避免_： scope, effort, diligence, coverage

### Post-Completion Steps

当前步骤之后的 **steps**。可见时，它们将 agent 拉入 **premature completion**——看到越多，拉力越强；防御是通过将步骤序列拆分为两个来隐藏它们。

_避免_： horizon, fog of war, lookahead

### Premature Completion

_Failure mode._ 在当前步骤真正完成之前结束它，因为 agent 的注意力滑向完成而非工作。一个步骤间的失败：它需要 **steps** 才能发生——一个没有 steps 且提前退出的 skill 不是 premature completion，而是在未满足要求下的薄 **legwork**。两个力之间的拉锯战：可见的 **post-completion steps**（向前的拉力）和 **completion criterion** 的清晰度（阻力——一个锐利、可检查的界限保持住；模糊的则让步）。模糊性是必要非充分条件：一个锐利的边界抵抗拉力，无论后面有多少步骤可见，因此从不匆忙的步骤不需要防御。对于确实如此的步骤，两个杠杆可用，但按顺序使用它们：**先锐化边界**——它是本地和廉价的。仅当标准本质上模糊_并且_你实际观察到匆忙行为时，你才**隐藏后面的步骤**——而隐藏只在跨真实 context 边界时有效（一个 user-invoked 的 hand-off 或 subagent 调度；内联 model-invoked 调用将后面的步骤保留在 context 中并且什么也清除不掉）。薄 legwork 的一个原因，但与之不同： legwork 即使一个步骤运行到完全完成时也可能是薄的。

_避免_： premature closure, the rush, rushing, shortcutting

## Pruning

保持 skill 精简——每个补救措施配对其治愈的失败。

### Single Source of Truth

每个含义恰好在唯一权威位置存在的期望状态，因此对 skill 行为的改变是在一个地方改变。**Duplication** 是其违反。

_避免_： home, canonical location

### Duplication

_Failure mode._ 对同一个含义给出多于一个 **single source of truth**。花费维护成本（改一个地方，你必须改其他所有地方）、花费 tokens、膨胀地位——重复一个含义在梯子上将其权重提升到超过其实际等级。**Leading word** 的意外反面，它有目的地通过重复一个 token 而不是含义来提升注意力。

_避免_： repetition, redundancy

### Relevance

一行是否仍然与 skill 的功能相关——保留什么的视角。一行要么通过从未与任务相关而失去 relevance （纯粹的说明，或应被披露的 **branch**），要么通过变得陈旧：随着其描述的行为或世界变化而过时。较短的 skills 更容易保持 relevance，因为每行检查成本更低。与 **no-op** 不同： relevance 问一行是否与任务相关，而不是它是否改变行为。

_避免_： load-bearing, staleness, freshness

### Sediment

_Failure mode._ 在 skill 中沉淀且从未被清除的旧内容层，因为添加感觉安全而移除感觉有风险——因此陈旧和无关的行积累，你必须通过它们核心向下钻取才能找到仍然活跃的内容。没有任何修剪纪律的 skill 的默认命运；**relevance** 的缓慢侵蚀，与 **duplication** 的重复含义相对。

_避免_： accretion, bloat, cruft, rot

### No-Op

_Failure mode._ 什么也不改变的指令，因为 model 已经默认做它——你支付负载来告诉 agent 它无论如何都会做的事。测试：一行是否相对于默认值改变行为？一行可以完全 **relevant** 但仍然是一个 no-op。使一个 **leading word** 免费的相同先验使一个 no-op 毫无价值。

Leading word 是一种_技术_； No-Op 是对一行的_判断_——而且它们交叉。一个太弱而无法击败默认值的 leading word 是一个 no-op （_be thorough_ 当 agent 已经是 thorough-ish 时），修复是能通过判断的更强者（_relentless_），而不是不同的技术。所以 No-Op 测试——它是否相对于默认值改变行为？——也是你如何评估一个 leading word 是否挣得它的重复。这是 model 相关的，不是读者相关的：两个人对一行是否 no-op 有分歧时，是对默认值有分歧，并通过运行 skill 来解决，而不是通过辩论。

_避免_： redundant instruction, restating the obvious, belaboring
