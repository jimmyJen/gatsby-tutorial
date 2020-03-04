---
title: 命令（Gatsby CLI）
tableOfContentsDepth: 2
---

Gatsby 命令行工具（CLI） 是啟動和運行 Gatsby 應用程序以及使用諸如運行開發服務器和構建 Gatsby 應用程序進行部署等功能的主要入口點。

_我們為 gatsby-cli 準備了一份 [README](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-cli/README.md) 文檔， 這份 [備忘錄](/docs/cheat-sheet/) 中包含了一些常見的命令以方便你打印_

## 如何使用 gatsby-cli

Gatsby CLI (`gatsby-cli`) 是一個全局可執行的 npm 包， Gatsby CLI 發佈在 [npm](https://www.npmjs.com/) 上，你應該運行 `npm install -g gatsby-cli` 全局安裝它以便在本地使用。

運行 `gatsby --help` 查看幫助。

你也可以使用 `package.json` script 中自行定義這些命令，通常大多數[starters](/docs/starters/)都會_為你_暴露一些命令。例如，如果你想在你的應用中使用 [`gatsby develop`](#develop) 命令，打開`package.json` 像這樣添加一份 script:

```json:title=package.json
{
  "scripts": {
    "develop": "gatsby develop"
  }
}
```

## API 命令

### `new`

```
gatsby new [<site-name> [<starter-url>]]
```

#### 參數

| 參數    | 說明                                                                                                                                                                                                     |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| site-name   | 你的 Gatsby 站點的名稱，會創建同名目錄。                                                                                                                                    |
| starter-url | Gatsby starter URL 或者本地的文件路徑。 默認使用 [gatsby-starter-default](https://github.com/gatsbyjs/gatsby-starter-default)，參見：[Gatsby starters](/docs/gatsby-starters/) |

> 注: `site-name` 應該只包含字母和數字。 如果名稱中使用了 `.`, `./` 或者 `<空格>`， `gatsby new` 會拋出一個錯誤。

#### 示例

- 使用默認的 Starter 創建一個名為 `my-awesome-site` 的 Gatsby 站點。

```shell
gatsby new my-awesome-site
```

- 使用 Starter:[gatsby-starter-blog](https://www.gatsbyjs.org/starters/gatsbyjs/gatsby-starter-blog/) 創建一個名為 `my-awesome-blog-site` 的站點:

```shell
gatsby new my-awesome-blog-site https://github.com/gatsbyjs/gatsby-starter-blog
```

- 如果你兩個參數都留空不填，CLI 會運行一個可交互的 shell，向你詢問一些輸入：

```shell
gatsby new
? What is your project called? › my-gatsby-project
? What starter would you like to use? › - Use arrow-keys. Return to submit.
❯  gatsby-starter-default
   gatsby-starter-hello-world
   gatsby-starter-blog
   (Use a different starter)
```

閱讀 [Gatsby starters docs](https://www.gatsbyjs.org/docs/gatsby-starters/) 獲取更新信息。

### `develop`

一旦你安裝了一個 Gatsby 站點，切換到項目根目錄下開啟開發服務器：

`gatsby develop`

#### 選項

|     選項      | 說明                                     |
| :-------------: | ----------------------------------------------- |
| `-H`, `--host`  | 設置主機地址。默認是 localhost                 |
| `-p`, `--port`  | 設置端口。默認是 8000                      |
| `-o`, `--open`  | 在瀏覽器中打開站點 |
| `-S`, `--https` | 使用 HTTPS                                      |

參照[本地 HTTPS 指南](/docs/local-https/)，瞭解如何使用 Gatsby 配置一個本地 HTTPS 的開發服務器。


#### 在其它設備上預覽修改

你可以使用帶 host 選項的 Gatsby 開發命令運行你的開發服務器，這樣你可以通過同一網絡下的其它設別訪問服務，運行：

```shell
gatsby develop -H 0.0.0.0
```

終端會照常打印日誌。但是會附加一個 URL，你可以通過同一網絡下的其它客戶端訪問這個 URL，以便查看站點在其它設備上如何展示（適用於手機真機調試）。

```
You can now view gatsbyjs.org in the browser.
⠀
  Local:            http://0.0.0.0:8000/
  On Your Network:  http://192.168.0.212:8000/ // highlight-line
```

**注**：在 Windows 上你無法訪問 0.0.0.0:8000 （但是在 Windows 上可以通過 localhost:8000 或者 "On Your Network" 中給出的 URL 訪問）

### `build`

在 Gatsby 站點的根目錄下，編譯你的應用為部署做好準備：

`gatsby build`

#### 選項

|            選項            | 說明                                                                                               |
| :--------------------------: | --------------------------------------------------------------------------------------------------------- |
|       `--prefix-paths`       | 構建站點時，使用 link paths 前綴（在你的配置中設置的 pathPrefix）                                       |
|        `--no-uglify`         | 構建站點時，不混淆 JS 包（方便調試）
| `--open-tracing-config-file` | 追蹤器配置文件（和 OpenTracing 兼容）。參見 [性能追蹤](/docs/performance-tracing/) |
| `--no-color`, `--no-colors`  | 禁用終端打印彩色輸出                                                                          |

除了上面這些構建選項，這裡有更多的選項[構建環境變量](/docs/environment-variables/#build-variables) 用於更加高級的配置，去調整構建的運行。例如，設置 `CI=true` 環境變量將會為[啞終端](https://en.wikipedia.org/wiki/Computer_terminal#Dumb_terminals)定製輸出。

### `serve`

在 Gatsby 站點的根目錄下，啟動一個服務器用於測試你站點的生產環境版本：

`gatsby serve`

#### 選項

|      選項      | 說明                                                                              |
| :--------------: | ---------------------------------------------------------------------------------------- |
|  `-H`, `--host`  | 設置主機地址。默認是 localhost                                                          |
|  `-p`, `--port`  | 設置端口。 默認是 9000                                                               |
|  `-o`, `--open`  | 在瀏覽器中打開站點                                         |
| `--prefix-paths` | 站點服務帶有路徑前綴（如果構建時你的 gatsby-config.js 使用了 pathPrefix) |

### `info`

在 Gatsby 站點根目錄下，獲取有用的環境信息，當你報告 bug 時，通常需要提供這些信息：

`gatsby info`

#### 選項

|       選項        | 說明                                             |
| :-----------------: | ------------------------------------------------------- |
| `-C`, `--clipboard` | 自動複製環境信息到剪切板 |

### `clean`

在 Gatsby 站點根目錄下， 清空緩存（`.cache` 目錄）和 `public 目錄：

`gatsby clean`

當您的本地項目似乎有問題或內容似乎沒有刷新時，此方法非常有用。 這通常可以解決下面這些問題：

- 數據過時，例如：某個文件/資源等沒有出現。
- GraphQ 錯誤，例如： GraphQL 資源應該存在，但是找不到。
- 依賴問題，例如：無效的版本，控制檯中的隱祕錯誤等等。
- 插件問題，例如：開發一個本地插件，但是變更似乎沒有生效。

### `plugin`

運行與 Gatsby 插件有關的命令。

#### `docs`

`gatsby plugin docs`

指導你使用和創建插件的文檔。

### Repl

使用你的 Gatsby 環境的上下文獲取一個 Node.js 的 REPL（交互式 shell）：

`gatsby repl`

Gatsby 將提示你鍵入命令並進行瀏覽。 當顯示以下內容時：`gatsby >`

你可以鍵入下面這些命令：

`babelrc`

`components`

`dataPaths`

`getNodes()`

`nodes`

`pages`

`schema`

`siteConfig`

`staticQueries`

當和[GraphQL explorer](/docs/introducing-graphiql/)一起使用時，這些 REPL 命令會非常有用，它有助於你理解 Gatsby 站點的數據。

想了解更多信息，查閱[Gatsby REPL 文檔](/docs/gatsby-repl/)。

### 禁用彩色輸出

除了顯式的 `--no-color` 選項，CLI 還考慮到了 `NO_COLOR` 環境變量的存在（參見 [no-color.org](https://no-color.org/)）。

## 如何為你的下一個項目更改默認的包管理器？

當你使用 `gatsby new` 首次創建一個新的項目時，你會被詢問是使用 yarn 還是 npm 作為包管理器。

```shell
Which package manager would you like to use ? › - Use arrow-keys. Return to submit.
❯  yarn
   npm
```

一旦你做出了選擇，在後續的項目中 CLI 不會再詢問你的偏好。

如果要為下一個項目更改此設置，則必須編輯由 CLI 自動創建的配置文件。
這個文件位於你係統中的這個位置： `~/.config/gatsby/config.json`

在裡面你將看到這樣的內容。

```json:title=config.json
{
  "cli": {
    "packageManager": "yarn"
  }
}
```

編輯你的 `packageManager` 的值，保存。然後你就可以愉快地使用 `gatsby new` 創建下一個項目了。
