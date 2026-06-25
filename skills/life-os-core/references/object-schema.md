# 对象与字段

这里定义含义、取值与关系；属性拼写和存储方式由实现层映射。所有对象都有标题和正文，正文承载愿景、成果标准、范围、来源说明、决策或复盘，不要求固定章节。

## 字段参考

| 对象 | 字段 |
| --- | --- |
| Action | 状态、计划执行日、截止日、实际完成日、优先级、行动类型、生活分类、所属 Project / Routine、Topics、People、Notable、重要事件日期 |
| Goal | 状态、计划时间范围、实际完成日、生活分类、Aspirations、Topics、Notable、重要事件日期 |
| Project / Routine | 状态、计划时间范围、实际完成日、生活分类、上级 Goal、Topics、Notable、重要事件日期 |
| Pillar | 状态、生活分类；价值与原则写正文 |
| Life Aspiration | 状态、生活分类、Pillar；愿景写正文 |
| Topic | 重要程度、生活分类 |
| Neurobit | 待处理标记、内容分类、质量评价、来源、Topics、Goals、Projects、Routines、Notable、重要事件日期 |
| Person | 状态、生活分类；联系方式和关系背景写正文 |
| Cycle | 周期种类、覆盖范围、按需追踪的指标、Notable；复盘写正文 |

未排期的 Action 可以没有状态；设置计划执行日后，记录实际状态。新建 Goal、Project 或 Routine 时先明确对象种类，状态和其他属性按已知事实补充；已在执行、暂停或完成时记录实际状态。已形成的知识笔记不必进入阅读队列。

## 状态与分类

| 字段 | 取值与含义 |
| --- | --- |
| Action 状态 | 待办、进行中、等待、完成；等待包括外部依赖或暂缓后重看 |
| Goal / Project / Routine 状态 | 活跃、暂停、已完成；缺失表示尚未声明生命周期状态。Routine 的活跃表示机制仍在运行，单次执行完成不代表 Routine 完成 |
| Pillar / Aspiration / Person 状态 | 活跃、非活跃；不使用状态时不据此推断失效 |
| Neurobit 待处理标记 | `queue` 表示仍待处理；缺失表示不在待处理队列，不区分处理中与已处理 |
| 生活分类 | Work、Personal、Connections |
| 行动类型 | Task、Meeting、Errand、Event |
| Action 优先级 | #1、#2、Urgent、Scheduled、Quick、Remember；是选做标签而非纯紧急度分数 |
| Neurobit 内容分类 | Media、Notes、Documents |
| Neurobit 质量评价 | Excellent、Very Good、Average、Poor |
| Topic 重要程度 | Critical、Essential、Beneficial、Tangential |

分类、优先级和评价均可不填；不凭标题猜测用户的价值排序。Goal 完成以目标结果为准，Project 完成以交付标准为准；子行动完成不等于上级完成。取消或放弃不等于完成，原因写正文。

Notable 标记值得记住的 Action、Goal、Project、Routine、Neurobit 或 Cycle。

## 关系

| 引用方 | 目标 | 数量（填写时） |
| --- | --- | --- |
| Life Aspiration | Pillar | 一个 |
| Goal | Life Aspiration | 一个或多个 |
| Project / Routine | Goal | 一个 |
| Action | Project / Routine | 两类合计最多一个；填写时选其一 |
| Action / Goal / Project / Routine / Neurobit | Topic | 一个或多个 |
| Action | Person | 一个或多个 |
| Neurobit | Goal / Project / Routine | 一个或多个 |

关系仅由引用方维护，允许暂缺，不编造上级或用错误类型凑上级。Goal 通过 Life Aspiration 对齐方向，不以 Goal、Project 或 Routine 作为父节点；Project / Routine 通过 Goal 对齐愿景，不重复保存愿景关系。Neurobit 可同时关联多个 Goal、Project 和 Routine，但不因关联 Project / Routine 而补写间接关联的 Goal。Cycle 不保存父周期关系。

## 时间字段

- **计划执行日**：打算执行的日期；等待项表示下次跟进或重看日期。未排期时为空。
- **截止日**：真实截止约束，与计划执行日独立。
- **计划时间范围**：Goal、Project 或 Routine 的开始和结束边界，可只知道一端；两端都有时开始不晚于结束。
- **实际完成日**：记录实际完成的日期。补录历史事项而时间未知时不填，不借用计划日期。
- **重要事件日期**：Notable 内容所代表的事件发生日，不是点击标记的日期。若标记的就是完成事件，可直接使用实际完成日。Cycle 使用自身覆盖范围。

改变计划不改变历史事实；未知日期不编造。
