# Banking OpenClaw MVP Backend / Runtime Engineering Checklist

## 1. 目标

这份 checklist 面向：

- `apps/api`
- `apps/gateway`
- `apps/tool-gateway`
- `packages/gateway-runtime`
- `packages/domain`

目标是把后端、运行时、工具治理相关的关键工程约束落成可执行检查项。

---

## 2. 必守规则

### 2.1 单文件行数限制

**每个代码文件不能超过 `500` 行。**

执行建议：

- `route`、`service`、`repository`、`adapter`、`schema`、`mapper` 分文件维护
- 文件接近 `350-400` 行时主动拆分
- 不允许出现：
  - 超长 service
  - 超长 runtime orchestrator
  - 超长 adapter 文件
  - 超长 prompt assembly 文件

### 2.2 权威状态单点明确

检查项：

- 正式对象是否统一由 `API Server + PostgreSQL` 持久化
- `gateway` 是否只持有运行态，不持有正式权威状态
- 是否避免多服务重复写同一主对象

### 2.3 事实与解释分离

检查项：

- `TaskExecution` 记录执行事实
- `AuditEvent` 记录治理事实
- `TaskEpisode` 记录劳动片段
- `Plan` 记录正式计划版本
- LLM 总结是否仅作为补充字段而非覆盖事实

---

## 3. API 层 Checklist

### 3.1 Route 保持薄

检查项：

- route 只做参数解析、schema 校验、调用 service、返回响应
- 不在 route 中直接拼 SQL
- 不在 route 中写长业务流程

### 3.2 Service 负责业务编排

检查项：

- service 是否只依赖明确接口
- service 是否围绕对象边界组织
- service 是否可测试，不依赖 HTTP 上下文细节

### 3.3 错误与幂等

检查项：

- 关键写接口是否考虑幂等
- 错误码是否标准化
- 日志里是否包含对象 ID

---

## 4. Runtime / Gateway Checklist

### 4.1 `RuntimeAdapter` 是唯一 runtime 边界

检查项：

- 上层业务是否只依赖 `RuntimeAdapter`
- 是否避免泄露底层 SDK 私有对象
- `Plan Agent` 和 `Execution Agent` 契约是否明确分开

### 4.2 Planning 和 Execution 分角色

检查项：

- `Plan Agent` 是否只做 planning
- `Execution Agent` 是否只基于正式 `Plan` 执行
- 是否避免一个超大 agent 承担所有职责

### 4.3 Context envelope 完整

检查项：

- 每次执行前是否组装：
  - session 摘要
  - task 信息
  - plan version
  - step 进度
  - 最近工具结果
  - 治理边界

### 4.4 自学习受控

检查项：

- learning candidate 是否来自执行事实
- 是否避免 assistant 直接覆盖正式 `AGENTS.md`
- 正式 skill / policy 更新是否需要审核

---

## 5. Tool Gateway Checklist

### 5.1 企业系统调用统一入口

检查项：

- 查询、草稿、预填、动作是否统一从 `Tool Gateway` 经过
- 是否统一处理鉴权、审计、审批、幂等、限流、重试

### 5.2 动作语义分层

检查项：

- 是否区分 `read / draft / prefill / action`
- 不同风险等级是否有不同控制策略

### 5.3 Connector 保持薄

检查项：

- connector 是否主要负责协议适配
- 是否避免在 connector 中堆业务语义

---

## 6. Prompt / Policy / Markdown Docs Checklist

### 6.1 Markdown policy 不是硬权限层

检查项：

- `AGENTS.md`、`TOOLS.md`、`approval-risk.md` 是否只承载行为与经验规则
- 真正权限和审批拦截是否由系统硬校验

### 6.2 文档版本化

检查项：

- 是否有 `docType / scopeType / version / checksum`
- 是否能回滚某一版 policy
- 是否能追溯某次执行用了哪版文档

### 6.3 Prompt 分角色

检查项：

- `plan-agent`
- `execution-agent`
- `summarizer`

是否分别维护、分别裁剪、分别测试

---

## 7. 可观测性 Checklist

日志上下文至少应带：

- `sessionId`
- `taskId`
- `planId`
- `executionId`
- `toolCallId`
- `requestId`

检查项：

- 是否能从用户请求追到后端执行链
- 是否能解释某次失败发生在哪一层
- 是否能知道本次执行使用了哪版 plan / prompt / policy

---

## 8. 后端反模式

- 单个后端代码文件超过 `500` 行
- route 里写长业务逻辑
- runtime 层直接依赖 SDK 私有对象
- 让 LLM 直接写权威业务主对象
- 在 `Tool Gateway` 里硬编码业务大脑
- 把 markdown policy 当成真正的权限控制层
- 没有对象 ID 的日志

---

## 9. 一句话结论

**后端与运行时 MVP 的最佳实践是：保持权威状态单点明确、让 runtime 边界清晰、让工具治理集中、让执行事实可审计，并严格执行单文件不超过 `500` 行。**
