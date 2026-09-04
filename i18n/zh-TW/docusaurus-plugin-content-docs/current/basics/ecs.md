---
sidebar_position: 4
---

# 實體組件系統 (ECS)

[ECS](https://github.com/ill-inc/biomes-game/tree/main/src/shared/ecs)，即 [Entity Component System](https://en.wikipedia.org/wiki/Entity_component_system)（實體組件系統），是 Biomes 用來儲存遊戲中**動態**資料的系統。（[Bikkie](./bikkie.md) 則是處理靜態資料的系統）。

## ECS 結構定義

ECS 結構定義使用 Python 定義，位於 [`src/ecs/defs.py`](https://github.com/ill-inc/biomes-game/tree/main/ecs/defs.py)。

這些定義會透過程式碼生成轉換為 TypeScript 定義，存放在 `src/shared/ecs/gen` 中。

一個單一的實體（Entity），例如玩家或 NPC，由許多可重複使用的組件（Component）組成，例如背包（Inventory）或位置（Position）。不同類型的實體會共享不同的組件集合。

除了資料定義之外，我們還定義：

- ECS 事件，供玩家（和特權服務）作為事件發送到[邏輯伺服器](./server-overview)
- 選擇器（Selectors），用於一次選取多個組件

## 更新結構定義

在更新結構定義後，執行 `./b gen:ecs` 以更新 ECS 定義。