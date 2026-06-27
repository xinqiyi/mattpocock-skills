<p>
 <a href="https://www.aihero.dev/s/skills-newsletter">
 <picture>
 <source media="(prefers-color-scheme: dark)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skills-repo-dark_2x.png">
 <source media="(prefers-color-scheme: light)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png">
 <img alt="Skills" src="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png" width="369">
 </picture>
 </a>
</p>

# Skills For Real Engineers

[![skills.sh](https://skills.sh/b/mattpocock/skills)](https://skills.sh/mattpocock/skills)

我每天都会使用的 agent skills，用于做真正的 engineering ——而不是 vibe coding。

开发真正的应用很难。像 GSD、 BMAD 和 Spec-Kit 这类方法试图通过主导流程来提供帮助，但它们同时也夺走了你的掌控权，且让流程中的 bug 难以排查。

这些 skills 被设计为小巧、易于调整、可组合。它们适用于任何 model，基于数十年的 engineering 经验。随意折腾它们，让它们成为你自己的。 enjoy。

如果你想跟踪这些 skills 的变更以及我创建的新 skills，可以加入我的 newsletter，已有约 60,000 名开发者订阅：

[订阅 Newsletter](https://www.aihero.dev/s/skills-newsletter)

## 快速开始（30 秒安装）

1. 运行 skills.sh 安装器：

```bash
npx skills@latest add mattpocock/skills
```

2. 选择你想要的 skills，以及要安装在哪些 coding agent 上。**确保选择了 `/setup-matt-pocock-skills`**。

3. 在 agent 中运行 `/setup-matt-pocock-skills`。它会：
 - 询问你想用的 issue tracker （GitHub、 Linear 或本地文件）
 - 询问你在 triage 时给 ticket 打什么 label （`/triage` 使用 labels）
 - 询问你想把我们创建的任何文档保存到哪里

4. 搞定，开始使用。

## 为什么会有这些 Skills

我创建这些 skills 是为了解决我在使用 Claude Code、 Codex 和其他 coding agent 时常见的失败模式。

### #1: Agent 没按我的意思做事

> "没有人确切知道自己想要什么"
>
> David Thomas & Andrew Hunt，[The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题所在**。软件开发中最常见的失败模式是 misalignment。你以为开发者知道你想要什么。然后你看到他们构建的东西——才发现它完全没理解你的意思。

在 AI 时代，这同样如此。你和 agent 之间存在沟通鸿沟。解决方案是 **grilling session**——让 agent 针对你要构建的内容向你提出详细问题。

**解决方案**是使用：

- [`/grill-me`](./skills/productivity/grill-me/SKILL.md) — 用于非代码场景
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) — 与 [`/grill-me`](./skills/productivity/grill-me/SKILL.md) 相同，但增加了更多功能（见下文）

这些是我最受欢迎的 skills。它们帮助你在开始之前与 agent 对齐，并深入思考你要做的变更。**每次**你想做变更时都使用它们。

### #2: Agent 太啰嗦了

> 通过统一语言，开发者之间的对话以及代码的表达都源自同一个 domain model。
>
> Eric Evans，[Domain-Driven-Design](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)

**问题所在**：在项目初期，开发者与软件的使用者（domain experts）通常说着不同的语言。

我在使用 agent 时也感受到了同样的张力。 Agents 通常被直接丢进一个项目，然后被要求边走边摸索 jargon。于是它们用 20 个词来表达本来 1 个词就够了的意思。

**解决方案**是建立一种共享语言。这是一个帮助 agent 解码项目中使用的 jargon 的文档。

<details>
<summary>
示例
</summary>

这是我 `course-video-manager` repo 中一份 [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md)。哪一段更容易阅读？

- **之前**："当课程某个 section 下的 lesson 被'实体化'（即在文件系统中分配了一个位置）时，会出现问题"
- **之后**："materialization cascade 存在问题"

这种简洁性在一次又一次的 session 中得到回报。

</details>

这已经内置于 [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) 中。它是一个 grilling session，但同时帮助你与 AI 构建共享语言，并将难以解释的决策记录在 ADR 中。

这有多强大，难以言表。它可能是这个 repo 中最酷的技术。试试看。

> [!TIP]
> 共享语言除了减少啰嗦之外，还有许多其他好处：
>
> - **变量、函数和文件命名一致**，使用共享语言
> - 因此，**agent 浏览代码库更容易**
> - Agent **在思考上消耗更少的 token**，因为它可以使用更简洁的语言

### #3: 代码跑不通

> "始终采取小而慎重的步骤。反馈的频率就是你的速度上限。永远不要接受太大的任务。"
>
> David Thomas & Andrew Hunt，[The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题所在**：假设你和 agent 在要构建什么上已经对齐了。当 agent **仍然**产出垃圾代码时该怎么办？

是时候审视你的 feedback loops 了。如果 agent 没有关于其代码实际运行情况的反馈，它就是在盲飞。

**解决方案**：你需要常规的 feedback loop 组合：静态类型、浏览器访问和自动化测试。

对于自动化测试， red-green-refactor 循环至关重要。这就是让 agent 先写一个会失败的测试，然后再修复测试。这有助于给 agent 提供一致的反馈水平，从而产生更好的代码。

我构建了一个 **[`/tdd`](./skills/engineering/tdd/SKILL.md) skill**，可以插入任何项目。它鼓励 red-green-refactor，并为 agent 提供关于好测试和坏测试的大量指导。

对于调试，我还构建了一个 **[`/diagnosing-bugs`](./skills/engineering/diagnosing-bugs/SKILL.md)** skill，将最佳调试实践封装在一个简单的 loop 中。

### #4: 我们构建了一团泥球

> "每天都投资于系统的设计。"
>
> Kent Beck，[Extreme Programming Explained](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)

> "最好的 module 是深层的。它们允许通过一个简单的 interface 访问大量功能。"
>
> John Ousterhout，[A Philosophy Of Software Design](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X)

**问题所在**：大多数用 agent 构建的应用都很复杂且难以修改。因为 agent 可以极大地加速编码，它们也同时加速了 software entropy。代码库以前所未有的速度变得更加复杂。

**解决方案**是一种全新的 AI 驱动开发方式：关心代码的设计。

这已经内置于这些 skills 的每一层：

- [`/to-prd`](./skills/engineering/to-prd/SKILL.md) 在创建 PRD 之前询问你正在涉及哪些 module

而最关键的是，[`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md) 可以帮助你挽救已经变成一团泥球的代码库。我建议每隔几天在你的代码库上运行一次。

### 总结

Software engineering 基础比以往任何时候都更重要。这些 skills 是我将这些基础浓缩为可重复实践的最大努力，帮助你交付职业生涯中最好的应用。 Enjoy。

## 参考

这些 skills 按照谁可以调用它们来划分。**User-invoked** skills 只有在你输入它们的名称时才能被调用（例如 `/grill-me`）；它们的工作是编排。**Model-invoked** skills 既可以由你调用，也可以在任务匹配时由 agent 自动调用；它们承载了可复用的纪律。一个 user-invoked skill 可以调用 model-invoked skills，但绝不能调用另一个 user-invoked skill。

### Engineering

我每天用于代码工作的 skills。

**User-invoked**

- **[ask-matt](./skills/engineering/ask-matt/SKILL.md)** — 询问哪种 skill 或流程适合你的情况。本 repo 中 user-invoked skills 的路由器。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)** — Grilling session，同时构建项目的 domain model，精炼术语并实时更新 `CONTEXT.md` 和 ADR。
- **[triage](./skills/engineering/triage/SKILL.md)** — 通过 triage 角色的状态机推动 issue。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)** — 扫描代码库以寻找深化机会，展示为可视化的 HTML 报告，然后对你选择的任何一个进行 grilling。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)** — 为本 repo 配置 engineering skills （issue tracker、 triage labels、 domain doc 布局）。使用其他 engineering skills 之前，每个 repo 运行一次。
- **[to-issues](./skills/engineering/to-issues/SKILL.md)** — 使用 vertical slices 将任何计划、 spec 或 PRD 分解为可独立抓取的 issues。
- **[to-prd](./skills/engineering/to-prd/SKILL.md)** — 将当前对话转换为 PRD 并发布到 issue tracker。无需访谈——只需综合你已经讨论过的内容。
- **[prototype](./skills/engineering/prototype/SKILL.md)** — 构建一个可丢弃的 prototype 来完善设计——可以是针对状态/业务逻辑问题的可运行终端应用，也可以是从同一路由切换的几种截然不同的 UI 变体。

**Model-invoked**

- **[diagnosing-bugs](./skills/engineering/diagnosing-bugs/SKILL.md)** — 针对棘手 bug 和性能回归的纪律性诊断 loop： reproduce → minimise → hypothesise → instrument → fix → regression-test。
- **[tdd](./skills/engineering/tdd/SKILL.md)** — 使用 red-green-refactor loop 的 test-driven development。一次一个 vertical slice 地构建功能或修复 bug。
- **[domain-modeling](./skills/engineering/domain-modeling/SKILL.md)** — 主动构建和精炼项目的 domain model ——对照 glossary 挑战术语，用 edge-case 场景进行压力测试，并实时更新 `CONTEXT.md` 和 ADR。
- **[codebase-design](./skills/engineering/codebase-design/SKILL.md)** — 设计深层 module 的共享纪律和词汇：通过小型 interface 暴露大量行为，放置在清晰的 seam 处，通过该 interface 可测试。

### Productivity

通用工作流工具，与具体代码无关。

**User-invoked**

- **[grill-me](./skills/productivity/grill-me/SKILL.md)** — 被关于计划或设计的 relentless 访谈直到决策树的每个分支都被解决。
- **[handoff](./skills/productivity/handoff/SKILL.md)** — 将当前对话压缩为 handoff 文档，以便另一个 agent 可以继续工作。
- **[teach](./skills/productivity/teach/SKILL.md)** — 在多个 session 中教用户一项新技能或概念，使用当前目录作为有状态的 teaching workspace。
- **[writing-great-skills](./skills/productivity/writing-great-skills/SKILL.md)** — 编写和编辑 skills 的参考：使 skill 可预测的词汇和原则。

**Model-invoked**

- **[grilling](./skills/productivity/grilling/SKILL.md)** — 对用户关于计划或设计进行 relentless 访谈直到决策树的每个分支都被解决。`grill-me` 和 `grill-with-docs` 背后的可复用 loop。

### Misc

我保留但很少使用的工具。

- **[git-guardrails-claude-code](./skills/misc/git-guardrails-claude-code/SKILL.md)** — 设置 Claude Code hooks 以阻止危险的 git 命令（push、 reset --hard、 clean 等）在执行前。
- **[migrate-to-shoehorn](./skills/misc/migrate-to-shoehorn/SKILL.md)** — 将测试文件从 `as` 类型断言迁移到 @total-typescript/shoehorn。
- **[scaffold-exercises](./skills/misc/scaffold-exercises/SKILL.md)** — 创建具有 sections、 problems、 solutions 和 explainers 的练习目录结构。
- **[setup-pre-commit](./skills/misc/setup-pre-commit/SKILL.md)** — 使用 Husky pre-commit hooks 配合 lint-staged、 Prettier、类型检查和 tests 进行设置。
