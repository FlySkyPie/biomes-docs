---
sidebar_position: 1
---

# 本機設定

## 環境設定

要在本機執行 Biomes，您需要擁有 64GB 的記憶體。

請注意，此儲存庫支援開發容器，因此快速設定環境的方式是跳過此章節並[啟動 codespace](#github-codespaces)。繼續閱讀以取得手動指示。

- 安裝 Node 版本管理工具 (https://github.com/nvm-sh/nvm)。

  ```bash
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash

  # 重新啟動終端機

  nvm install v20
  nvm use v20
  ```

- 安裝 [yarn](https://yarnpkg.com/)
  ```bash
  npm install -g yarn
  ```
- 安裝 [Git LFS](https://git-lfs.github.com/) 然後再複製儲存庫，否則二進位檔案將會有錯誤的內容。
  - 在 Ubuntu 上，
    ```bash
    sudo apt-get install git-lfs
    ```
  - 在 MacOS 上，
    ```bash
    brew install git-lfs
    ```
- 安裝 [Python 版本 >=3.9,<=3.10](https://www.python.org/)
- 安裝 [clang 版本 >= 14](https://clang.llvm.org/)
- 安裝 [Bazel](https://bazel.build/install)
  ```bash
  npm install -g @bazel/bazelisk
  ```
- 複製儲存庫
  ```bash
  git clone https://github.com/ill-inc/biomes-game.git
  ```
- 執行 `git lfs pull` 以確保 LFS 檔案是最新的。
- 設定 Python 虛擬環境（選擇性，但建議使用）
  ```bash
  python -m venv .venv
  source .venv/bin/activate
  ```
- 執行 `pip install -r requirements.txt` 以安裝 Python 依賴項。
- 安裝 [Redis 7.0.8](https://redis.io/)
  ```bash
  curl -s https://download.redis.io/releases/redis-7.0.8.tar.gz | tar xvz -C ${HOME} \
    && make -j`nproc` -C ${HOME}/redis-7.0.8 \
    && sudo make install -C ${HOME}/redis-7.0.8 \
    && rm -rf ${HOME}/redis-7.0.8
  ```

## 執行 Biomes

- 在 Biomes 儲存庫目錄中，
  ```bash
  ./b data-snapshot run
  ```
- 造訪 `http://localhost:3000`。

## 程式開發環境

- 建議的程式編輯器是 [VSCode](https://code.visualstudio.com/)。

## 在容器中進行開發

如果您想直接使用一個預先準備好的開發環境（讓您可以跳過以上所有「環境設定」步驟），您可以利用 VS Code 的「在容器中開發」功能。請參閱 [.devcontainer/README.md](https://github.com/ill-inc/biomes-game/blob/main/.devcontainer/README.md) 以了解設定說明。

### GitHub Codespaces

基於「在容器中開發」的支援，您也可以在此儲存庫中啟動 [GitHub Codespace](https://github.com/features/codespaces)，[點擊此處](https://github.com/codespaces/new?hide_repo_select=true&ref=main&repo=677467268&skip_quickstart=true)。請確保選擇「16-core」或更高級別的「機器類型」（這應該配備所需的 64GB 記憶體）。如果您建立了 codespace，應始終在 VS Code 中開啟，而非在瀏覽器中，這樣您才能存取系統所預期的 `localhost:3000` 開發伺服器。

## 常見問題與解決方案

### 啟動時 Discord 錯誤

透過在 [biomes.config.dev.yaml](https://github.com/ill-inc/biomes-game/blob/main/biomes.config.dev.yaml) 中加入以下內容來停用 Discord Webhook：

```
discordHooksEnabled: false
```

### 使用社群登入（Twitch/Discord/Google）時發生錯誤

如果您沒有所需的 API 金鑰，社群登入將無法運作。因此，它們在本地建置中無法使用，不應使用。

### 無效的資源路徑

「找不到（404）」錯誤，格式為「找不到 `<asset-path>/<name>-<hash>.<extension>`」，通常是由於來自先前的 `./b data-snapshot run` 的資源過期所致。

若要解決此問題，請執行：

```bash
./b data-snapshot uninstall
./b data-snapshot pull
```

以取得最新的資源。

### 登入或建立帳戶時失敗

請勿使用社群登入。請改用下方顯示的開發者工作流程建立新帳戶。

- 從 `http://localhost:3000`，點擊「login」。
  <img width="800px" alt="登入" src="/img/create-account-1.png" />
  <br/>
- 在登入對話框中，選擇「Login with Dev」。
  <br/>
  <img width="400px" alt="開發者登入" src="/img/create-account-2.png" />
  <br/>
- 在開發者登入頁面中，「Create [a] New Account」。
  <br/>
  <img width="400px" alt="建立帳戶" src="/img/create-account-3.png" />
  <br/>
- 這將顯示初始場景。
  <br/>
  <img width="600px" alt="進入遊戲" src="/img/create-account-4.png" />