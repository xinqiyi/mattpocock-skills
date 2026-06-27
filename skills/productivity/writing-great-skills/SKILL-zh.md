---
name: writing-great-skills
description: Reference for writing and editing skills well — the vocabulary and principles that make a skill predictable.
disable-model-invocation: true
---

一个 skill 的存在是为了从随机系统中 wrangle 确定性。**Predictability**—— agent 每次运行采用相同的_过程_，而不是产生相同的输出——是根本美德；下面的每个杠杆都服务于它。

**粗体术语**在 [`GLOSSARY-zh.md`](GLOSSARY-zh.md) 中定义；在那里查找完整含义。

## Invocation

两个选择，权衡不同的成本：

- **Model-invoked** skill 保留 **description**，因此 agent 可以自主触发它 _并且_ 其他 skills 可以访问它（你仍然可以输入它的名称）。它贡献于 **context load**—— description 每轮都坐在窗口中。机制：省略 `disable-model-invocation`，并编写面向 model 的描述，带有丰富的触发措辞（"Use when the user wants …, mentions …"）。
- **User-invoked** skill 从 agent 可达范围内移除 description：只有你，输入它的名称，可以调用它——并且没有其他 skill 可以。零 context load，但它花费 **cognitive load**：_你_ 是必须记住它存在的索引。机制：设置 `disable-model-invocation: true`；`description` 变成面向人类的——一行摘要，触发列表被移除。

仅在 agent 必须自己访问该 skill 时，或其他 skill 必须时，选择 model-invocation。如果它只能手动调用，使其为 user-invoked 并不支付 context load。

当 user-invoked skills 增长到超过你能记住的范围时，堆积起来的 cognitive load 由 **router skill** 解决：一个 user-invoked skill 命名其他 skills 以及何时使用每个。

## 编写 Description

一个 model-invoked 的 **description** 做两件事——陈述 skill 是什么，并列出应该触发它的 **branches**。每个词增加 **context load**，因此 description 挣得比 body 更严厉的修剪：

- **前置 skill 的 leading word**—— description 是做其 invocation 工作的地方。
- **每个 branch 一个触发器。** 重命名单个 branch 的同义词是 **duplication**——"build features using TDD … asks for test-first development" 是一个 branch 写了两次。折叠它们；只保留真正不同的 branches。
- **切断已在 body 中的同一性。** 将 description 保持为触发器，加上任何"when another skill needs …"可达子句。

## 信息层级

一个 skill 由两种内容类型构建——**steps** 和 **reference**——它们可以自由混合：一个 skill 可以是全 steps、全 reference、或两者。核心决策是使用哪一种以及每个在**信息层级**上的位置，这是一个按 agent 需要材料的紧急程度排序的梯子：

1. **In-skill step** ——`SKILL.md` 中的有序动作，主层： agent 做什么，按顺序。每个步骤以一个 **completion criterion** 结束，告诉 agent 工作完成的条件。使其_可检查_（agent 能区分完成与未完成？）并在必要时_穷尽_（"每个修改过的 model 都已说明"，而不是"生成变更列表"）——模糊的标准会招致 **premature completion**。
2. **In-skill reference** ——`SKILL.md` 中的定义、规则或事实，按需查阅。通常是一个合法的扁平同级集（一个审查的所有规则在同一个梯级上）——很好的安排，不是异味。_这个 skill 全是 reference。_
3. **External reference** ——从 `SKILL.md` 中推入单独文件的 reference，通过 **context pointer** 访问，仅在指针触发时加载。（范围从_已披露的_ reference ——同级的文件如 `GLOSSARY.md`，仍是 skill 的一部分——到完全**external reference**，存在于 skill 系统之外且任何 skill 都可以指向。）

一个要求高的 completion criterion 驱动彻底的 **legwork**—— agent 在工作中进行的挖掘——无论 skill 是否有 steps，因为"每个规则都应用了"像"每个步骤都完成了"一样绑定 flat reference。

推得过少，顶层膨胀；推得过多，你隐藏了 agent 实际需要的材料。这种张力就是整个决策。

