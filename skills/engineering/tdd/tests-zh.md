# 好测试与坏测试

## 好测试

**Integration-style**：通过真实的 interfaces 测试，而不是内部部分的 mocks。

```typescript
// 好：测试可观察行为
test("user can checkout with valid cart", async () => {
 const cart = createCart();
 cart.add(product);
 const result = await checkout(cart, paymentMethod);
 expect(result.status).toBe("confirmed");
});
```

特征：

- 测试用户/调用者关心的行为
- 仅使用公共 API
- 在内部重构中存活
- 描述 WHAT，而不是 HOW
- 每个测试一个逻辑断言

## 坏测试

**Implementation-detail 测试**：与内部结构耦合。

```typescript
// 坏：测试 implementation 细节
test("checkout calls paymentService.process", async () => {
 const mockPayment = jest.mock(paymentService);
 await checkout(cart, payment);
 expect(mockPayment.process).toHaveBeenCalledWith(cart.total);
});
```

红旗：

- Mocking 内部协作者
- 测试私有方法
- 断言调用次数/顺序
- 重构时测试失败但行为未改变
- 测试名称描述 HOW 而不是 WHAT
- 通过外部手段而不是 interface 验证

```typescript
// 坏：绕过 interface 验证
test("createUser saves to database", async () => {
 await createUser({ name: "Alice" });
 const row = await db.query("SELECT * FROM users WHERE name = ?", ["Alice"]);
 expect(row).toBeDefined();
});

// 好：通过 interface 验证
test("createUser makes user retrievable", async () => {
 const user = await createUser({ name: "Alice" });
 const retrieved = await getUser(user.id);
 expect(retrieved.name).toBe("Alice");
});
```
