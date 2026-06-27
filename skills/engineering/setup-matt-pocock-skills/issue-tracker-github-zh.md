# Issue tracker: GitHub

此 repo 的 issues 和 PRDs 作为 GitHub issues 存在。所有操作使用 `gh` CLI。

## 约定

- **创建 issue**：`gh issue create --title "..." --body "..."`。多行 body 使用 heredoc。
- **读取 issue**：`gh issue view <number> --comments`，通过 `jq` 过滤 comments 并同时获取 labels。
- **列出 issues**：`gh issue list --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'` 配合适当的 `--label` 和 `--state` 过滤器。
- **评论 issue**：`gh issue comment <number> --body "..."`
- **应用 / 移除 labels**：`gh issue edit <number> --add-label "..."` / `--remove-label "..."`
- **关闭**：`gh issue close <number> --comment "..."`

从 `git remote -v` 推断 repo ——`gh` 在 clone 内运行时自动完成此操作。

## Pull requests 作为 triage surface

**PRs 作为请求面：否。**（如果此 repo 将外部 PRs 视为 feature requests，设置为 `yes`；`/triage` 读取此标志。）

当设置为 `yes` 时， PRs 通过与 issues 相同的 labels 和 states 运行，使用 `gh pr` 等效命令：

- **读取 PR**：`gh pr view <number> --comments` 和 `gh pr diff <number>` 用于 diff。
- **列出外部 PRs 进行 triage**：`gh pr list --state open --json number,title,body,labels,author,authorAssociation,comments` 然后只保留 `authorAssociation` 为 `CONTRIBUTOR`、`FIRST_TIME_CONTRIBUTOR` 或 `NONE` 的（排除 `OWNER`/`MEMBER`/`COLLABORATOR`）。
- **评论 / 标签 / 关闭**：`gh pr comment`、`gh pr edit --add-label`/`--remove-label`、`gh pr close`。

GitHub 在 issues 和 PRs 之间共享一个编号空间，因此一个裸 `#42` 可能是其中任何一个——用 `gh pr view 42` 解析并回退到 `gh issue view 42`。

## 当 skill 说"发布到 issue tracker"

创建一个 GitHub issue。

## 当 skill 说"获取相关 ticket"

运行 `gh issue view <number> --comments`。
