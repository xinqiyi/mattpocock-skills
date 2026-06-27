---
name: obsidian-vault
description: Search, create, and manage notes in the Obsidian vault with wikilinks and index notes. Use when user wants to find, create, or organize notes in Obsidian.
---

# Obsidian Vault

## Vault 位置

`/mnt/d/Obsidian Vault/AI Research/`

主要平铺在根级别。

## 命名约定

- **Index notes**：聚合相关主题（例如 `Ralph Wiggum Index.md`、`Skills Index.md`、`RAG Index.md`）
- 所有笔记名称使用**标题大小写**
- 不使用文件夹来组织——使用 links 和 index notes 代替

## 链接

- 使用 Obsidian `[[wikilinks]]` 语法：`[[Note Title]]`
- 笔记在底部链接到依赖/相关笔记
- Index notes 只是 `[[wikilinks]]` 的列表

## 工作流

### 搜索笔记

```bash
# 按文件名搜索
find "/mnt/d/Obsidian Vault/AI Research/" -name "*.md" | grep -i "keyword"

# 按内容搜索
grep -rl "keyword" "/mnt/d/Obsidian Vault/AI Research/" --include="*.md"
```

或直接在 vault 路径上使用 Grep/Glob 工具。

### 创建新笔记

1. 文件名使用**标题大小写**
2. 作为学习单元编写内容（根据 vault 规则）
3. 在底部向相关笔记添加 `[[wikilinks]]`
4. 如果是编号序列的一部分，使用分层编号方案

### 查找相关笔记

搜索整个 vault 中的 `[[Note Title]]` 以找到反向链接：

```bash
grep -rl "\\[\\[Note Title\\]\\]" "/mnt/d/Obsidian Vault/AI Research/"
```

### 查找 Index Notes

```bash
find "/mnt/d/Obsidian Vault/AI Research/" -name "*Index*"
```
