# Issue tracker: Local Markdown

此 repo 的 issues 和 PRDs 作为 markdown 文件存在于 `.scratch/` 中。

## 约定

- 每个功能一个目录：`.scratch/<feature-slug>/`
- PRD 是 `.scratch/<feature-slug>/PRD.md`
- 实现 issues 是 `.scratch/<feature-slug>/issues/<NN>-<slug>.md`，从 `01` 开始编号
- Triage 状态记录在每个 issue 文件顶部附近的 `Status:` 行中（参见 `triage-labels-zh.md` 了解 role 字符串）
- Comments 和对话历史在 `## Comments` 标题下追加到文件底部

## 当 skill 说"发布到 issue tracker"

在 `.scratch/<feature-slug>/` 下创建一个新文件（如果需要则创建目录）。

## 当 skill 说"获取相关 ticket"

读取引用路径处的文件。用户通常会直接传递路径或 issue 编号。
