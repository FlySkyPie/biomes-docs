---
sidebar_position: 6
---

# 託管 Biomes

### 概述

_注意：託管 Biomes 是一個複雜的過程，需要大量的計算和硬體資源。_

Biomes 以一系列服務的形式託管在 [Kubernetes](https://kubernetes.io/) 叢集中，執行於 [Google Cloud Platform (GCP)](https://cloud.google.com/) 上。叢集部署的具體細節可以在 [deploy/k8](https://github.com/ill-inc/biomes-game/blob/main/deploy/k8) 中找到，包括每個服務的副本數量和記憶體需求。

我們目前沒有關於自行託管 Biomes 的全面文件，但如果您有 GCP 和 Kubernetes 的經驗，設定方式應該會很熟悉。

### 效能分析

我們使用 [Prometheus](https://prometheus.io/) 收集指標，並使用 [Grafana](https://grafana.com/) 進行視覺化。兩者的 Kubernetes 配置可以在 [deploy/k8/monitoring](https://github.com/ill-inc/biomes-game/blob/main/deploy/k8/monitoring) 中找到。