---
name: life-os-core
description: 定义 Life OS 的对象、字段、关系、状态和时间语义，不依赖具体软件。
---

# Life OS 数据模型

## 对象

| 对象 | 含义 |
| --- | --- |
| Pillar | 核心价值、意义与希望践行的原则 |
| Life Aspiration | 长期、宽广且有情感意义的愿景 |
| Goal | 有衡量方式和时间边界的阶段结果 |
| Project | 有明确完成点和交付标准的多步骤工作 |
| Routine | 持续重复的推进或维护机制 |
| Action | 可直接执行的具体事项，可归属 Project 或 Routine |
| Topic | 主题索引，关联知识和工作，不代替项目 |
| Neurobit | 文章、视频、书籍、文档、会议记录、思考或笔记 |
| Person | 需要保留关系背景与互动上下文的人物或机构 |
| Cycle | 覆盖指定周期的记录；Daily、Weekly、DuoCycle、Annual 分别对应日、周、双月和年 |

Goal、Project、Routine 是独立对象。会议安排是 Action，值得独立保存的会议内容是 Neurobit。

字段、状态、关系数量和时间含义见[对象与字段](references/object-schema.md)。
