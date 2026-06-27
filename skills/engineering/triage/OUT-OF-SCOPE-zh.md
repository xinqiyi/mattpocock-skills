# Out-of-Scope 知识库

repo 中的 `.out-of-scope/` 目录存储被拒绝的 feature requests 的持久记录。它服务于两个目的：

1. **机构记忆** ——为什么一个功能被拒绝，这样当 issue 关闭时理由不会丢失
2. **去重** ——当一个新的 issue 进来匹配先前的拒绝时， skill 可以暴露先前的决定，而不是重新争论它

## 目录结构

```
.out-of-scope/
├── dark-mode.md
├── plugin-system.md
└── graphql-api.md
```

每个**概念**一个文件，而不是每个 issue。请求同一事物的多个 issues 被分组在一个文件下。

## 文件格式

文件应以放松、可读的风格编写——更像一个简短的 design document 而不是数据库条目。使用段落、代码示例和示例以使理由清晰，并对第一次遇到它的人有用。

```markdown
# Dark Mode

此项目不支持 dark mode 或面向用户的 theming。

## 为什么这属于 out of scope

渲染 pipeline 假设在 `ThemeConfig` 中定义的单色板。
支持多个主题需要：

- 一个包裹整个 component tree 的 theme context provider
- 每个 component 的 theme-aware 样式解析
- 用户主题偏好的持久化层

这是一个重大的架构变更，与项目在内容创作上的
重点不一致。 Theming 是嵌入或重新分发输出的下游
消费者关心的问题。

```ts
// 当前的 ThemeConfig interface 不是为运行时切换设计的：
interface ThemeConfig {
 colors: ColorPalette; // 单色板，构建时解析
 fonts: FontStack;
}
```

## 先前的请求

- #42 —— "Add dark mode support"
- #87 —— "Night theme for accessibility"
- #134 —— "Dark theme option"
```

### 命名文件

使用简短、描述性的 kebab-case 名称表示概念：`dark-mode.md`、`plugin-system.md`、`graphql-api.md`。名称应足够可识别，使得浏览目录的人在不用打开文件的情况下就能理解什么被拒绝了。

### 编写理由

理由应是实质性的——不是"我们不想要这个"而是为什么。好的理由引用：

- 项目范围或理念（"此项目专注于 X； theming 是下游关心的问题"）
- 技术约束（"支持此功能需要 Y，这与我们的 Z 架构冲突"）
- 战略决策（"我们选择使用 A 而不是 B 因为……"）

理由应是持久的。避免引用临时情况（"我们现在太忙了"）——那些不是真正的拒绝，而是推迟。

## 何时检查 `.out-of-scope/`

在 triage 期间（步骤 1：收集 context），读取 `.out-of-scope/` 中的所有文件。在评估新 issue 时：

- 检查请求是否与现有的 out-of-scope 概念匹配
- 匹配按概念相似性，而不是关键词——"night theme"匹配 `dark-mode.md`
- 如果有匹配，向维护者暴露它："这与 `.out-of-scope/dark-mode.md` 类似——我们之前拒绝了这个因为[理由]。你还有同样的感觉吗？"

维护者可以：

- **确认** ——新 issue 被添加到现有文件的"Prior requests"列表，然后关闭
- **重新考虑** —— out-of-scope 文件被删除或更新， issue 通过正常 triage 路径
- **不同意** —— issues 相关但有区别，通过正常 triage 继续

## 何时写入 `.out-of-scope/`

仅当一个 **enhancement**（不是 bug）被*拒绝*为 `wontfix` 时。这同样适用于 enhancement PRs，就像适用于 issues ——一个被拒绝的 PR 在这里记录，这样相同的请求不会作为新代码返回。

当某些东西因为**已实现**而被关闭为 `wontfix` 时，**不要**在这里写入。那是一个已构建的功能，而不是被拒绝的；记录它会用错误的拒绝污染去重检查。相反，关闭 comment 指向功能已经存在的位置。

流程：

1. 维护者决定一个 feature request 属于 out of scope
2. 检查是否存在匹配的 `.out-of-scope/` 文件
3. 如果有：将新 issue 追加到"Prior requests"列表
4. 如果没有：用概念名称、决策、理由和第一个先前的请求创建新文件
5. 在 issue 上发布 comment 解释决策并提及 `.out-of-scope/` 文件
6. 用 `wontfix` label 关闭 issue

## 更新或删除 Out-of-Scope 文件

如果维护者改变了对先前被拒绝的概念的看法：

- 删除 `.out-of-scope/` 文件
- Skill 不需要重新打开旧的 issues ——它们是历史记录
- 触发重新考虑的新 issue 通过正常 triage 继续
