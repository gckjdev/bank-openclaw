# Banking OpenClaw MVP Engineering & Coding Best Practices

## 1. 文档目的

这份文档用于沉淀当前 `Banking OpenClaw` MVP 架构下的关键工程实践与编码约定。

它不重复主架构文档里的系统设计，而是回答更落地的问题：

- 代码应该怎样组织
- 前端、后端、数据库、运行时分别该怎么写
- 哪些做法适合 MVP
- 哪些做法会让工程过早复杂化

适用范围：

- `apps/web`
- `apps/gateway`
- `apps/api`
- `apps/tool-gateway`
- `packages/*`
- `PostgreSQL` 与迁移
- prompt / policy / skills / markdown 文档体系

---

## 2. 跨层通用原则

### 2.0 文件尺寸纪律

这条规则在 MVP 阶段应视为强约束：

**单个代码文件不得超过 `500` 行。**

最佳实践：

- 当文件接近 `350-400` 行时就开始主动拆分
- 优先按职责拆分，而不是等到文件失控后再机械切块
- 拆分方向可包括：
  - routes / controllers / services / repositories
  - page / section / hook / view-model
  - prompt assembly / policy loader / runtime adapter
  - domain schema / mapper / calculator

避免：

- 用一个超长文件承载多个职责
- 因为“暂时方便”持续堆逻辑直到难以维护
- 用注释分段代替真实模块拆分

### 2.1 先保证闭环，再追求完美抽象

MVP 的首要目标是稳定打通主链路：

```text
Session
  -> Planning Interpretation
  -> Task Materialization
  -> Plan Commit
  -> Execution Agent
  -> Tool Gateway
  -> Audit
```

最佳实践：

- 优先保证闭环可运行、可调试、可审计
- 优先做清晰边界，而不是一次性做完所有终局能力
- 抽象只为当前架构服务，不为假想未来服务

避免：

- 过早拆成大量微服务
- 在 MVP 阶段引入太多异构语言和运行时
- 为“可能以后会用到”设计过深层的可插拔体系

### 2.2 Schema-first，边界先于实现

所有跨模块、跨进程、跨端边界都应先定义 schema。

最佳实践：

- 用 `Zod` 定义 DTO、协议和边界校验
- 先定义请求、响应、事件、领域对象，再写业务逻辑
- `gateway`、`api`、`web` 共用同一套核心类型和 schema

避免：

- 控制器先写逻辑，最后再补类型
- 各层重复各自定义一份字段名
- 用隐式约定替代显式 schema

### 2.3 权威状态必须单点明确

当前 MVP 下，正式对象权威存储在 `API Server + PostgreSQL`。

最佳实践：

- `Assistant Gateway` 负责运行态和控制面
- `API Server` 负责权威对象写入与查询
- `PostgreSQL` 是核心正式对象的唯一权威数据库

避免：

- 在 `gateway` 本地维护另一套正式任务状态
- 用缓存、内存或 markdown 文档冒充正式业务权威源
- 让多个服务各自直接修改同一类主对象

### 2.4 事实与解释分离

这条原则对 agent 系统尤其重要。

最佳实践：

- `TaskExecution` 记录事实
- `AuditEvent` 记录治理事实
- `TaskEpisode` 记录劳动片段
- `Plan` 记录当前计划版本
- LLM 的解释和总结应是补充，不应覆盖系统事实

避免：

- 把模型总结当成权威执行事实
- 用自然语言大段描述代替结构化关键字段
- 让 agent 自己决定所有对象边界

### 2.5 Markdown policy 是行为上下文，不是硬权限源

当前架构已经采用服务端托管 markdown 文档来承载 `AGENTS.md`、`TOOLS.md`、`approval-risk.md` 等内容。

最佳实践：

- markdown 文档承载行为指导、经验规则、成长协议
- 真正的权限、审批和风险拦截应由后端逻辑和 `Tool Gateway` 硬校验
- 运行时可以裁剪 markdown 内容后再注入模型

避免：

- 仅靠 prompt 约束高风险动作
- 把权限控制完全写进 `AGENTS.md`
- 把整份长文无差别注入每次运行

---

## 3. 前端 Best Practices

适用对象：`apps/web`

### 3.1 前端只做工作台与管理台，不承载业务真相

最佳实践：

- 员工工作入口和后台管理入口放在同一个前端应用中
- 前端通过 `gateway-client` 或 API client 读取数据
- 所有正式状态变更都通过后端接口完成

避免：

- 前端直接拼装领域主逻辑
- 在浏览器里维护一套脱离后端的任务状态机
- 直接从前端访问企业系统工具接口

### 3.2 区分 server state 和 UI state

最佳实践：

- `TanStack Query` 管理 `Session / Task / Plan / Execution` 这类 server state
- `Zustand` 只管理局部 UI state
- 表单状态和查询缓存不要混在一个 store 里

