---
"mattpocock-skills": patch
---

扩展 **`triage`** skill，使其能够对外部 pull requests 进行 triage，将 PR 视为带有代码的 issue，通过相同的 roles 和 state machine 运行。 PR 与 issues 一起内联流转（受 per-repo setup 开关控制）， discovery 仅暴露外部 PR，仅限 bug 的"reproduce"步骤被泛化为一个单一的"verify the claim"步骤，冗余检查将已实现的请求解析为 `wontfix` 而不污染 out-of-scope 知识库。`setup-matt-pocock-skills` 新增了 GitHub/GitLab 的 PR-as-a-request-surface 开关。
