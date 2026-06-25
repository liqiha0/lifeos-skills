# 三个 Primary Zones

Zones 是面向工作场景的入口，所有内容仍来自八个逻辑集合。视图不得复制实体或改变权威 schema。

## 目录

- Focus Zone：Quick Add、Top Priorities、Compact、Category、Month、Revisit/Waiting、Routines、Daily
- Alignment Zone：Pillars & Aspirations、GPR、Timeline、Cycles、Visualization
- Knowledge Zone：Topics、Neurobits、Resurfacing、People、Remembrance

## Focus Zone

Focus Zone 是每日执行终端，覆盖以下区块。

### Quick Add

提供三种低切换捕获入口：

- 新建 Action：同时填写未来或今天的 Do-Date，并连接一个 Project 或 Routine。
- 新建 Neurobit：默认 `Status = queue`，随后在 Weekly Loop 补充 Topic/GPR；People 与 Cycle 上下文由查询派生。
- 新建或打开今天的 Daily Cycle。

Quick Add 是捕获动作，不是新的对象类型。

### Top Priorities Today

筛选语义：

```text
Do-Date <= Today
AND Status != done
AND Priority IN [priority_1, priority_2, urgent]
```

视图保持 3-5 个候选核心任务；晨间启动从中确认最重要的 2-3 个，并直接开始 `priority_1`。如果候选过多，在晚间闭店重新分配 Do-Date 或优先级。

### Compact

筛选全部到期未完成 Actions：

```text
Do-Date <= Today
AND Status != done
```

固定排序：

```text
priority_1
-> priority_2
-> urgent
-> scheduled
-> quick
-> remember
```

同优先级先按 Do-Date 升序，再按 Name 升序，确保结果稳定。

### Category

沿用 Compact 的筛选，并按 `work`、`personal`、`connections` 分组。Category 只改变观察角度，不改变主链关系。

### Month

显示所有 `Status != done` 且已有 Do-Date 的 Actions，以 Do-Date 放入月历。晚间闭店在此重新分配未完成项：移动到明天、本周稍后或更远日期会直接更新 Do-Date。

系统不允许 Action 缺少 Do-Date。过期未完成项必须在闭店或 Weekly Loop 中重新排期，不能无限堆积。

### Revisit / Waiting

筛选：

```text
Status == waiting
```

- **Waiting**：等待他人或外部条件；People Relation 标明相关人员，Do-Date 表示下一次跟进或重看日期。
- **Revisit / Pause**：暂缓推进但需要在未来重新判断；仍使用 `waiting`，由未来 Do-Date 控制浮现。

`Pause` 不增加新的 Action Status。GPR 层的暂停使用权威值 `on_hold`。

### Routine Management

GPR 筛选：

```text
Type == routine
AND Status == active
```

Routine 的每次原子执行记录为 Action，并把 GPR Relation 指向该 Routine；这样执行仍进入 Do-Date、Priority、Status 与 Cycle 闭环。

### Daily Entry

Cycles 筛选：

```text
Daily Cycle
AND Period == Today
```

Daily Entry 承载晨间启动、必要的日间记录、晚间复盘、Tracking Fields 和 `Notable` 判断；当日 Actions 由 `do_date == Daily Period` 查询浮现。

## Alignment Zone

Alignment Zone 用于战略规划、自上而下分解和自下而上复盘。

### Pillars & Aspirations

- 这是联合 UI 视图，数据来自独立的 Pillars 与 Life Aspirations 事实源。
- 显示 `Status == active` 的 Pillars 与 Life Aspirations。
- 以 `Parent Pillar` 展示 Pillar -> Life Aspiration。
- 由 Goal 的 `Aspirations Relation` 反向聚合并展示 Aspiration 的 Goals，检查愿景是否已被具体目标承接。

### Active Goals / GPR Breakdown

筛选活跃 GPR，并按主链展开：

```text
Goal
  <- Project / Routine (Parent GPR)
    <- Action (GPR Relation)
```

- 第一层：`Type == goal AND Status == active`。
- 第二层：反查 `Parent GPR` 指向当前 Goal 的 active Project/Routine。
- 第三层：反查 `GPR Relation` 指向当前 Project/Routine 且未完成的 Actions。
- AutoData 汇总子 GPR 数、Action 完成情况和关联 Topic，不作为手工录入字段。

### Goal Timeline

筛选 `Type IN [goal, project]` 且 `Status != completed`，使用 Timeline 展示目标与项目的时间分布。Routine 保持长期重复语义，不强行放入项目式完成时间线。

### Cycle Reviews

提供当前 Weekly、DuoCycle 与 Annual 的入口；以 Period 包含关系展示 Annual、DuoCycle、Weekly、Daily。高层记录不存在时不构成异常。复盘操作见 `operating-cycles.md`。

### Visualization

从八个集合的真实字段和关系生成可视化，例如：

- Category 下完成 Actions 的趋势；
- Goal/Project 的 Action 完成情况；
- Neurobits 的 Queue、In Progress、Finished 分布；
- Cycle Tracking Fields 与 Auto-Data Summary 趋势。

Visualization 是派生视图，不引入新的业务状态或手工统计字段。

## Knowledge Zone

Knowledge Zone 是知识输入、处理、情境浮现、People 与年度回忆入口。

### Topic Vault

展示 Topics。每个 Topic 通过反向查询聚合：

- `Topic Relation` 指向当前 Topic 的 Neurobits；
- `Topic Relation` 指向当前 Topic 的 active GPR；
- `Topic Relation` 指向当前 Topic 的未完成 Actions；
- 上述 Actions 的 `People Relation` 所引用的 People。

### Queue / Processing

Neurobits 处理视图：

```text
Status IN [queue, in_progress]
```

按 Category 分为 `media`、`notes`、`documents`。Weekly Loop 中为 Queue 项补充正向的 Topic Relation 和按需的 GPR Relation；People 与 Cycle 上下文由相关 Action/GPR 和时间范围查询派生。随后把状态推进到 `in_progress` 或 `finished`。

### Resurfacing

知识不靠手动搬运，而由关系重新浮现：

- 在 Topic 中反查 `Topic Relation`，浮现相关 Neurobits、GPR、Actions，并从 Actions 派生 People；
- 在 GPR 中反查 Neurobit 的 `GPR Relation`，并展示 GPR 自己正向引用的 Topics；
- 在 People 中反查 Action 的 `People Relation`，再从这些 Actions 的 GPR/Topic 上下文派生 Projects、Neurobits 与 Topics；
- 在 Cycle 中以 Period 包含或重叠查询 Cycle、Actions、GPR 与 Neurobits，浮现本周期内容。

### People

按 `personal`、`work`、`connections` 分组；反查 Action 的 `People Relation` 展示 Actions/Meetings，再从这些 Actions 的 `GPR Relation`、`Topic Relation` 及相关 Neurobits 派生 GPR、Neurobits 和 Topics。People 不维护反向 Relation；Meeting 是 Action Type，不建立独立会议集合。

### Remembrance

按当前 Annual Cycle 的时间范围聚合 Actions、GPR、Neurobits、Cycles 中 `Notable == true` 的源对象，并按月份、类型或 Cycle 分组。Remembrance Collection 位于 Knowledge Zone，同时服务 Annual Review；它不形成额外顶层领域，也不复制源内容。
