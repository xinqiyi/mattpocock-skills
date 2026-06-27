---
name: setup-pre-commit
description: Set up Husky pre-commit hooks with lint-staged (Prettier), type checking, and tests in the current repo. Use when user wants to add pre-commit hooks, set up Husky, configure lint-staged, or add commit-time formatting/typechecking/testing.
---

# 设置 Pre-Commit Hooks

## 这会设置什么

- **Husky** pre-commit hook
- **lint-staged** 在所有已 stage 文件上运行 Prettier
- **Prettier** 配置（如果缺失）
- **typecheck** 和 **test** 脚本在 pre-commit hook 中

## 步骤

### 1. 检测包管理器

检查 `package-lock.json`（npm）、`pnpm-lock.yaml`（pnpm）、`yarn.lock`（yarn）、`bun.lockb`（bun）。使用存在的那个。如果不明确，默认为 npm。

### 2. 安装依赖

安装为 devDependencies：

```
husky lint-staged prettier
```

### 3. 初始化 Husky

```bash
npx husky init
```

这会创建 `.husky/` 目录并将 `prepare: "husky"` 添加到 package.json。

### 4. 创建 `.husky/pre-commit`

写入此文件（Husky v9+ 不需要 shebang）：

```
npx lint-staged
npm run typecheck
npm run test
```

**适配**：用检测到的包管理器替换 `npm`。如果 repo 在 package.json 中没有 `typecheck` 或 `test` 脚本，省略那些行并告知用户。

### 5. 创建 `.lintstagedrc`

```json
{
 "*": "prettier --ignore-unknown --write"
}
```

### 6. 创建 `.prettierrc`（如果缺失）

仅当没有 Prettier 配置存在时创建。使用这些默认值：

```json
{
 "useTabs": false,
 "tabWidth": 2,
 "printWidth": 80,
 "singleQuote": false,
 "trailingComma": "es5",
 "semi": true,
 "arrowParens": "always"
}
```

### 7. 验证

- [ ] `.husky/pre-commit` 存在且可执行
- [ ] `.lintstagedrc` 存在
- [ ] package.json 中的 `prepare` 脚本是 `"husky"`
- [ ] `prettier` 配置存在
- [ ] 运行 `npx lint-staged` 验证其工作

### 8. 提交

Stage 所有更改/创建的文件并使用消息提交：`Add pre-commit hooks (husky + lint-staged + prettier)`

这将通过新的 pre-commit hooks 运行——这是所有内容都正常工作的良好冒烟测试。

## 说明

- Husky v9+ 不需要 hook 文件中的 shebangs
- `prettier --ignore-unknown` 跳过 Prettier 无法解析的文件（图片等）
- Pre-commit 先运行 lint-staged （快速，仅 stage），然后完整 typecheck 和 tests
