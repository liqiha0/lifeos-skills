---
name: life-os-core
description: 定义并运行 Life OS 的领域模型与工作流。
---

# Life OS Core

Life OS 围绕三个结果运行：

- **Alignment**：让行动沿主链对齐长期方向。
- **Focus**：把系统收窄为当前可执行的少数重点。
- **Knowledge resurfacing**：让知识、人物和经验在工作情境中重新出现。

## 核心结构

三个 Primary Zones 是工作入口，八个逻辑集合是实体事实源：

- **Alignment Zone**：Pillars、Life Aspirations、GPR 分解、Timeline 与 Cycle Reviews。
- **Focus Zone**：Quick Add、今日重点、Actions、Routines、Daily Entry 与 Do-Date 排程。
- **Knowledge Zone**：Topic Vault、Neurobits、People 与 Remembrance Collection。
- **八个集合**：Actions、GPR、Pillars、Life Aspirations、Topic Vault、Neurobits、Cycles、People。

GPR 合并 Goal、Project、Routine，以 `Type` 区分；Pillars 与 Life Aspirations 是独立事实源，仅在 Alignment Zone 的 Pillars & Aspirations 视图中联合展示；Cycles 包含 Daily、Weekly、DuoCycle、Annual 四个固定种类。

## 五层主链

概念层级保持自上而下：

```text
Pillar -> Life Aspiration -> Goal -> Project / Routine -> Action
```

权威 Relation 自下而上单向写入：

```text
Action -> Project / Routine -> Goal -> Life Aspiration -> Pillar
```

下游对象引用上游对象。界面可按概念层级反向展开，但不向上游对象回填子项 Relation。

- Pillar 是持续存在的人生领域、核心价值观和长期定位。
- Life Aspiration 是长期、宽广、情感化的愿景。
- Goal 是具体、可衡量且有时间点的结果。
- Project 是一次性、多步骤、有明确完成点的推进单元。
- Routine 是长期重复的推进或维持机制，与 Project 同级。
- Action 是有 Do-Date 的原子执行单元，必须连接 Project 或 Routine。

Topic Vault、Neurobits 与 People 提供知识和人物上下文，不形成替代主链。

## Cycles 与 Remembrance

```text
Daily -> Weekly -> DuoCycle -> Annual
```

这是时间范围的展示层级，不是 Relation 链。Daily、Weekly、DuoCycle、Annual 是固定的 Cycle 种类；实例创建时只需确定 Period，实际记录发生时才按需追加 Tracking Fields 或 `notable: true`。包含、聚合和年度归属由周期范围派生，高层 Cycle 尚未创建时低层记录仍有效。`Notable` 用于 Actions、GPR、Neurobits 与 Cycles；选中项进入按年度组织的 Remembrance Collection，源对象保持原位。

## 路由

- 对象、字段、关系、枚举：`references/object-schema.md`
- Alignment、Focus、Knowledge 三个 Zone：`references/zones.md`
- Daily、Weekly、DuoCycle、Annual 与 Remembrance：`references/operating-cycles.md`
