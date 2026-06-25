# Life OS 的 Obsidian 实现

## 目录

- 实现边界
- Zone-first 与八集合事实源
- Properties 映射
- 最小存储、缺失值与文件名
- Collections 与父子关系
- 单向关系权威表
- 日期与时间范围
- AutoData 等价映射
- Remembrance 映射
- Obsidian CLI 协议与验收

## 实现边界

core 的 `object-schema.md` 是对象、字段、枚举和关系的权威来源。本文件只定义 Obsidian 等价映射。不要写 Notion 按钮、Formula 2.0、Relation/Rollup 教程，也不要因 Obsidian 能力差异删减字段或关系。

## Zone-first 与八集合事实源

推荐 vault 结构：

```text
Life OS/
  Zones/
    Alignment Zone.md
    Focus Zone.md
    Knowledge Zone.md

  Collections/
    Actions/
    GPR/
    Pillars/
    Life Aspirations/
    Topic Vault/
    Neurobits/
    Cycles/
      Daily/
      Weekly/
      DuoCycle/
      Annual/
    People/

  Templates/
    Action.md
    Goal.md
    Project.md
    Routine.md
    Pillar.md
    Life Aspiration.md
    Topic.md
    Neurobit.md
    Daily.md
    Weekly.md
    DuoCycle.md
    Annual.md
    Person.md
```

- `Zones/` 是入口页面，只嵌入 Base/查询、命令入口和当前 Cycle 链接。
- `Collections/` 是八个实体事实源；Pillars 与 Life Aspirations 必须保持独立。
- 模板可以分文件，但 Goal/Project/Routine 实例全部进入 `Collections/GPR/`；Pillar 实例进入 `Collections/Pillars/`，Life Aspiration 实例进入 `Collections/Life Aspirations/`。
- Cycle 路径固定为：Daily -> `Collections/Cycles/Daily/`，Weekly -> `Collections/Cycles/Weekly/`，DuoCycle -> `Collections/Cycles/DuoCycle/`，Annual -> `Collections/Cycles/Annual/`。四个子目录仍属于同一个 Cycles 事实源。
- 不在八集合之外另建 Cycles 或 Remembrance 文件，也不把 Goals/Projects/Routines、Daily/Weekly 拆成新的逻辑库。

## Properties 映射

YAML 属性名与稳定枚举值统一使用英文 `snake_case`。字段语义保持与 core 一致，例如：

- `Do-Date` -> `do_date`
- `GPR Relation` -> `gpr_relation`
- `Notable` -> `notable`

- Relation 值使用 wikilink 列表，例如 `gpr_relation: ["[[Project - Launch]]"]`。
- 稳定枚举值使用英文 `snake_case`，例如 `status: in_progress`。

## 最小存储、缺失值与文件名

只保存必要字段：建立对象、维持其生命周期、时间安排和主链/实际上下文所需的源事实。必填字段和必填链路在创建时写入；例如 Action 的 `do_date`、`gpr_relation`，Goal 的 `aspirations_relation` 与 Project/Routine 的 `parent_gpr`。Cycle 默认不写 frontmatter；种类由其 `Collections/Cycles/` 下的直接子目录、Period 由 ISO 文件名确定，实际记录发生时才按需追加 `tracking_fields` 或 `notable: true`。

- 可选日期、文本、关系和布尔值只在有真实值时写入；清空时删除属性，不写空字符串、`[]` 或 `false`。`notable: true` 是唯一需要写入的 Notable 值。
- `auto_data`、`auto_data_summary` 及其他查询派生字段绝不写入 YAML；查询结果或正文复盘承载摘要。
- 缺失属性表示未提供、未关联或不适用，不等于可被筛选的空值。查询只以正向事实匹配：枚举相等/属于明确集合、日期存在且满足范围、关系包含目标、或 `notable == true`。Cycle 的 Period 从目录和文件名解析，不用 Property。不要用空数组、属性存在性或否定空值来推断业务状态。
- `file.name` 是全部对象的标题和排序名称，Topic 也一样；YAML 不写 `name` 或 `topic_name`。文件名去除扩展名后应为面向人的唯一标题；重命名时使用 Obsidian CLI，让 wikilink 跟随 vault 设置更新。

## Collections 与父子关系

任何 Relation 都只在权威写入方保存。创建、修改或移动对象时，禁止向被引用对象双向回填；反向展示只使用 Obsidian backlinks、Base 或查询。

### Pillars 与 Life Aspirations

- Pillar 写入 `Collections/Pillars/`，不写 `type` 或 `parent_pillar`。
- Life Aspiration 写入 `Collections/Life Aspirations/`，不写 `type`；`parent_pillar` 必须是 `Collections/Pillars/` 中 Pillar 的 wikilink。
- Pillar 的子愿景由 Base 查询 `parent_pillar` 反向聚合；Life Aspiration 的 Goals 由 Goal 的 `aspirations_relation` 反向聚合。

### GPR

- `type: goal` 不写 `parent_gpr`，`aspirations_relation` 必须连接 Life Aspiration。
- `type: project` 或 `type: routine` 的 `parent_gpr` 必须连接 Goal，不写 `aspirations_relation`。
- Actions 使用 `gpr_relation` 连接 Project/Routine；GPR 不保存反向 Actions 字段。

### Cycles

