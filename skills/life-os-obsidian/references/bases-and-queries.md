# Bases、查询与 Zone 组装

使用 Obsidian CLI 创建和验收 Base/查询。先用本机 `obsidian help` 确认命令与当前 Bases 语法；本文件规定业务筛选和排序语义，不依赖某个易变的 UI 配置步骤。

## 目录

- Focus Zone
- Alignment Zone 与 GPR AutoData
- Knowledge Zone
- Cycles 与 Remembrance
- 工作流入口与查询验收

## Focus Zone

| 视图 | 数据源 | 筛选 | 排序/分组 |
| --- | --- | --- | --- |
| Quick Add | Templates | Action、Neurobit、Daily 三个 CLI 创建入口 | 创建后读回 Properties |
| Top Priorities Today | Actions | `do_date <= today`；`status in [to_do, in_progress, waiting]`；`priority in [priority_1, priority_2, urgent]` | 自定义优先级顺序；最多展示 3-5 个，晨间确认 2-3 个 |
| Compact | Actions | `do_date <= today`；`status in [to_do, in_progress, waiting]` | `priority_1 -> priority_2 -> urgent -> scheduled -> quick -> remember`，再按 `do_date`、`file.name` |
| Category | Actions | 同 Compact | 按 `work`、`personal`、`connections` 分组 |
| Month | Actions | `status in [to_do, in_progress, waiting]`；`do_date` 在显示月份 | 以 `do_date` 月历展示；重新安排直接修改 `do_date` |
| Revisit / Waiting | Actions | `status == waiting` | `do_date` 升序；`people_relation` 分组可选 |
| Routine Management | GPR | `type == routine`；`status == active` | `category`、`file.name`；同时嵌入状态为 `to_do`、`in_progress` 或 `waiting` 的关联 Actions |
| Daily Entry | `Collections/Cycles/Daily/` | `file.name == today` | 单条当前 Daily |

Top Priorities 与 Compact 必须包含过期未完成项，因为 `do_date <= today`。Month 是重新分配 `do_date` 的入口；不能用 `due_date` 代替。

## Alignment Zone

| 视图 | 数据源 | 筛选 | 关系/展示 |
| --- | --- | --- | --- |
| Pillars & Aspirations | Pillars + Life Aspirations | `status == active` | 联合两个独立事实源，以 `parent_pillar` 展开 Pillar -> Life Aspiration，并由 Goal 的 `aspirations_relation` 反向聚合 Goals |
| Active Goals | GPR | `type == goal`；`status == active` | 显示 `aspirations_relation`、`timeline` 与查询派生摘要 |
| GPR Breakdown | GPR + Actions | active Goal/Project/Routine；Actions `status in [to_do, in_progress, waiting]` | 从 Goal 反查 `parent_gpr` 子项，再反查 `gpr_relation` Actions |
| Goal Timeline | GPR | `type in [goal, project]`；`status in [active, not_started, on_hold]` | 从 `timeline` interval 派生起止 |
| Cycle Reviews | Cycles | 当前 Annual/DuoCycle/Weekly | 以目录和 ISO 文件名解析的 Period 包含关系展开已有低层 Cycle；缺失高层记录不报错 |
| Visualization | 八集合 | 只使用权威字段 | 完成趋势、Goal 进度、Neurobits 状态、Cycle 数据 |

### GPR AutoData

对每个 Goal/Project/Routine 派生：

```text
child_gpr_count = count(GPR where parent_gpr contains current file)
action_total = count(Actions where gpr_relation contains current file)
action_done = count(same set where status == done)
topics = unique(topic_relation from current GPR and related Actions)
people = unique(people_relation from related Actions)
```

显示这些结果即可；不要添加手工维护的替代字段。

## Knowledge Zone

| 视图 | 数据源 | 筛选 | 分组/上下文 |
| --- | --- | --- | --- |
| Topic Vault | Topic Vault | 全部 | `significance`、`category` |
| Neurobits Queue | Neurobits | `status in [queue, in_progress]` | `category`: media/notes/documents |
| Resurfacing | Neurobits/GPR/Actions/People | 只反查权威正向 Relation；Cycle 上下文由对象日期与 Cycle 文件名解析的 Period 提供 | 在上下文页嵌入 |
| People | People | 全部或 `status == active` | `category`: personal/work/connections |
| Remembrance Collection | Actions/GPR/Neurobits/Cycles | `notable == true`；落入当前 Annual | 按月份、集合类型或日期范围分组 |

