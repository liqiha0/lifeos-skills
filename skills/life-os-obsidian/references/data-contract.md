# 存储映射

字段语义与关系数量见 `life-os-core` 的 `references/object-schema.md`。下表只定义对应的存储键与类型；属性名和枚举使用英文 `snake_case`。

## 字段

| 核心字段 | Property / 存储方式 |
| --- | --- |
| 标题、说明 | 文件名、正文；不另写 `name` |
| 对象种类 | 集合目录；Goal / Project / Routine 不另存类型属性 |
| 行动类型 | Action 的 `type`：Text |
| 状态、分类 | `status`、`category`：Text |
| 优先级、重要程度、质量评价 | `priority`、`significance`、`rating`：Text |
| 计划执行日、截止日、实际完成日 | `do_date`、`due_date`、`completed_on`：Date |
| Goal / Project / Routine 计划范围 | `timeline_start`、`timeline_end`：Date，可单端 |
| Aspiration → Pillar | `pillar`：链接列表 |
| Goal → Aspirations | `aspirations`：链接列表 |
| Project / Routine → Goal | `goal`：链接列表，填写时一个 |
| Action → Project 或 Routine | `project` 或 `routine`：链接列表，二选一且填写时一个 |
| Neurobit → Goal / Project / Routine | `goal`、`project`、`routine`：链接列表，各自按需关联一个或多个，可同时存在 |
| 主题、人物 | `topic`、`people`：链接列表 |
| 来源 | `source`：Text，URL、附件链接或简短说明；详细出处写正文 |
| Notable、重要事件日期 | `notable`：Checkbox；`notable_on`：Date |
| Cycle 种类、范围 | 子目录与文件名，见下文 |
| Cycle 追踪指标 | 有实际使用时存数值等原始指标，不预填整套字段 |

日期写 `YYYY-MM-DD`。关系写带引号的 wikilink 列表，即使只有一个值；重名时用完整 vault 内路径。Goal、Project、Routine 无统一必填 Property；`status` 只在 active、on_hold 或 completed 时写入。Action 仅在有 `do_date` 时写 `status`，未排期 Action 可以没有 Properties。其余遵循核心层的按需原则。省略空值及 `notable: false`；反向关系和统计不落盘。

关系键直接表达目标对象。Action 的 `project` 与 `routine` 可以都缺省，但不能同时填写；Action 不重复保存所属 Project / Routine 的 `goal`。

`completed_on`、`notable_on` 只记录已知事实日期，历史记录不猜测回填。

## 枚举映射

| 对象 / 字段 | 核心取值 → 存储值（同序） |
| --- | --- |
| Action 状态 | 待办、进行中、等待、完成 → `to_do`、`in_progress`、`waiting`、`done` |
| Goal / Project / Routine 状态 | 活跃、暂停、已完成 → `active`、`on_hold`、`completed` |
| Pillar / Aspiration / Person 状态 | 活跃、非活跃 → `active`、`inactive` |
| Neurobit 待处理标记 | 待处理 → `queue`；缺失表示不在待处理队列 |
| Action 类型 | Task、Meeting、Errand、Event → `task`、`meeting`、`errand`、`event` |
| 生活分类 | Work、Personal、Connections → `work`、`personal`、`connections` |
| Neurobit 分类 | Media、Notes、Documents → `media`、`notes`、`documents` |
| Action 优先级 | #1、#2、Urgent、Scheduled、Quick、Remember → `priority_1`、`priority_2`、`urgent`、`scheduled`、`quick`、`remember` |
| Topic 重要程度 | Critical、Essential、Beneficial、Tangential → `critical`、`essential`、`beneficial`、`tangential` |
| Neurobit 质量 | Excellent、Very Good、Average、Poor → `excellent`、`very_good`、`average`、`poor` |

## Cycle 文件名

相对于 Cycles 集合目录（默认 `Collections/Cycles/`）：

| 子目录 | 文件名 | 日期范围 |
| --- | --- | --- |
| Daily | `YYYY-MM-DD.md` | 当日 |
| Weekly | `YYYY-Www.md` | ISO 周一至周日；YYYY 是 ISO 周年，可能与日历年不同 |
| DuoCycle | `YYYY-MM.md` | 起始月与次月；起始月为 01、03、05、07、09、11 |
| Annual | `YYYY.md` | 日历年 |

Cycle 默认无 frontmatter；不另外保存类型、Period 或父周期属性。

## 最小示例

尚未排期与归属的 Action 不写 frontmatter，只保留文件名和正文。

明确了项目归属的 Action 可写为（链接仅示意）：

```yaml
---
do_date: 2026-09-14
status: to_do
project:
  - "[[示例项目]]"
---
```