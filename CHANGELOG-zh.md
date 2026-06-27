# mattpocock-skills

## 1.0.1

### Patch Changes

- [`d20ee26`](https://github.com/mattpocock/skills/commit/d20ee2684e2a9442698ac3c1e0f2c5b68c4cf296) 感谢 [@mattpocock](https://github.com/mattpocock)！——让 **`teach`** skill 以复用为先。现在 lesson 是从 `./assets/` 中的可复用 **components** 构建的——样式表、测验组件、模拟器、图表助手。复用是默认行为： agent 在编写 lesson 前读取 `./assets/`，从已有内容构建，并将任何新的可复用内容提取到 component 中而不是内联。

## 1.0.0

### Major Changes

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) 感谢 [@mattpocock](https://github.com/mattpocock)！——新增 **`ask-matt`** skill ——一个 user-invoked 路由器，指向适合你情况的 skill 或 flow。

 **Breaking：** `ask-matt` 在本 repo 中路由到其他 user-invoked skills，因此它期望这些 skills 已被安装。

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) 感谢 [@mattpocock](https://github.com/mattpocock)！——新增共享的 design skills 并将现有 skills 重新接入它们。

 - 新的 **`codebase-design`** skill ——深层 module 词汇（module、 interface、 depth、 seam、 adapter）以及通过小型 interface 暴露大量行为的原则。之前存在于 `improve-codebase-architecture/LANGUAGE.md` 中的语言现在迁移到这里，经过通用化以便跨 skills 复用。
 - 新的 **`domain-modeling`** skill ——主动构建和精炼项目的 domain model，对照 glossary 对术语进行压力测试，并保持 `CONTEXT.md` 和 ADR 更新。
 - `improve-codebase-architecture` 现在从 `/codebase-design` 获取其架构词汇，从 `/domain-modeling` 获取其 domain model。
 - `tdd` 现在依靠 `/codebase-design` 获取 interface 设计指导——其内联的 `deep-modules.md` / `interface-design.md` 笔记已被移除，转而使用共享 skill。
 - `grill-with-docs` 现在通过 `/domain-modeling` 内联构建 domain model。

 **Breaking：** 这些 skills 现在依赖于新的 `codebase-design` / `domain-modeling` skills，因此你也必须安装它们。

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) 感谢 [@mattpocock](https://github.com/mattpocock)！——移除 **`caveman`** 和 **`zoom-out`** skills。

 - `caveman` 是另一个我正在测试的 skill 的副本，从未打算公开。
 - `zoom-out` 在实践中未被使用，因此已从 repo 中移除。

 **Breaking：** 两个 skills 已被移除。

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) 感谢 [@mattpocock](https://github.com/mattpocock)！——将 **`diagnose`** skill 重命名为 **`diagnosing-bugs`**。

 **Breaking：** 以 `/diagnosing-bugs` 调用它——旧的 `/diagnose` 名称不再存在。

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) 感谢 [@mattpocock](https://github.com/mattpocock)！——用 **`writing-great-skills`** 替换 **`write-a-skill`**。

 - 移除了 `write-a-skill`。
 - 新增了 `writing-great-skills`（及其 `GLOSSARY.md`）——编写和编辑 skills 的参考：使 skill 可预测的词汇和原则，在句子级别消除 no-op。
 - 将 `grilling` 作为 model-invoked skill 暴露——`grill-me` 和 `grill-with-docs` 背后的可复用 interview loop。

 **Breaking：** `write-a-skill` 已被移除；使用 `writing-great-skills` 代替。

### Minor Changes

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) 感谢 [@mattpocock](https://github.com/mattpocock)！——新增 **`resolving-merge-conflicts`** skill ——用于解决进行中的 git merge 或 rebase 冲突的 loop。独立使用，不依赖其他 skills。

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) 感谢 [@mattpocock](https://github.com/mattpocock)！——将 skill 分类从 **Commands / Skills** 重命名为 **User-invoked / Model-invoked**，并新增 `docs/invocation.md` 定义这种划分： user-invoked skills 只有在你输入时才能被调用，其存在目的是编排； model-invoked skills 也可以在任务匹配时自动调用。一个 user-invoked skill 可以调用 model-invoked skills，但绝不能调用另一个 user-invoked skill。

### Patch Changes

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) 感谢 [@mattpocock](https://github.com/mattpocock)！——收紧 **`review`** skill： fail-fast ref 检查、单一来源规则、消除 no-op。
