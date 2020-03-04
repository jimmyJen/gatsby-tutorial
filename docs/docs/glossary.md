---
title: 詞彙表
disableTableOfContents: true
---

import HorizontalNavList from "../../www/src/components/horizontal-nav-list.js"

當你不熟悉 Gatsby 時，可能會不瞭解很多詞彙。本詞彙表旨在為你提供一篇足夠長的的常用術語概述，以及這些術語在 Gatsby 站點中意味著什麼。

<HorizontalNavList
items={"ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("")}
slug={props.slug}
/>

## A

### AST（抽象語法樹）

Abstract Syntax Tree（抽象語法樹）：在兩種語言之間的 [編譯](#compiler) 階段，源代碼的樹結構表示。例如 [gatsby-transformer-remark](/packages/gatsby-transformer-remark/) 會從 [Markdown](#markdown) 文件創建一個 AST，通過 [Remark](#remark) 解析器來描述一個樹結構 Markdown 文檔

### API（應用程序編程接口）

應用程序編程接口：一種應用程序與另一應用程序進行通信的方法。例如 [數據源插件](#source-plugin) 通常會使用API來獲取其數據。

### Accessibility（可訪問性）

為了消除阻礙殘疾人與網站互動或訪問網站障礙的一種包容性做法。如果為了實現可訪問性正確設計、開發和編輯了站點，則通常所有用戶對信息和功能都有同等的訪問權。閱讀 [Gatsby 對可訪問性的承諾](/blog/2019-04-18-gatsby-commitment-to-accessibility/)。

## B

### Babel

一個讓你編寫最現代的 [JavaScript](#javascript) 代碼的工具，並在 [構建](#build) 時將代碼 [編譯](#compiler) 為大多數瀏覽器可以理解的代碼。

### Backend（後端）

[公眾（public）](#public) 看不到的幕後。通常指你的 [CMS](#cms) 的控制面板。這些通常由服務器端編程語言提供支持，例如Node.js，PHP，Go，ASP.net，Ruby 或 Java。

### Build（構建）

在 Gatsby 中，這是獲取代碼和內容並將其打包到可以託管和訪問的網站中的過程。我們通常說 _構建時……_。另請參見：[後端](#backend) and [服務器端](#server-side)。

## C

### Cache（緩存）

一個可以再次使用的本地信息存儲方式，使我們可以從某一位置更快地計算和查找。Gatsby 使用緩存來存儲信息，因此在你進行開發時，它可以更快地構建你的網站，而無需重複相同的工作。

### CLI（命令行界面）

命令行界面：通過 [命令行](#command-line) 在計算機上運行並通過鍵盤交互的應用程序。

Gatsby 有兩個命令行界面。一個是[`gatsby`](/docs/gatsby-cli/)，用於 Gatsby 日常開發；另一個是 [`gatsby-dev`](/contributing/setting-up-your-local-dev-environment/#gatsby-repo-install-instructions)，供那些參與貢獻 Gatsby 項目的人使用。

### Client-side（客戶端）

客戶端是指由用戶瀏覽器在計算機網絡中以 [客戶與服務關係](https://en.wikipedia.org/wiki/Client%E2%80%93server_model) 執行的操作。在 Gatsby 中，當使用依賴於 [瀏覽器 DOM](#dom) 中的對象（例如 `window` 或 `navigator`）的 [客戶端包](/docs/using-client-side-only-packages/)時很重要。另請參見：[服務器端](#server-side)，[前端](#frontend) 和 [後端](#backend)。

### CMS（內容管理系統）

內容管理系統：你可以在其中管理內容並將其保存到數據庫或文件中以供日後訪問。內容管理系統包括 WordPress，Drupal，Contentful 和 Netlify CMS 等等。

### Command Line（命令行）

一個基於文本的界面，用於在計算機上運行命令。Mac 和 Windows 的默認命令行應用程序分別是 `Terminal` 和 `Command Prompt`。

### Compiler（編譯器）

編譯器是將一種語言編寫的代碼轉換為另一種語言的程序。例如，[Gatsby](#gatsby) 可以將 [React](#react) 應用程序編譯成靜態 [HTML](#html) 文件。

### Component（組件）

組件是由 [React](#react) 提供支持的獨立且可重複使用的代碼塊，這些代碼結合起來構成你的網站或應用。

組件中可以包含組件。[pages](#page) 和 [templates](#template) 是兩個很好的實際例子。

### Config（配置）

配置文件。`gatsby-config.js` 告訴 Gatsby 有關你的網站的信息。一個常見的配置選項是你的網站元數據（meatadata），它可以為你的 SEO 元標記（meta tag）提供支持。

### CSS（級聯樣式表）

[CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) 代表級聯樣式表，它是 [HTML](#html) 和 [JavaScript](#javascript) 在網絡平臺（Web Platform）中的重要組成部分。CSS 是一種使網頁樣式化的語言，旨在使網頁高度向後兼容。隨著向終端用戶推出新功能，[CSS 解析器](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/#CSS_parsing) 可以安全地忽略不受支持的功能，並增強其支持的屬性。CSS通過 _級聯_ 設計來完成任務，這是使用 [CSS Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/CSS_Grid_and_Progressive_Enhancement) 等新技術進行樣式設置的基礎。Gatsby支持多種 [樣式設計方法](/docs/styling/)，包括常規 CSS 文件，CSS 模塊和 CSS-in-JS。

## D

### Data Source（數據源）

內容和數據的起點。通常通過 [數據源插件](#source-plugin) 集成到 Gatsby 中。數據源通常是 [無頭 CMS](#headless-cms)，但它也可以包含 Markdown，JSON 或 YAML 文件。

### Database（數據庫）

數據庫是數據或內容的結構化集合。通常 [CMS](#cms) 使用 [後端技術](#backend) 將數據保存到數據庫。通常可以通過 [數據源插件](#source-plugin) 在 Gatsby 中訪問它們。

### Decoupled（解耦）

解耦描述了關注點的分離。對於 [Gatsby](#gatsby)，這通常意味著將 [前端](#frontend) 與 [後端](#backend) 分離，比如 [Drupal 解耦](https://dri.es/how-to-decouple-drupal-in-2019) 或 [無頭 WordPress](https://www.smashingmagazine.com/2018/10/headless-wordpress-decoupled/)。

### Deploy（部署）

[構建](#build) 你的網站或應用並上傳到 [託管服務提供商](#hosting) 的過程。

### Development Environment（開發環境）

開發代碼時的[環境](#environment)。它可以通過使用 [CLI](#cli) `gatsby develop` 來訪問，並提供額外的錯誤報告和幫助你在為 [生產環境](#production-environment) 構建之前進行調試的功能。

### DOM（文檔對象模型）

文檔對象模型（通常稱為 “the DOM”）是一種標準的瀏覽器 API，它通過表示內存中 HTML 文檔的結構，將網頁連接到腳本或編程語言。開發人員通常通過 [HTML](#html) 標記（在Gatsby 中以 [JSX](#jsx) 編寫）以及 [React](https://reactjs.org/docs/react-dom.html) 和 [原生 JavaScript](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction#DOM_and_JavaScript) 與 DOM 進行交互。它充分利用 DOM 的另一個重要方面是編寫 [可訪問的](#accessibility) HTML 標記，以將頁面結構暴露給殘疾人輔助技術。

## E

### ECMAScript

ECMAScript（通常稱為ES）是腳本語言的規範。[JavaScript](#javascript) 是 ECMAScript 的一種實現。通常，開發人員會使用 [Babel](#babel) 將最新的 ECMAScript 代碼 [編譯](#compiler) 成更廣泛支持的 JavaScript。

### Environment（環境）

Gatsby 所運行的環境。例如在編寫代碼時，你可能希望儘可能多地進行調試，但這在上線的網站或應用程序中是不可取的。因此，Gatsby 可以根據所處的環境來更改其行為。

Gatsby 默認情況下支持兩種環境，即 [開發環境](#development-environment) 和 [生產環境](#production-environment)。

### Environment Variables（環境變量）

[環境變量](/docs/environment-variables/) 允許你根據應用程序的 [環境](#environment) 自定義其行為。例如你可能希望在開發過程中從測試 CMS 獲取內容，並在 [構建](#build) 網站時連接到生產 CMS。使用環境變量，可以為每個環境設置不同的 URL。

## F

### Filesystem（文件系統）

文件的組織方式。在 Gatsby 中，這意味著將文件與您的網站或應用程序的代碼放在同一位置，而不是從外部 [數據源](#data-source) 中提取數據。Gatsby 中常見的文件系統用法包括 Markdown 內容，圖像，數據文件和其他資源。

### Frontend（前端）

您的網站或應用程序面向 [公眾](#public) 的界面，使用 Web 技術（HTML，CSS 和 JavaScript）提供。有關網絡平臺（Web Platform）如何將這些技術整合在一起的更多信息，請查看 [瀏覽器的工作原理](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)。

## G

### Gatsby

Gatsby是一個現代的網站框架，它利用[React](#react)，[GraphQL](#graphql) 和現代的 [JavaScript](#javascript) 等最新網絡技術，為每個網站或應用程序構建卓越的性能。通過 Gatsby，無需成為網站性能專家，你就可以輕鬆創建非常快速的引人入勝的 Web 體驗。

### GraphQL

一種可讓您將數據提取到您的網站或應用中的 [查詢](#query) 語言。這是 [Gatsby 用於管理網站數據的界面](/docs/graphql/)。

## H

### HTML（超文本標記語言）

每個 Web 瀏覽器都能理解的標記語言。它表示 Hypertext Markup Language（超文本標記語言）。[HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) 為您的 Web 內容提供了一種通用的信息結構，定義了標題，段落等內容。這也是提供可訪問網站的關鍵。

### Headless CMS（無頭 CMS）

一個僅處理 [後端](#backend) 而不同時處理後端和 [前端](#frontend) 的 [CMS](#cms) 內容管理方式。這種類型的設置也稱為 [解耦](#decoupled)。

### Hosting

保留您的網站或應用程序副本的託管服務提供商，並允許 [公眾](#public) 訪問。[Gatsby的常見託管服務提供商](/docs/deploying-and-hosting/) 包括了 Netlify，AWS，S3，Surge，Heroku 等。

### Hot module replacement（模塊熱替換）

運行 `gatsby develop` 時使用的一項功能。在文本編輯器中實時保存代碼後，可在打開的瀏覽器窗口中自動替換模塊或代碼塊，從而實時更新網站。

### Hydration（補水）

一旦網站由 Gatsby [構建](#build) 並加載到網絡瀏覽器中，[客戶端](#client-side) JavaScript 資源就會被下載並將該站點轉變為可以操縱的 [DOM](#dom)。這個過程通常稱為 “補水”，因為它運行了一些用於生成 Gatsby 頁面的 JavaScript 相同代碼，但是這個過程中諸如 `window` 之類的瀏覽器 DOM API 可用。

## I

## J

### JAMStack

JAMStack 是指使用 [JavaScript](#javascript)，[API](#api) 和（[HTML](#html)）標記的現代 Web 體系架構。引用自 [JAMStack.org](https://jamstack.org)：“這是一種構建網站和應用程序的新方法，可提供更強的性能，更高的安全性，更低的擴展成本以及更好的開發人員體驗。”

### JavaScript

一種編程語言，可以幫助我們使網頁內容動態可交互。[JavaScript](https://developer.mozilla.org/en-US/docs/Web/Javascript) 是在瀏覽器中廣泛使用的 Web 技術。它也可以與 [Node.js](#node) 在服務器端一起使用。它是 [ECMAScript](#ECMAScript) 規範的一種實現。

### JSX

JSX 是 JavaScript 的擴展，允許開發人員在同一代碼段中編寫 HTML 和自定義組件。[React團隊推薦](https://reactjs.org/docs/introducing-jsx.html) 使用它來描述 [UI](#UI) 如何呈現。JSX 可能會讓您想起模板語言，但是它具有 JavaScript 的全部功能。需要注意的重要細節：由於 JSX 使用 JavaScript，因此必須交換標記中的某些 HTML 屬性，以避免使用 JavaScript 中的保留字（例如 `htmlFor` 和 `className`）。

## K

## L

### Linting

Linting 是運行程序時分析代碼是否存在潛在錯誤的過程。Gatsby 項目使用 [prettier](https://prettier.io/) 來識別和修復常見樣式問題。React 項目中常用的另一個 linter 的例子是 [eslint-jsx-plugin-a11y](https://github.com/evcohen/eslint-plugin-jsx-a11y)，它檢查開發中常見的 [可訪問性](#accessibility) 問題。

## M

### MDX

一種用來支持在內容中顯示 [React](#react) [組件](#component) 的 [Markdown](#markdown) 擴展。

### Markdown

一種使用純文本編寫 HTML 內容的方法，使用特殊字符來表示內容類型，比如表示的 [標題](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements) 的井號、下劃線強調文本的星號。

## N

### NPM

一個 [Node](#node) 的 [包](#package) 管理器。允許您安裝和更新項目所依賴的其他軟件包。[Gatsby](#gatsby) 和 [React](#react) 是兩個項目依賴項例子。另請參見：[Yarn](#yarn)。

### Node（節點）

Gatsby 使用 [數據節點](/docs/node-interface/) 來表示一條數據。一個 [數據源](#data-source) 會創建多個數據節點。

### Node.js

一個讓你在計算機上運行 [JavaScript](#javascript) 的程序。Gatsby 由 Node 驅動。

## O

## P

### Package（包）

包通常描述一個 [JavaScript](#javascript) 程序，該程序具有有關應如何分發和使用它的附加信息，例如其版本號。[NPM](#npm) 和 [Yarn](#yarn) 管理和安裝您的項目使用的軟件包。[Gatsby](#gatsby) 本身也是是一個包。

### Page（頁面）

一個 [HTML](#html) 頁面。

通常也指存在於 `/src/pages/` 的 [組件](#component)，它們通過 [Gatsby](#gatsby) 轉換為頁面。也可以指 `gatsby-node.js` 文件中 [動態生成的頁面](/docs/creating-and-modifying-pages/#creating-pages-in-gatsby-nodejs)。

### Plugin（插件）

為 Gatsby 添加功能的沒有被囊括其中的附加代碼。常見的 [Gatsby 插件](/plugins/) 包括分別負責拉取和處理數據的 [數據源](#source-plugins) 和 [數據轉換](#transformer) 插件。

### Production Environment（生產環境）

[部署](#deploy) 後用戶將要體驗到的 [構建好](#build) 的站點或應用所在的 [環境](#environment)。它可以通過在 [CLI](#cli) 中使用 `gatsby build` 或 `gatsby serve` 訪問到。

### Programmatically（以編程的方式）

某些事件會根據你的代碼和配置自動執行。例如，您可以 [配置](#config) 項目來為每個撰寫的博客文章創建一個 [頁面](#page)，或者讀取並顯示當前年份，在站點頁腳中作為版權的一部分。

### Progressive enhancement（漸進式增強）

漸進增強是一種強調首先從服務器加載核心頁面內容，然後再加載其他內容的 Web 策略，而無需加載[JavaScript](#javascript)。然後，此策略會在終端用戶的瀏覽器或網絡連接允許的情況下，在當前內容上逐步添加更復雜的表示層和功能層。Gatsby 的默認方法是提前 [構建](#build) 頁面，這意味著內容將首先加載出來，然後和隨著腳本的下載和執行漸漸增強。

### Public（公眾）

通常是指公眾成員（而不是你的團隊成員），也可以是指你 [構建好](#build) 的站點或應用中的 `/public` 文件夾。

## Q

### Query（查詢）

從某處請求特定數據的過程。Gatsby 中通常使用 [GraphQL](#graphql) 進行查詢。

## R

### React

一個用於構建用戶界面的代碼庫（使用[JavaScript](#javascript))編寫）。它是 [Gatsby](#gatsby) 用於構建頁面和組織內容的框架。

### Remark（備註）

一種用於將[Markdown](#markdown) 轉換為其他格式的解析器，其他格式包括 [HTML](#html) 和 [React](#react)代碼。

### Runtime（運行時）

運行時是指一個程序正在運行（或可執行）的時間。它可以指這麼幾件事：[Node.js](#nodejs) 時執行 JavaScript 代碼的 [服務器端](#server-side) 運行時；另一方面，[客戶端 JavaScript](#client-side) 是指執行傳統 JavaScript 代碼的瀏覽器運行時。Gatsby會在 [構建時](#build) 編譯您的站點，並 [使用 React 運行時進行補水](#hydration) 來提供快速的，交互式的和動態的用戶體驗。

### Routing（路由）

路由是一種根據網絡請求（通常是 URL ）在網站或應用中加載正確內容的機制。例如，它允許將 `/about-us` 之類的 URL 路由到適當的[page](#page)，[template](#template) 或 [component](#component)。

## S

### Schema（數據模式）

一種數據在系統中如何存儲的準確表示方式，例如數據庫中的表和字段，或 JSON 文件結構。在 Gatsby 中，GraphQL 模式表示所有可查詢的數據，或作為 Gatsby 數據層的一部分的組件可以查詢的數據。

### Server-side（服務器端）

[客戶端-服務器關係](https://en.wikipedia.org/wiki/Client%E2%80%93server_model) 的服務器端部分是指：管理訪問集中式資源或計算機網絡中的服務的由計算機程序執行的操作。Gatsby 使用服務器端技術 [Node.js](#nodejs) 在構建時編譯頁面，而不是在[瀏覽器運行時](#runtime) 中使用 [客戶端](#client-side) JavaScript 來提供頁面 。另請參見：[frontend](#frontend) 和 [backend](#backend)。

### Source Code（源代碼）

源代碼是位於 `/src/` 目錄中的代碼，它們構成網站或應用程序的獨特內容。它由 [JavaScript](#javascript) 文件組成，有時也包括 [CSS](#css) 和其他文件。

源代碼在 [構建](#build) 站點時被編譯，以使得 [公眾](#public) 能看見。

### Source Plugin（數據源插件）

一個向 Gatsby 添加額外 [數據源](#data-source) 的插件，數據源可以之後被 [頁面](#page) 和 [組件](#component) [查詢](#query) 到。

### Starter

一個預先配置好的 Gatsby 項目，可以用作項目的起點。可以在 [Gatsby Starter 庫](/starters/) 中找到它們，並使用 [Gatsby CLI](/docs/starters/) 安裝它們。

### Static（靜態）

Gatsby 構建頁面的靜態版本，可以輕鬆地被 [託管](#hosting)。這與動態系統不同，在動態系統中，每個頁面都是即時生成的。保持靜態狀態可帶來顯著的性能提升，因為只有每次更改內容或代碼時才需要工作。

它也可以指 `/static` 文件夾，該文件夾會自動複製到每個 [構建](#build) 後的文件裡的 `/public` 中，用於不需要由 Gatsby 處理但確實需要存在於 [public](#public) 文件夾中的文件。

## T

### Template（模板）

一個 [以編程的方式](#programmatically) 由 Gatsby 轉換為頁面的 [組件](#component)。

### Theme（主題）

Gatsby 主題就像 WordPress 主題一樣，可組合（與其他主題），可擴展（具有更多邏輯）和可替換（[組件遮蔽](/blog/2019-04-29-component-shadowing/)）。Gatsby 主題可以施加在使用它的 Gatsby 應用的任何方面，也可以提供任意數量的設置來打開或關閉功能。

### Transformer（數據轉換插件）

一個可以將一種數據類型轉換為另一種數據類型的 [插件](#plugin)。例如你可以將電子表格轉換為 [JavaScript](#javascript) 數組。

## U

### UI（用戶界面）

UI 是指用戶界面。在人機交互領域，UI 是人機之間交互的空間。這種交互的目的是允許從人的角度對機器進行有效的操作和控制，機器同時反饋有助於用戶決策過程的信息（例如錯誤消息或通知）。

## V

## W

### Webpack

一個 Gatsby 用來打包網站代碼的 [JavaScript](#javascript) 應用程序。它會自動在 [構建](#build) 是執行。

## X

## Y

### Yarn

一個 [包](#package) 管理器，有些人相對於 [NPM](#npm) 更喜歡它。它也是 [開發 Gatsby 應用](/contributing/setting-up-your-local-dev-environment/#using-yarn) 所需要的。

## Z
