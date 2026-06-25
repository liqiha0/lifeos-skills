# 八个逻辑集合权威 Schema

本文件是对象、字段、关系和稳定枚举的权威规范。字段使用英文显示名；平台存储稳定枚举时使用英文 `snake_case`。未定义稳定枚举的字段保持开放，不自行发明业务值。

## 目录

- 全局关系约束
- Actions
- GPR
- Pillars
- Life Aspirations
- Topic Vault
- Neurobits
- Cycles
- People

## 全局关系约束

```text
写入方                         被引用方
Life Aspiration  ------------> Pillar
Goal             ------------> Life Aspiration
Project / Routine ------------> Goal
Action           ------------> Project / Routine

Action           ------------> Topic / Person
GPR              ------------> Topic
Neurobit         ------------> Topic / GPR

Cycles（按 Period 派生）
Daily ⊆ Weekly ⊆ DuoCycle ⊆ Annual
```

- 每条语义关系遵循单一权威方向：箭头左侧的下游对象保存指向箭头右侧上游或上下文对象的 Relation。反向关联只通过反向链接或查询派生，不在被引用对象中双向回填。
- Pillars 与 Life Aspirations 是独立事实源；Life Aspiration 通过 `Parent Pillar` 连接 Pillar。
- Goal、Project、Routine 共用 GPR，通过 `Type` 和 `Parent GPR` 自关联。
- Goal 通过 `Aspirations Relation` 连接 Life Aspiration；Project/Routine 的上级是 Goal；Action 的上级是 Project 或 Routine。
- Action 必须填写 Do-Date 和 GPR Relation；GPR Relation 的目标类型必须是 Project 或 Routine。
- Cycles 不保存 Relation。Daily、Weekly、DuoCycle、Annual 的时间层级、聚合和年度归属只由 Period 的包含或重叠关系派生；任何高层 Cycle 都可尚未创建。
- `Notable` 用于 Actions、Neurobits、GPR 与 Cycles。Topic、People、Pillar/Life Aspiration 不使用此字段。
- 平台生成的反向查询结果只是权威正向 Relation 的投影，不能写成第二份源事实。

## Actions

职责：保存所有原子执行单元。

| 字段 | 类型 | 规则 |
| --- | --- | --- |
| Name | Title | 动作名称 |
| Do-Date | Date | 必填；计划执行日期，决定何时进入 Focus Zone |
| Due Date | Date | 可空；仅表示真实外部截止日期，不能替代 Do-Date |
| Priority | Select | 使用下方权威枚举 |
| Status | Select | 使用下方权威枚举 |
| Type | Select | 使用下方权威枚举 |
| Category | Select | 使用下方权威枚举 |
| Notable | Boolean | 是否汇入年度 Remembrance Collection |
| GPR Relation | Relation | 必填；连接一个 Project 或 Routine |
| Topic Relation | Relation | 相关 Topic，可多值 |
| People Relation | Relation | 相关 Person，可多值；Meeting 也使用此关系 |

### Actions 枚举

| 字段 | 显示名 | 存储值 |
| --- | --- | --- |
| Priority | `#1` | `priority_1` |
| Priority | `#2` | `priority_2` |
| Priority | `Urgent` | `urgent` |
| Priority | `Scheduled` | `scheduled` |
| Priority | `Quick` | `quick` |
| Priority | `Remember` | `remember` |
| Status | `To Do` | `to_do` |
| Status | `In Progress` | `in_progress` |
| Status | `Done` | `done` |
| Status | `Waiting` | `waiting` |
| Type | `Task` | `task` |
| Type | `Meeting` | `meeting` |
| Type | `Errand` | `errand` |
| Type | `Event` | `event` |
| Category | `Work` | `work` |
| Category | `Personal` | `personal` |
| Category | `Connections` | `connections` |

`Revisit` 与 `Pause` 是 Focus Zone 的视图语义，不扩张 Action Status 枚举。等待外部条件或暂缓的 Action 使用 `waiting`；Do-Date 表示下一次重新查看或执行的日期。

## GPR

职责：在一个集合中管理 Goals、Projects、Routines，并用自关联表达分解关系。

| 字段 | 类型 | 规则 |
| --- | --- | --- |
| Name | Title | 对象名称 |
| Type | Select | `Goal`、`Project`、`Routine` |
| Status | Select | 权威状态枚举 |
| Category | Select | `Work`、`Personal`、`Connections` |
| Timeline | Date Range | Goal 或 Project 的时间跨度；Routine 可表达当前运行范围 |
| Parent GPR | Self Relation | Goal 留空；Project/Routine 必须连接一个 Goal |
| Aspirations Relation | Relation | 仅 Goal 使用，必须连接 Life Aspiration |
| Topic Relation | Relation | 相关 Topic，可多值 |
| AutoData | Derived | 聚合子 GPR 数、Actions 完成情况、关联 Topic 等；不手工维护 |
| Notable | Boolean | 是否汇入年度 Remembrance Collection |

### GPR 枚举

| 字段 | 显示名 | 存储值 |
| --- | --- | --- |
| Type | `Goal` | `goal` |
| Type | `Project` | `project` |
| Type | `Routine` | `routine` |
| Status | `Active` | `active` |
| Status | `Not Started` | `not_started` |
| Status | `Completed` | `completed` |
| Status | `On Hold` | `on_hold` |
| Category | `Work` | `work` |
| Category | `Personal` | `personal` |
| Category | `Connections` | `connections` |

Goal 具体、可衡量且有时间点；Project 有明确完成点；Routine 长期重复且与 Project 同级。不得用 Type 之外的独立逻辑库替代 GPR。

## Pillars

职责：管理持续存在的人生领域、核心价值观和长期定位。

