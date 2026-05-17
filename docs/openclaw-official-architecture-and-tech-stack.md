# OpenClaw 官方架构与技术栈补充研究

## 一、写这篇的目的

前面的几篇 OpenClaw 相关文档，主要回答的是：

- OpenClaw 对银行员工 AI Assistant 有什么启发
- 银行版产品应该怎样做银行化改造
- 产品架构和 PRD 应怎样组织

但如果进一步追问：

**OpenClaw 官方到底怎样定义自己的技术架构？**

以及：

**它背后的核心技术栈到底是什么？**

那么就需要回到 OpenClaw 的官方文档和公开 GitHub 信息里去补一层更扎实的技术视角。

这篇文档的目标，就是把这层补上。

---

## 二、先说结论

如果用一句话概括 OpenClaw 的官方技术架构，我会这样定义：

**OpenClaw 是一个以 Gateway 为控制平面、以 WebSocket 为协议骨架、以 session 为上下文边界、以 embedded agent runtime 为执行核心、并通过 memory / skills / cron / sandbox / multi-agent 组织成持续运行系统的 local-first agent operating layer。**

它不是：

- 一个更复杂的聊天机器人
- 一个单纯的模型 UI
- 一个只有工具调用能力的 Copilot

它更像是：

**一层运行中的 Agent 系统外壳。**

---

## 三、官方定义下的核心架构

根据 OpenClaw 官方文档，最核心的系统层可以拆成九部分：

1. Gateway 控制平面
2. WebSocket 协议层
3. Session 与 Queue 层
4. Agent Runtime / Agent Loop
5. Memory 系统
6. Skills 系统
7. Cron / Heartbeat 主动运行层
8. Sandbox / Security 风险控制层
9. Multi-agent 路由与隔离层

### 1. Gateway 控制平面

官方文档里最重要的判断是：

**Gateway 才是 OpenClaw 的系统中枢。**

它负责：

- 承接所有 messaging surfaces
- 对外暴露统一的 WebSocket API
- 管理 provider connections
- 管理 pairing、device trust、node trust
- 发出 chat、agent、presence、health、heartbeat、cron 等事件
- 承载 canvas host

这意味着 OpenClaw 的架构重心不在某个聊天窗口，而在一个常驻、长生命周期的控制平面。

### 2. WebSocket 协议层

OpenClaw 的控制面不是传统 REST 风格，而是基于 WebSocket 的 typed protocol。

其核心特点是：

- 首帧必须是 `connect`
- 后续统一采用 `req / res / event` 三类消息
- 支持 feature discovery
- 支持 device pairing 与 node pairing
- side-effecting methods 要求 `idempotency key`

这说明 OpenClaw 从设计上就更像一个**实时控制系统**，而不是一个普通 Web 应用。

### 3. Session 与 Queue 层

这一层是 OpenClaw 和普通聊天机器人差异最大的地方之一。

官方把 session 作为一等对象处理：

- 不同来源的消息会被路由到不同 session
- DM、群聊、cron、webhook 都可以各自隔离
- session 有自己的 transcript、生命周期和 reset 规则

同时，官方还明确实现了 queue / lane 体系：

- 同一 session 一次只允许一个活跃 run
- 系统整体并发由 global lane 控制
- `steer / followup / collect` 等模式决定新消息如何插入当前运行

所以 OpenClaw 的上下文连续性，不是“模型好像记得”，而是建立在**显式的 session 机制与并发控制机制**之上的。

### 4. Agent Runtime / Agent Loop

官方对 agent loop 的定义是：

`intake -> context assembly -> model inference -> tool execution -> streaming replies -> persistence`

这意味着：

- 真正的运行单位不是“一次 LLM 调用”
- 而是一整个带状态、带工具、带事件流、带持久化的 agent loop

官方还明确说：

- OpenClaw 在 Gateway 中运行单个 embedded agent runtime
- runtime 底座建立在 `Pi agent core` 之上
- OpenClaw 自己再叠加 session、tool wiring、channel delivery 等系统层

