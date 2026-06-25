# Life OS 的 Obsidian 存储

字段名、取值和语义见 [SKILL.md](../SKILL.md)；本文只规定如何存储。字段名和取值原样作为 Property 键和值。

## 对象 → 文件

每个对象对应一个 Markdown 文件：文件名作标题，文件正文作对象正文，字段写入 frontmatter Properties。对象类型由目录决定，不写 `name`，也不写表示对象类型的属性（Action 的 `type` 表示行动类型）。

| 对象 | 目录 |
| --- | --- |
| Pillar | `Collections/Pillars/` |
| Life Aspiration | `Collections/Life Aspiration/` |
| Goal | `Collections/Goals/` |
| Project | `Collections/Projects/` |
| Routine | `Collections/Routines/` |
| Action | `Collections/Actions/` |
| Topic | `Collections/Topic Vault/` |
| Neurobit | `Collections/Neurobits/` |
| Person | `Collections/People/` |
| Daily | `Collections/Daily/` |

## 字段 → Property

| 字段类型 | Property 类型 |
| --- | --- |
| 枚举、文本 | Text |
| 数字 | Number |
| 日期 | Date（`YYYY-MM-DD`） |
| 布尔 | Checkbox |
| 关系 | List，元素为 `"[[标题]]"`；只有一个值时也写成列表 |

## Daily

文件名为 `YYYY-MM-DD.md`，通常没有 frontmatter。Daily 的每条记录是一个列表项，以作者开头：

```markdown
- **用户**：这周先把作品集做完，读书计划暂停
- **Agent**：已把「整理作品集」的下一步行动安排到明天
- **Agent（推测）**：最近两周的记录都与作品集无关，已放上案头
```

## Desk

案头是 vault 根目录下的 `Desk.md`，不在 `Collections/` 中。字段写入 frontmatter，正文每条以作者开头，格式同 Daily：

```markdown
---
user:
  - "[[示例项目]]"
agent:
  - "[[调研示例工具]]"
---
- **Agent**：「示例项目」最近两周没有进展，是否还要放在案头？
```

## 示例

```yaml
---
do_date: 2026-09-14
due_date: 2026-09-20
human_required: true
project:
  - "[[示例项目]]"
---
```
