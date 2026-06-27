# 何时 Mock

仅在**系统边界**处 mock：

- 外部 APIs （支付、邮件等）
- 数据库（有时——优先使用测试 DB）
- 时间/随机性
- 文件系统（有时）

不要 mock：

- 你自己的 classes/modules
- 内部协作者
- 你控制的任何东西

## 为 Mockability 设计

在系统边界处，设计易于 mock 的 interfaces：

**1. 使用依赖注入**

将外部依赖传入而不是在内部创建它们：

```typescript
// 易于 mock
function processPayment(order, paymentClient) {
 return paymentClient.charge(order.total);
}

// 难以 mock
function processPayment(order) {
 const client = new StripeClient(process.env.STRIPE_KEY);
 return client.charge(order.total);
}
```

**2. 优先选择 SDK 风格的 interfaces 而非通用 fetcher**

为每个外部操作创建特定的函数，而不是一个带条件逻辑的通用函数：

```typescript
// 好：每个函数独立可 mock
const api = {
 getUser: (id) => fetch(`/users/${id}`),
 getOrders: (userId) => fetch(`/users/${userId}/orders`),
 createOrder: (data) => fetch('/orders', { method: 'POST', body: data }),
};

// 坏： Mocking 需要在 mock 内部进行条件逻辑
const api = {
 fetch: (endpoint, options) => fetch(endpoint, options),
};
```

SDK 方法意味着：
- 每个 mock 返回一种特定形态
- 测试设置中没有条件逻辑
- 更容易看到测试使用了哪些 endpoints
- 每个 endpoint 的类型安全
