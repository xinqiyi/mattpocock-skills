# Deepening

如何在给定依赖的情况下安全地深化一组浅层 modules。假设已掌握 [SKILL-zh.md](SKILL-zh.md) 中的词汇——**module**、**interface**、**seam**、**adapter**。

## 依赖分类

在评估深化候选对象时，对其依赖进行分类。分类决定了深化后的 module 如何在其 seam 上进行测试。

### 1. In-process

纯计算、 in-memory 状态、无 I/O。总是可深化——合并 modules 并通过新的 interface 直接测试。不需要 adapter。

### 2. Local-substitutable

有本地测试替身的依赖（用于 Postgres 的 PGLite、 in-memory filesystem）。如果替身存在即可深化。深化后的 module 使用在测试套件中运行的替身进行测试。 Seam 是内部的； module 外部 interface 处没有 port。

### 3. Remote but owned （Ports & Adapters）

你自己的跨网络边界服务（microservices、内部 APIs）。在 seam 处定义一个 **port**（interface）。深层 module 拥有逻辑；传输层作为 **adapter** 注入。测试使用 in-memory adapter。生产环境使用 HTTP/gRPC/queue adapter。

建议的表述：*"在 seam 处定义一个 port，为生产环境实现一个 HTTP adapter，为测试实现一个 in-memory adapter，这样逻辑即使部署在网络上也位于一个深层 module 中。"*

### 4. True external （Mock）

你无法控制的第三方服务（Stripe、 Twilio 等）。深化后的 module 将外部依赖作为注入的 port；测试提供一个 mock adapter。

## Seam 纪律

- **一个 adapter 意味着假设性的 seam。两个 adapter 意味着真正的 seam。** 除非至少有两个 adapter 是合理的（通常是生产环境 + 测试），否则不要引入 port。单 adapter 的 seam 只是间接层。
- **内部 seams vs 外部 seams。** 一个深层 module 可以有内部 seams （对其 implementation 私有，由其自己的测试使用）以及其 interface 处的外部 seam。不要仅仅因为测试使用就通过 interface 暴露内部 seams。

## 测试策略：替换，不要分层

- 一旦针对深化后 module 的 interface 的测试存在，旧的浅层 modules 上的单元测试就变成了浪费——删除它们。
- 在深化后 module 的 interface 处编写新测试。**Interface 就是测试面**。
- 测试断言通过 interface 的可观察结果，而不是内部状态。
- 测试应该能在内部重构中存活——它们描述行为，而不是 implementation。如果一个测试在 implementation 变更时必须改变，它就是在测试越过 interface。
