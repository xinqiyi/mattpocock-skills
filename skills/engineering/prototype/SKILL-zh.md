---
name: prototype
description: Build a throwaway prototype to flesh out a design — a runnable terminal app for state/business-logic questions, or several radically different UI variations toggleable from one route.
disable-model-invocation: true
---

# Prototype

一个 prototype 是**回答问题的可丢弃代码。** 问题决定形态。

## 选择一个分支

确定正在回答的问题——从用户的 prompt、周围的代码，或者如果用户在场则通过询问：

- **"这个逻辑 / 状态模型感觉对吗？"** → [LOGIC-zh.md](LOGIC-zh.md)。构建一个小型的交互式终端应用，通过难以在纸上推理的 cases 来推动 state machine。
- **"这应该看起来什么样？"** → [UI-zh.md](UI-zh.md)。在单个路由上生成几种截然不同的 UI 变体，通过 URL search param 和浮动的底部栏切换。

两个分支产生非常不同的 artifact ——选错了就会浪费整个 prototype。如果问题确实有歧义且用户无法联系，默认选择与周围代码更匹配的分支（backend module → logic；一个 page 或 component → UI），并在 prototype 顶部说明假设。

## 两者都适用的规则

1. **从一开始就是可丢弃的，并明确标记。** 将 prototype 代码放在它实际会被使用的地方附近（在其正在设计的 module 或 page 旁边），以便 context 显而易见——但命名时让一个随意的读者可以看出它是一个 prototype，而不是生产代码。对于可丢弃的 UI route，遵守项目已使用的路由约定；不要发明新的顶层结构。
2. **一个命令运行。** 无论项目现有的 task runner 支持什么——`pnpm <name>`、`python <path>`、`bun <path>` 等。用户必须能够不假思索地启动它。
3. **默认不持久化。** 状态存在于内存中。持久化是 prototype_正在检查_的东西，而不是它应该依赖的东西。如果问题明确涉及数据库，使用一个 scratch DB 或本地文件，并带有清晰的"PROTOTYPE — wipe me"名称。
4. **跳过打磨。** 没有测试，没有超过使 prototype _可运行_ 所需的错误处理，没有抽象。关键是快速学到东西然后删除它。
5. **暴露状态。** 每次 action （logic）后或每次变体切换（UI）后，打印或渲染完整的相关状态，以便用户可以看到什么发生了变化。
6. **完成后删除或吸收。** 当 prototype 回答完问题后，将其删除或将验证过的决策融入真实代码——不要让它留在 repo 中腐烂。

## 完成后

从 prototype 中唯一值得保留的东西就是_答案_。将其捕获在某个持久的媒介中（commit message、 ADR、 issue，或 prototype 旁边的 `NOTES.md`），连同它正在回答的问题一起。如果用户在场，这一步是一个快速的对话；如果不在，留下占位符，以便他们（或你，下次经过时）在删除 prototype 之前填写结论。
