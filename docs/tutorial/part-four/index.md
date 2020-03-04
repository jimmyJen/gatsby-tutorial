---
title: Gatsby 中的數據
typora-copy-images-to: ./
disableTableOfContents: true
---

歡迎來到教程的第 4 章！你已經完成一半啦！希望你覺得事情開始變得輕鬆起來 😀

## 回顧教程的上半部分

目前，你已經學習瞭如何使用 React.js，並瞭解到創建你 _自己_ 的組件作為站點的構建模塊是多麼有效。

你還通過使用 CSS 模塊探索了樣式組件。

## 教程裡還有什麼?

在接下來的 4 章教程中（包含本章），你將深入 Gatsby 的數據層。它是一個非常強大的功能，使你能夠輕鬆地從 Markdown、WordPress、headless CMS 和其他各種各樣的數據源來構建站點。

**注意：** Gatsby 的數據層是由 GraphQL 驅動的。如果你想看 GraphQL 的深入教程，我們推薦這個網站：[如何 GraphQL](https://www.howtographql.com/)。

## Gatsby 中的數據

一個網站有四個部分：HTML、CSS、JS 和 數據。教程的上半部分專注於前三項。現在讓我們學習怎麼使用 Gatsby 站點中的數據。

**什麼是數據?**

一個非常計算機科學風格的答案是：數據就是 `"字符串"（strings）`、整數 （`42`）、 對象 （`{ pizza: true }`）等等.

但是在 Gatsby 裡面，一個更有用的答案是：“所有在 React 組件外的東西”。

目前你已經 _直接_ 在組件裡面添加了一些文字和圖片。這對於許多網站來說是一個 _非常好_ 的構建方式。但是，你會經常想要在組件 _外面_ 存儲數據並按照需要把輸入傳入到組件 _裡面_。

如果你正在用 WordPress（它為其他貢獻者添加和維護內容提供了一個非常漂亮的界面）和 Gatsby 構建一個站點，網站的 _數據_（頁面和文章）都在 WordPress 裡面，你只要按需把數據 _提取_ 到你的組件裡。

數據也可以在 Markdown、CSV之類的文件格式裡，甚至數據庫、API 等各種各樣的形式。

**Gatsby 的數據層讓你從這些（或其他任意）數據源中直接提取數據到你的組件裡**——以你想要的形態存放。

## 對比非結構化數據與 GraphQL

### 我必須用 GraphQL 和數據源插件來把數據提取到 Gatsby 站點嗎？

當然不是！你可以直接用節點 `createPages` API 把非結構化數據提取到 Gatsby 頁面，而不是通過 GraphQL 數據層。這對於小型網站來說是一個非常好的選擇。但是 GraphQL 和數據源插件可以幫你在構建複雜網站的時候節省時間。

閱讀 [在不用 GraphQL 的情況下使用 Gatsby ](/docs/using-gatsby-without-graphql/) 這篇指南來學習如何使用節點 `createPages` API 來提取數據到你的 Gatsby 站點裡！它包含了一個示例網站。

### 什麼時候用非結構化數據？什麼時候用 GraphQL？

如果你構建的是一個小型網站，一個非常高效的搭建的方式就是使用上面提到的 `createPages` API 提取非結構化數據。然後當這個站點變得越來越複雜，或者你要開發其他更加複雜的網站，亦或者你想要轉換數據，做這幾步：

1.  瀏覽 [插件庫](/plugins/)，尋找你想用的數據源插件或數據轉換插件
2.  如果找不到，閱讀 [插件編寫](/docs/creating-plugins/) 指南，考慮創建一個你自己的插件！

### Gatsby 的數據層是如何用 GraphQL 來把數據提取到組件裡的

把數據加載到 React 組件裡有許多方式。[GraphQL](http://graphql.org/) 是一個非常受歡迎且強大的技術。

GraphQL 是 Facebook 發明的，它能幫助項目工程師 _提取_ 所需數據到組件裡。

GraphQL 是一種 **q**uery **l**anguage（查詢語言）（名字中 QL 的由來）。如果你熟悉 SQL，它們的使用方式很像。通過一種特殊的語法，你就能描述你想要的組件中的數據，然後它就能把數據傳給你。

Gatsby 使用 GraphQL 來使組件能夠聲明其所需的數據。

## 創建一個新的示例站點

在教程的這一部分，你要創建另一個新站點。你將建立一個名為 “Pandas Eating Lots” 的 Markdown 博客網站。這個博客的目的是展示一些性感熊貓在線吃竹的圖片和視頻。在此過程中，你將逐步深入 GraphQL 和 Gatsby 的 Markdown 支持。

打開一個新的終端窗口，然後在名為 “tutorial-part-four” 的目錄中運行以下命令，創建一個新的Gatsby網站。然後導航到這個新目錄：

```shell
gatsby new tutorial-part-four https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-four
```

然後在項目的根目錄安裝其他一些所需的依賴項。你將使用 Typography 主題 “Kirkham”，和一個 CSS-in-JS 的庫 ["Emotion"](https://emotion.sh/):

```shell
npm install --save gatsby-plugin-typography typography react-typography typography-theme-kirkham gatsby-plugin-emotion @emotion/core
```

建立一個和 [第 3 章](/tutorial/part-three) 中建立的站點相似的新站點. 這個站點要有一個佈局組件和兩個頁面組件:

```jsx:title=src/components/layout.js
import React from "react"
import { css } from "@emotion/core"
import { Link } from "gatsby"

import { rhythm } from "../utils/typography"

export default ({ children }) => (
  <div
    css={css`
      margin: 0 auto;
      max-width: 700px;
      padding: ${rhythm(2)};
      padding-top: ${rhythm(1.5)};
    `}
  >
    <Link to={`/`}>
      <h3
        css={css`
          margin-bottom: ${rhythm(2)};
          display: inline-block;
          font-style: normal;
        `}
      >
        Pandas Eating Lots
      </h3>
    </Link>
    <Link
      to={`/about/`}
      css={css`
        float: right;
      `}
    >
      About
    </Link>
    {children}
  </div>
)
```

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <h1>Amazing Pandas Eating Things</h1>
    <div>
      <img
        src="https://2.bp.blogspot.com/-BMP2l6Hwvp4/TiAxeGx4CTI/AAAAAAAAD_M/XlC_mY3SoEw/s1600/panda-group-eating-bamboo.jpg"
        alt="Group of pandas eating bamboo"
      />
    </div>
  </Layout>
)
```

```jsx:title=src/pages/about.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <h1>About Pandas Eating Lots</h1>
    <p>
      We're the only site running on your computer dedicated to showing the best
      photos and videos of pandas eating lots of food.
    </p>
  </Layout>
)
```

```javascript:title=src/utils/typography.js
import Typography from "typography"
import kirkhamTheme from "typography-theme-kirkham"

const typography = new Typography(kirkhamTheme)

export default typography
export const rhythm = typography.rhythm
```

`gatsby-config.js` (必須在項目根目錄, 不是 src 目錄)

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-plugin-emotion`,
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

添加以上文件然後照常運行 `gatsby develop`，你應該會看到：

![開始頁](start.png)

你有了另一個小型網站，包含一個佈局和兩個頁面。

現在我們來開始查詢數據 😋

## 你的第一個 GraphQL 查詢語句

建立網站時，您可能需要重用一些常用的數據——比如 _網站標題_。查看 `/about/`頁面，你會發現佈局組件（網站標題）以及 `about.js` 頁面的 `<h1 />` 頁面標題）中都有網站標題（`Pandas Eating Lots`）。

但是如果你以後想更改站點名稱怎麼辦？你必須在所有組件中搜索標題並修改每處地方。這既麻煩又容易出錯，尤其是對於大型的複雜的站點。相反，你可以將標題存儲在一個位置，並從其他文件引用該位置。你只要在這一個地方更改標題，Gatsby 就會將更新後的標題提取到引用它的文件中。

這些常用數據的存放位置就是 `gatsby-config.js` 文件中的 `siteMetadata` 對象。添加你的網站標題到 `gatsby-config.js` 文件裡：

```javascript:title=gatsby-config.js
module.exports = {
  // highlight-start
  siteMetadata: {
    title: `Title from siteMetadata`,
  },
  // highlight-end
  plugins: [
    `gatsby-plugin-emotion`,
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

重啟開發服務器。

### 使用頁面查詢（page query）

現在網站標題能被查詢到了。用 [頁面查詢](/docs/page-query) 把它添加到 `about.js` 文件裡：

```jsx:title=src/pages/about.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-next-line
export default ({ data }) => (
  <Layout>
    <h1>About {data.site.siteMetadata.title}</h1> {/* highlight-line */}
    <p>
      We're the only site running on your computer dedicated to showing the best
      photos and videos of pandas eating lots of food.
    </p>
  </Layout>
)

// highlight-start
export const query = graphql`
  query {
    site {
      siteMetadata {
        title
      }
    }
  }
`
// highlight-end
```

成功了！🎉

![從 siteMetadata 中提取的頁面標題](site-metadata-title.png)

在以上 `about.js` 文件的改動中，獲取 `title` 的基礎 GraphQL 查詢語句是：

```graphql:title=src/pages/about.js
{
  site {
    siteMetadata {
      title
    }
  }
}
```

> 💡 在 [第 5 章教程](/tutorial/part-five/#introducing-graphiql) 中，你將看到一個工具，它使我們可以交互式地通過 GraphQL 瀏覽可獲得的數據，並幫我們編寫類似於上面的查詢語句。

Page queries live outside of the component definition -- by convention at the end of a page component file -- and are only available on page components.

頁面查詢定義在組件之外（一般在頁面組件文件的最後），並且只在頁面組件中可用。

### 使用 StaticQuery（靜態查詢）

[StaticQuery](/docs/static-query/) 是 Gatsby v2 中引入的新 API，這個 API 允許非頁面組件（比如你的 `layout.js` 組件）通過 GraphQL 查詢語句來獲得數據。讓我們使用最新引入的 hook 版本 — [`useStaticQuery`](/docs/use-static-query/).

我們繼續對 `src/components/layout.js` 文件內容做一些更改。用 `useStaticQuery` hook 和一個 `{data.site.siteMetadata.title}` 引用來使用這個數據。改完後你的文件將如下所示：

```jsx:title=src/components/layout.js
import React from "react"
import { css } from "@emotion/core"
// highlight-next-line
import { useStaticQuery, Link, graphql } from "gatsby"

import { rhythm } from "../utils/typography"
// highlight-start
export default ({ children }) => {
  const data = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
          }
        }
      }
    `
  )
  return (
    // highlight-end
    <div
      css={css`
        margin: 0 auto;
        max-width: 700px;
        padding: ${rhythm(2)};
        padding-top: ${rhythm(1.5)};
      `}
    >
      <Link to={`/`}>
        <h3
          css={css`
            margin-bottom: ${rhythm(2)};
            display: inline-block;
            font-style: normal;
          `}
        >
          {data.site.siteMetadata.title} {/* highlight-line */}
        </h3>
      </Link>
      <Link
        to={`/about/`}
        css={css`
          float: right;
        `}
      >
        About
      </Link>
      {children}
    </div>
    // highlight-start
  )
}
// highlight-end
```

