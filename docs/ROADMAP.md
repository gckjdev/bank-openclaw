# 路线图

本文件用于把 `docs` 目录下现有文档，按一条更适合推进讨论和落地规划的路线组织起来。

如果 `README.md` 更像“分类索引”，那么这份 `ROADMAP.md` 更像“阅读与推进路径”。

它回答的是：

- 如果要把这套方向真正往前推进，应该先看什么，后看什么
- 每个阶段解决的核心问题是什么
- 当前文档分别位于哪一段
- 下一步还缺哪些关键补件

这份路线图按六个阶段组织：

1. 战略判断
2. 人力重构与业务目标
3. 产品方向
4. 产品架构
5. 任务与编排体系
6. 落地推进与下一步输出

---

## 一、阶段 1：战略判断

这一阶段要解决的核心问题是：

- 银行到底需要 AI Agent 员工，还是需要给员工配置 AI Assistant Agent
- 为什么银行不应该一开始就追求“万能 AI 员工”
- 为什么主路线应是 `Assistant-first, Agent-native`

### 建议先读

#### 1. `bank-ai-agent-vs-assistant.md`

适合作为最短入门稿，快速建立方向判断。

#### 2. `bank-ai-agent-complete-analysis.md`

这是当前最完整的总论稿，适合建立整体框架。  
建议把它当作战略层的主文档。

#### 3. `bank-ai-agent-opinion-article.md`

适合对外表达和观点化输出。  
如果你需要用一篇更像文章的形式对外阐述路线，这篇更合适。

#### 4. `bank-ai-agent-solution-outline.md`

适合内部汇报或售前表达。  
比观点稿更偏结构化。

### 这个阶段的产出

读完这一阶段，团队应至少形成以下共识：

- 银行短中期主形态应是 Assistant，而不是全面 Employee-first
- Agent-native 是架构与演进方向，不是第一版产品形态
- 这条路线的目标不是停留在工具，而是逐步长出数字产能

---

## 二、阶段 2：人力重构与业务目标

这一阶段要解决的核心问题是：

- 如果银行真实目标是通过这套体系支撑较大规模人员压降，前面的分析要如何重写
- AI 路线怎么和 headcount 压降挂钩
- 哪些岗位更适合成为压降承接对象
- 如何从任务自动化率推到 FTE 和 headcount

### 建议阅读顺序

#### 1. `bank-ai-headcount-reduction-analysis.md`

这是从“提效逻辑”切换到“人员压降逻辑”的核心文档。  
建议把它视为“组织目标重写版总论”。

#### 2. `three-core-questions-under-headcount-reduction.md`

这篇把原先三个核心问题重定义为：

- 劳动可见性
- 机器产能化
- 编制映射

适合帮助管理层和产品团队统一新语境。

#### 3. `bank-role-substitution-priority-matrix.md`

回答：

- 哪些岗位最适合优先承接 2 万人压降目标
- 哪些岗位更适合做增强，而不是主压降对象

#### 4. `fte-substitution-estimation-framework.md`

回答：

- 如何从任务自动化率推到工时节省
- 再推到 FTE 替代
- 最后推到 headcount 压降

### 这个阶段的产出

读完这一阶段，团队应至少形成以下共识：

- 组织目标如果是压降人力，Assistant-first 的意义会发生变化
- AI 路线必须从“提效”升级成“劳动承接与编制映射”
- 要先明确岗位替代优先级，再谈产品投入

---

## 三、阶段 3：产品方向

这一阶段要解决的核心问题是：

- 如果以 OpenClaw 这类 Personal AI Assistant 为参考，银行到底应该借鉴什么
- 什么不能照搬
- 银行版产品究竟应该是什么，而不应该是什么

### 建议先读

#### 1. `openclaw-analysis-for-banking-assistant.md`

这篇是关键分水岭。  
它回答：

- OpenClaw 值得学的是什么
- 银行绝不能照搬的是什么
- 为什么银行真正应该做的是“工作身份驱动的 Assistant OS”

### 再读

#### 2. `banking-openclaw-prd-draft.md`

这篇把产品方向落到了更清晰的 PRD 结构上，按：

- 目标用户
- 场景
- 功能
- 权限
- 记忆
- 主动能力
- 升级路径

来组织。

### 这个阶段的产出

读完这一阶段，团队应至少形成以下共识：

- 银行版产品不是“Personal AI Assistant 银行版”
- 正确方向是“绑定工作身份、围绕任务运行、服务岗位劳动承接的 Assistant OS”
- 第一版应聚焦具体岗位，不做通用聊天产品

---

## 四、阶段 4：产品架构

这一阶段要解决的核心问题是：

- 这种产品在架构上应该如何分层
- 为什么需要身份层、任务层、记忆层、技能层、治理层
- 如何从产品概念走向系统设计

### 建议先读

#### 1. `banking-openclaw-product-architecture-sketch.md`

这是当前最核心的架构草图文档。  
建议把它当作“银行版 Assistant OS”的文字版架构总览。

它重点定义了：

- 交互接入层
- 身份与权限层
- 任务上下文层
- 记忆与知识层
- 技能与工具层
- Agent 编排与执行层
- 治理与审计层
- 度量与替代分析层

### 推荐搭配阅读

#### 2. `banking-openclaw-prd-draft.md`

