---
name: life-os-obsidian
description: 在 Obsidian 中设计、搭建和维护个人 Life OS，包括 Zone-first 结构、属性关系、工作台、知识浮现和周期复盘。
---

# Life OS Obsidian

本 skill 将 Life OS 模型落地到 Obsidian。

## 开始前

1. 涉及 Life OS 概念时，先加载 `life-os-core`，并遵循该 skill 的 reference 路由。
2. 涉及目录、属性、模板或视图时，读 `references/obsidian-implementation.md`。
3. 实际操作 Obsidian vault 时使用 Obsidian CLI。

## 默认结构

采用 Zone-first：

- `Alignment Zone/`：Pillars、Aspirations、Alignment、Mindset & Identity。
- `Focus Zone/`：Goals、Projects、Routines、Actions。
- `Knowledge Zone/`：Neurobits、Topics、People、Remembrance。
- `Cycles/`：Daily、Weekly、DuoCycles、Annual。

Goals 在概念上属于 Pipelines，在 Obsidian 中落到 Focus Zone。

## 实施原则

- 笔记承载实体，Properties 表达字段，wikilinks 表达关系，Bases/查询表达视图。
- 先建立每天会使用的最小闭环，再扩展完整模型。
- 使用本 Life OS 的分层约束：草稿可空；活跃 Aspiration/Goal 必须对齐；Project/Routine 可凭明确原因独立；Action 可独立。
- 允许独立 Action 和未整理 Inbox 项。
- 公式、按钮、状态名和优先级枚举属于 Obsidian 实现层。
- 属性名可用中文；稳定枚举值使用英文小写。

## 两种关系模式

完整 Life OS 模式：

```text
Pillar <- Aspiration <- Goal <- Project / Routine <- Action
```

最小压缩适配：

```text
Pillar <- Goal <- Project <- Action
```

最小模式是本地取舍，不是完整 Life OS 模型。完整模式要求 Active Goal 关联主要 Aspiration；最小模式省略 Aspiration 时，Active Goal 至少关联一个 Pillar。两种模式都执行 Project/Routine 的独立原因规则。

## 默认落地顺序

1. Actions、DO Date 和 Today 工作台。
2. Projects、Routines 与 Goals。
3. Pillars、Aspirations 与完整对齐关系。
4. Neurobits、Topics 和项目知识浮现。
5. Weekly Review、DuoCycles、Remembrance。

## 完成标准

- Goals 等对象的 Life OS 概念归属与 Obsidian 落地区域已明确区分；
- 属性只为实际查询或关系服务；
- Inbox 可以低摩擦捕获；
- 重要对象能在相关上下文重新浮现；
- 核心模型、Life OS 设计和 Obsidian 实现已明确区分。