又成功了！🎉

![均從 siteMetadata 中提取的頁面標題和佈局標題](site-metadata-two-titles.png)

為什麼在這裡使用兩種不同的查詢語句？這些例子是對查詢類型、格式設置以及在何處使用的簡要介紹。目前你只要記住：只有頁面可以進行頁面查詢。非頁面組件（例如 Layout）可以使用 StaticQuery。本教程的 [第 7 章](/tutorial/part-seven/) 會對此進行更深入的說明。

讓我們改回真正的標題。

Gatsby 的核心原則之一是 _創作者需要與他們創造的東西有實時聯繫_（[感謝 Bret Victor](http://blog.ezyang.com/2012/02/transcript-of-inventing-on-principle/)）。換句話說，當你對代碼進行任何更改時，你應該立馬看到該更改的效果。你在 Gatsby 中改變輸入，在屏幕上就能看到新的輸出。

因此幾乎在任何時候，你所做的更改都會立即生效。再次編輯 `gatsby-config.js` 文件，這次將 `title` 改回 “Pandas Eating Lots”。所做的更改應很快顯示在你的網站頁面上。

![兩個標題都是 Pandas Eating Lots](pandas-eating-lots-titles.png)

## 接下來

下面你會在教程的 [第 5 章](/tutorial/part-five/) 中學習到如何使用 GraphQL 和數據源插件提取到你的 Gatsby 站點之中。
