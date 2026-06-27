# `setup-matt-pocock-skills` 的 Verify/Check Mode

本项目不会为 `setup-matt-pocock-skills` 添加专门的 verify/check 模式（或单独的 verify skill）。

## 为什么这属于 out of scope

第二个 skill ——或一个 `--verify` 标志——用于检查 `docs/agents/*.md` artifacts 是否仍然匹配 seed-template schema，这将重复现有 setup skill 已经在对话中处理的工作。

预期的工作流是：**运行 `/setup-matt-pocock-skills` 并告诉它验证你当前的设置。** Skill 是 prompt 驱动的，因此维护者可以将其限定为验证通过（"不要重写任何东西，只需检查我现有的文件是否与当前 seed templates 匹配并报告差异"），而无需单独的代码路径。添加一个标志或一个兄弟 skill 会将一个已经可以通过自然语言入口点表达的功能分割开来。

将配置管理保持在一个单一的 skill 中也避免了当 seed templates 演化时两个 skills 相互偏离的维护成本。

## 先前的请求

- #106 —— Feature request: verify/check mode for setup-matt-pocock-skills
