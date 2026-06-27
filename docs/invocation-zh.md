# Model-invoked vs User-invoked

本 repo 中的每个 `SKILL.md` 都是一个 skill。划分它们的一个轴是 **invocation**——谁能访问它：

- **User-invoked** —— **只能由人类通过输入其名称**来访问。在 frontmatter 中设置 `disable-model-invocation: true`。`description` 是**面向人类的**：供浏览 slash-commands 的人阅读的一行摘要。去除触发列表（"Use when the user says …"）。
- **Model-invoked** —— 可由 **model 或用户**访问。默认设置：省略 `disable-model-invocation`。`description` 是**面向 model 的**，保留丰富的触发措辞（"Use when the user wants …, mentions …, asks for …"），以便自动调用触发。判断一个 skill 是否应保持 model-invoked 的标准是：_model 自主调用它是否有用？_（复用是提取 skill 的原因，但不是判断标准。）

因为 user-invoked skill 没有 description，除了人类之外没有任何东西可以访问它——其他 skill 也无法触发它。因此， user-invoked skill 可以调用 model-invoked skills，但绝不能访问另一个 user-invoked skill。

Bucket `README.md` 和顶层 `README.md` 将条目分组为 **User-invoked** 和 **Model-invoked**。

## 之间的依赖关系

依赖关系以 **`/skill` 风格的散文式调用**（"Run the `/grilling` skill"）来表达，而不是深层的 `../other-skill/FILE.md` 交叉引用。共享的参考文档位于拥有它们的 skill 内部；其他 skills 通过调用该 skill 来获取这些材料，而不是跨文件夹链接。

## Passive vs Active domain 工作

仅仅是_阅读_ `CONTEXT.md` 获取词汇是一行的散文式指引，而不是 `domain-modeling` skill。只有主动的构建/精炼纪律（挑战术语、 edge-case 场景、编写 ADR、实时更新 `CONTEXT.md`）才是 `domain-modeling`。
