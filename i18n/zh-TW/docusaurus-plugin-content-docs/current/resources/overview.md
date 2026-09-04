---
sidebar_position: 1
---

# 概述

## 問題

Biomes 是一個使用 [Three.js](https://threejs.org/) 渲染器的 [Next.js](https://nextjs.org/) 應用程式。
React 使用 React 狀態來管理資源，而 Three.js 沒有現成的狀態管理解決方案；
通常重新整理 Three.js 應用程式會重設場景。

這裡有幾個問題：

1. 如何持久化 Three.js 遊戲狀態。
2. 當 Three.js 遊戲狀態變更時，如何更新 React 狀態以觸發重新渲染。
3. 當 React 狀態變更時，如何更新 Three.js 遊戲狀態。

此外，還有定義資源之間依賴關係的問題。例如，如果玩家的生命值低於某個值，我們可能想要更改玩家的外觀。我們要如何描述玩家外觀和生命值之間的這種依賴關係？

## 資源系統

資源系統的建立是為了解決這些問題，它由幾個組件組成，主要如下：

1. `BiomesResourcesBuilder`：用於定義資源。
2. `TypedResourceDeps`：用於定義資源之間的依賴關係，使用[依賴注入](https://en.wikipedia.org/wiki/Dependency_injection)。
3. `TypedResources`：用於存取資源。
4. `ReactResources`：用於從 React 元件中存取資源。
5. `ResourcePaths`：帶有路徑的型別化資源金鑰，用於定義查詢參數。