**Progressive disclosure**是沿梯子向下的移动——从 `SKILL.md` 中出来进入链接文件——这样顶层保持清晰。机制： skill 文件夹中的一个链接 `.md` 文件，以其所包含的内容命名（这个 skill 将其完整定义披露给 `GLOSSARY.md`）。有些 skills 以不止一种方式被使用，每种不同的方式是一个 **branch**——不同的运行采取不同的路径通过 skill。 Branching 是清晰的披露测试：内联每个 branch 都需要的内容，将只有某些 branch 需要的内容推到指针后面。**Context pointer** 的_措辞_，而不是其目标，决定 agent 何时以及多可靠地访问该材料。

梯子决定一段内容_放在多深_，**co-location** 决定一旦放在那里_什么与它并列_：将概念的定义、规则和注意事项保持在一个标题下而不是分散，这样阅读一个部分会带着相邻部分一起。

## 何时拆分

**Granularity** 是你多精细地划分 skills，每次划分花费两种负载之一，因此只在拆分值得时才拆分。两种划分：

- **按 invocation** ——当你有一个应独立触发它的不同 **leading word** 时，拆分出一个 **model-invoked** skill，或者当另一个 skill 必须访问它时。你为新的始终加载的 **description** 支付 **context load**，因此那种独立访问必须值得。
- **按 sequence** ——当仍然在前面步骤（一个步骤的 **post-completion steps**）诱使 agent 匆忙完成当前步骤时，拆分一个 **steps** 的运行（**premature completion**）。将它们保持在视野之外鼓励 agent 在当前任务上做更多 **legwork**。

## 修剪

将每个含义保持在一个**single source of truth**中：一个权威位置，这样改变行为是在一个地方编辑。

检查每一行的 **relevance**：它是否仍然影响该 skill 的功能？

然后逐句而非仅仅逐行地 hunt **no-ops**：对每个句子独立运行 no-op 测试，当它失败时，删除整个句子而不是从中修剪词语。要激进——大多数失败的散文应被删除，而不是被重写。

## Leading Words

**Leading word** 是一个已经存在于 model 的 pretraining 中的紧凑概念， agent 在运行 skill 时用它来思考（例如 _lesson_、_fog of war_、_tracer bullets_）。在文本中重复出现（虽然不一定是——一个强的 leading word 可能只需要一次），它累积一个分布式定义，并以最少的 token 锚定整个行为区域，通过招募 model 已有的先验知识。

它两次服务于 predictability。在 body 中它锚定 _execution_：每次该词出现时， agent 都采用相同的行为。在 description 中它锚定 _invocation_：当同一个词存在于你的 prompts、 docs 和 code 中时， agent 将共享语言与 skill 联系起来，并更可靠地触发它。

Hunt for opportunities to refactor skills to use leading words. 在三个地方展开的三元组（**duplication**），花一句话来指向一个想法的 description ——每一个都是乞求**折叠**为单个 token 的段落。示例包括：

- "fast, deterministic, low-overhead" -> _tight_——一种跨阶段重新陈述的质量——变为一个预训练的单词（一个 _tight_ loop）。
- "a loop you believe in" -> _red_——将模糊的门控转换为二进制的可观察状态（循环在 bug 上变 _red_，或者不变）。

你赢两次：更少的 tokens，_并且_为 agent 悬挂其思考提供了更锐利的钩子。假设每个 skill 都携带了 leading words 可以取代的重述——去找它们。

## Failure Modes

使用这些来诊断用户对 skill 可能遇到的问题。

- **Premature completion** ——在一个步骤真正完成之前就结束它，注意力滑向_完成_。防御，按顺序：首先锐化 completion criterion （便宜、本地化）；只有当它本质上模糊_并且_你观察到匆忙行为时，才通过拆分来隐藏 post-completion steps （sequence 划分）。
- **Duplication** ——相同的含义在不止一个地方。花费维护成本和 tokens，并膨胀一个含义在梯子上的地位超过其实际等级。
- **Sediment** ——停滞的层，因为添加感觉安全而移除感觉有风险而累积。任何没有修剪纪律的 skill 的默认命运。
- **Sprawl** ——一个 skill 太长，即使每行都是活跃且独特的。损害可读性和可维护性并浪费 tokens。解药是梯子：将 **reference** 披露到指针后面，并通过 **branch** 或 sequence 拆分，以便每条路径只携带它需要的内容。
- **No-op** ——一条 agent 已经默认遵守的行，因此你支付负载来说话。测试：它是否相对于默认行为改变了行为？一个弱的 leading word (_be thorough_ 当 agent 已经是 thorough-ish 时) 是一个 no-op；修复是更强的词 (_relentless_)，而不是不同的技术。
