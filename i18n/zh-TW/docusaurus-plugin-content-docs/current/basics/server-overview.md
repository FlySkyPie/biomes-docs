---
sidebar_position: 5
---

# 伺服器架構

Biomes 伺服器功能分散在多個微服務中，以便根據需求有效擴展。

![伺服器架構](/img/biomes-server-architecture.png)

- 當玩家載入遊戲時，他們會從 `web` 伺服器載入客戶端。
- 接著客戶端從 `asset` 伺服器載入資源，並與 `sync` 伺服器建立連線，以取得玩家位置附近的 ECS 資料。
- 玩家的互動主要將 ECS 事件發送到 `logic` 伺服器，但也可以直接呼叫 `web`、`chat`、`oob` 和 `map`。
- 互動主要透過 ECS 更新傳遞給玩家，這些更新透過 `sync` 伺服器同步到客戶端。
- 其他伺服器並非直接由玩家驅動，但其對 ECS 組件的更改會透過 `sync` 伺服器以類似方式同步。例如 `newton` 獨立於任何玩家互動來移動掉落的物品。`trigger`、`task`、`newton`、`anima` 和 `gaia` 都屬於這種模式。

在本機執行時，您可以透過指定伺服器名稱來指定您有興趣執行的伺服器子集，例如 `./b web trigger`。伺服器會自動啟動其依賴的其他伺服器以確保正常執行。

# [伺服器](https://github.com/ill-inc/biomes-game/tree/main/src/server)

## [Web](https://github.com/ill-inc/biomes-game/tree/main/src/server/web)

- 基於 NextJS 的 Web 伺服器
- 提供所有 API 端點、主要首頁和管理員網站
- 無狀態

## [Logic](https://github.com/ill-inc/biomes-game/tree/main/src/server/logic)

- 處理玩家的高階事件，通常是編輯地形的事件

大多數玩家事件會透過 [ECS](ecs.md) 建立邏輯伺服器事件。

邏輯伺服器事件由 [`server/logic/events/all.ts`](https://github.com/ill-inc/biomes-game/blob/main/src/server/logic/events/all.ts) 中的 ECS 事件處理器定義。

如果您打算修改或新增面向玩家的遊戲互動或邏輯，這裡很可能是您應該開始的地方。

## [Asset](https://github.com/ill-inc/biomes-game/tree/main/src/server/asset)

- 只是 Web 伺服器的另一個副本
- 不同層級的伺服器，因為它們由於執行 Python 而具有不同的特性
- 生成玩家的網格

## [Trigger](https://github.com/ill-inc/biomes-game/tree/main/src/server/trigger)

- 監聽 Firehose，並具有基於時間的處理器 - 兩者都是觸發器的輸入
- 觸發器產生遊戲更新，它們：
  - 解鎖配方
  - 處理任務進度
  - 處理到期/冷卻/超時

## [Chat](https://github.com/ill-inc/biomes-game/tree/main/src/server/chat)

- 使用分散式鎖定來維持單一實例
- 將聊天訊息分發到同步伺服器
- 處理聊天的發布-訂閱資訊流以確保分發和儲存
- 發布有關私訊的 Firehose 事件

## [Task](https://github.com/ill-inc/biomes-game/tree/main/src/server/task)

- 處理長期運行的非同步任務
- 與 Firestore 互動，產生遊戲事件，與加密貨幣互動
- API 是間接的，您可以透過在 Firestore 中建立任務來排程任務

## [Sync](https://github.com/ill-inc/biomes-game/tree/main/src/server/sync)

- 客戶端的 WebSocket 終止端點
- 維護整個世界的副本作為複本，並將相關部分提供給連接到它的客戶端
- 代表客戶端發布遊戲事件

## [OOB](https://github.com/ill-inc/biomes-game/tree/main/src/server/oob)

- 同步伺服器的副本，用於直接帶外提供單個實體
- 用於向客戶端載入遠端資料

## [Newton](https://github.com/ill-inc/biomes-game/tree/main/src/server/newton)

- 處理掉落物、其物理行為以及撿起時機

## [Anima](https://github.com/ill-inc/biomes-game/tree/main/src/server/anima)

- 處理世界中 NPC 的 AI，進行分片以便每個伺服器只處理一部分

## [Map](https://github.com/ill-inc/biomes-game/tree/main/src/server/map)

- 定期生成地圖的世界俯視圖

## [Replica](https://github.com/ill-inc/biomes-game/tree/main/src/server/replica)

- 為了消除扇出成本對遊戲的直接影響，任何需要世界副本的人都應訂閱複本層
- 維護世界的副本，直接訂閱世界
- 支援當前遊戲 API 的訂閱部分

## [Gaia](https://github.com/ill-inc/biomes-game/tree/main/src/server/gaia_v2)

Gaia 權威性地控制遊戲中所有「自然」遊戲模擬：

- 光照
- 黏液擴散
- 植物生長與再生
- 農耕

## [Redis / Redis Bridge](https://github.com/ill-inc/biomes-game/tree/main/src/redis)

- 世界資料的主要儲存，並能在此基礎上提供交易功能。
- Bridge 組件將 Redis 中發生的更新映射到 Firehose，一次只有一個 bridge 在執行。

## ETCD

- 分散式鎖定使用執行的 etcd 伺服器來維護