- Daily、Weekly、DuoCycle、Annual 分别创建在各自的 `Collections/Cycles/` 子目录中，默认不写 YAML；实际记录发生时才按需追加 `tracking_fields` 或 `notable: true`，不写任何 Cycle Relation、Period 或类型 Property。
- 文件名是 Cycle 的 Period：Daily 为 `YYYY-MM-DD`，Weekly 为 ISO 周 `YYYY-Www`，DuoCycle 为起始奇数月 `YYYY-MM`（覆盖该月及次月），Annual 为 `YYYY`。Base 从目录和文件名解析 Period。
- Daily 的单日 Period 落入 Weekly 的范围、Weekly 落入 DuoCycle 的范围、DuoCycle 落入 Annual 的范围时，Base 以解析后的 Period 显示层级并聚合内容。
- 高层 Cycle 尚未创建时，低层 Cycle 仍是有效记录；后续创建高层 Cycle 后，查询自动纳入已有低层记录，不回填 Properties。
- 只在相邻高层种类目录查找完整包含当前范围的容器：零个匹配合法；多个匹配是同级日期范围重叠的查询异常，必须显示异常而不任选一个容器。

## 单向关系权威表

| 权威写入方 | Property | 被引用方 |
| --- | --- | --- |
| Life Aspiration | `parent_pillar` | Pillar |
| Goal | `aspirations_relation` | Life Aspiration |
| Project/Routine | `parent_gpr` | Goal |
| Action | `gpr_relation` | Project/Routine |
| Action | `topic_relation` | Topic |
| Action | `people_relation` | Person |
| GPR | `topic_relation` | Topic |
| Neurobit | `topic_relation` | Topic |
| Neurobit | `gpr_relation` | Goal/Project/Routine |

Topic、People、Pillar、Life Aspiration、Goal 与 Project/Routine 的反向列表都从上表字段查询，不添加镜像 Property。Cycle 层级完全由目录和 ISO 文件名派生，不建立 Relation 或 Period Property。

## 日期、时间范围与 Cycle 文件名

- `do_date`、`due_date` 使用 ISO 日期：`YYYY-MM-DD`；GPR 的 `timeline` 使用 ISO 8601 interval 文本：`YYYY-MM-DD/YYYY-MM-DD`。
- Cycle 不写日期 Property。路径与文件名格式固定为：`Daily/YYYY-MM-DD.md`、`Weekly/YYYY-Www.md`、`DuoCycle/YYYY-MM.md`、`Annual/YYYY.md`；DuoCycle 的月份必须为 `01`、`03`、`05`、`07`、`09`、`11`，表示从该月起的两个月。
- Base 需要时间线或范围筛选时，从 interval 或 Cycle 文件名派生起止日期；Cycle 的层级和聚合以解析后的区间包含或重叠派生，不要新增未在 core schema 中定义的业务字段。

## AutoData 等价映射

Obsidian 中的 AutoData 由 Bases/查询计算，不写回 YAML。

GPR AutoData 至少可聚合：

- 反查 `parent_gpr` 指向当前 Goal 的 Project/Routine 数；
- 反查 `gpr_relation` 指向当前 Project/Routine 的 Actions 总数与 Done 数；
- `topic_relation`；
- 经 Actions 的 `people_relation` 浮现的相关 People。

Cycle Auto-Data Summary 至少可聚合：

- 查询解析后的 Period 落入当前 Cycle 范围的低层 Cycle；
- 用户实际采用的 `tracking_fields`；
- Weekly 下属 Daily 的平均值与总量。

查询结果是权威关系的投影。需要保留复盘结论时写入正文，不创建派生 Properties 快照。

## Remembrance 映射

Remembrance Collection 是 Knowledge Zone 中针对 Actions、GPR、Neurobits、Cycles 的 Base/查询：

```text
notable == true
AND object date overlaps current Annual Period
```

Action 使用 `do_date`，GPR 使用 `timeline`，Cycle 使用从目录与文件名解析的 Period；Neurobit 优先使用相关 GPR 的 `timeline`，无可用关系时使用 Obsidian 原生 `file.ctime`。`file.ctime` 仅用于查询，不写入业务 Properties，也不新增 Remembrance 实体。

## Obsidian CLI 协议

实际 vault 操作必须遵循：

1. 先运行 `obsidian help`，再按需运行 `obsidian help <command>`，以本机安装版本输出为准。
2. 用 CLI 选择目标 vault、列目录、读取 Zone/Collection/Template 和当前 Properties。
3. 用 CLI 的 create、property、append/prepend、move 等命令完成修改；不得直接写 vault 文件。
4. 用 CLI 读回每个已改文件和 Properties。
5. 用 CLI 执行 search、backlinks、links 或 base query，验收关系和视图。

命令形态示意：

```text
obsidian vault=<vault> help
obsidian vault=<vault> create ...
obsidian vault=<vault> read ...
obsidian vault=<vault> property:set ...
obsidian vault=<vault> search ...
obsidian vault=<vault> base:query ...
```

这些只是命令族示意；参数名必须取自本机 `obsidian help`，不要凭记忆猜测。CLI 不可用或某命令缺失时，报告限制并停止对应 vault 写操作，不改用 GUI 或直接文件写入。

## CLI 验收

至少验证：

- 三个 Zone、八个 Collection 路径及四个 Cycle 子目录存在；
- 每种模板的必要 Properties 可由 CLI 读回，空值、派生字段和冗余标题属性不存在；
- 所有稳定枚举均为 core 映射中的 snake_case 值；
- 每个 Action 有 `do_date`，且 `gpr_relation` 目标是 Project/Routine；
- Goal -> Aspiration -> Pillar、Project/Routine -> Goal 可沿 wikilink 查询；Daily -> Weekly -> DuoCycle -> Annual 按目录与 ISO 文件名解析的 Period 查询，且高层 Cycle 缺失不视为错误；
- Focus 的到期未完成、Top Priorities、Waiting、Routine、Daily 查询命中预期对象；
- Knowledge Queue、Topic resurfacing、People 与年度 Remembrance 查询命中预期对象；
- 不存在额外 Remembrance 实体文件，Pillars 与 Life Aspirations 保持为两个独立 Collections，其余逻辑集合未被拆开。
