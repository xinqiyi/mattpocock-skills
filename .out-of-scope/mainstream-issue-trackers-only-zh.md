# Issue tracker 集成仅限于主流工具

`setup-matt-pocock-skills` 仅为**主流** issue trackers 提供一级支持。请求为小众、新出现或单一供应商的实验性 tracker 添加支持，属于 out of scope。

## 为什么这属于 out of scope

每个 issue-tracker backend 都会将特定的 CLI 形态硬编码到 skills 中（commands、 flags、输出解析）。每个新 backend 都是永久的维护负担——它必须随着工具的 CLI 演化而持续工作，并且必须持续针对 `/to-prd`、`/to-issues`、`/triage` 等工具进行测试。这个成本只值得为实际有相当比例用户使用的 tracker 来支付。

"Mainstream" 是一个判断性调用，而不是数字门槛：

- GitHub、 GitLab 和 Backlog.md 是我们认为主流的那种工具——广为人知、广泛使用、早已过了实验阶段。
- 一个全新的以 agent 为中心的工具，有几百个 GitHub stars，即使设计再有趣也不算。

Stars、年龄和下载量在做判断时是有用的信号，但都不构成规则。规则是：一个典型的 engineer 能否识别这个工具，并且合理地为其团队选择了它？

针对非主流 tracker 的逃生出口已经存在：

- `local markdown` 用于轻量的 repo 内追踪。
- `other/custom` 用于想自己配置的用户。

两者都不需要核心 skills 了解具体工具。

## 先前的请求

- #99 —— "Add dex as an issue tracker backend"（dex 在请求时大约 3 个月大，约 300 stars）
