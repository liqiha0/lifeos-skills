# 最小 YAML 模板

所有模板实例由 Obsidian CLI 创建。日期和 wikilink 是占位符；创建时替换为真实值。标题由文件名提供，不写 `name` 或 `topic_name`。仅在有真实值时追加可选属性；Cycle 默认不创建 frontmatter。

## 目录

- Action
- Goal、Project、Routine
- Pillar、Life Aspiration
- Topic、Neurobit
- Daily、Weekly、DuoCycle、Annual
- Person

## Action

```yaml
---
do_date: 2026-07-28
priority: scheduled
status: to_do
type: task
category: personal
gpr_relation: ["[[Project - {{project}}]]"]
---
```

创建完成前必须填写 `do_date`、一个 Project/Routine 的 `gpr_relation`，以及真实的 `priority`、`type`、`category`。
可按需追加正向的 `topic_relation`、`people_relation`；不得向对应 Topic 或 Person 回填反向属性。Action 的周期归属只由 `do_date` 查询。

## Goal

```yaml
---
type: goal
status: not_started
category: personal
timeline: "2026-01-01/2026-12-31"
aspirations_relation: ["[[Life Aspiration - {{aspiration}}]]"]
---
```

Goal 必须填写 Life Aspiration 的 `aspirations_relation`；不写 `parent_gpr`。
可按需追加正向的 `topic_relation`。

## Project

```yaml
---
type: project
status: not_started
category: personal
timeline: "2026-07-28/2026-08-31"
parent_gpr: ["[[Goal - {{goal}}]]"]
---
```

Project 必须填写 Goal 的 `parent_gpr`；不写 `aspirations_relation`。
可按需追加正向的 `topic_relation`。

## Routine

```yaml
---
type: routine
status: active
category: personal
timeline: "2026-07-28/2026-12-31"
parent_gpr: ["[[Goal - {{goal}}]]"]
---
```

Routine 必须填写 Goal 的 `parent_gpr`；每次原子执行由关联该 Routine 的 Action 记录。
可按需追加正向的 `topic_relation`。

## Pillar

```yaml
---
category: personal
status: active
---
```

## Life Aspiration

```yaml
---
category: personal
status: active
parent_pillar: ["[[Pillar - {{pillar}}]]"]
---
```

Life Aspiration 必须填写 Pillar 的 `parent_pillar`。

## Topic

```yaml
---
significance: beneficial
category: personal
---
```

Topic 不写 Neurobits、GPR 或 Actions 的反向属性；相关对象由查询聚合。

## Neurobit

```yaml
---
category: notes
status: queue
---
```

可按需追加正向的 `topic_relation` 和 `gpr_relation`；不得向 Topic 或 GPR 回填反向属性。

## Daily

写入 `Collections/Cycles/Daily/`。

文件名使用 `YYYY-MM-DD.md`，例如 `2026-07-28.md`。默认不写 frontmatter；Weekly 尚未创建时照常记录，之后由文件名解析的 Period 归入 Weekly。

## Weekly

写入 `Collections/Cycles/Weekly/`。

文件名使用 ISO 周 `YYYY-Www.md`，例如 `2026-W31.md`。默认不写 frontmatter；DuoCycle 尚未创建时照常记录。

## DuoCycle

写入 `Collections/Cycles/DuoCycle/`。

文件名使用起始奇数月 `YYYY-MM.md`，例如 `2026-07.md` 表示 2026 年 7–8 月。默认不写 frontmatter；Annual 尚未创建时照常记录。

## Annual

写入 `Collections/Cycles/Annual/`。

文件名使用 `YYYY.md`，例如 `2026.md`。所有 Cycle 默认不写 frontmatter；种类由所在子目录、Period 由 ISO 文件名确定。有实际值时才追加 `tracking_fields` 或 `notable: true`，不写 Cycle Relation、Period 或类型 Property。

## Person

```yaml
---
category: connections
status: active
---
```

Person 不写 Actions 或 GPR 的反向属性；相关工作上下文由查询聚合。
