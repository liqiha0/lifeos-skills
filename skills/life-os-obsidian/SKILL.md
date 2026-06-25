---
name: life-os-obsidian
description: 在 Obsidian 中实现 Life OS 的结构、模板、查询与运行流程。
---

# Life OS Obsidian

本 skill 只负责把 `life-os-core` 的规格等价映射到 Obsidian，不重新定义领域模型。

## 路由

1. 先加载 `life-os-core`；涉及对象字段或枚举时再读取其 `object-schema.md`。
2. 目录、关系、Properties、日期、AutoData 与 CLI：`references/obsidian-implementation.md`。
3. 创建实体笔记时读取 `references/templates.md`，使用对应类型的最小 YAML。
4. 创建 Zone、Base、查询或工作流入口时读取 `references/bases-and-queries.md`。

## 硬性约束

- 三个 Zone 只做入口；八个 Collections 是实体事实源。
- Properties 只保存对象当前必需的源事实；省略空值、`false`、派生字段、反向聚合关系和与 `file.name` 重复的标题属性。
- 单向关系是硬约束：每条语义关系只有一个权威正向 Property；被引用对象不得回填反向 Property，反向关联只用 backlinks、Base 或查询派生。
- GPR 以 `type` 区分 Goal、Project、Routine；Cycles 以 `Collections/Cycles/Daily|Weekly|DuoCycle|Annual/` 子目录区分种类，不写类型 Property；Pillars 与 Life Aspirations 分属独立 Collections，并用 wikilink 表达父子关系。
- Cycles 不保存 Relation、Period 或类型 Property，Action 不保存 Cycle 引用；Cycle 的层级、聚合和年度归属由目录与 ISO 文件名派生，高层 Cycle 可缺失。
- Action 必须有 `do_date`，并通过 `gpr_relation` 关联一个 Project 或 Routine。
- 所有 Properties 名和稳定枚举值统一使用英文 `snake_case`；领域显示名服从 core 的 `object-schema.md`。
- Remembrance 是 Knowledge Zone 和 Annual Review 中的年度查询，不创建额外实体文件。
- 实际 vault 的创建、读取、修改、移动、查询和验收全部使用 Obsidian CLI；不得改用文件系统写入或图形界面。