这说明 OpenClaw 并不是从零写了一个“全栈自主 Agent 内核”，而是：

**在 agent core 之上，做了更完整的系统外壳。**

### 5. Memory 系统

官方 Memory 文档里有一个非常关键的表述：

**模型只会记得被写到磁盘上的东西，没有隐藏状态。**

OpenClaw 的记忆显式落在：

- `MEMORY.md`
- `memory/YYYY-MM-DD.md`
- `DREAMS.md`

然后通过 memory plugin / backend 提供：

- semantic recall
- keyword recall
- hybrid search
- auto flush
- dreaming / promotion

这说明 OpenClaw 的长期记忆不是黑盒，而是：

**文件系统上的显式知识资产 + 检索后端。**

### 6. Skills 系统

官方把 skills 定义成 AgentSkills-compatible 的 skill folders。

它们不是随便附加的 prompt，而是正式能力层的一部分，具备：

- 目录结构
- `SKILL.md`
- 优先级与覆盖关系
- allowlist
- gating
- env 注入
- snapshot
- watcher
- plugin-shipped skills

因此，skills 的本质不是“多写几段提示词”，而是：

**把“如何使用工具做某类任务”系统化。**

### 7. Cron / Heartbeat 主动运行层

如果没有这一层，OpenClaw 仍然只是一个响应式助手。

OpenClaw 的主动性主要由两部分支撑：

- `cron`：内建 scheduler，支持持久化任务与定时唤醒
- `heartbeat`：系统级活性检查、提醒和后续触达机制

这两层合起来，使 OpenClaw 可以：

- 在未来时点执行任务
- 定期检查事项
- 把后台运行结果重新接回聊天或 webhook

所以它的“常驻感”和“主动性”，并不是体验包装，而是系统结构的一部分。

### 8. Sandbox / Security 风险控制层

官方文档对安全边界说得很坦率：

**OpenClaw 默认是 personal assistant trust model，不是 hostile multi-tenant security boundary。**

它提供的风险控制机制包括：

- gateway auth
- device pairing / node pairing
- allowlist / dmPolicy / groupPolicy
- tool policy
- exec approvals
- sandbox

sandbox 本身又支持：

- `off / non-main / all`
- `agent / session / shared`
- `docker / ssh / openshell`

这说明 OpenClaw 是认真做了风险约束，但它的默认设计目标并不是“在一个共享系统中隔离彼此敌对的用户”。

### 9. Multi-agent 路由与隔离层

官方文档强调：

一个 agent 不是一个简单 persona，而是一整套隔离的 brain scope，包括：

- workspace
- state directory
- auth profiles
- session store

所以 multi-agent 的本质是：

**一个 Gateway 承载多个隔离 agent brain。**

这和简单的“多角色 prompt 切换”是完全不同的。

---

## 四、官方技术栈清单

如果按层来列，OpenClaw 的技术栈大致可以归纳如下。

### 1. 基础运行时

- `Node.js`
- `TypeScript`
- `pnpm`
- `tsx`

### 2. 控制平面与协议

- `WebSocket`
- `JSON Schema`
- `TypeBox`

### 3. 渠道接入

- `Baileys`：WhatsApp
- `grammY`：Telegram
- `Bolt`：Slack
- `discord.js`：Discord
- `signal-cli`：Signal
- `BlueBubbles`：iMessage 桥接

### 4. Agent / 模型运行层

- `Pi agent core`
- 多 provider model abstraction
- `ACP (Agent Client Protocol)`

### 5. 记忆与检索

- `Markdown` 文件记忆
- `SQLite`
- `sqlite-vec`
- `BM25 + vector hybrid search`
- `QMD`
- `LanceDB`
- `Honcho`

### 6. 技能与扩展

- `AgentSkills`
- plugin system
  - connector plugins
  - memory plugins
  - tool plugins
  - provider plugins

