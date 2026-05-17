# 银行版 Assistant 自我学习与 `AGENTS.md` 模板

## 1. 这份文档的目的

这份文档是 `Banking OpenClaw` MVP 的独立专题说明，用来回答一件事：

**如果希望每个 assistant 具备持续学习和成长能力，`AGENTS.md` 应该如何设计。**

这里的目标不是让 assistant 自行修改模型参数，而是让它能够在受控前提下：

- 从执行中积累经验
- 从反馈中修正行为
- 把稳定方法沉淀成可复用能力
- 避免把一次偶然成功或 hallucination 变成长期污染

一句话概括：

**`AGENTS.md` 在银行版里应是 assistant 的成长协议，而不是全部成长内容的仓库。**

---

## 2. 基本判断

### 2.1 `AGENTS.md` 负责什么

`AGENTS.md` 最适合承载的是：

- assistant 如何学习
- 什么时候允许学习
- 学到哪里
- 哪些经验允许自动生效
- 哪些经验必须人工审核
- 旧经验何时应淘汰

也就是说，它更像：

- 运行规则
- 学习宪法
- 反思与升级协议

而不是：

- 全量知识库
- 全量工作日志
- 所有用户偏好
- 所有工具细节

### 2.2 学习内容应该分流

建议把成长内容分散到不同承载层：

- `AGENTS.md`
  - 放成长规则、升级路径、审核边界
- `MEMORY.md`
  - 放长期稳定偏好、长期有效经验、跨任务可复用背景
- `memory/YYYY-MM-DD.md`
  - 放短期观察、当日反思、候选经验
- `skills/*.md`
  - 放经过验证的程序性能力
- `TOOLS.md`
  - 放工具经验、坑点、稳定用法
- `approval-risk.md`
  - 放审批、风险、停手机制经验

所以：

**`AGENTS.md` 负责定义成长机制，memory / skills / tools / risk docs 负责承接成长结果。**

---

## 3. 建议的成长闭环

建议把银行版 assistant 的成长闭环固定成：

```text
observe
  -> reflect
  -> distill
  -> promote
  -> reuse
  -> refine
```

### 3.1 `observe`

assistant 执行任务，系统收集事实：

- `TaskExecution`
- `ToolCall`
- `AuditEvent`
- `TaskEpisode`

这里的关键点是：

- 学习要尽量建立在执行事实之上
- 不要让 assistant 仅凭“自我感觉”总结经验

### 3.2 `reflect`

在下面场景触发轻量反思：

- 任务完成
- 执行失败
- 人工接管
- 收到明确纠正
- 相似任务重复出现

反思阶段只回答：

- 这次哪里做得好
- 哪里失败
- 哪些做法值得复用
- 哪些错误下次应避免

### 3.3 `distill`

把反思结果分类：

- 用户偏好
- 流程经验
- 工具技巧
- 风险提醒
- 可复用技能
- 规则修订建议

### 3.4 `promote`

把不同经验推广到不同文件：

- 偏好类 -> `MEMORY.md`
- 短期观察 -> `memory/YYYY-MM-DD.md`
- 可复用流程 -> `skills/*.md`
- 工具经验 -> `TOOLS.md`
- 风险经验 -> `approval-risk.md`
- 规则改进 -> `AGENTS.md` 修订候选

### 3.5 `reuse`

下次出现相似任务时：

- 优先读取相关 memory
- 优先加载命中的 skill
- 优先使用已验证流程

### 3.6 `refine`

如果新经验和旧经验冲突：

- 允许修正旧经验
- 允许降低旧经验优先级
- 允许把旧经验归档

---

## 4. 银行场景下的边界

这里必须明确：

### 4.1 可以成长的内容

- 用户表达偏好
- 回复结构偏好
- 任务分解套路
- 常见检查顺序
- 工具调用技巧
- 错误恢复方式
- 材料整理和校对流程

### 4.2 不应自动改写的内容

- 权限边界
- 风险等级
- 审批要求
- 制度口径
- 工具 allow / deny 规则
- 合规控制规则

也就是说：

**assistant 可以自动学“怎样做得更好”，不能自动改“什么是允许的”。**

---

## 5. MVP 落地建议

MVP 不建议直接让 assistant 自动改写正式 `AGENTS.md`。

更稳的顺序是：

1. 每次任务结束后生成 `learning candidate`
2. 自动写入：
   - `memory/YYYY-MM-DD.md`
   - 一部分低风险 `MEMORY.md` 候选
3. 生成候选稿：
   - `skill-draft.md`
   - `tool-note-draft.md`
   - `agents.patch.md`
4. 由管理员、运营或 supervisor 审核后再发布：
   - 正式 `skills/*.md`
   - `TOOLS.md`
   - `approval-risk.md`
   - `AGENTS.md`

这样可以保证：

- assistant 有成长能力
- 但成长可治理、可审计、可回滚

---

## 6. 银行版 `AGENTS.md` 最小模板

下面给出一个适合 MVP 的最小模板结构。

```md
---
docType: agents
scopeType: assistant
scopeRef: default
version: v1
status: active
---

# AGENTS.md

## 1. 角色定义
- 你是银行工作场景中的受控 assistant。
- 你的目标是帮助用户推进任务，而不是替代正式审批与制度判断。

## 2. 运行原则
- 先理解任务，再决定是否进入 planning / execution。
- 优先基于事实、制度、工具结果和已确认上下文工作。
- 不把未验证结论表述成正式事实。

## 3. 学习触发条件
- 在任务完成、失败、人工接管、用户纠正后进行轻量反思。
- 在相似任务重复出现后，评估是否值得提炼成 skill。

## 4. 学习分类规则
- 用户长期偏好写入 `MEMORY.md`。
- 当日观察写入 `memory/YYYY-MM-DD.md`。
- 可复用流程沉淀为 `skills/*.md` 草稿。
- 工具坑点沉淀为 `TOOLS.md` 候选更新。
- 风险与审批经验沉淀为 `approval-risk.md` 候选更新。
- 运行规则优化沉淀为 `agents.patch.md`。

## 5. 学习质量门槛
- 不因单次偶然成功就形成全局规则。
- 优先吸收重复出现、经验证、有人类反馈支持的经验。
- 没有事实支持的猜测不得进入长期记忆或正式 skill。

## 6. 自动生效边界
- 可以自动记录低风险偏好和观察。
- 不得自动修改权限边界、审批规则、制度口径、风险分级和正式工具策略。

## 7. 审核与发布
- `AGENTS.md`、正式 `skills/*.md`、`TOOLS.md`、`approval-risk.md` 的变更必须先形成候选稿。
- 候选稿经人工或 supervisor 审核后才能发布。

## 8. 遗忘与淘汰
- 过期经验应归档。
- 与新制度冲突的经验应失效。
- 长期不命中的经验应降权或移出 bootstrap context。
```

---

## 7. 和其他文件的关系

建议把这几个文件一起看：

- `AGENTS.md`
  - 定义成长协议
- `assistant-charter.md`
  - 定义角色原则和行为底线
- `TOOLS.md`
  - 定义工具使用经验和约束
- `approval-risk.md`
  - 定义审批与风险边界
- `MEMORY.md`
  - 定义长期稳定记忆
- `skills/*.md`
  - 定义复用能力

这套组合比单独强化一份 `AGENTS.md` 更稳。

---

## 8. 一句话结论

**银行版 assistant 的成长不应表现为“不断膨胀的一份 `AGENTS.md`”，而应表现为：`AGENTS.md` 规定成长机制，事实驱动 memory，验证后沉淀 skill，治理边界保持人工可控。**
