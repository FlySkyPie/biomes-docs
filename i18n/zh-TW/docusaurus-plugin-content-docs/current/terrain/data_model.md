---
sidebar_position: 1
---

# 資料模型

### 詞彙表

- `Shard`（分片）：一個 32x32x32 的[張量](https://en.wikipedia.org/wiki/Tensor)。
- `BlockId`（方塊 ID）：對應到特定方塊類型的整數值，例如 `1: 草地`, `2: 泥土`。所有方塊類型在 [terrain.json](https://github.com/ill-inc/biomes-game/blob/main/src/shared/asset_defs/terrain.ts) 中定義。
- `Seed`（種子）：地形的初始狀態。
- `Voxel`（體素）：單一方塊。
- `Subvoxel`（次體素）：大小為完整方塊 1/8 的單一方塊。每個完整方塊包含 `8x8x8` 個次體素。

## 分片 (Shards)

整個體素 3D 世界被分割成分片。
分片資料儲存為緩衝區，需要解壓縮才能讀取。

每個分片包含以下資料。

> _注意：除 `Box` 外，所有位置都是相對於分片最低點座標定義的。_
>
> _例如：`[0, 0, 0]` 對應最低點座標，`[31, 31, 31]` 對應最高點座標，而 `[10, 16, 19]` 位於最低點和最高點座標之間。_

### Box

`v0`：分片的最低點座標。

`v1`：分片的最高點座標。

### ShardSeed

Biomes 地形具有一些我們稱之為 `seed`（種子）的初始狀態。世界中的每個體素都有一些初始方塊類型，
儲存在 `ShardSeed` 中。`ShardSeed(x, y, z)` 儲存該位置初始方塊類型的 `BlockId`。

種子分片由 [galois](https://github.com/ill-inc/biomes-game/tree/main/src/galois/py/notebooks) 中定義的腳本生成。

### ShardDiff

當體素的方塊類型被修改並與種子分片不同時，我們將此差異儲存在 diff 分片中。由於大多數方塊不會被更新，這些是稀疏張量——它們最多儲存 `32x32x32` 個條目。換句話說，我們**只**儲存更新。

與 `ShardSeed` 一樣，這些分片定義了位置到 `BlockId` 的對應關係。

### ShardShape

地形主要由完整的方塊（完美的立方體）構成。然而，所有體素都可以使用塑形工具轉換為不同的形狀，
例如樓梯、柵欄、窗戶、桌子等。形狀分片儲存有關每個體素當前形狀的資訊。

形狀資料編碼為一個整數，包含兩個部分：

1. `ShapeId`：例如樓梯。形狀在 [shapes.json](https://github.com/ill-inc/biomes-game/blob/main/src/shared/asset_defs/gen/shapes.json) 中定義。
2. `IsomorphismId`：例如垂直翻轉且朝北的樓梯。

### ShardPlacer

每個使用者都有一個 `BiomesId` 作為其唯一識別碼。放置者分片將體素與最後修改它的使用者的 `BiomesId` 對應起來。

### ShardOccupancy

可放置物品（例如音響、電視或花朵）不是體素，但它們仍然佔據空間。佔用張量記錄了非體素物品所佔據的空間，以便其他物品不會放置在它們佔據的空間中。每個位置對應到佔據該位置的實體的 `BiomesId`（如果有的話）。