### 7. 工具执行与隔离

- `Docker`
- `SSH`
- `OpenShell`
- LSP-style generated tools

### 8. 主动运行与部署

- cron scheduler
- heartbeat
- `launchd`
- `systemd`
- `Tailscale`
- `SSH tunnel`
- `WSL2`

如果只保留最核心的一组技术名词，可以压缩成：

- `Node.js`
- `TypeScript`
- `WebSocket`
- `TypeBox`
- `Baileys`
- `grammY`
- `Bolt`
- `discord.js`
- `SQLite`
- `Docker`

---

## 五、和 GitHub 源码的简单映射

如果只做轻量映射，可以先这样理解：

| 架构概念 | 大致源码位置 | 说明 |
| --- | --- | --- |
| Gateway 控制平面 | `src/gateway/` | 系统中枢，路由、RPC、事件、session、health、presence 主要都在这里 |
| Gateway 主实现 | `src/gateway/server.impl.ts` | 官方文档直接提到的主服务实现 |
| Gateway 方法 | `src/gateway/server-methods.ts` 及相关方法文件 | `health`、`status`、`agent`、`send`、`sessions.*`、`cron.*` 等入口 |
| 协议层 | `src/gateway/protocol/` | WebSocket 协议、schema、版本等 |
| 客户端连接层 | `src/gateway/client.ts` | request timeout、reconnect backoff 等逻辑 |
| 插件系统 | `src/plugins/` | connector / memory / tool / provider 扩展点 |
| Sandbox Docker 路径 | `scripts/docker/sandbox/` | 官方文档直接提到 Dockerfile 和 sandbox 配置 |
| Session 持久化 | `~/.openclaw/agents/<agentId>/sessions/` | `sessions.json` 和 transcript JSONL |
| Cron 持久化 | `~/.openclaw/cron/` | `jobs.json` 与 `jobs-state.json` |
| Multi-agent 状态目录 | `~/.openclaw/agents/<agentId>/` | 每个 agent 各自一套状态与会话 |

这张映射表的意义不是“已经完成源码阅读”，而是帮助我们先建立一层：

**官方概念大概在代码库的哪一块。**

---

## 六、这对我们自己的产品研究意味着什么

补完官方文档之后，我觉得有三个判断比之前更明确。

### 1. OpenClaw 值得学的，确实不是聊天体验，而是系统结构

官方资料越看越能确认：

- 它真正的壁垒不只是模型接入
- 而是 Gateway、session、queue、memory、skills、cron、sandbox 这些系统层

也就是说，OpenClaw 的价值更接近：

**Agent 系统操作层**

而不是：

**聊天机器人产品**

### 2. 它的“常驻感”来自控制平面，而不是来自人格化

常驻、主动、持续上下文、跨触点这些体验，并不是表层设计风格，而是由：

- 长生命周期 Gateway
- session 路由
- cron
- heartbeat
- memory

这些机制一起支撑出来的。

### 3. 银行场景要借鉴时，更应该借鉴“系统骨架”而不是“个人助理叙事”

对于银行版 Assistant 而言，真正应该重点借鉴的是：

- 控制平面
- 任务与上下文隔离
- 记忆分层
- 技能系统
- 主动任务调度
- 风险控制层

而不是照搬：

- personal identity
- 自由增长记忆
- 主机级全权限代理
- 过强的个人化叙事

---

## 七、最后的总结

如果把这次补充研究压缩成一句话，我的结论是：

**OpenClaw 的本质不是“带很多工具的聊天助手”，而是“一个以 Gateway 为中枢、以 session 为边界、以 agent loop 为核心、以 memory / skills / cron / sandbox 为支柱的 local-first agent operating layer”。**

这也进一步说明，前面文档里把银行版方向定义成：

**Assistant OS + Task Engine + Governed Skill Runtime + Banking Control Plane**

并不是抽象过度，反而更接近真正应该学习的那一层。
