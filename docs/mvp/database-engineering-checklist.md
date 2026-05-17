# Banking OpenClaw MVP Database Engineering Checklist

## 1. 目标

这份 checklist 面向：

- `PostgreSQL`
- `packages/db`
- migration
- 数据建模与索引设计

目标是把数据库层的关键工程约束沉淀成可执行检查项。

---

## 2. 必守规则

### 2.1 单文件行数限制

**每个数据库相关代码文件不能超过 `500` 行。**

适用对象包括：

- schema 定义文件
- repository 文件
- migration helper 文件
- seed 文件

### 2.2 PostgreSQL 是核心权威库

检查项：

- 核心对象是否统一落在 `PostgreSQL`
- 是否避免一半对象进数据库、一半对象靠文件和缓存凑
- 是否避免本地运行态反向成为正式权威源

---

## 3. 建模 Checklist

### 3.1 表结构显式

检查项：

- 主对象关系是否使用明确字段和外键表达
- 表名、主键、外键命名是否稳定一致
- 一个表是否只回答一个核心问题

### 3.2 JSON 字段只用于快照和弹性结构

适合放 JSON 的内容：

- `plan_snapshot_json`
- `context_envelope_json`
- markdown 文档正文与轻量元数据
- 需要保留版本快照的结构

检查项：

- 是否避免把整个领域模型都塞进一个 JSON 列
- 是否避免把可查询维度全部藏进 JSON

### 3.3 历史优先于覆盖

检查项：

- `TaskExecution / ToolCall / AuditEvent` 是否保留关键历史
- 关键状态变化是否通过事件、版本或历史记录保留
- 是否避免覆盖式更新抹掉审计线索

---

## 4. Migration Checklist

### 4.1 所有变更都通过 migration

检查项：

- 所有表结构改动是否都有 migration
- migration 是否纳入版本控制
- 不允许手工改库后不补 migration

### 4.2 Migration 保持可读可回滚

检查项：

- migration 是否按一次明确变更组织
- 是否避免一个 migration 同时做过多不相关修改
- 是否能清楚说明这次改动影响的对象边界

---

## 5. 索引 Checklist

### 5.1 先为真实查询路径建索引

优先关注：

- `task_id`
- `plan_id`
- `execution_id`
- `status`
- `created_at`
- `task_type`
- `role_profile_code`

检查项：

- 是否针对真实读写路径建索引
- 是否为列表页、详情页、聚合页考虑索引
- 是否避免“见字段就建索引”

### 5.2 聚合分析路径可解释

检查项：

- `TaskEpisode -> LaborLedger -> SubstitutionScore` 聚合路径是否清晰
- 关键统计维度是否能稳定查询
- 是否能解释分析结果来源

---

## 6. Repository / DB 访问 Checklist

### 6.1 Repository 不混业务编排

检查项：

- repository 是否只负责 db 访问
- 是否避免在 repository 中写大量流程控制
- repository 是否容易被集成测试覆盖

### 6.2 查询语义显式

检查项：

- 查询接口是否按业务目的命名
- 是否避免一个通用 repository 方法承担过多场景

---

## 7. 数据库反模式

- 单个数据库相关代码文件超过 `500` 行
- 没有 migration 的表结构变更
- 用 JSON 字段替代清晰建模
- 频繁覆盖更新导致丢历史
- 为不真实的查询场景提前建大量索引
- repository 中堆积业务大脑

---

## 8. 一句话结论

**数据库层 MVP 的最佳实践是：以 `PostgreSQL` 为单一权威库，显式建模、受控 migration、面向真实查询建索引，并严格执行单文件不超过 `500` 行。**