| 字段 | 类型 | 规则 |
| --- | --- | --- |
| Name | Title | Pillar 名称 |
| Category | Select | `Work`、`Personal`、`Connections` |
| Status | Select | `Active` 或 `Inactive` |
| Statement | Text | 核心价值观或长期定位陈述 |

Pillar 的子愿景由 Life Aspirations 中 `Parent Pillar` 的反向查询聚合，不维护反向关系字段。

### Pillars 枚举

| 字段 | 显示名 | 存储值 |
| --- | --- | --- |
| Category | `Work` | `work` |
| Category | `Personal` | `personal` |
| Category | `Connections` | `connections` |
| Status | `Active` | `active` |
| Status | `Inactive` | `inactive` |

## Life Aspirations

职责：管理长期、宽广、情感化的愿景。

| 字段 | 类型 | 规则 |
| --- | --- | --- |
| Name | Title | Life Aspiration 名称 |
| Category | Select | `Work`、`Personal`、`Connections` |
| Status | Select | `Active` 或 `Inactive` |
| Statement | Text | 情感化愿景陈述 |
| Parent Pillar | Relation | 必填；连接 Pillars 中的一个 Pillar |

Life Aspiration 的 Goals 由 GPR 中 Goal 的 `Aspirations Relation` 反向查询聚合，不维护反向关系字段。

### Life Aspirations 枚举

| 字段 | 显示名 | 存储值 |
| --- | --- | --- |
| Category | `Work` | `work` |
| Category | `Personal` | `personal` |
| Category | `Connections` | `connections` |
| Status | `Active` | `active` |
| Status | `Inactive` | `inactive` |

## Topic Vault

职责：作为知识主题导航站；相关 Neurobits、GPR、Actions 与 People 上下文由正向 Relation 的反向查询聚合。

| 字段 | 类型 | 规则 |
| --- | --- | --- |
| Topic Name | Title | 主题名称 |
| Significance | Select | 使用下方重要等级枚举 |
| Category | Select | `Personal`、`Work`、`Connections` |

### Topic Vault 枚举

| 字段 | 显示名 | 存储值 |
| --- | --- | --- |
| Significance | `Critical` | `critical` |
| Significance | `Essential` | `essential` |
| Significance | `Beneficial` | `beneficial` |
| Significance | `Tangential` | `tangential` |
| Category | `Personal` | `personal` |
| Category | `Work` | `work` |
| Category | `Connections` | `connections` |

## Neurobits

职责：统一保存文章、视频、书籍、思考、会议记录、PDF 与其他知识碎片。

| 字段 | 类型 | 规则 |
| --- | --- | --- |
| Name | Title | 内容或资源标题 |
| Category | Select | `Media`、`Notes`、`Documents` |
| Status | Select | `Queue`、`In Progress`、`Finished` |
| Rating / Quality | Select | 使用下方质量评价枚举，可空 |
| URL / Source | URL/File/Text | 原始链接、附件或来源说明 |
| Topic Relation | Relation | 所属 Topic |
| GPR Relation | Relation | Goal、Project 或 Routine 的项目上下文 |
| Notable | Boolean | 是否汇入年度 Remembrance Collection |

### Neurobits 枚举

| 字段 | 显示名 | 存储值 |
| --- | --- | --- |
| Category | `Media` | `media` |
| Category | `Notes` | `notes` |
| Category | `Documents` | `documents` |
| Status | `Queue` | `queue` |
| Status | `In Progress` | `in_progress` |
| Status | `Finished` | `finished` |
| Rating / Quality | `Excellent` | `excellent` |
| Rating / Quality | `Very Good` | `very_good` |
| Rating / Quality | `Average` | `average` |
| Rating / Quality | `Poor` | `poor` |

## Cycles

职责：统一管理日记、周复盘、双月复盘和年终总结，并形成时间层级。

| 字段 | 类型 | 规则 |
| --- | --- | --- |
| Name | Title | 周期名称 |
| Period | Date/Date Range | Daily 使用日期；其余使用覆盖的时间范围；是唯一周期归属事实，不要求作为平台 Property 落盘 |
| Tracking Fields | Number/Select group | 按需记录睡眠、运动、专注等数据，不统一强制 |
| Auto-Data Summary | Derived | 按 Period 聚合覆盖范围内的 Tracking Fields，不手工维护 |
| Notable | Boolean | 本 Cycle 是否进入年度 Remembrance Collection |

Daily、Weekly、DuoCycle、Annual 的层级由 Period 包含关系派生，不保存父周期字段，也不要求任一高层 Cycle 先存在。Remembrance 是 Knowledge Zone 中按年度查询 `Notable` 的集合，并在 Annual Cycle 中复盘，不建立额外顶层对象。

Cycle 种类是 Daily、Weekly、DuoCycle、Annual 四个固定领域种类，不是实例字段。派生直接上层时，只查询下一层同类种类且完整包含当前范围的记录：零个结果表示高层尚未创建，当前 Cycle 仍有效；一个结果表示唯一归属；多个结果表示同级范围重叠的数据异常，查询必须提示异常而不能任意归属。

## People

职责：个人 CRM，保存重要联系人、客户、合作伙伴、家人朋友；相关工作上下文由 Action 的 People Relation 反向查询聚合。

| 字段 | 类型 | 规则 |
| --- | --- | --- |
| Name | Title | 人名或机构名 |
| Category | Select | 使用下方关系分类枚举 |
| Status | Select | `Active` 或 `Inactive` |
| Contact Info | Text/Email | 电话、邮箱、社交账号等 |

### People 枚举

| 字段 | 显示名 | 存储值 |
| --- | --- | --- |
| Category | `Personal` | `personal` |
| Category | `Work` | `work` |
| Category | `Connections` | `connections` |
| Status | `Active` | `active` |
| Status | `Inactive` | `inactive` |
