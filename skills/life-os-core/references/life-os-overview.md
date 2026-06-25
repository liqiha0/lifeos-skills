# Life OS 总览

## 目标

这套 Life OS 围绕三个结果设计：

1. **Focus**：聚焦当前最重要、可执行的工作。
2. **Alignment**：让近期行动服务于长期愿景和核心价值。
3. **Knowledge resurfacing in context**：让知识在正确的工作情境中重新出现。

它通过统一结构管理 open loops、方向、执行、知识与复盘，帮助用户主动掌控生活。

## 四个顶层模块

| 模块 | 职责 |
| --- | --- |
| Pillars | 明确核心价值与 Life Aspirations |
| Pipelines | 将愿景转化为 Goals、Projects、Routines 和 Actions |
| Vaults | 捕获、关联并在情境中浮现知识与人脉信息 |
| Cycles | Review, Learn & Plan，持续校准系统 |

## 概念链

```text
Pillars
  -> Life Aspirations
    -> Goals
      -> Projects / Routines
        -> Actions
```

概念链用于表达方向与执行之间的关系。实际约束取决于对象所处的生命周期。

## 关系策略

默认使用完整关系模式；最小压缩模式用于低维护或迁移场景。

| 对象 | 草稿 / Inbox | Active / Processed | 设计理由 |
| --- | --- | --- | --- |
| Pillar | 不适用 | 无上级 | 系统顶层价值 |
| Aspiration | 可暂不关联 | 至少关联 1 个 Pillar | 活跃愿景必须有价值依据 |
| Goal | 可暂不关联 | 必须关联 1 个主要 Aspiration | Goal 是战略承诺 |
| Project | 可暂不关联 | 关联 Goal，或填写独立原因 | 支持维护、义务和探索型项目 |
| Routine | 可暂不关联 | 关联 Goal，或填写独立原因 | 支持基线维护 |
| Action | 可不关联 | 可不关联 | 临时、行政、维护和应急行动合法 |
| Neurobit | 可不关联 | 至少有 1 个上下文关系 | 没有上下文就无法可靠浮现 |

独立原因使用：

- `maintenance`：维持已有系统、健康、家庭或资产。
- `obligation`：法律、行政、合同或他人承诺。
- `exploration`：尚未形成 Goal 的有限探索。
- `standalone`：规模不足以建立上层对象的一次性事项。

Cycles 应检查长期游离的活跃对象，决定补充关系、继续独立、降级、归档或删除。
