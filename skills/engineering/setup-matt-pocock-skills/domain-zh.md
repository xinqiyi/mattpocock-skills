# Domain Docs

Engineering skills 在探索代码库时应如何消费此 repo 的领域文档。

## 探索前，阅读这些

- **`CONTEXT.md`** 在 repo 根目录，或
- **`CONTEXT-MAP.md`** 在 repo 根目录（如果存在）——它指向每个 context 的 `CONTEXT.md`。阅读与主题相关的每个。
- **`docs/adr/`**——阅读涉及你即将工作的区域的 ADR。在多 context repos 中，也检查 `src/<context>/docs/adr/` 寻找 context 范围的决策。

如果这些文件中的任何一个不存在，**静默继续**。不要标记它们的缺失；不要建议提前创建它们。`/domain-modeling` skill （通过 `/grill-with-docs` 和 `/improve-codebase-architecture` 访问）在术语或决策实际被解析时懒加载创建它们。

## 文件结构

单 context repo （大多数 repos）：

```
/
├── CONTEXT.md
├── docs/adr/
│ ├── 0001-event-sourced-orders.md
│ └── 0002-postgres-for-write-model.md
└── src/
```

多 context repo （根目录存在 `CONTEXT-MAP.md`）：

```
/
├── CONTEXT-MAP.md
├── docs/adr/ ← 系统级决策
└── src/
 ├── ordering/
 │ ├── CONTEXT.md
 │ └── docs/adr/ ← context 特定决策
 └── billing/
 ├── CONTEXT.md
 └── docs/adr/
```

## 使用 Glossary 的词汇

当你的输出命名一个领域概念时（在 issue 标题、重构提议、假设、测试名称中），使用 `CONTEXT.md` 中定义的术语。不要漂移到 glossary 明确避免的同义词。

如果你需要的概念还不在 glossary 中，那是一个信号——要么你在发明项目不使用的语言（重新考虑），要么有一个真实的缺口（标记给 `/domain-modeling`）。

## 标记 ADR 冲突

如果你的输出与现有 ADR 矛盾，显式暴露它，而不是静默覆盖：

> _Contradicts ADR-0007 (event-sourced orders) — but worth reopening because …_
