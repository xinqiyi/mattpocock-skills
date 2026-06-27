---
name: scaffold-exercises
description: Create exercise directory structures with sections, problems, solutions, and explainers that pass linting. Use when user wants to scaffold exercises, create exercise stubs, or set up a new course section.
---

# Scaffold Exercises

创建通过 `pnpm ai-hero-cli internal lint` 的练习目录结构，然后用 `git commit` 提交。

## 目录命名

- **Sections**：`exercises/` 内的 `XX-section-name/`（例如 `01-retrieval-skill-building`）
- **Exercises**： section 内的 `XX.YY-exercise-name/`（例如 `01.03-retrieval-with-bm25`）
- Section 编号 = `XX`， exercise 编号 = `XX.YY`
- 名称使用 dash-case （小写、连字符）

## 练习变体

每个练习至少需要以下子文件夹之一：

- `problem/` ——学生工作区，包含 TODOs
- `solution/` ——参考实现
- `explainer/` ——概念性材料，没有 TODOs

在存根时，除非计划另有规定，默认使用 `explainer/`。

## 必需文件

每个子文件夹（`problem/`、`solution/`、`explainer/`）需要一个 `readme.md`，它：

- **不能为空**（必须有实际内容，即使只是一个标题行）
- 没有损坏的链接

在存根时，创建一个最小 readme，包含标题和描述：

```md
# Exercise Title

Description here
```

如果子文件夹有代码，它还需要一个 `main.ts`（>1 行）。但对于存根，仅 readme 的练习也可以。

## 工作流

1. **解析计划** ——提取 section 名称、 exercise 名称和变体类型
2. **创建目录** ——为每个路径执行 `mkdir -p`
3. **创建存根 readmes** ——每个变体文件夹一个 `readme.md`，带标题
4. **运行 lint** ——`pnpm ai-hero-cli internal lint` 以验证
5. **修复任何错误** ——迭代直到 lint 通过

## Lint 规则摘要

Linter （`pnpm ai-hero-cli internal lint`）检查：

- 每个练习都有子文件夹（`problem/`、`solution/`、`explainer/`）
- 至少存在 `problem/`、`explainer/` 或 `explainer.1/` 之一
- `readme.md` 存在于主要子文件夹中且非空
- 没有 `.gitkeep` 文件
- 没有 `speaker-notes.md` 文件
- readmes 中没有损坏的链接
- readmes 中没有 `pnpm run exercise` 命令
- 每个子文件夹需要 `main.ts`，除非是仅 readme

## 移动/重命名练习

在重新编号或移动练习时：

1. 使用 `git mv`（而不是 `mv`）重命名目录——保留 git history
2. 更新数字前缀以保持顺序
3. 移动后重新运行 lint

示例：

```bash
git mv exercises/01-retrieval/01.03-embeddings exercises/01-retrieval/01.04-embeddings
```

## 示例：从计划存根

给定一个计划如：

```
Section 05: Memory Skill Building
- 05.01 Introduction to Memory
- 05.02 Short-term Memory (explainer + problem + solution)
- 05.03 Long-term Memory
```

创建：

```bash
mkdir -p exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer
mkdir -p exercises/05-memory-skill-building/05.02-short-term-memory/{explainer,problem,solution}
mkdir -p exercises/05-memory-skill-building/05.03-long-term-memory/explainer
```

然后创建 readme 存根：

```
exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer/readme.md -> "# Introduction to Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/explainer/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/problem/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/solution/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.03-long-term-memory/explainer/readme.md -> "# Long-term Memory"
```
