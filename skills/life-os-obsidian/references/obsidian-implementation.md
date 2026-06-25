# Life OS 的 Obsidian 实现

## 定位

这是 Life OS 的 Obsidian 实现规范。目标是保留 Focus、Alignment、Contextual Resurfacing 和 Cycles 的系统语义，同时利用 Markdown、Properties、wikilinks、Bases 或查询视图。

## Zone-first 目录

```text
Alignment Zone/
  Pillars/
  Aspirations/
  Alignment.md
  Mindset & Identity.md

Focus Zone/
  Goals/
  Projects/
  Routines/
  Actions/

Knowledge Zone/
  Neurobits/
  Topics/
  People/
  Remembrance/

Cycles/
  Daily Entries/
  Weekly Reviews/
  DuoCycles/
  Annual Reviews/

Dashboards/
Inbox/
Archives/
Templates/
```

目录表达工作区域，Properties 和 links 表达实体关系。目录层级保持简洁。

## 完整关系模式

| 实体 | 推荐关系 | 说明 |
| --- | --- | --- |
| Aspiration | `关联支柱` | draft 可空；active 至少 1 个 |
| Goal | `主要愿景` | draft 可空；active 必须 1 个主要愿景 |
| Project | `关联目标` | active 默认关联；否则填写独立原因 |
| Routine | `关联目标` | active 默认关联；否则填写独立原因 |
| Action | `关联项目` 或 `关联惯例` | 独立 Action 可留空 |
| Neurobit | `关联主题`、`关联项目`、`关联目标`、`关联人物` | inbox 可空；processed 至少 1 个 |

约束取决于生命周期和对象层级，只建立真实、有用的关系。

## 最小压缩模式

当用户需要低维护成本时，可以省略 Aspirations 和 Routines 的显式数据库关系：

```text
Goal -> Pillar
Project -> Goal
Action -> Project
```

建议字段：

| 实体 | 最小属性 |
| --- | --- |
| Action | `状态`、`执行日`、可选 `关联项目` |
| Project | `状态`、可选 `关联目标` |
| Goal | `状态`、可选 `关联支柱` |
| Neurobit | 可选 `关联主题`、`关联项目` |

最小压缩模式省略 Aspiration 时，Active Goal 至少关联一个 Pillar。

## Action 与 Today 工作台

推荐属性：

```yaml
状态: todo
执行日: 2026-07-10
截止日:
关联项目:
关联惯例:
等待对象:
跟进日:
```

- `执行日` 对应 DO Date。
- `截止日` 只在存在真实外部截止时间时填写。
- `状态` 可先使用 `todo` / `done`，按需要增加 `paused`。
- 独立 Action 可以没有项目或惯例。
- 有关联项目表示 Project 来源，有关联惯例表示 Routine 来源，两者都为空表示独立 Action。

Today 工作台的稳定过滤语义：

```text
状态 != done
AND 执行日 <= today
```

可按工具能力增加等待、暂停或无日期视图。

按实际工作方式配置 `优先级` 枚举和顺序。

## Goals、Projects 与 Routines

完整模式：

```yaml
# Goal
状态: draft
主要愿景:
目标日期:

# Project
状态: active
关联目标:
独立原因:

# Routine
状态: active
关联目标:
独立原因:
```

Project 有完成点；Routine 是重复过程。Active Project/Routine 没有关联 Goal 时，`独立原因` 使用 `maintenance`、`obligation`、`exploration` 或 `standalone`。Routine 的单次执行需要 DO Date、Today 展示、独立完成状态或执行上下文时生成 Action；轻量打卡记录在 Daily Entry。

## Pillars 与 Aspirations

Pillars 文件通常只需要标题和正文。`Alignment.md` 是非实体工作页，用于保存价值、意义和方向反思，以及 2–6 个 Pillars 的提炼过程。

Aspiration 推荐属性：

```yaml
关联支柱:
状态: draft
```

本 Life OS 使用 `draft`、`active`、`archived`。Aspiration 在 `draft` 时可无 Pillar，进入 `active` 前至少关联一个 Pillar。完整模式的 Goal 在 `draft` 时可无 Aspiration，进入 `active` 前必须关联一个主要 Aspiration；最小模式则至少关联一个 Pillar。

## Neurobits 与 Topics

Neurobit 推荐按需使用：

```yaml
关联主题:
关联项目:
关联目标:
关联人物:
notable: false
状态: inbox
```

`inbox` 状态允许所有关系为空；处理时至少填写一个 Topic、Project、Goal 或 Person 关系，再将状态改为 `processed`。

Topic 页面应包含：

- 对该主题的持续综合正文；
- 指向相关 Neurobits 的聚合视图；
- 相关 Projects 和 Goals。

## People 与 Remembrance

Person 页面保存稳定背景和互动上下文，通过关系或查询聚合相关 Neurobits、Projects 和 Actions。

Remembrance 是 Notable 内容的聚合区。`notable` 是可选标记。

## Cycles

推荐目录与 Life OS 层级一致：

```text
Annual Reviews
DuoCycles
Weekly Reviews
Daily Entries
```

DuoCycle 模板可包含：

- 本周期成功标准；
- 1–3 个重点；
- 周期结束评估与学习。

Weekly 和 Daily 模板内容按实际使用设计。Inbox 清理、Project 检查、量化追踪和 Buffer Time 等按需要加入。

## 设计层次

设计或解释系统时区分：

- **核心模型**：对象、关系、生命周期和运行原则。
- **Life OS 设计**：我们选定的约束和工作流。
- **Obsidian 实现**：目录、YAML、wikilinks、Bases 和查询设计。

## 验证清单

- Pillars 表示核心价值，而不是生活领域。
- Goals 位于 Focus Zone。
- Aspirations 位于 Pillars 与 Goals 之间。
- 独立 Actions 和未整理 Inbox 项合法。
- DuoCycles 是默认中期复盘周期。
- 优先级、Daily/Weekly 清单和量化指标保持可调整。