避免：

- 用一个全局 store 装所有数据
- 手工维护接口 loading / retry / cache 逻辑
- 把服务端对象拷贝多份到 UI 层四处修改

### 3.3 路由按业务目的组织

最佳实践：

- 面向页面目的设计路由，例如：
  - `/workbench/session/:sessionId`
  - `/tasks/:taskId`
  - `/admin/roles`
  - `/admin/task-templates`
  - `/admin/labor`
  - `/admin/substitution`
- 页面组件薄，业务拼装逻辑放到 hooks / services / view-model helpers

避免：

- 按技术层拆成过深的页面目录
- 一个页面同时承担会话、执行、分析、管理多种职责

### 3.4 流式交互与普通查询分离

最佳实践：

- 普通对象查询走 HTTP
- 流式会话、执行状态、增量输出走 `gateway` 的流式连接
- 前端要明确区分“已落库事实”和“正在流式生成中的临时内容”

避免：

- 把流式输出直接当成最终权威结果
- 流式状态与持久化状态混为一谈

---

## 4. API / 后端 Best Practices

适用对象：`apps/api`

### 4.1 Route 要薄，Service 要清晰

最佳实践：

- route 层只做：
  - 参数解析
  - schema 校验
  - 调用 service
  - 返回标准响应
- service 层负责业务流程
- repository / db 层负责持久化

避免：

- 在 route 里直接写大量业务逻辑
- route 里直接拼 SQL
- service 与 repository 互相穿透

### 4.2 领域对象边界要稳定

当前 MVP 的关键对象包括：

- `Session`
- `TaskInstance`
- `Plan`
- `TaskExecution`
- `ToolCall`
- `AuditEvent`
- `ProactiveJob`
- `TaskEpisode`
- `LaborLedger`
- `SubstitutionScore`

最佳实践：

- 一个对象回答一个核心问题
- 对象关系清晰，一对多/多对一显式定义
- 需要快照的地方显式保存 snapshot JSON

避免：

- 一个表同时承担多种语义
- 为了“省表”把不同对象硬塞在一起
- 频繁改变对象语义名称

### 4.3 接口要幂等、可追踪、可审计

最佳实践：

- 对关键写操作考虑幂等语义
- 关键操作记录：
  - `session_id`
  - `task_id`
  - `plan_id`
  - `execution_id`
  - `request_id`
- 输出标准错误码与错误分类

避免：

- 同一动作重复提交但无法识别
- 日志里缺少关键对象 ID
- 错误消息只返回一句自然语言

### 4.4 不让 LLM 直接写权威主对象

最佳实践：

- LLM 输出 planning 结果和 execution 决策
- 系统根据这些结果完成 `Task Materialization`、`Plan Commit`、审批落库和审计写入
- LLM 是决策参与者，不是权威数据写库者

避免：

- 让模型直接决定正式状态写入结构
- 让模型绕过后端对象层直接“提交业务事实”

---

## 5. Assistant Gateway / Runtime Best Practices

适用对象：`apps/gateway`、`packages/gateway-runtime`

### 5.1 RuntimeAdapter 是硬边界

最佳实践：

- 上层业务只依赖统一 `RuntimeAdapter`
- runtime provider 的差异被收口在 adapter 内部
- `Plan Agent` 与 `Execution Agent` 使用明确分离的输入输出契约

避免：

- 在业务代码中散落 SDK 私有对象
- 让 `gateway-runtime` 直接绑定某个模型 SDK 细节

### 5.2 Planning 和 Execution 必须分角色

最佳实践：

- `Plan Agent` 只做理解、规划、候选任务判断
- `Execution Agent` 只基于正式 `Plan` 推进执行
- prompt、markdown policy、skills 可以按角色裁剪

避免：

- 一个超大 agent 同时兼任 planning、execution、approval、audit
- 没有正式 `Plan` 就直接进入执行态

### 5.3 每次执行都喂完整上下文包

最佳实践：

- 运行时组装固定结构的 context envelope
- 至少包含：
  - session 摘要
  - task 信息
  - plan 当前版本
  - 已完成 / 待完成步骤
  - 最近工具结果
  - 治理边界

避免：

- 只把用户最后一句话发给模型
- 不带 plan version 执行
- 让模型自己从长对话推断当前进度

### 5.4 自学习要事实驱动

最佳实践：

- 从 `TaskExecution / ToolCall / AuditEvent / TaskEpisode` 提炼 learning candidate
- `AGENTS.md` 只定义成长协议
- 正式 `skills/*.md`、`TOOLS.md`、`approval-risk.md` 更新必须可审核

避免：

- 让 assistant 直接覆盖正式 `AGENTS.md`
- 只根据一次成功就自动发布新 skill

---

