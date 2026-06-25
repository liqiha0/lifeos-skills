# Pipelines 与行动引擎

## 对象

Pipelines 是执行层，包含：

- **Goals**：具体、近期、战术性且通常有时间边界的结果。
- **Projects**：一次性、多步骤、有明确成果的推进单元。
- **Routines**：规律重复的过程。
- **Actions**：可直接执行的具体事项。

本 Life OS 只使用一种 Action 实体。Action 可以关联 Project、关联 Routine，或独立存在。

## 概念关系

```text
Life Aspiration
  -> Goal
    -> Project -> Action
    -> Routine -> Action 或 Daily Entry 记录
```

Goal 可以通过 Project、Routine 或两者推进。

## 生命周期与关系约束

- Active Goal 必须关联一个主要 Aspiration。
- Active Project 和 Active Routine 默认关联一个 Goal。
- 无 Goal 时填写 `独立原因`：`maintenance`、`obligation`、`exploration` 或 `standalone`。
- `exploration` 配置范围或复查日期。
- Action 的上级关系始终可选；与 Project/Routine 有明确关系时再建立。
- 行政、维护、临时和应急 Action 可以独立存在。

周期复盘应列出活跃且无 Goal 的 Project/Routine，检查其独立原因是否仍成立。

## Routine 的执行记录

Routine 的一次执行在以下情况生成 Action：

- 需要具体 DO Date。
- 需要进入 Today 工作台。
- 需要独立完成状态、备注、附件或执行上下文。
- 错过后需要重新安排。

- 轻量、重复且只需打卡的执行记录在 Daily Entry 中。

## DO Date

DO Date 表示用户**打算执行该事项的日期**，不同于必须完成或交付的 Due Date。

- `DO Date`：计划动手处理的日期。
- `Due Date`：真实存在的外部截止日期。

Today 呈现 DO Date 已到且尚未完成的 Action。

## Focus Zone

Focus Zone 把庞大后台系统收窄为当下可执行界面：

- 只呈现当前需要关注的事项。
- 用 DO Date 和状态控制浮现。
- 按执行情境或专注顺序组织。

优先级名称和顺序可以根据实际工作方式调整。

## Backlog、Pause、Waitlist 与 Resurface

- **Backlog**：尚未进入近期执行窗口的事项集合。
- **Pause**：暂时停止推进的状态。
- **Waitlist / Waiting On**：等待他人或外部条件的事项集合。
- **Resurface**：让事项在未来条件满足或日期到达时重新出现的机制。

## 最小状态

Action 默认使用：

- `todo`
- `done`
- 可选 `paused`

等待事项可增加：

- `等待对象`
- `跟进日`

`执行日` 专用于计划执行日期；状态、等待和跟进使用独立字段。
