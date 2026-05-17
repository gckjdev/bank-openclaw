# 文档索引

本目录用于沉淀围绕“银行 AI Assistant / Agent / 数字岗位 / 人力重构”方向的分析、方案、架构和任务体系设计文档。

为了便于查阅，下面按主题进行分类索引，并给出推荐阅读顺序。

---

## 一、推荐阅读顺序

如果你是第一次进入这套文档，建议按下面顺序阅读：

### 1. 先理解总体判断

1. `bank-ai-agent-vs-assistant.md`
2. `bank-ai-agent-complete-analysis.md`
3. `bank-ai-agent-solution-outline.md`

### 2. 再理解 headcount 压降逻辑

4. `bank-ai-headcount-reduction-analysis.md`
5. `three-core-questions-under-headcount-reduction.md`
6. `bank-role-substitution-priority-matrix.md`
7. `fte-substitution-estimation-framework.md`

### 3. 再看产品方向与 OpenClaw 参考

8. `openclaw-analysis-for-banking-assistant.md`
9. `openclaw-official-architecture-and-tech-stack.md`
10. `banking-openclaw-product-architecture-sketch.md`
11. `banking-openclaw-technical-architecture.md`
12. `banking-openclaw-prd-draft.md`
13. `banking-openclaw-prd-mvp.md`

### 4. 最后看任务体系与实现方法

14. `cognitive-substitution-and-orchestration-for-banking-assistant.md`
15. `task-definition-and-examples-for-banking-assistant.md`
16. `task-template-vs-dynamic-instance-in-banking-assistant.md`
17. `long-tail-task-handling-in-banking-assistant.md`
18. `composite-tasks-in-banking-assistant.md`

---

## 二、战略与总论

这一组文档回答的是：

- 银行到底更需要 AI Assistant 还是 AI Agent 员工
- 为什么银行的主路径应是 `Assistant-first, Agent-native`
- 银行与传统 IT 的核心区别是什么

### `bank-ai-agent-vs-assistant.md`

最早的基础分析稿。  
适合快速把握核心判断。

### `bank-ai-agent-complete-analysis.md`

完整总稿，整合了：

- AI 员工 vs AI 助手
- 三阶段演进
- 与传统银行 IT 的区别

### `bank-ai-agent-opinion-article.md`

偏对外观点文章，适合做公众号、行业表达、创始人观点稿。

### `bank-ai-agent-solution-outline.md`

偏方案化表达，适合内部汇报、售前讲解、方案前言。

---

## 三、Assistant-first / Agent-native 讨论框架

这一组文档回答的是：

- 为什么路线应是 `Assistant-first, Agent-native`
- 关键问题应该怎么拆
- 如何把抽象问题变成可评估、可推进的问题地图

### `assistant-first-agent-native-question-map.md`

问题地图总文档。  
包含 12 个核心问题、优先级、难度、验证方式。

### `assistant-first-agent-native-evaluation-template.md`

评估表模板。  
适合内部开会逐条打分、排序、分配 owner。

### `assistant-first-agent-native-role-example-credit-approval.md`

以“授信审批岗”为例，把 12 个问题完整跑了一遍。

---

## 四、headcount 压降与人力重构

这一组文档回答的是：

- 如果银行目标是压降 2 万人，这套体系应如何重构分析逻辑
- 哪些岗位最适合成为压降承接对象
- 如何从任务自动化率推导 FTE 和 headcount 变化

### `bank-ai-headcount-reduction-analysis.md`

从“提效逻辑”重写为“人员压降逻辑”的核心分析。

### `three-core-questions-under-headcount-reduction.md`

把三个核心问题重定义为：

- 劳动可见性
- 机器产能化
- 编制映射

### `bank-role-substitution-priority-matrix.md`

岗位替代优先级矩阵。  
直接回答哪些岗位更适合承接 2 万人压降目标。

### `fte-substitution-estimation-framework.md`

FTE 替代测算框架。  
回答如何从任务自动化率走到 FTE 和 headcount 压降数字。

---

## 五、OpenClaw 参考与银行版产品设计

这一组文档回答的是：

- OpenClaw 对银行员工 AI Assistant 的启发是什么
- OpenClaw 官方定义下的技术架构和技术栈是什么
- 银行版产品应如何银行化改造
- 架构和 PRD 应该如何从草图走向可落地版本

### `openclaw-analysis-for-banking-assistant.md`

系统分析 OpenClaw 值得借鉴什么、不能照搬什么。

### `openclaw-official-architecture-and-tech-stack.md`

补充基于 OpenClaw 官方文档的技术研究。  
重点包括：

- Gateway 控制平面
- Session / Queue / Agent Loop
- Memory / Skills / Cron / Sandbox / Multi-agent
- 技术栈清单
- 和 GitHub 源码的轻量映射

### `banking-openclaw-product-architecture-sketch.md`

银行版 OpenClaw 的文字版架构草图。  
包括：

- 交互层
- 身份与权限层
- 任务上下文层
- 记忆与知识层
- 技能与工具层
- 编排层
- 治理层
- 度量层

### `banking-openclaw-technical-architecture.md`

银行版 OpenClaw 的技术架构展开稿。  
重点补全：

- 前后端与控制平面拆分
- Agent Runtime / Workflow / Tool / Memory 等核心服务
- 身份、权限、审计与安全边界
- 存储、消息、观测与部署建议

### `banking-openclaw-prd-draft.md`

银行版 OpenClaw 的 PRD 雏形。  
按：