## 6. Tool Gateway Best Practices

适用对象：`apps/tool-gateway`

### 6.1 企业系统调用统一从 Tool Gateway 过

最佳实践：

- 查询、草稿、预填、低风险动作统一走 `Tool Gateway`
- 统一处理：
  - 鉴权
  - 审批
  - 审计
  - 幂等
  - 限流
  - 重试

避免：

- `Execution Agent` 直接调用企业系统
- 前端绕过 `Tool Gateway` 直连内部系统

### 6.2 工具按动作语义分层

最佳实践：

- 区分：
  - `read`
  - `draft`
  - `prefill`
  - `action`
- 不同动作类型使用不同审批与风控策略

避免：

- 用一个模糊的“tool call”承载所有风险等级
- 查询类与正式动作类完全共用一套处理链

### 6.3 Connector 要薄，领域语义在上层

最佳实践：

- connector 负责协议适配和重试
- 领域语义保留在 `Execution Agent` 或 composite tools / skills
- `Tool Gateway` 是治理边界，不是业务大脑

避免：

- 在 connector 中写大量业务规则
- 每个银行系统都演化出一套独立领域模型

---

## 7. 数据库 Best Practices

数据库层的细化约束已拆到独立文档：

- `docs/mvp/database-engineering-checklist.md`

主文档这里保留三条总原则：

- `PostgreSQL` 是核心正式对象的单一权威数据库
- schema、migration、索引都应围绕真实对象和真实查询路径设计
- JSON 字段用于快照和弹性结构，不替代显式建模

---

## 8. Prompt / Policy / Skills Best Practices

适用对象：`packages/prompts`、`packages/assistant-policies`、`packages/skills`

### 8.1 Prompt 分角色，不做超级总 prompt

最佳实践：

- `plan-agent`、`execution-agent`、`summarizer` 分开维护
- 共享原则单独维护，再按角色编译
- prompt 尽量短、稳定、可测试

避免：

- 一个巨型 prompt 承担所有角色
- 角色差异只靠几句临时条件判断

### 8.2 Markdown 文档要版本化、可审核

最佳实践：

- `AGENTS.md`、`TOOLS.md`、`approval-risk.md`、`assistant-charter.md` 服务端托管
- 使用轻量 frontmatter：
  - `docType`
  - `scopeType`
  - `version`
- 保存 `checksum`

避免：

- 文档内容无版本、无生效状态
- 多个人直接改正文但无审计

### 8.3 Skill 是经过验证的程序，不是随手记录

最佳实践：

- 只有重复出现、被验证过的流程才升级成正式 skill
- skill 内必须包含：
  - 适用场景
  - 前置条件
  - 步骤
  - 风险点
  - 验证方式

避免：

- 任何一次成功都沉淀成 skill
- 把业务制度原文直接当 skill

---

## 9. 测试与质量 Best Practices

测试层的细化约束已拆到独立文档：

- `docs/mvp/testing-quality-checklist.md`

主文档这里保留三条总原则：

- 测试预算优先投入关键主链路和高风险边界
- 单元、集成、e2e 应各自保护不同层次的问题
- mock 必须保留真实系统边界，不能把复杂性抹平

---

## 10. 日志、调试与可观测性 Best Practices

### 10.1 所有关键日志都带对象 ID

建议日志上下文至少包含：

- `sessionId`
- `taskId`
- `planId`
- `executionId`
- `toolCallId`
- `requestId`

### 10.2 先保证可解释，再追求复杂监控

MVP 阶段优先级应是：

- 能看懂一次执行过程
- 能定位失败在哪一层
- 能追溯某个输出基于哪版 plan / prompt / policy

避免：

- 一开始就堆复杂 APM，但关键日志缺失
- 无法把一条用户请求串到后端执行事实

---

## 11. 明确应避免的反模式

- 把 LLM 输出当正式数据库写入真相源
- 用前端本地状态替代权威任务状态
- 在 `gateway` 和 `api` 中重复维护同一对象
- 在 `Tool Gateway` 内硬编码大量业务语义
- 把所有规则写成一份超长 prompt
- 把 markdown policy 当成真正的权限控制层
- 为尚未验证的未来需求过度设计
- 忽视执行历史与审计留痕

---

## 12. 一句话结论

**当前这版 MVP 工程最佳实践的核心，不是“把每层都做到最强”，而是：围绕主链路建立清晰边界、统一类型、显式状态、受控运行时和可审计事实。**

---

## 13. 配套 Checklist

为了让团队更容易落地，建议结合下面两份专题 checklist 一起使用：

- `docs/mvp/frontend-engineering-checklist.md`
- `docs/mvp/backend-runtime-engineering-checklist.md`
- `docs/mvp/database-engineering-checklist.md`
- `docs/mvp/testing-quality-checklist.md`
