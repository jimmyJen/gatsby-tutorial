---
title: 安裝開發環境
typora-copy-images-to: ./
disableTableOfContents: true
---

在開始創建第一個 Gatsby 網站之前，你需要熟悉一些核心 Web 技術，並確保已安裝完所有必需的依賴、工具及環境。

## 熟悉命令行

命令行是基於文本的人機交互界面，用於在計算機上運行命令，也經常被稱為終端。在本教程中，我們混用這兩種叫法。像在 Mac 系統上使用 Finder 或在 Windows 系統上使用資源管理器一樣。Finder 和資源管理器是圖形用戶界面（GUI）。而命令行是一種強大的基於文本的與計算機交互的方式。

找到並打開計算機的命令行界面（CLI）。 根據你使用的操作系統，請參閱 **[Mac 指令](http://www.macworld.co.uk/feature/mac-software/how-use-terminal-on-mac-3608274/)**，**[Windows 指令](https://www.quora.com/How-do-I-open-terminal-in-windows) **或 **[Linux 指令](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/)**。

## 為 Node.js 安裝 Homebrew

要安裝 Gatsby 和 Node.js，建議使用 [Homebrew](https://brew.sh/)。 開始做一些設置可以免去以後很多麻煩！

如何在計算機上安裝或驗證 Homebrew 是否安裝成功：

1. 打開命令行終端。
2. 命令行輸入並運行 `brew -v` 來查看是否安裝了 Homebrew 及其版本號。
3. 如果沒有，根據操作系統（Mac，Linux 或 Windows）參照 [Homebrew及其說明](https://docs.brew.sh/Installation) 下載並安裝。
4. 安裝完 Homebrew 後，重複步驟2進行驗證。

### Mac 用戶：需安裝 Xcode 命令行工具

1. 打開命令行終端。
2. 在 Mac 上，通過運行 `xcode-select --install` 安裝 Xcode 命令行工具。
    1. 如果安裝失敗，請使用 Apple 開發者帳戶登錄 [直接從Apple網站下載](https://developer.apple.com/download/more/)。
3. 在提示你開始安裝後，再次按提示接受軟件許下載工具。

## ⌚ 安裝 Node.js 和 npm 包管理工具

Node.js 是可以在 Web 瀏覽器之外運行 JavaScript 代碼的環境。Gatsby 是使用 Node.js 構建的。 要啟動和運行 Gatsby，你需要在計算機上安裝最新版本 Node.js。

_注意：Gatsby 支持的最低 Node.js 版本是 Node v8.0.0，可以隨時使用最新版本。_

1. 打開命令行終端。
2. 運行 `brew update` 以確保 Homebrew 更新到最新版。
3. 運行 `brew install node` 一次性安裝 Node 和 npm。

完成安裝步驟後，請確保所有內容均已正確安裝：

### 驗證 Node.js 是否安裝成功

1. 打開命令行終端。
2. 運行 `node --version`。 （如果你不熟悉命令行，“運行 `command`”的意思是“在命令提示符下鍵入 `node --version`，然後按下 Enter 鍵。”現在開始，這就是我們所說的“運行 `command` ”）。
2. 運行 `npm --version`。

每個命令的輸出結果應為版本號。 你的版本號可能與下面顯示的版本不同！ 如果輸入的命令沒有顯示版本號，請返回並確保 Node.js 是否已安裝成功。

![命令行檢查 node 和 npm 的版本](01-node-npm-versions.png)

## 安裝Git

Git 是一個免費的開源分佈式版本管理系統，旨在快速高效地管理從小型到大型項目的所有內容。 當你創建一個 Gatsby “starter”（模版）站點時，Gatsby 會在後臺使用 Git 來下載並安裝啟動程序所需的文件。 你將需要安裝 Git 才能設置你的第一個 Gatsby 網站。

根據操作系統去相應下載和安裝 Git，參照指南：

- [macOS 下安裝 Git](https://www.atlassian.com/git/tutorials/install-git#mac-os-x)
- [Windows 下安裝 Git](https://www.atlassian.com/git/tutorials/install-git#windows)
- [Linux 下安裝 Git](https://www.atlassian.com/git/tutorials/install-git#linux)

## 使用Gatsby CLI

通過 Gatsby CLI 命令行工具，你可以快速創建由 Gatsby 支持的新站點，還可以運行用於開發 Gatsby 站點的其他命令。 它是一個已發佈的 npm 包。

Gatsby CLI 可通過 npm 安裝，命令行運行 `npm install -g gatsby-cli` 進行全局安裝。

_**注意**: 當你首次安裝並運行 Gatsby 時，會看到一條通知你有關為 Gatsby 命令收集的匿名使用數據的短消息，你可以閱讀更多關於如何在 [遠程測試文檔](/docs/telemetry) 中提取和使用該數據的信息。_

要查看可用的命令，請運行 `gatsby --help`。

![命令行查看 gatsby 命令](05-gatsby-help.png)

> 💡 如果由於權限問題而無法成功運行 Gatsby CLI，則可能需要查看 [有關修復npm權限的文檔](https://docs.npmjs.com/getting-started/fixing-npm-permissions) 或 [本指南](https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md)。

## 創建一個 Gatsby 網站

現在，你可以使用 Gatsby CLI 工具創建第一個 Gatsby 站點。 使用該工具，你可以下載 “starter”（具有某些默認配置的部分構建的網站模版），以幫助你更快地創建特定類型的網站。 在此使用的 “Hello World” 的 “starter” 是具有 Gatsby 網站所需的所有基本知識的模版。

1. 打開命令行終端。
2. 運行`gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world`。 (_注意：根據你的下載速度，此過程所花費的時間會有所不同。為簡便起見，下面的gif動畫在部分安裝過程中已暫停_)。
3. 運行`cd hello-world`。
4. 運行`gatsby develop`。

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./03-create-site.mp4" />
  <p>Sorry! You browser doesn't support this video.</p>
</video>

剛剛發生了什麼？

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

- `new` 是 gatsby 命令，用於創建新的 Gatsby 項目。
- 在這裡，`hello-world` 是一個任意標題-你可以輸入任何內容。 CLI 工具會將新站點的代碼放置在名為 “hello-world” 的新文件夾中。
- 最後，GitHub URL 指向一個包含你要使用的模版代碼的代碼存儲庫。

```shell
cd hello-world
```

- 意思是將當前目錄（cd）定位到 “hello-world” 子文件夾。 每當你要為站點運行任何命令時，都需要定位到該站點的目錄中（即命令行終端需要指向站點代碼所在的目錄）。

```shell
gatsby develop
```

- 此命令啟動了一個本地的開發服務器。 你將能夠在本地開發環境中（在你的本地計算機上，而不是發佈到網絡上）查看新站點並與之交互。

### 本地查看你的網站

在瀏覽器中打開一個新標籤，然後打開網址 [**http://localhost:8000**](http://localhost:8000/)。

![查看首頁](04-home-page.png)

恭喜！ 這是你第一個 Gatsby 網站的開始！ 🎉

只要你的開發服務器正在運行，你就可以通過鏈接 [**_http://localhost:8000_**](http://localhost:8000/) 在本地訪問該網站。這就是你通過運行 `gatsby develop` 命令開啟的進程。 要停止運行該進程（或 “停止運行開發服務器” ），請返回到命令行終端窗口，按住 “Control” 鍵，然後單擊 “c” 鍵（ctrl+c）。 要重新啟動，請再次運行 `gatsby develop`！

**注意：** 如果你正在使用 VM 虛擬機（如 “vagrant”）並希望能通過你的本地 IP 地址進行訪問，請運行 `gatsby develop -- --host=0.0.0.0`。 現在，開發服務器已經運行在 “localhost” 和你的本地 IP 上。

## 設置代碼編輯器

代碼編輯器是專門設計用於編寫計算機代碼的程序。 有很多很棒的可供選擇。

### 下載 VS Code

Gatsby 文檔有時包含的屏幕截圖是來自於 VS Code，因此，如果你還沒有首選的代碼編輯器，則建議使用 VS Code，確保你的屏幕看起來像教程和文檔中的屏幕截圖一樣。 如果選擇使用 VS Code，請訪問 [VS Code網站](https://code.visualstudio.com/#alt-downloads) 並下載適合你平臺的版本。

### 安裝 Prettier 插件

建議使用 [Prettier](https://github.com/prettier/prettier)，該工具可幫助你格式化代碼以避免錯誤。

你可以在編輯器中直接使用 Prettier，安裝 [Prettier VS Code plugin](https://github.com/prettier/prettier-vscode)：

1. 在 VS Code 上打開擴展視圖（查看=>擴展）。
2. 搜索 “Prettier - Code formatter”。
3. 單擊 “安裝”。 （安裝後，系統將提示你重新啟動 VS Code 以啟用擴展。較新版本的 VS Code 將在下載後自動啟用該擴展。）

> 💡 如果你不是使用 VS Code，請查看 Prettier 文檔獲取 [安裝指引](https://prettier.io/docs/en/install.html) 或查看 [其他編輯器集成](https://prettier.io/docs/en/editors.html)。

## ➡️ 下一步

總結，在本節中介紹了：

- 學習命令行及其使用方法
- 安裝並瞭解 Node.js、npm CLI 工具、項目版本管理工具 Git 和 Gatsby CLI 工具
- 使用 Gatsby CLI 工具創建了一個新的 Gatsby 網站
- 運行 Gatsby 本地開發服務器並在本地訪問了你的站點
- 下載了代碼編輯器
- 安裝了 Prettier 代碼格式化程序

現在，繼續閱讀 [**瞭解Gatsby模塊構建**](/tutorial/part-one/)。

## 參考引用

### 核心技術概述

不要求精通這些方面的知識-如果你不是專家，請不要擔心！ 在本教程系列的課程中，你將學到很多東西。 以下是構建 Gatsby 網站時將使用的一些主要 Web 技術：

- **HTML**：每個 Web 瀏覽器都能理解的標記語言。 它代表超文本標記語言。 HTML 為你的 Web 內容提供了通用的信息結構，定義了標題，段落等內容。
- **CSS**：一種樣式表示語言，用於設置 Web 內容（字體，顏色，佈局等）的樣式。 它代表層疊樣式表。
- **JavaScript**：一種編程語言，可幫助我們創建動態和交互式的網頁。
- **React**：用於構建用戶界面的代碼框架（使用 JavaScript 構建）。這是 Gatsby 用來建立頁面和內容結構的框架。
- **GraphQL**：一種數據查詢語言，可助你將數據提取到你的網站中。 這是 Gatsby 用於管理網站數據的接口。

### 什麼是網站？

要全面瞭解網站是什麼（包括 HTML 和 CSS 簡介），請查看“ [**建立你的第一個網頁**](https://learn.shayhowe.com/html-css/building-your-first-web-page/)”。 這是開始學習 Web 知識的好地方。 有關 [**HTML**](https://www.codecademy.com/learn/learn-html)，[**CSS**](https://www.codecademy.com/learn/learn-css) 和 [**JavaScript**](https://www.codecademy.com/learn/introduction-to-javascript)，請參閱 Codecademy 的教程。[**React**](https://reactjs.org/tutorial/tutorial.html) 和 [**GraphQL**](http://graphql.org/graphql-js/) 也有自己的入門教程 。

### 瞭解有關命令行的更多信息

有關使用命令行的較好的介紹，請查看適用於 Mac 和 Linux 用戶的 [**Codecademy的命令行教程**](https://www.codecademy.com/courses/learn-the-command-line/lessons/navigation/exercises/your-first-command)，以及 [**此教程**](https://www.computerhope.com/issues/chusedos.htm)（適用於Windows用戶）。 即使你是 Windows 用戶，Codecademy 教程的第一頁也是非常有價值的內容。 它不僅介紹如何與之交互，還解釋了什麼是命令行。

### 瞭解有關npm的更多信息

npm 是一個 JavaScript 包管理器。 一個包是一個你可以選擇將其包含在項目中的代碼模塊。 如果你已經下載並安裝了 Node.js，則 npm 已經受附帶安裝了！

npm 有三個不同的組成部分：npm 的網站，npm 註冊表和 npm 命令行界面（CLI）。

- 在 npm 網站上，你可以瀏覽在 npm 註冊表中可用的 JavaScript 代碼包。
- npm 註冊表是一個大型數據庫，其中包含有關 npm 上可用的 JavaScript 代碼包的信息。
- 確定所需的代碼包後，可以使用 npm CLI 將其安裝到項目中或全局安裝（如其他 CLI 工具一樣）。npm CLI 是與 npm 註冊表對話的對象——通常，你僅需與 npm 網站或 npm CLI 進行交互。

> 💡 查看 npm 的介紹，“[**什麼是npm？**](https://docs.npmjs.com/getting-started/what-is-npm)”。

### 瞭解有關 Git 的更多信息

你無需瞭解 Git 即可完成本教程，但這是一個非常有用的工具。 如果你想了解有關版本控制、Git 和 GitHub 的更多信息，請查看 GitHub 的 [Git手冊](https://guides.github.com/introduction/git-handbook/)。

