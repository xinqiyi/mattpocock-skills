---
name: writing-fragments
description: Writing, explore — mine raw fragments, no structure yet.
disable-model-invocation: true
---

<what-to-do>

这是纯粹的 **explore**：拓宽可以写作的空间，而不承诺结构——承诺结构是 _exploit_，一个单独的 skill 的工作。运行一个产生 fragments 的 grilling session，对用户想写的任何东西进行 relentless 访谈。强加阶段、大纲或文章结构这里属于 out of scope。

当 fragments 从对话的任何一方出现时，将它们追加到一个单一的 markdown 文件中。

如果用户没有传递路径，询问一次在哪里保存文档，然后在剩余的 session 中记住它。

从用户说的第一句话开始捕获 fragments，包括初始 prompt。

在第一次写入时，在顶部放一个单一的 H1，带一个工作标题（以后可以更改）和其他什么都不放——没有 metadata、没有 TOC、没有日期。

</what-to-do>

<supporting-info>

## 什么是 fragment

Fragment 是任何可能保留到最终文章中的文本片段。它必须是_作者可读的_——作者能看出它是什么意思——但它不需要定义其术语或对冷启动读者可理解。标准是"这是一段好写作吗？"，而不是"这是一个自包含的论证吗？"

Fragments 有意地是异质的。可以作为 fragment 的例子：

- 一个你想在某个地方使用但还不知道在哪里的犀利句子。
- 一个带有一行理由的声明。
- 一个 vignette：发生的一件事、一个代码 snippet、一个场景、一个类比。
- 一个半成品想法："关于 X 感觉像 Y 的东西，以后再说。"
- 一个引用、一段对话、一句偶然听到的话。
- 一个凭感觉联系在一起的相关观察的列表。
- 一个抱怨、一次坦白、一个笑点。
- 一个**leading word**——一个紧凑的隐喻或新词，整个文章可以挂靠（一个命名想法的术语，就像 _tracer bullets_ 或 _fog of war_ 命名整个模式）。

其中， leading word 是最有价值的 fragment 要落地。它是承重的：在 explore 中正确地命名它，它会在后面塑造结构、过渡和标题——在整个 exploit 阶段都带来回报。当对话环绕一个重复的想法时，推动为它造一个词。

小说家的日记是这个模式：多年无结构的观察，后来被开采作为原始材料。 Fragments 就是观察。

## 文件格式

```markdown
# 工作标题

第一个 fragment 在这里。

它可以是多段。它可以包括列表、代码、引用——任何
fragment 自然采取的形态。

---

第二个 fragment。

---

> 用户想保留的引用行。

对它的反应。

---

- 一组相关观察的聚类
- 凭感觉联系在一起
- 并希望彼此靠近
```

Fragments 由水平分割线（`\n---\n`）分隔。正文内没有标题。没有标签。除了它们被添加的顺序之外没有顺序。

## 写作节奏

静默追加。不要为每个 fragment 请求许可。在对话中顺便提到你添加了什么（"adding that"），但不要用保存对话框打断对话。

每次写入前：从磁盘重新读取文件。用户可能在轮次之间编辑、重新排序或删除了 fragments ——保留他们的更改。永远不要覆盖文件；只追加（或者，如果用户要求，原地编辑特定 fragment）。

用户可以随时说"cut the last one"、"rewrite that one sharper"、"merge those two"。将这些作为一级指令处理。

</supporting-info>