PRD 和架构一起看，会更容易把“用户价值”和“系统结构”对上。

### 这个阶段的产出

读完这一阶段，团队应至少形成以下共识：

- 产品不是一个聊天框，而是一层 Assistant OS
- 任务层和治理层是架构中最关键的两层
- 度量与替代分析不能后补，必须从底层设计时预留

---

## 五、阶段 5：任务与编排体系

这一阶段要解决的核心问题是：

- 什么叫“任务”
- 任务应该预设还是动态生成
- 模板不匹配怎么办
- 复合任务怎么处理
- 如何让 AI 替代认知，并和已有流程系统做 orchestration

这一组文档建议整体阅读，因为它们之间强相关。

### 1. `cognitive-substitution-and-orchestration-for-banking-assistant.md`

回答：

- 如何把人的认知工作拆成认知单元
- 为什么应采用“AI 负责认知，流程系统负责执行”的模式

建议把它作为“方法论总文档”先读。

### 2. `task-definition-and-examples-for-banking-assistant.md`

回答：

- 任务到底是什么
- 为什么任务首先是工作语义对象，而不是系统节点对象

并给出三个具体例子：

- KYC / 开户预审
- 授信补件与风险归纳
- 客户经理会前准备与跟进排序

### 3. `task-template-vs-dynamic-instance-in-banking-assistant.md`

回答：

- 任务类型应预设
- 任务实例应动态生成

提出：

- `Task Template`
- `Task Detector`
- `Task Instance`

三层结构。

### 4. `long-tail-task-handling-in-banking-assistant.md`

回答：

- 当模板匹配不到时怎么办
- 为什么要设计开放任务缓冲层
- 如何把高频长尾沉淀成新模板

### 5. `composite-tasks-in-banking-assistant.md`

回答：

- 当用户输入涉及多个任务类型时，如何识别复合任务
- 如何拆成子任务
- 如何生成任务图并编排执行

### 这个阶段的产出

读完这一阶段，团队应至少形成以下共识：

- 任务体系是整个产品能否长成 Agent-native 的关键
- 任务不是 IT 节点，而是工作语义对象
- 要同时支持：
  - 标准任务
  - 长尾任务
  - 复合任务

---

## 六、阶段 6：落地推进与 workshop 工具

这一阶段要解决的核心问题是：

- 团队如何围绕这些问题推进内部讨论
- 如何做优先级判断
- 如何做岗位实例分析

### 建议先读

#### 1. `assistant-first-agent-native-question-map.md`

这是问题地图总文档。  
适合做内部 workshop 的母文档。

#### 2. `assistant-first-agent-native-evaluation-template.md`

适合把问题进一步落成：

- 打分表
- owner 分配
- 优先级排序

#### 3. `assistant-first-agent-native-role-example-credit-approval.md`

用授信审批岗跑完整套问题框架，适合做样板分析。

### 这个阶段的产出

读完这一阶段，团队应至少形成以下共识：

- 应先选岗位、再选任务、再选自动化级别
- 讨论不能只停留在观点，要落到问题地图、模板和实例

---

## 七、如果只读 8 篇，建议读哪些

如果时间有限，建议优先读这 8 篇：

1. `bank-ai-agent-complete-analysis.md`
2. `bank-ai-headcount-reduction-analysis.md`
3. `three-core-questions-under-headcount-reduction.md`
4. `bank-role-substitution-priority-matrix.md`
5. `fte-substitution-estimation-framework.md`
6. `openclaw-analysis-for-banking-assistant.md`
7. `banking-openclaw-product-architecture-sketch.md`
8. `task-definition-and-examples-for-banking-assistant.md`

这 8 篇基本能建立：

- 战略判断
- 人力重构逻辑
- 产品方向
- 架构方向
- 任务体系

---

## 八、建议的下一批补件

如果要继续把路线从“讨论”推进到“设计和执行”，最值得优先补的文档是：

### 1. 任务对象模型设计稿

建议内容：

- Task Template 字段设计
- Task Instance 字段设计
- Task Graph 结构设计
- 状态机
- 生命周期
- 权限模型

### 2. 任务模板库样例文档

建议内容：

- 列出首版 10-20 个银行任务模板
- 对应岗位
- 输入输出
- 可调用技能
- 编排关系
- 自动化级别

### 3. 第一版 MVP PRD

建议内容：

- 只选 1-2 个岗位
- 只做少数高频任务
- 明确边界，不追求大而全

### 4. 技术架构说明

建议内容：

- 数据流
- 权限流
- 编排流
- 审计流
- 任务流

---

## 九、最短总结

当前 `docs` 目录下的文档，已经大致形成了一条完整路线：

### 战略

银行应走 `Assistant-first, Agent-native`

### 业务目标

AI 路线最终要服务于岗位劳动承接和组织级人力重构

### 产品

银行版方向不是个人助理，而是工作身份驱动的 Assistant OS

### 架构

产品必须以任务层和治理层为中心构建

### 实现

要通过：

- 认知替代
- 任务体系
- 编排系统

把 AI 和已有流程系统连接起来

如果把这份路线图再压缩成一句话，那就是：

**先回答为什么做，再回答为了什么岗位做，再回答产品怎么长、架构怎么搭、任务怎么组织，最后再进入真正的落地设计。**
