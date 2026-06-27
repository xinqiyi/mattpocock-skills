---
name: setup-matt-pocock-skills
description: Configure this repo for the engineering skills — set up its issue tracker, triage label vocabulary, and domain doc layout. Run once before first use of the other engineering skills.
disable-model-invocation: true
---

# Setup Matt Pocock's Skills

搭建 engineering skills 假设存在的 per-repo 配置：

- **Issue tracker**—— issues 所在的位置（默认 GitHub；本地 markdown 也原生支持）
- **Triage labels**——用于五个 canonical triage roles 的字符串
- **Domain docs**——`CONTEXT.md` 和 ADR 所在的位置，以及读取它们的消费规则

这是一个 prompt 驱动的 skill，而不是确定性的脚本。探索、呈现你发现的、向用户确认、然后写入。

## 流程

### 1. 探索

查看当前 repo 以了解其起始状态。读取已有的内容；不要假设：

- `git remote -v` 和 `.git/config`——这是一个 GitHub repo 吗？是哪一个？
- repo 根目录的 `AGENTS.md` 和 `CLAUDE.md`——两者中是否存在？是否已经有 `## Agent skills` section？
- repo 根目录的 `CONTEXT.md` 和 `CONTEXT-MAP.md`
- `docs/adr/` 和任何 `src/*/docs/adr/` 目录
- `docs/agents/`——这个 skill 先前的输出是否已经存在？
- `.scratch/`——表明本地 markdown issue tracker 约定已经在使用的标志

### 2. 呈现发现并询问

总结哪些存在和哪些缺失。然后引导用户**逐一**完成三个决策——呈现一个 section，获得用户的答案，然后进入下一个。不要一次性抛出所有三个。

假设用户不知道这些术语的含义。每个 section 以一个简短的解释开始（它是什么、为什么这些 skills 需要它、如果他们选择不同会有什么变化）。然后展示选择和默认项。

**Section A — Issue tracker。**

> 解释："issue tracker" 是本 repo issues 所在的工具。诸如 `to-issues`、`triage`、`to-prd` 和 `qa` 等 skills 需要读写它——它们需要知道是调用 `gh issue create`、在 `.scratch/` 下写一个 markdown 文件，还是遵循你描述的其他 workflow。选择你实际为此 repo 追踪工作的地方。

默认姿态：这些 skills 是为 GitHub 设计的。如果 `git remote` 指向 GitHub，提议使用 GitHub。如果 `git remote` 指向 GitLab （`gitlab.com` 或自托管主机），提议使用 GitLab。否则（或如果用户偏好），提供：

- **GitHub**—— issues 存在于 repo 的 GitHub Issues 中（使用 `gh` CLI）
- **GitLab**—— issues 存在于 repo 的 GitLab Issues 中（使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI）
- **本地 markdown**—— issues 作为 `.scratch/<feature>/` 下的文件存在于此 repo 中（适合没有 remote 的 solo 项目或 repos）
- **其他**（Jira、 Linear 等）——请用户用一段话描述 workflow； skill 将记录为自由格式的散文

如果——并且仅当——用户选择了 **GitHub** 或 **GitLab**，问一个后续问题：

> 解释：开源 repos 经常以 pull requests 的形式接收 feature requests，而不仅仅是 issues —— PR 是带有代码的 issue。如果你开启这个，`/triage` 将*外部* PRs 拉入同一个队列，并通过与 issues 相同的 labels 和 states 运行它们（协作者正在进行的 PRs 不受影响）。如果 PRs 对你不构成请求面，就保持关闭。

- **PRs 作为请求面**——是 / 否（默认：否）。将答案记录在 `docs/agents/issue-tracker.md`。对于本地 markdown 和其他 tracker，跳过此问题——没有 PRs。

**Section B — Triage label 词汇。**

