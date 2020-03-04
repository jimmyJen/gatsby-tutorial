---
title: 快速開始
---

快速開始適用於中高級開發人員。有關 Gatsby 的入門介紹，[請查看我們的教程](/tutorial/)！

## 使用 Gatsby CLI

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-quick-start-with-gatsby-create-develop-and-build-gatsby-sites-from-the-command-line"
  lessonTitle="Quick Start with Gatsby: Create, Develop, and Build Gatsby Sites From the Command Line"
/>

**注**: 視頻中使用的是 `npx`，它是一個工具，允許你執行一個 npm 包命令而不需要事先安裝這個包。 在你的電腦安裝 gatsby-cli 之後，運行命令 `npx gatsby new` 和運行命令 `gatsby new` 的效果相同。

### 安裝 Gatsby CLI.

```shell
npm install -g gatsby-cli
```

### 創建站點

```shell
gatsby new gatsby-site
```

### 切換到站點目錄

```shell
cd gatsby-site
```

### 啟動開發服務器

```shell
gatsby develop
```

Gatsby 將會啟動一個熱更新的開發環境，你可以通過 `localhost:8000` 訪問。

嘗試修改位於 `src/pages` 下的頁面中的 JavaScript。 保存變更，瀏覽器中的內容會自動熱更新。

### 構建生產版本

```shell
gatsby build
```

Gatsby 將會為你站點的生產版本執行一些優化工作，生產靜態 HTML 和預加載的 JavaScript 代碼包。

### 在本地啟動生產版本服務器

```shell
gatsby serve
```

Gatsby 會啟動一個本地 HTML 服務器，用於測試你構建的站點。在執行這個命令前，記得先使用 `gatsby build` 構建你的站點。

### 查看 CLI 指令文檔

在終端中運行 `gatsby --help` 指令，可查看 CLI 指令的詳細文檔。

對於特殊的指令，運行 `gatsby COMMAND_NAME --help` ，例如 `gatsby new --help`。

想要了解更多關於 Gatsby CLI 的內容，訪問文檔中的 [CLI 參考](/docs/gatsby-cli/) 章節。
