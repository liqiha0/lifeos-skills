---
name: life-os-obsidian
description: 在 Obsidian 中读写 Life OS 对象时，使用集合目录、Properties、链接和文件名映射。
---

# Life OS 存储映射

对象含义见 `life-os-core`；字段键、枚举与文件名规则见[存储映射](references/data-contract.md)。

## 集合目录

以下为 vault 内默认相对路径。已有不同映射时沿用，不自动搬迁。

```text
Collections/{Actions,Goals,Projects,Routines,Pillars,Aspirations,Topic Vault,Neurobits,People}/
Collections/Cycles/{Daily,Weekly,DuoCycle,Annual}/
```

对象种类由集合目录区分；Life Aspiration 对应 `Aspirations/`，Topic 对应 `Topic Vault/`。Goal、Project、Routine 不另存 `type`，Action 的 `type` 表示行动类型。标题使用文件名，说明写正文，结构化字段使用 Properties。完成对象留在原集合。

## 读写约束

仅通过 Obsidian CLI 读写，以本机 `obsidian help <command>` 为准；CLI 不可用时停止，不退回直接文件操作或 GUI。修改前读取目标与必要的关联对象，修改后读回并检查关系目标解析。批量迁移须单独确认范围与备份。
