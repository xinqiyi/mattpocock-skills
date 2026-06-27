# Matt Pocock Skills

一组由 Claude Code 加载的 agent skills （slash commands 和行为）。 Skills 按 bucket 组织，由 `/setup-matt-pocock-skills` 输出的按 repo 配置来消费。

## 语言

**Issue tracker**：
托管 repo issues 的工具—— GitHub Issues、 Linear、本地 `.scratch/` markdown 约定，或类似的工具。如 `to-issues`、`to-prd`、`triage` 和 `qa` 等 skills 会读写它。
_避免_： backlog manager、 backlog backend、 issue host

**Issue**：
**Issue tracker** 内部的一个可追踪的独立工作单元——一个 bug、 task、 PRD 或由 `to-issues` 产生的 slice。
_避免_： ticket （仅在引用外部系统称其为 ticket 时使用）

**Triage role**：
在 triage 过程中应用于 **Issue** 的 canonical 状态机 label （例如 `needs-triage`、`ready-for-afk`）。每个 role 通过 `docs/agents/triage-labels.md` 映射到 **Issue tracker** 中的真实 label 字符串。

## 关系

- 一个 **Issue tracker** 包含多个 **Issues**
- 一个 **Issue** 一次携带一个 **Triage role**

## 已标记的歧义

- "backlog" 之前被用来既指托管 issue 的 *工具*，又指其中的 *工作集合*——已解决：工具是 **Issue tracker**；"backlog" 不再作为 domain term 使用。
- "backlog backend" / "backlog manager" ——已解决：合并为 **Issue tracker**。
