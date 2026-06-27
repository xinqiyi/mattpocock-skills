# Engineering

我每天用于代码工作的 skills。

## User-invoked

只有在你输入时才能访问（`disable-model-invocation: true`）。

- **[ask-matt](./ask-matt/SKILL.md)** — 询问哪种 skill 或流程适合你的情况。本 repo 中 user-invoked skills 的路由器。
- **[grill-with-docs](./grill-with-docs/SKILL.md)** — Grilling session，同时构建项目的 domain model，精炼术语并实时更新 `CONTEXT.md` 和 ADR。
- **[triage](./triage/SKILL.md)** — 通过 triage roles 的状态机推动 issue。
- **[improve-codebase-architecture](./improve-codebase-architecture/SKILL.md)** — 扫描代码库以寻找深化机会，展示为可视化的 HTML 报告，然后对你选择的任何一个进行 grilling。
- **[setup-matt-pocock-skills](./setup-matt-pocock-skills/SKILL.md)** — 为本 repo 配置 engineering skills （issue tracker、 triage labels、 domain doc 布局）。每个 repo 运行一次。
- **[to-issues](./to-issues/SKILL.md)** — 使用 vertical slices 将任何计划、 spec 或 PRD 分解为可独立抓取的 issues。
- **[to-prd](./to-prd/SKILL.md)** — 将当前对话转换为 PRD 并发布到 issue tracker。
- **[prototype](./prototype/SKILL.md)** — 构建一个可丢弃的 prototype ——用于状态/逻辑问题的可运行终端应用，或几种可切换的 UI 变体。

## Model-invoked

可由 model 或用户访问（丰富的触发措辞，以便 model 可以自动调用）。

- **[diagnosing-bugs](./diagnosing-bugs/SKILL.md)** — 针对棘手 bug 和性能回归的纪律性诊断 loop： reproduce → minimise → hypothesise → instrument → fix → regression-test。
- **[tdd](./tdd/SKILL.md)** — 使用 red-green-refactor loop 的 test-driven development。一次一个 vertical slice 地构建功能或修复 bug。
- **[domain-modeling](./domain-modeling/SKILL.md)** — 主动构建和精炼项目的 domain model ——挑战术语、用场景进行压力测试、实时更新 `CONTEXT.md` 和 ADR。
- **[codebase-design](./codebase-design/SKILL.md)** — 设计深层 module 的共享纪律和词汇：小型 interface、清晰的 seam、通过 interface 可测试。
