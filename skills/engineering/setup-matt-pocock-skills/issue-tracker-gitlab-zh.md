# Issue tracker: GitLab

此 repo 的 issues 和 PRDs 作为 GitLab issues 存在。所有操作使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI。

## 约定

- **创建 issue**：`glab issue create --title "..." --description "..."`。多行 description 使用 heredoc。传递 `--description -` 打开编辑器。
- **读取 issue**：`glab issue view <number> --comments`。使用 `-F json` 获取机器可读输出。
- **列出 issues**：`glab issue list -F json` 配合适当的 `--label` 过滤器。
- **评论 issue**：`glab issue note <number> --message "..."`。 GitLab 将 comments 称为"notes"。
- **应用 / 移除 labels**：`glab issue update <number> --label "..."` / `--unlabel "..."`。多个 labels 可以用逗号分隔或重复该标志。
- **关闭**：`glab issue close <number>`。`glab issue close` 不接受关闭评论，所以先用 `glab issue note <number> --message "..."` 发布解释，然后关闭。
- **Merge requests**： GitLab 将 PRs 称为"merge requests"。使用 `glab mr create`、`glab mr view`、`glab mr note` 等——与 `gh pr ...` 相同的形态，用 `mr` 替换 `pr`，用 `note`/`--message` 替换 `comment`/`--body`。

从 `git remote -v` 推断 repo ——`glab` 在 clone 内运行时自动完成此操作。

## Merge requests 作为 triage surface

**MRs 作为请求面：否。**（如果此 repo 将外部 merge requests 视为 feature requests，设置为 `yes`；`/triage` 读取此标志。）

当设置为 `yes` 时， MRs 通过与 issues 相同的 labels 和 states 运行，使用 `glab mr` 等效命令：

- **读取 MR**：`glab mr view <number> --comments` 和 `glab mr diff <number>` 用于 diff。
- **列出外部 MRs 进行 triage**：`glab mr list -F json`，然后只保留作者不是项目成员/所有者的 MRs （贡献者的 MR，而不是维护者进行中的工作）。
- **评论 / 标签 / 关闭**：`glab mr note`、`glab mr update --label`/`--unlabel`、`glab mr close`。

与 GitHub 不同， GitLab 分别编号 issues 和 MRs，因此 `#42` 在你知道维护者指的是哪个 surface 时是不含糊的。

## 当 skill 说"发布到 issue tracker"

创建一个 GitLab issue。

## 当 skill 说"获取相关 ticket"

运行 `glab issue view <number> --comments`。
