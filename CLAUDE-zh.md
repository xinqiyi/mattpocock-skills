Skills 按 bucket 文件夹组织在 `skills/` 目录下：

- `engineering/` — 日常代码工作
- `productivity/` — 日常非代码 workflow 工具
- `misc/` — 保留但很少使用
- `personal/` — 与个人设置相关，不推广
- `in-progress/` — 尚未准备好发布的草稿
- `deprecated/` — 不再使用

`engineering/`、`productivity/` 或 `misc/` 中的每个 skill 必须在顶层 `README.md` 中有引用，并在 `.claude-plugin/plugin.json` 中有条目。`personal/`、`in-progress/` 和 `deprecated/` 中的 skills 不得出现在其中任何一个中。

顶层 `README.md` 中的每个 skill 条目必须将 skill 名称链接到其 `SKILL.md`。

每个 bucket 文件夹都有一个 `README.md`，列出该 bucket 中的每个 skill 并附上一行描述， skill 名称链接到其 `SKILL.md`。 Bucket `README.md` 和顶层 `README.md` 将条目分组为 **User-invoked** 和 **Model-invoked**。

每个 `SKILL.md` 要么是 user-invoked （`disable-model-invocation: true`，只能由人类访问），要么是 model-invoked （model 或用户都可以访问）。完整定义、描述约定以及为什么 user-invoked skill 可以调用 model-invoked skills 但不能调用另一个 user-invoked skill，请参见 [docs/invocation.md](./docs/invocation.md)。
