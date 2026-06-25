# Daemon 数据模型

对象、字段、取值、关系与时间语义，不依赖存储后端。修改边界见 [SKILL.md](../SKILL.md)。

## 通用规则

- 每个对象都有标题和正文；说明、成果标准、范围、来源与决策写在正文。
- 字段名和取值使用下文的标识符，后端默认原样存储。
- 所有字段可选。缺失的字段取默认值；无默认值的字段缺失表示未知。只保存已知的非默认值，有意义的零值照常保存。
- 关系由引用方单向保存，可以暂缺；不保存反向关系、间接关系或派生数据。
- 状态回到默认值时删除该字段。各对象状态独立：Action 完成不等于其 Project 或 Routine 完成。

字段类型：枚举（`a | b`）、文本、数字、日期、布尔（只存 `true`）、关系（`→ 目标对象`，括号内为数量）。

共享字段：

- `parent` → 上级对象。上级链路为 Pillar → Life Aspiration → Goal → Project 或 Routine → Action，可接的类型和数量见各对象；上级的类型由被引用的对象本身决定
- `topic` → Topic（一个或多个）
- `people` → Person（一个或多个）
- `notable`：布尔，值得记住

## 对象

### Pillar

核心价值、意义与希望践行的原则。

### Life Aspiration

长期、宽广且有情感意义的愿景。正文写明希望的状态和边界。

- `status`: `inactive`；缺失 = 有效
- `parent` → Pillar（一个）

### Goal

希望达到或保持的结果，正文写明判断标准。只划定一段时间、没有判断标准的（如"Q3 要做的事"）不建成 Goal。

- `status`: `on_hold | completed | cancelled`；缺失 = 活跃。`on_hold` 是暂时搁置，`cancelled` 是明确不再推进，原因写正文，两者都不算完成。
- `parent` → Life Aspiration（一个或多个）
- `topic`、`notable`

### Project

为 Goal 交付的一批具体工作，有明确完成点，以交付标准判定完成。说不清完成点或看不到尽头的是 Goal；更大的事拆成阶段，只建当前阶段。

- `status`：同 Goal
- `parent` → Goal（一个）
- `topic`、`notable`

### Routine

持续重复的推进或维护机制，没有完成点。

- `status`：同 Goal
- `cadence`: `daily | weekly | monthly | quarterly | per_release | as_needed`
- `parent` → Goal（一个）
- `topic`、`notable`

### Action

可直接执行的具体事项。会议安排是 Action。

- `status`: `waiting | done`；缺失 = 待办。是否正在推进由案头表达
- `type`: `task | meeting | errand | event`
- `priority`：数字，越大越重要；缺失 = 0。只表示重要程度，时间约束用日期表达
- `do_date`：日期，计划执行或跟进的日期
- `due_date`：日期，真实的最晚完成约束
- `completed_on`：日期，实际完成日；标记 `done` 时一并记录
- `human_required`：布尔，必须本人完成
- `parent` → Project 或 Routine（一个）
- `topic`、`people`、`notable`

三个日期彼此独立；`do_date`、`due_date` 与 `status` 无关。完成日确实未知时才省略 `completed_on`，未知日期不能用其他日期代替。

### Topic

主题索引，关联知识与工作，不代替项目。

- `significance`: `critical | essential | beneficial | tangential`

### Neurobit

文章、视频、书籍、文档、会议记录、思考或笔记。值得保存的会议内容是 Neurobit。

- `status`: `queue`（待处理）；缺失 = 不在待处理队列
- `category`: `media | notes | documents`
- `rating`: `excellent | very_good | average | poor`
- `source`：文本
- `goal` → Goal、`project` → Project、`routine` → Routine（各一个或多个，可同时关联）
- `topic`、`notable`

### Person

需要保留关系背景与互动上下文的人物或机构。

- `status`: `inactive`；缺失 = 有效

### Desk

案头：此刻正在推进或思考的事，可以同时有多件。全局只有一份，不属于任何集合，也没有生命周期：开始关注时放上来，完成或放下时移走，不保留历史，结论写入相关对象或 Daily。

- `focus` → 任意对象（一个或多个）

还不是对象的念头、备忘、待拍板的分歧写在正文，每条注明作者。案头只引用对象，不改变它们的状态；对象在不在案头与它是否完成无关。

### Daily

人类与 Agent 共写的日志，每天一篇，记录决定、状态、观察，以及 Agent 做了什么、为什么。每条注明作者，Agent 的推断标为推测。

- 日期用来标识对象本身，不作为字段保存
- 按需记录指标，字段名自定
- `notable`

Daily 不与其他对象建立字段关系，正文中可以链接任何对象。只有 Action 有日期字段；Routine 的 `cadence` 不是具体日期。
