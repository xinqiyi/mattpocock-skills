# CONTEXT.md 格式

## 结构

```md
# {Context 名称}

{一两句话描述这个 context 是什么以及为什么存在。}

## 语言

**Order**：
{该术语的一两句话描述}
_避免_： Purchase、 transaction

**Invoice**：
在交付后发送给客户的要求付款的请求。
_避免_： Bill、 payment request

**Customer**：
下达订单的个人或组织。
_避免_： Client、 buyer、 account
```

## 规则

- **要有主见。** 当多个词存在表示同一个概念时，选择最好的一个并将其他列在 `_Avoid_` 下。
- **保持定义简洁。** 最多一两句话。定义它_是_什么，而不是它做什么。
- **只包括该项目 context 特有的术语。** 通用编程概念（timeouts、 error types、 utility patterns）不属于，即使项目广泛使用它们。在添加术语之前，问：这是这个 context 特有的概念，还是一个通用的编程概念？只有前者才属于。
- **在自然聚类出现时，将术语分组在副标题下。** 如果所有术语属于一个单一的内聚区域，平铺列表也是可以的。

## 单 context 与多 context repos

**单 context （大多数 repos）：** 在 repo 根目录有一个 `CONTEXT.md`。

**多 contexts：** 在 repo 根目录有 `CONTEXT-MAP.md`，列出 contexts、它们所在的位置以及它们之间的关系：

```md
# Context Map

## Contexts

- [Ordering](./src/ordering/CONTEXT.md) — 接收和跟踪客户订单
- [Billing](./src/billing/CONTEXT.md) — 生成发票并处理付款
- [Fulfillment](./src/fulfillment/CONTEXT.md) — 管理仓库拣货和发货

## 关系

- **Ordering → Fulfillment**： Ordering 发出 `OrderPlaced` events； Fulfillment 消费它们以开始拣货
- **Fulfillment → Billing**： Fulfillment 发出 `ShipmentDispatched` events； Billing 消费它们以生成发票
- **Ordering ↔ Billing**：共享 `CustomerId` 和 `Money` 类型
```

Skill 推断适用哪种结构：

- 如果 `CONTEXT-MAP.md` 存在，读取它以找到 contexts
- 如果只有根目录的 `CONTEXT.md` 存在，则是单 context
- 如果两者都不存在，在第一个术语被解析时懒加载创建一个根目录 `CONTEXT.md`

当存在多个 contexts 时，推断当前话题与哪个相关。如果不清楚，询问。
