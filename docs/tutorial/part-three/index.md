---
title: 創建嵌套的佈局組件
typora-copy-images-to: ./
disableTableOfContents: true
---

歡迎來到 Gatsby 第三部分教程！

## 本教程的內容

在這一部分中，你將學習 Gatsby 插件的使用和創建 “佈局” 組件。

Gatsby 插件是用於向 Gatsby 網站添加功能的 JavaScript 包。 Gatsby 被設計為可擴展的，這意味著插件也是像 Gatsby 一樣是可以擴展和修改的。

佈局組件用於你要在多個頁面上共享的網站公共部分。例如，站點通常有公共的頁眉和頁腳的佈局組件。其他常見的要添加到佈局的公共的內容是是側邊欄或導航菜單。例如，在此頁面上，頂部標題是 gatsbyjs.org 的佈局組件的一部分。

讓我們深入瞭解第三部分教程。

## 使用插件

你很可能已經熟悉插件的概念。許多軟件系統都支持添加自定義插件來添加新功能，甚至可以修改軟件的核心功能。Gatsby 插件的工作方式也一樣。

社區成員（像你一樣！）可以貢獻插件（只需少量 JavaScript 代碼），其他人可以在構建 Gatsby 網站時使用你的插件。

> 目前已經有數百個插件！ 瞭解 Gatsby [插件庫](/plugins/)。

我們的插件的目標是使它們易於安裝和使用。你可能會在幾乎所有構建的 Gatsby 網站中使用到插件。在學習本教程的其餘部分時，你將有很多機會練習安裝和使用插件。

對於使用插件的初步介紹，我們將為 Typography.js 安裝並使用 Gatsby 插件。

[Typography.js](https://kyleamathews.github.io/typography.js/) 是一個 JavaScript 庫，可為你網站的排版生成全局基本樣式。該庫具有一個 [相應的Gatsby插件](/packages/gatsby-plugin-typography)，可以在 Gatsby 網站中配合使用。

### ✋ 新建一個 Gatsby 網站

正如我們在 [第二部分](/tutorial/part-two/) 中提到的那樣，此時最好關閉本教程前面部分的命令行終端窗口和項目文件，以使你桌面的內容保持整潔。 然後打開一個新的命令行終端窗口並運行以下命令，在名為 `tutorial-part-three` 的目錄中創建一個新的 Gatsby 站點，然後定位到該新目錄：

```shell
gatsby new tutorial-part-three https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-three
```

### ✋ 安裝和配置 `gatsby-plugin-typography`

使用插件有兩個主要步驟：安裝和配置。

1. 命令行使用 npm 安裝 `gatsby-plugin-typography` 依賴包。

```shell
npm install --save gatsby-plugin-typography react-typography typography typography-theme-fairy-gates
```

> 注意：Typography.js 需要一些其他的 javascript 依賴包，因此說明中包括了這些 javascript 依賴包。每個插件的安裝指引中都會列出類似的其他要求。

2. 編輯項目根目錄下的文件 `gatsby-config.js` ，如下：

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

`gatsby-config.js` 文件是 Gatsby 會自動識別的另一個特殊文件。 要在這裡添加插件和網站配置。

> 如果需要，請查看 [關於 gatsby-config.js 的文檔](/docs/gatsby-config/) 以瞭解更多信息。

3. Typography.js 需要一個配置文件。 在 `src` 目錄中創建一個名為 `utils` 的新目錄。 然後在 `utils` 中添加一個名為 `typography.js` 的新文件，並將以下內容複製到該文件中：

```javascript:title=src/utils/typography.js
import Typography from "typography"
import fairyGateTheme from "typography-theme-fairy-gates"

const typography = new Typography(fairyGateTheme)

export const { scale, rhythm, options } = typography
export default typography
```

4. 啟動開發服務器。

```shell
gatsby develop
```

加載網站後，如果你使用 Chrome 開發人員工具檢查生成的 HTML，你會發現排版插件向 `<head>` 元素添加了一個帶生成的 CSS 樣式的 `<style>` 元素：

![typography-styles](typography-styles.png)

### ✋ 進行一些內容和樣式更改

將以下內容複製到你的 `src/pages/index.js` 文件中，以便你更好的看到 Typography.js 生成的 CSS 樣式的效果。

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div>
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </div>
)
```

你的網站現在應如下所示：

![no-layout](no-layout.png)

讓我們快速改進。許多網站在頁面中間居中顯示一列文本。 要創建此樣式，請將以下樣式添加到 `src/pages/index.js` 中的 `<div>` 元素中。

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  // highlight-next-line
  <div style={{ margin: `3rem auto`, maxWidth: 600 }}>
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </div>
)
```

