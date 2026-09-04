---
sidebar_position: 2
---

# 管理員頁面

當在本機執行伺服器時，我們的管理員頁面位於 [`localhost:3000/admin`](http://localhost:3000/admin)。

本機的修改不會同步至正式環境。

我們的管理頁面混合了靜態與動態內容檢查器和編輯器。

- 靜態內容由 [Bikkie](./bikkie.md) 驅動，是管理員頁面的預設登陸頁面。
- 動態內容由 [ECS](./ecs.md) 驅動

## 頁面

### 使用者頁面

您可以使用管理員頁面左上角的「Search Username or ID」搜尋框來搜尋使用者、檢查和修改其狀態。
![管理員使用者](/img/admin-user.png)

### ECS 實體

您可以使用管理員頁面左上角的「Search ECS Entity」搜尋框來按 ID 搜尋 ECS 實體並檢查其內容。
![管理員 ECS](/img/admin-ecs.png)

此外，在遊戲中以管理員帳號身分遊玩時，您可以叫出遊戲內 ECS 編輯器以獲得簡化版的編輯器，並使用波浪號（`）鍵檢查 ID。
![遊戲內 ECS](/img/admin-ingame-ecs.png)

### Bikkie

我們通用的 [Bikkie](./bikkie.md) 編輯器，也是管理員頁面的預設登陸頁面
![Bikkie](/img/admin-page.png)

### 任務

顯示任務進度的節點圖。這是一個專用的 Bikkie 編輯器。
![任務](/img/admin-quests.png)

### 機器人

顯示已宣稱的土地區域。
![機器人](/img/admin-robots.png)

### 大型任務

關於大型任務的未完成頁面，這些是多玩家合作任務。目前這些任務只能重設分數。

### 玩家

列出所有在遊戲中註冊的玩家

### 玩家預設

預設是一項內部測試功能，用於儲存和載入特定的玩家狀態。
![預設](/img/admin-presets.png)

### 邀請

Biomes 曾經是邀請制，因此這個功能負責建立和追蹤邀請碼的使用情況。

### 原始碼映射

當我們在正式環境中看到當機或錯誤時，客戶端程式碼已經被混淆。我們使用此工具從混淆的呼叫堆疊映射回實際的原始碼。
![原始碼映射](/img/admin-source-map.png)

### Bikkie 日誌

記錄 Bikkie 的更新

### 物品檢查

一系列腳本，用於識別可能出現問題的 Biscuit
![物品檢查](/img/admin-item-check.png)

### 圖片選擇器

選擇玩家製作的圖片以顯示在首頁
![圖片選擇器](/img/admin-image-selector.png)

### 粒子編輯器

建立和修改遊戲中使用的粒子
![粒子編輯器](/img/admin-particle-editor.png)

## 遊戲內編輯器和調整

管理員玩家可以存取多個遊戲內視窗來檢查和管理遊戲。

主要編輯器可以透過按下波浪號（`）來存取，包含調整項目以及簡化的 Bikkie 和 ECS 編輯器。
![遊戲內 ECS](/img/admin-ingame-ecs.png)

## 美術編輯器

我們在 [localhost:3000/art](http://localhost:3000/art) 有一個單獨的美術頁面，讓我們能夠為新方塊、玻璃和植物使用新的紋理。
![美術編輯器](/img/admin-art.png)