Topic 页面只从正向字段反查：

```text
neurobits = Neurobits where topic_relation contains current file
gpr = GPR where topic_relation contains current file and status == active
actions = Actions where topic_relation contains current file and status in [to_do, in_progress, waiting]
people = unique(people_relation from actions)
```

People 页面同样不读取反向字段：

```text
actions = Actions where people_relation contains current file
gpr = unique(gpr_relation from actions)
topics = unique(topic_relation from actions and gpr)
neurobits = Neurobits where gpr_relation contains any gpr or topic_relation contains any topics
```

## Cycles 与 Remembrance

### Cycle 层级派生

Cycle 没有父子 Relation。对任意已存在的高层 Cycle，以日期范围派生已有下层记录：

```text
daily = Collections/Cycles/Daily/ where Period(file.name) is within Period(current Weekly.file.name)
weekly = Collections/Cycles/Weekly/ where Period(file.name) is within Period(current DuoCycle.file.name)
duo_cycles = Collections/Cycles/DuoCycle/ where Period(file.name) is within Period(current Annual.file.name)
```

缺失的高层 Cycle 不产生异常，也不要求回填任何属性。

每次只查询相邻高层目录：Daily 查 `Weekly/`、Weekly 查 `DuoCycle/`、DuoCycle 查 `Annual/`。匹配零个容器合法；匹配多个容器表示同级解析后的 Period 重叠，显示为数据异常，不任意选择归属。

### Cycle Auto-Data Summary

对当前 Cycle 聚合：

```text
children = lower-level Cycles where Period(file.name) is within current Cycle Period
tracking = aggregate tracking_fields from Daily records where Period(file.name) is within current Cycle Period
notable = Actions/GPR/Neurobits/Cycles where notable == true and dates overlap this Cycle/Annual
```

### Annual Remembrance

年度归属优先级：

1. Cycle 的解析后 Period 落入当前 Annual Period；
2. Action 的 `do_date` 落入 Annual Period；
3. GPR 的 `timeline` 与 Annual Period 重叠；
4. Neurobit 优先沿 `gpr_relation` 使用关联对象的 `timeline`，否则使用 Obsidian 原生 `file.ctime`。

Remembrance 只显示源对象链接、类型、日期/周期和必要摘要。不创建额外实体文件，也不复制正文。

## Daily / Weekly / DuoCycle / Annual 入口

- Focus Zone 嵌入今天的 Daily 和当前 Weekly 链接。
- Alignment Zone 嵌入当前 Weekly、DuoCycle、Annual 及 GPR Review 查询。
- Knowledge Zone 嵌入当前 Annual 的 Remembrance 和 Neurobits Processing。
- 每次复盘结束后通过 CLI 写入复盘正文，并读回确认。

## 查询验收

使用 Obsidian CLI 逐项验证：

1. 状态为 `to_do`、`in_progress` 或 `waiting` 的到期 Actions 同时出现在 Compact；高优先级子集出现在 Top Priorities。
2. `waiting` Actions 出现在 Revisit / Waiting，并按下一次 `do_date` 排序。
3. active Routines 及其 Actions 出现在 Routine Management。
4. Goal 能沿 `aspirations_relation` 到 Life Aspiration，再沿 `parent_pillar` 到 Pillar。
5. Project/Routine 能沿 `parent_gpr` 到 Goal；Action 能沿 `gpr_relation` 到 Project/Routine。
6. Topic 与 Person 页面能反向浮现相关四类对象。
7. 对已有 Weekly、DuoCycle、Annual，日期范围查询覆盖其对应低层 Cycle；没有高层 Cycle 时，低层 Cycle 仍可创建和查询。
8. Actions、GPR、Neurobits、Cycles 中 `notable == true` 且属于当前年度的对象均进入 Remembrance Collection。
