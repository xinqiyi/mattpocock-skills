---
name: migrate-to-shoehorn
description: Migrate test files from `as` type assertions to @total-typescript/shoehorn. Use when user mentions shoehorn, wants to replace `as` in tests, or needs partial test data.
---

# Migrate to Shoehorn

## 为什么用 shoehorn？

`shoehorn` 让你在测试中传递部分数据，同时保持 TypeScript 满意。它用类型安全的替代方案替换 `as` 断言。

**仅测试代码。** 绝不在生产代码中使用 shoehorn。

`as` 在测试中的问题：

- 被训练不使用它
- 必须手动指定目标类型
- 双重 as （`as unknown as Type`）用于故意错误的数据

## 安装

```bash
npm i @total-typescript/shoehorn
```

## 迁移模式

### 大型对象，只需少量属性

之前：

```ts
type Request = {
 body: { id: string };
 headers: Record<string, string>;
 cookies: Record<string, string>;
 // ...20 个更多属性
};

it("gets user by id", () => {
 // 只关心 body.id 但必须伪造整个 Request
 getUser({
 body: { id: "123" },
 headers: {},
 cookies: {},
 // ...伪造所有 20 个属性
 });
});
```

之后：

```ts
import { fromPartial } from "@total-typescript/shoehorn";

it("gets user by id", () => {
 getUser(
 fromPartial({
 body: { id: "123" },
 }),
 );
});
```

### `as Type` → `fromPartial()`

之前：

```ts
getUser({ body: { id: "123" } } as Request);
```

之后：

```ts
import { fromPartial } from "@total-typescript/shoehorn";

getUser(fromPartial({ body: { id: "123" } }));
```

### `as unknown as Type` → `fromAny()`

之前：

```ts
getUser({ body: { id: 123 } } as unknown as Request); // 故意错误类型
```

之后：

```ts
import { fromAny } from "@total-typescript/shoehorn";

getUser(fromAny({ body: { id: 123 } }));
```

## 何时使用每个

| 函数 | 用例 |
| -------------- | --------------------------------------------- |
| `fromPartial()` | 传递仍然通过类型检查的部分数据 |
| `fromAny()` | 传递故意错误的数据（保留自动完成） |
| `fromExact()` | 强制完整对象（以后可与 fromPartial 互换） |

## 工作流

1. **收集需求** ——询问用户：
 - 哪些测试文件有 `as` 断言导致问题？
 - 他们是在处理大型对象，其中只有一些属性重要吗？
 - 他们需要传递故意错误的数据来进行错误测试吗？

2. **安装和迁移**：
 - [ ] 安装：`npm i @total-typescript/shoehorn`
 - [ ] 查找带有 `as` 断言的测试文件：`grep -r " as [A-Z]" --include="*.test.ts" --include="*.spec.ts"`
 - [ ] 用 `fromPartial()` 替换 `as Type`
 - [ ] 用 `fromAny()` 替换 `as unknown as Type`
 - [ ] 从 `@total-typescript/shoehorn` 添加 imports
 - [ ] 运行类型检查以验证
