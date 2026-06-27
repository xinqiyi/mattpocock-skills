---
name: handoff
description: Compact the current conversation into a handoff document for another agent to pick up.
argument-hint: "下一个 session 将用于什么？"
disable-model-invocation: true
---

编写一个 handoff 文档，总结当前对话，以便一个新的 agent 可以继续工作。保存到用户操作系统的临时目录——而不是当前 workspace。

在文档中包含一个"suggested skills"部分，建议 agent 应调用的 skills。

不要复制已捕获在其他 artifact 中的内容（PRDs、 plans、 ADRs、 issues、 commits、 diffs）。而是通过路径或 URL 引用它们。

编辑任何敏感信息，如 API keys、 passwords 或个人身份信息。

如果用户传递了参数，将其视为下一个 session 关注点的描述，并相应地定制文档。
