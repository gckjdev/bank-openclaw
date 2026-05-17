# Banking OpenClaw MVP Testing & Quality Checklist

## 1. 目标

这份 checklist 面向：

- 单元测试
- 集成测试
- 端到端测试
- mock 策略
- 质量门槛

目标是保证 MVP 优先把测试预算投入到关键闭环和高风险边界。

---

## 2. 必守规则

### 2.1 单文件行数限制

**每个测试相关代码文件不能超过 `500` 行。**

适用对象包括：

- test file
- fixture file
- mock helper file
- e2e scenario file

### 2.2 先测关键闭环，不堆低价值测试

检查项：

- 是否优先覆盖真正高风险主链路
- 是否避免写大量噪音 snapshot 测试
- 是否避免把测试预算浪费在低价值文案细节上

---

## 3. 测试分层 Checklist

### 3.1 单元测试

优先覆盖：

- 规则判断
- 聚合逻辑
- mapper / transformer
- calculator
- schema 校验相关纯函数

检查项：

- 是否保持纯函数可测试
- 是否避免单元测试依赖完整运行环境

### 3.2 集成测试

优先覆盖：

- API
- repository
- gateway protocol
- tool gateway adapters

检查项：

- 是否覆盖真实边界而不是只测 happy path
- 是否覆盖错误、失败、审批等待等场景

### 3.3 端到端测试

优先覆盖：

- session -> planning -> task materialization
- task -> plan commit -> execution
- execution -> tool gateway -> audit
- task episode -> labor ledger -> substitution

检查项：

- 是否选择最关键业务闭环而不是全站铺满
- 是否覆盖核心后台页面和关键状态切换

---

## 4. Mock 策略 Checklist

### 4.1 Mock 保留真实边界

检查项：

- mock LLM 输出时是否保留角色差异
- mock tool 结果时是否保留成功 / 失败 / 审批等待场景
- mock 是否避免把真实边界抹平

### 4.2 Fixture 保持可读

检查项：

- fixture 是否按场景组织
- 是否避免一个超长 fixture 文件承载所有场景
- 是否避免神秘字段或无语义样例

---

## 5. 质量门槛 Checklist

### 5.1 每次改动至少回答三个问题

检查项：

- 这次改动影响了哪个闭环
- 是否新增了新的失败路径
- 是否需要补单元、集成或 e2e 中的至少一层验证

### 5.2 高风险改动必须验证

高风险改动包括：

- 任务对象语义变化
- plan / execution 协议变化
- tool gateway 调用链变化
- 劳动记账 / 替代分析计算变化
- prompt / policy 裁剪逻辑变化

检查项：

- 是否至少做了有针对性的验证

---

## 6. 推荐工具

- 单元 / 集成测试：`Vitest`
- 前端 / 工作台 e2e：`Playwright`

---

## 7. 测试反模式

- 单个测试文件超过 `500` 行
- 所有测试都只走 happy path
- mock 结果过于理想化
- 用大量 snapshot 替代关键行为验证
- 不知道这条测试到底在保护哪个闭环

---

## 8. 一句话结论

**测试与质量 MVP 的最佳实践是：把验证资源集中到关键闭环、真实边界和高风险变更上，并严格执行单文件不超过 `500` 行。**