![with-layout2](with-layout2.png)

很好。你已經安裝並配置了第一個 Gatsby 插件！

## 創建佈局組件

現在讓我們繼續學習佈局組件。為了準備好這一部分，請在項目中添加幾個新頁面：“關於我們” 頁面和 “聯繫我們” 頁面。

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div>
    <h1>About me</h1>
    <p>I’m good enough, I’m smart enough, and gosh darn it, people like me!</p>
  </div>
)
```

```jsx:title=src/pages/contact.js
import React from "react"

export default () => (
  <div>
    <h1>I'd love to talk! Email me at the address below</h1>
    <p>
      <a href="mailto:me@example.com">me@example.com</a>
    </p>
  </div>
)
```

讓我們看看 “關於我們” 頁面的新外觀：

![about-uncentered](about-uncentered.png)

嗯，如果兩個新頁面的內容像首頁一樣居中，那就對了。擁有某種全局導航會很好，因此訪問者可以輕鬆找到並訪問每個子頁面。

你將通過創建第一個佈局組件來實現這些效果。

### ✋ 創建你的第一個佈局組件

1. 創建一個新目錄 `src/components`。

2. 在上面的目錄中創建一個非常基本的佈局組件文件 `src/components/layout.js`：

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

3. 將此新的佈局組件引入到你的 `src/pages/index.js` 頁面組件中：

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout" // highlight-line

export default () => (
  <Layout> {/* highlight-line */}
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </Layout> {/* highlight-line */}
)
```

![with-layout2](with-layout2.png)

太好了，佈局有效！首頁的內容仍居中。

但是，請嘗試導航至 `/about/` 或 `/contact/`。 這些頁面上的內容仍不會居中。

4. 將佈局組件引入 `about.js` 和 `contact.js` 中（就像上一步 `index.js` 文件一樣）。

有了這個公共佈局組件，你所有三個頁面的內容都居中！

### ✋ 添加網站標題

1. 將以下代碼添加到新的佈局組件：

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    <h3>MySweetSite</h3> {/* highlight-line */}
    {children}
  </div>
)
```

如果你進入三個頁面中的任何一個頁面，都會看到添加的是相同的標題，例如 `/about/` 頁面：

![with-title](with-title.png)

### ✋ 在頁面之間添加導航鏈接

1. 將以下內容複製到佈局組件文件中：

```jsx:title=src/components/layout.js
import React from "react"
// highlight-start
import { Link } from "gatsby"

const ListLink = props => (
  <li style={{ display: `inline-block`, marginRight: `1rem` }}>
    <Link to={props.to}>{props.children}</Link>
  </li>
)
// highlight-end

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {/* highlight-start */}
    <header style={{ marginBottom: `1.5rem` }}>
      <Link to="/" style={{ textShadow: `none`, backgroundImage: `none` }}>
        <h3 style={{ display: `inline` }}>MySweetSite</h3>
      </Link>
      <ul style={{ listStyle: `none`, float: `right` }}>
        <ListLink to="/">Home</ListLink>
        <ListLink to="/about/">About</ListLink>
        <ListLink to="/contact/">Contact</ListLink>
      </ul>
    </header>
    {/* highlight-end */}
    {children}
  </div>
)
```

![with-navigation2](with-navigation2.png)

很好！ 一個三個子頁面的網站，具有基本的全局導航。

_擴展：_ 使用新的 “佈局組件” 功能，嘗試向 Gatsby 網站添加頁眉、頁腳、全局導航和側邊欄等！

## 下一步

繼續閱讀 [本教程的第四部分](/tutorial/part-four/)，你將在此開始學習 Gatsby 的數據層並以編碼的方式創建頁面！
