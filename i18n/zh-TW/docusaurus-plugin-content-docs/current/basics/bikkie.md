---
sidebar_position: 3
---

# Bikkie

[Bikkie](https://github.com/ill-inc/biomes-game/blob/main/src/shared/bikkie) 是一個統一的系統，用於靜態內容的定義、計算和編輯。

在本機執行時，我們的管理員介面可以在 [`localhost:3000/admin`](http://localhost:3000/admin) 找到，並有許多功能可協助修改 Bikkie。

每個內容項目都是一個「Biscuit」，具有唯一的 ID 和一組 `attributes`（屬性），其中包括 `action`、`sellPrice` 等，甚至還有如 `farming` 等複雜行為。

需要注意的是，Bikkie 僅控制靜態內容，例如物品定義。任何動態內容，例如玩家背包、地形或生物位置，可以基於 Biscuit 定義，但實際上是存在於 `ECS` 中。

## Biscuit 編輯器

![管理員頁面](/img/admin-page.png)

主要的管理員頁面就是通用的 Biscuit 編輯器。在這裡，您可以找到 Biomes 中的任何 Biscuit，並檢查、新增、刪除或修改其上的任何屬性。

![石頭](/img/admin-stone.png)

將 Biscuit 篩選為 `/blocks` 只顯示遊戲中的方塊 Biscuit，搜尋 `stone` 則顯示不同類型的石頭方塊。

在 Biscuit 編輯器中，您可以看到一些屬性，包括 `drop` 屬性，它控制在方塊被破壞時掉落物中建立的物品。

每個屬性都有一個特定領域的編輯器，可以進行編輯。在這裡，我將 `stone` 的 100% 掉落率更改為 `stick` 的 20% 掉落率。

![石頭編輯](/img/admin-stone-edit.png)

修改這些屬性後，您可以儲存更新。在本機所做的更改不會同步到正式環境，而是保留為本機修改。任何修改幾乎會立即在修改的環境中生效。

## 領域特定編輯器

除了通用的 Biscuit 編輯器（對較簡單的 Biscuit（如方塊和物品）已足夠使用）之外，我們還有任務、物品檢查和粒子編輯器，可在導覽列中找到。

## Biscuit 結構定義

Biscuit 結構定義在 [shared/bikkie/schema/biomes.ts](https://github.com/ill-inc/biomes-game/blob/main/src/shared/bikkie/schema/biomes.ts) 中定義，屬性則在 [shared/bikkie/schema/attributes.ts](https://github.com/ill-inc/biomes-game/blob/main/src/shared/bikkie/schema/attributes.ts) 中定義。

管理員 UI 在 [client/components/admin/bikkie/attributes/AttributeValueEditor.tsx](https://github.com/ill-inc/biomes-game/blob/main/src/client/components/admin/bikkie/attributes/AttributeValueEditor.tsx) 中定義。

Biscuit 最常見的用法是查詢屬性值。這可以通過多種方式實現，但一種常見的方法是將 Biscuit 用作物品：

```ts
const val = anItem(BISCUIT_ID).attribute;
```