> 解释：当 `triage` skill 处理一个传入的 issue 时，它通过一个 state machine 移动它—— needs evaluation、 waiting on reporter、 ready for an AFK agent to pick up、 ready for a human、或 wont fix。为此，它需要应用与你*实际配置的*字符串匹配的 labels （或 issue tracker 中的等效物）。如果你的 repo 已经使用不同的 label 名称（例如 `bug:triage` 而不是 `needs-triage`），在这里映射它们，以便 skill 应用正确的而不是创建重复的。

五个 canonical roles：

- `needs-triage`——维护者需要评估
- `needs-info`——等待报告者
- `ready-for-agent`——完全指定， AFK 就绪（agent 可以在没有人类 context 的情况下拾取）
- `ready-for-human`——需要人类实现
- `wontfix`——不会执行

默认：每个 role 的字符串等于其名称。询问用户是否想覆盖任何内容。如果他们的 issue tracker 没有现有的 labels，默认值就可以了。

**Section C — Domain docs。**

> 解释：一些 skills （`improve-codebase-architecture`、`diagnosing-bugs`、`tdd`）读取 `CONTEXT.md` 文件以了解项目的领域语言，以及 `docs/adr/` 以了解过去的架构决策。它们需要知道 repo 是有一个全局 context 还是多个（例如具有分离的前端/后端 contexts 的 monorepo），以便它们查找正确的位置。

确认布局：

- **单 context**—— repo 根目录的一个 `CONTEXT.md` + `docs/adr/`。大多数 repos 如此。
- **多 context**——根目录的 `CONTEXT-MAP.md` 指向每个 context 的 `CONTEXT.md` 文件（通常是 monorepo）。

### 3. 确认和编辑

向用户展示以下内容的草稿：

- 要添加到正在编辑的 `CLAUDE.md` / `AGENTS.md` 中的 `## Agent skills` 块（参见步骤 4 了解选择规则）
- `docs/agents/issue-tracker.md`、`docs/agents/triage-labels.md`、`docs/agents/domain.md` 的内容

让他们在写入之前编辑。

### 4. 写入

**选择要编辑的文件：**

- 如果 `CLAUDE.md` 存在，编辑它。
- 否则如果 `AGENTS.md` 存在，编辑它。
- 如果两者都不存在，询问用户要创建哪一个——不要替他们选择。

当 `CLAUDE.md` 已经存在时，永远不要创建 `AGENTS.md`（反之亦然）——总是编辑已经存在的那个。

如果所选文件中已经存在 `## Agent skills` 块，原地更新其内容，而不是追加副本。不要覆盖用户对周围 sections 的编辑。

该块：

```markdown
## Agent skills

### Issue tracker

[issues 追踪位置以及外部 PRs 是否为 triage surface 的一行摘要]。参见 `docs/agents/issue-tracker.md`。

### Triage labels

[label 词汇的一行摘要]。参见 `docs/agents/triage-labels.md`。

### Domain docs

[布局的一行摘要——"single-context"或"multi-context"]。参见 `docs/agents/domain.md`。
```

然后使用此 skill 文件夹中的 seed templates 作为起点编写三个 docs 文件：

- [issue-tracker-github-zh.md](./issue-tracker-github-zh.md) — GitHub issue tracker
- [issue-tracker-gitlab-zh.md](./issue-tracker-gitlab-zh.md) — GitLab issue tracker
- [issue-tracker-local-zh.md](./issue-tracker-local-zh.md) — 本地 markdown issue tracker
- [triage-labels-zh.md](./triage-labels-zh.md) — label 映射
- [domain-zh.md](./domain-zh.md) — domain doc 消费规则 + 布局

对于"其他"issue trackers，使用用户的描述从头编写 `docs/agents/issue-tracker.md`。

### 5. 完成

告诉用户设置已完成，以及哪些 engineering skills 现在会读取这些文件。提到他们以后可以直接编辑 `docs/agents/*.md`——只有在他们想要切换 issue tracker 或从头重新开始时才需要重新运行这个 skill。
