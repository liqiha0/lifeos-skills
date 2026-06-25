# Vaults 与情境化知识

## 目的

Vaults 捕获和组织信息，为 Pipelines 提供资源，并让相关知识在正确的工作情境中重新浮现。

主要对象：

- **Neurobits**：纳入情境化知识网络的信息对象。
- **Media / Notes / Documents**：Neurobits 的主要媒介类型。
- **Topics**：持续积累和提炼的知识领域。
- **People**：与人相关的稳定信息和互动上下文。
- **Notable / Remembrance**：标记并集中回顾值得记忆的内容。

## Neurobits

> Neurobit 是用户选择纳入情境化知识网络、需要通过 Topic、Project、Goal 或 Person 等关系重新浮现的信息对象。

- Inbox 中的捕获项可以暂时没有关系。
- 探索型知识可以只关联 Topic。
- 项目资料可以关联 Project 或 Goal。

## Topics

Topic 页面用于：

- 持续综合和提炼某个知识领域。
- 聚合相关 Neurobits。
- 连接相关 Projects、Goals 和外部知识。

Topic 是包含主题理解和相关关系的知识对象。

## People

Person 页面承载背景、互动、会议、合作和相关资料。相关 Action 通过关系在 Person 页面浮现。

## Contextual Resurfacing

```text
捕获信息
  -> 建立一个或多个有意义的上下文关系
    -> 在 Topic / Project / Goal / Person 上下文中浮现
      -> 在实际工作或复盘中使用
```

关系建立后，相关内容在对应上下文中浮现。

## 捕获与处理

使用两阶段约束：

- `inbox`：允许所有关系为空，保证低摩擦捕获。
- `processed`：至少建立一个有意义的上下文关系，例如 Topic、Project、Goal 或 Person。

最小处理动作：选择至少一个上下文关系，并将状态从 `inbox` 改为 `processed`。

处理阶段判断信息是否值得进入情境化知识网络，并据此保留、归档或删除。

## Notable 与 Remembrance

- `Notable` 是可选标记，用于标识值得回顾的内容。
- Remembrance 聚合被标记的内容。
- 可以按年度或周期组织集合。
