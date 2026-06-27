---
name: ubiquitous-language
description: Extract a DDD-style ubiquitous language glossary from the current conversation, flagging ambiguities and proposing canonical terms. Saves to UBIQUITOUS_LANGUAGE.md. Use when user wants to define domain terms, build a glossary, harden terminology, create a ubiquitous language, or mentions "domain model" or "DDD".
disable-model-invocation: true
---

# Ubiquitous Language

从当前对话中提取并形式化领域术语为一个一致的 glossary，保存到本地文件。

## 流程

1. **扫描对话**中的领域相关名词、动词和概念
2. **识别问题**：
 - 同一个词用于不同概念（歧义）
 - 不同词用于相同概念（同义词）
 - 模糊或过载的术语
3. **提出规范的 glossary**，带有有见地的术语选择
4. **写入 `UBIQUITOUS_LANGUAGE.md`** 到工作目录，使用以下格式
5. **在对话中内联输出摘要**

## 输出格式

编写一个具有以下结构的 `UBIQUITOUS_LANGUAGE.md` 文件：

```md
# Ubiquitous Language

## Order lifecycle

| 术语 | 定义 | 要避免的别名 |
| ----------- | --------------------------------------------- | -------------------- |
| **Order** | 客户购买一个或多个商品的请求 | Purchase, transaction|
| **Invoice** | 交付后发送给客户的要求付款的请求 | Bill, payment request|

## People

| 术语 | 定义 | 要避免的别名 |
| -------------- | --------------------------------------- | --------------------- |
| **Customer** | 下达订单的个人或组织 | Client, buyer, account|
| **User** | 系统中的认证身份 | Login, account |

## 关系

- 一个 **Invoice** 属于恰好一个 **Customer**
- 一个 **Order** 产生一个或多个 **Invoices**

## 示例对话

> **Dev：** "当 **Customer** 下达 **Order** 时，我们立即创建 **Invoice** 吗？"
> **领域专家：** "不——**Invoice** 只有在 **Fulfillment** 被确认后才生成。单个 **Order** 可以产生多个 **Invoices**，如果 items 在不同的 **Shipments** 中发货。"
> **Dev：** "所以如果一个 **Shipment** 在发运前被取消，就不会有 **Invoice** 存在？"
> **领域专家：** "完全正确。**Invoice** 生命周期与 **Fulfillment** 绑定，而不是 **Order**。"

## 已标记的歧义

- "account" 被用来同时指代 **Customer** 和 **User**——这些是不同的概念：**Customer** 下达订单，而 **User** 是一个认证身份，可能代表也可能不代表一个 **Customer**。
```

## 规则

- **要有主见。** 当多个词存在于同一个概念时，选择最好的一个并将其他列为要避免的别名。
- **显式标记冲突。** 如果一个术语在对话中被歧义地使用，在"Flagged ambiguities" section 中标出来，附上明确的建议。
- **只包括对领域专家相关的术语。** 跳过 modules 或 classes 的名称，除非它们在领域语言中有含义。
- **保持定义简洁。** 最多一句话。定义它_是_什么，而不是它做什么。
- **展示关系。** 使用粗体术语名称，并在明显时表达基数。
- **只包括领域术语。** 跳过通用编程概念（array、 function、 endpoint），除非它们有领域特定的含义。
- **当自然聚类出现时，将术语分组到多个表格中**（例如按子域、生命周期或参与者）。每个组获得自己的标题和表格。如果所有术语属于一个单一的内聚领域，一个表格也可以——不要强行分组。
- **编写一个示例对话。** 一个开发者与领域专家之间简短的对话（3-5 轮），展示术语如何自然地相互作用。对话应澄清相关概念之间的边界，并显示术语被精确使用的场景。