- 目标用户
- 场景
- 功能
- 权限
- 记忆
- 主动能力
- 升级路径

组织。

### `banking-openclaw-prd-mvp.md`

聚焦首版落地范围的 MVP PRD。  
适合在较完整的 PRD 雏形基础上，进一步收敛：

- MVP 目标用户与岗位范围
- 首版关键场景与功能边界
- 非目标范围与阶段划分
- 验收指标与上线节奏

---

## 六、路线图与推进规划

这一组文档回答的是：

- 从当前文档体系出发，项目应该按什么节奏推进
- 哪些事项适合按阶段拆解
- 中长期规划如何与 MVP 对齐

### `ROADMAP.md`

项目路线图与阶段性推进文档。  
适合用于：

- 对齐阶段目标
- 安排里程碑
- 拆分近期与中期工作包

---

## 七、认知替代与流程编排

这一组文档回答的是：

- 如何让 AI 替代人的部分认知劳动
- 如何把认知输出接到已有流程系统中

### `cognitive-substitution-and-orchestration-for-banking-assistant.md`

系统说明：

- 什么是认知替代
- 为什么要拆成认知单元
- 为什么应采用“AI 负责认知，流程系统负责执行”的双引擎模式

---

## 八、任务体系设计

这一组文档回答的是：

- “任务”到底是什么
- 任务应该预设还是动态生成
- 模板匹配不到怎么办
- 多任务组合需求怎么处理

### `task-definition-and-examples-for-banking-assistant.md`

定义“任务是工作语义对象，不是系统节点对象”，并给出 3 个具体例子：

- KYC / 开户预审
- 授信补件与风险归纳
- 客户经理会前准备与跟进排序

### `task-template-vs-dynamic-instance-in-banking-assistant.md`

回答：

- 任务类型应该预设
- 任务实例应该动态生成

提出：

- `Task Template`
- `Task Detector`
- `Task Instance`

三层结构。

### `long-tail-task-handling-in-banking-assistant.md`

回答：

- 模板不匹配时怎么办
- 为什么要有长尾任务缓冲层
- 如何把高频长尾晋升为新模板

### `composite-tasks-in-banking-assistant.md`

回答：

- 当用户输入涵盖多个任务类型时，如何做复合任务识别、拆解、任务图编排和结果汇总

---

## 九、按使用场景查阅

如果你当前是按工作目标来找文档，可以参考下面这个索引。

### 场景 A：我要给管理层讲“银行为什么要做这件事”

建议先看：

- `bank-ai-agent-vs-assistant.md`
- `bank-ai-agent-complete-analysis.md`
- `bank-ai-agent-opinion-article.md`

### 场景 B：我要讲“如果目标是压降 2 万人，该怎么思考”

建议先看：

- `bank-ai-headcount-reduction-analysis.md`
- `three-core-questions-under-headcount-reduction.md`
- `bank-role-substitution-priority-matrix.md`
- `fte-substitution-estimation-framework.md`

### 场景 C：我要开始设计产品和架构

建议先看：

- `openclaw-analysis-for-banking-assistant.md`
- `banking-openclaw-product-architecture-sketch.md`
- `banking-openclaw-technical-architecture.md`
- `banking-openclaw-prd-draft.md`
- `banking-openclaw-prd-mvp.md`

### 场景 D：我要定义任务体系和 orchestration

建议先看：

- `cognitive-substitution-and-orchestration-for-banking-assistant.md`
- `task-definition-and-examples-for-banking-assistant.md`
- `task-template-vs-dynamic-instance-in-banking-assistant.md`
- `long-tail-task-handling-in-banking-assistant.md`
- `composite-tasks-in-banking-assistant.md`

### 场景 E：我要组织内部 workshop / 评审会

建议先看：

- `assistant-first-agent-native-question-map.md`
- `assistant-first-agent-native-evaluation-template.md`
- `assistant-first-agent-native-role-example-credit-approval.md`

---

### 场景 F：我要安排项目路线图和阶段推进

建议先看：

- `ROADMAP.md`
- `banking-openclaw-prd-mvp.md`
- `banking-openclaw-technical-architecture.md`

---

## 十、建议的后续补充方向

如果要继续往下沉淀，最自然的下一批文档是：

### 1. 任务对象模型设计稿

定义：

- Task Template
- Task Instance
- Task Graph
- 字段
- 状态机
- 生命周期

### 2. 任务模板库样例文档

直接列出一批银行首版可用的任务模板，方便收敛范围。

### 3. 第一版 MVP PRD

聚焦 1-2 个岗位，明确首版功能边界。

### 4. 技术架构说明

把架构草图进一步展开成：

- 数据流
- 权限流
- 编排流
- 审计流

---

## 十一、最短总结

本目录当前已经形成四条主线：

1. **战略主线**：银行为什么应走 `Assistant-first, Agent-native`
2. **人力主线**：如何把 AI 路线与 headcount 压降连接起来
3. **产品主线**：银行版 OpenClaw / Assistant OS 应该如何设计
4. **任务主线**：如何定义任务、识别任务、处理长尾和复合任务

如果你想快速进入状态，建议先读：

- `bank-ai-agent-complete-analysis.md`
- `bank-ai-headcount-reduction-analysis.md`
- `banking-openclaw-product-architecture-sketch.md`
- `banking-openclaw-prd-mvp.md`
- `task-definition-and-examples-for-banking-assistant.md`

这四篇足以建立整体框架。
