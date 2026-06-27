# HTML 报告格式

架构审查被渲染为一个自包含的 HTML 文件，放在 OS 临时目录中。 Tailwind 和 Mermaid 都来自 CDN。 Mermaid 可靠地处理图形状图表；手写的 divs 和内联 SVG 处理更具编辑性的可视化（mass diagrams、 cross-sections）。混合使用两者——不要事事依赖 Mermaid，它会开始看起来千篇一律。

## Scaffold

```html
<!doctype html>
<html lang="zh-CN">
 <head>
 <meta charset="utf-8" />
 <title>Architecture review — {{repo name}}</title>
 <script src="https://cdn.tailwindcss.com"></script>
 <script type="module">
 import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";
 mermaid.initialize({ startOnLoad: true, theme: "neutral", securityLevel: "loose" });
 </script>
 <style>
 /* Tailwind 不能干净覆盖的小型自定义层：
 虚线 seam 线、手绘风格的箭头头部等。 */
 .seam { stroke-dasharray: 4 4; }
 .leak { stroke: #dc2626; }
 .deep { background: linear-gradient(135deg, #0f172a, #1e293b); }
 </style>
 </head>
 <body class="bg-stone-50 text-slate-900 font-sans">
 <main class="max-w-5xl mx-auto px-6 py-12 space-y-12">
 <header>...</header>
 <section id="candidates" class="space-y-10">...</section>
 <section id="top-recommendation">...</section>
 </main>
 </body>
</html>
```

## Header

Repo 名称、日期和紧凑的图例：实心框 = module、虚线 = seam、红色箭头 = leakage、粗深色框 = deep module。没有引言段落——直接进入 candidates。

## Candidate Card

Diagram 承载重量。散文是稀疏的、朴素的，并使用 glossary 术语（来自 `/codebase-design` skill）而不做作。

每个 candidate 是一个 `<article>`：

- **标题** ——简短，命名深化（例如"Collapse the Order intake pipeline"）。
- **Badge 行** ——推荐强度（`Strong` = 翡翠色、`Worth exploring` = 琥珀色、`Speculative` = 石板色），加上依赖类别的标签（`in-process`、`local-substitutable`、`ports & adapters`、`mock`）。
- **文件** ——等宽字体列表，`font-mono text-sm`。
- **之前 / 之后图表** ——核心。两列，并排。见下面的模式。
- **问题** ——一句话。什么疼。
- **解决方案** ——一句话。什么改变。
- **收益** ——要点，每个 ≤6 词。例如"Tests hit one interface"、"Pricing logic stops leaking"、"Delete 4 shallow wrappers"。
- **ADR 提示**（如果适用）——在琥珀色盒子中的一行。

没有解释的段落。如果 diagram 需要一段话才能被理解，重新画 diagram。

## Diagram 模式

选择适合 candidate 的模式。混合它们。不要使每个 diagram 看起来一样——多样性是重点的一部分。

### Mermaid 图（依赖/调用流的通用工具）

当重点在于"X 调用 Y 调用 Z，看看这团糟"时，使用 Mermaid `flowchart` 或 `graph`。将其包裹在 Tailwind 样式化的卡片中，使其不显得突兀。使用 classDef 将泄漏边着色为红色，深层 module 着色为深色。 Sequence diagrams 很适合"之前：6 次往返；之后：1 次"。

```html
<div class="rounded-lg border border-slate-200 bg-white p-4">
 <pre class="mermaid">
 flowchart LR
 A[OrderHandler] --> B[OrderValidator]
 B --> C[OrderRepo]
 C -.leak.-> D[PricingClient]
 classDef leak stroke:#dc2626,stroke-width:2px;
 class C,D leak
 </pre>
</div>
```

### 手写盒子和箭头（当 Mermaid 的布局与你对抗时）

Modules 作为带边框和标签的 `<div>`s。箭头作为内联 SVG `<line>` 或 `<path>` 元素，绝对定位在相对容器上。当你想要"之后"图感觉像一个带灰色内部细节的厚边框深色 module 时使用这个—— Mermaid 无法以正确的权重渲染这个。

### 横截面（适合分层浅层性）

堆叠水平条带（`h-12 border-l-4`）以显示调用通过的层。之前：6 个薄层各自什么都不做。之后：1 个厚条带标记了整合后的职责。

### Mass diagram （适合"interface 与 implementation 一样宽"）

每个 module 两个矩形——一个用于 interface 表面积，一个用于 implementation。之前： interface 矩形几乎与 implementation 矩形一样高（浅层）。之后： interface 矩形短， implementation 矩形高（深层）。

### 调用图折叠

之前：渲染为嵌套盒子的函数调用树。之后：同一棵树折叠为一个盒子，现在内部的调用在其中淡出显示。

## 样式指导

- 偏向编辑风格，而不是企业 dashboard。慷慨的空白。标题可选的衬线字体（`font-serif` 与 stone/slate 搭配良好）。
- 有节制地使用颜色：一种强调色（翡翠色或靛蓝色）加上用于泄漏的红色和用于警告的琥珀色。
- 保持 diagrams 约 320px 高，以便之前/之后舒适地并排显示而无需滚动。
- 在 diagram 内部使用 `text-xs uppercase tracking-wider` 用于 module 标签——它们应读作示意图，而不是 UI。
- 唯一的 scripts 是 Tailwind CDN 和 Mermaid ESM 导入。报告否则是静态的——没有 app 代码，没有超出 Mermaid 自身渲染的交互性。

## Top Recommendation Section

一个较大的卡片。 Candidate 名称、一句为什么、指向其 card 的锚链接。就这样。

## 语气

简洁的普通英语——但架构名词和动词直接来自 `/codebase-design` skill。简洁不是漂移的借口。

**准确使用：** module、 interface、 implementation、 depth、 deep、 shallow、 seam、 adapter、 leverage、 locality。

**绝不替代：** component、 service、 unit （用于 module）· API、 signature （用于 interface）· boundary （用于 seam）· layer、 wrapper （用于 module，当你的意思是 module）。

**符合风格的说法：**

- "Order intake module is shallow — interface nearly matches the implementation."
- "Pricing leaks across the seam."
- "Deepen: one interface, one place to test."
- "Two adapters justify the seam: HTTP in prod, in-memory in tests."

**收益要点**用 glossary 术语命名收益：*"locality: bugs concentrate in one module"*、*"leverage: one interface, N call sites"*、*"interface shrinks; implementation absorbs the wrappers"*。不要写 *"easier to maintain"* 或 *"cleaner code"*——这些术语不在 glossary 中且不值得它们的位置。

没有含糊、没有清嗓子、没有"it's worth noting that …"。如果一个句子可以是一个要点，让它成为一个要点。如果一个要点可以被删掉，删掉它。如果一个术语不在 `/codebase-design` glossary 中，在发明一个新术语之前找一个在 glossary 中的。
