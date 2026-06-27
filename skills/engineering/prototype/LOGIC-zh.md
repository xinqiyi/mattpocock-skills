# Logic Prototype

一个小型的交互式终端应用，让用户手动驱动一个状态模型。当问题涉及**业务逻辑、状态转换或数据形态**时使用——那些在纸上看似乎合理，但只有通过真实 cases 推动时才感觉不对的事情。

## 何时这是正确的形态

- "我不确定这个 state machine 是否处理了 X 然后 Y 的 edge case。"
- "这个 data model 真的让我能表示...这种情况吗？"
- "我想在编写之前感受一下 API 应该是什么样的。"
- 任何用户想要**按按钮并观察状态变化**的情况。

如果问题是"这应该看起来什么样"——错误的分支。使用 [UI-zh.md](UI-zh.md)。

## 流程

### 1. 陈述问题

在编写代码之前，写下你正在设计的 state model 和问题。一段话，放在 prototype 的 README 或文件顶部的注释中。回答错误问题的 logic prototype 纯粹是浪费——明确问题以便以后可以检查，无论用户是在观看还是之后返回查看。

### 2. 选择语言

使用宿主项目使用的语言。如果项目没有明显的 runtime （如 docs repo），询问。

匹配项目现有的工具约定——不要仅仅为了 prototype 添加新的 package manager 或 runtime。

### 3. 将逻辑隔离到可移植的 module 中

将实际逻辑——正在回答问题的部分——放在一个小型、纯的 interface 后面，可以稍后提取并放入真实的代码库。其周围的 TUI 是可丢弃的；逻辑 module 不应是可丢弃的。

正确的形态取决于问题：

- **纯 reducer**——`(state, action) => state`。当 actions 是离散事件且状态是单一值时适用。
- **State machine**——显式的 states 和 transitions。当"现在哪些 actions 是合法的"是问题的一部分时适用。
- **一组纯 functions**操作纯数据类型。当没有隐含的当前状态——只有转换时适用。
- **一个具有清晰方法面的 class 或 module**当逻辑确实拥有正在进行的内部状态时。

选择最适合所回答问题的形态，*而不是*最容易连接到 TUI 的。保持纯性：无 I/O、无 terminal 代码、无 `console.log` 用于控制流。 TUI 导入并调用它；没有反向流动。

这就是使 prototype 在其生命周期之外仍然有用的原因。当问题被回答后，验证过的 reducer / machine / function 集可以被提升到真实的 module 中—— TUI shell 被删除。

### 4. 构建最小的暴露状态的 TUI

将其构建为**轻量级 TUI**——每次 tick，清除屏幕（`console.clear()` / `print("\033[2J\033[H")` / 等效命令）并重新渲染整个帧。用户应该始终看到一个稳定的视图，而不是不断增长的滚动回退。

每个帧有两个部分，按此顺序：

1. **当前状态**，美化打印且对 diff 友好（每行一个字段，或格式化 JSON）。字段名或 section 标题使用**粗体**，较不重要的 context （timestamps、 IDs、派生值）使用**暗色**。原生 ANSI 转义码是可以的——`\x1b[1m` 粗体、`\x1b[2m` 暗色、`\x1b[0m` 重置。除非项目中已有一个，否则不需要引入样式库。
2. **键盘快捷键**，列在底部：`[a] add user [d] delete user [t] tick clock [q] quit`。键用粗体，描述用暗色，或反过来——无论如何清晰即可。

行为：

1. **初始化状态**——一个单一的 in-memory 对象/struct。启动时渲染第一个帧。
2. **每次读取一个按键（或一行）**，分派给修改状态的 handler。
3. **重新渲染**每次 action 后的完整帧——不追加，替换。
4. **循环直到退出。**

整个帧应该适合一个屏幕。

### 5. 用一个命令使其可运行

向项目现有的 task runner （`package.json` scripts、`Makefile`、`justfile`、`pyproject.toml`）添加一个脚本。用户应该运行 `pnpm run <prototype-name>` 或等价命令——永远不需要记住路径。

如果宿主项目没有 task runner，只需将命令放在 prototype 的 README 顶部。

### 6. 交付

给用户运行命令。他们将自己驱动它；有趣的时刻是当他们说"等等，这不应该可能"或"嗯，我以为 X 会不一样"时——这些是_想法_中的 bug，而这正是全部的意义。如果他们想添加新的 actions，添加它们。 Prototypes 不断演化。

### 7. 捕获答案

当 prototype 完成其工作后，问题的答案是唯一值得保留的东西。如果用户在场，问他们学到了什么。如果不在，在 prototype 旁边留下 `NOTES.md`，以便在 prototype 被删除之前可以填写答案（或由你填写，如果你观看了 session）。

## 反模式

- **不要添加测试。** 需要测试的 prototype 不再是 prototype。
- **不要连接到真实数据库。** 使用 in-memory store，除非问题专门涉及持久化。
- **不要泛化。** 没有"如果我们以后想支持 X 的话。"一个 prototype 回答一个问题。
- **不要模糊逻辑和 TUI 之间的界限。** 如果 reducer / state machine 引用了 `console.log`、 prompts 或 terminal 转义码，它就不再可移植。将 TUI 保持为纯 module 上的薄壳。
- **不要将 TUI shell 发布到生产环境。** Shell 是为从 terminal 手动驱动而优化的。它后面的逻辑 module 才是值得保留的部分。
