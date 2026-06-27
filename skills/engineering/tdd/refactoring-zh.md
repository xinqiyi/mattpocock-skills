# 重构候选对象

在 TDD 循环后，寻找：

- **重复** → 提取 function/class
- **长方法** → 拆分为私有 helpers （在公共 interface 上保持测试）
- **浅层 modules** → 合并或深化
- **Feature envy** → 将逻辑移到数据所在的位置
- **基本类型痴迷** → 引入 value objects
- **现有代码**被新代码揭示出问题
