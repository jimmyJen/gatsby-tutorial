---
title: 讓網站準備好上線
typora-copy-images-to: ./
disableTableOfContents: true
---

哇哦！你已經完成這麼多了！你學到了如何：

- 創建一個新的 Gatsby 站點
- 創建頁面和組件
- 為組件添加樣式
- 為網站添加插件
- 使用數據源和轉換數據
- 使用 GraphQL 來查詢頁面數據
- 以編程的方式利用數據創建頁面

在這最後一章中，你將學習到一些讓網站準備好上線的步驟。我們將介紹一個強大的站點診斷工具 [Lighthouse](https://developers.google.com/web/tools/lighthouse/)，並在過程中介紹一些你通常希望在 Gatsby 網站中使用的插件。

## 使用 Lighthouse 審查

引用自 [Lighthouse 官方介紹頁面](https://developers.google.com/web/tools/lighthouse/)：

> Lighthouse 是一個開源的自動化工具，用於改進網絡應用的質量。你可以將其作為一個 Chrome 擴展程序運行，或從命令行運行。 你為 Lighthouse 提供一個你要審查的網址，它將針對此頁面運行一連串的測試，然後生成一個有關頁面性能的報告。

Chrome 開發者工具中包含了 Lighthouse。運行審查功能然後解決發現的錯誤並執行建議的改進措施，是使網站能正常運行的好方法。它使你確保自己的網站儘可能快速，儘可能高可用。

試試看它吧！

首先，你需要使用生產環境打包。Gatsby 的開發服務器是為了快速開發而優化過的，雖然這個版本與生產版本極其相似，但是優化完全不一樣。

### ✋ 創建一個生產環境版本

1.  停止開發服務器（如果它正在運行）並執行以下命令：

```shell
gatsby build
```

> 💡 和你在 [第 1 章](/tutorial/part-one/) 中學到的一樣，這個命令會構建網站的生產版本，並把靜態文件輸出到 `public` 目錄中。

2.  在本地查看你的生產環境版本。運行：

```shell
gatsby serve
```

當啟動完成後，你可以個在 [`localhost:9000`](http://localhost:9000) 這個頁面訪問你的網站。

### 運行一次 Lighthouse 審查

現在你將要第一次運行 Lighthouse 測試。

1.  如果你還沒有這麼做過，請：在 Chrome 隱身模式下打開你的網站，這樣就沒有瀏覽器擴展幹擾測試。然後打開 Chrome 開發者工具。

2.  點擊 “Audits” 標籤，然後你會在屏幕上看到這樣的內容：

![開始 Lighthouse 審查](./lighthouse-audit.png)

3.  點擊 “Perform an audit...” （所有可用的審查類型應該默認被選中了）。然後點擊 “Run audit”（然後就會運行一個一分鐘左右的審查）。審查完成時，你應該能看到像這樣的結果：

![Lighthouse 審查結果](./lighthouse-audit-results.png)

如你所見，Gatsby 網站開箱即用，性能已經非常好了。但 PWA、可訪問性（Accessibility）、最佳實踐（Best Practices）和 SEO 等方面還有提高的空間，分數還能更高。在提高後你的網站將對訪問者和搜索引擎更加友好。

## 增加一個清單（manifest）文件

看起來你的網站在 “漸進式 Web 應用”（Progressive Web App）類別中的得分很低。讓我們來解決這個問題。

首先我們要搞清楚：什麼是 PWA？

它是一個利用現代瀏覽器功能，利用像 app 一樣的功能和好處，來豐富網絡體驗的常規網站。查看 [Google概述](https://developers.google.com/web/progressive-web-apps/) 中定義的 PWA 網站體驗。

包含 Web 應用清單文件是 [PWA 的三個公認基準要求](https://alistapart.com/article/yes-that-web-project-should-be-a-pwa#section1) 之一。

[Google](https://developers.google.com/web/fundamentals/web-app-manifest/) 提到：

> Web 應用的清單是一個簡單的 JSON 文件。它告訴瀏覽器 Web 應用的信息，以及當用戶 “安裝” Web 應用時它要如何呈現。

[Gatsby 的清單插件](/packages/gatsby-plugin-manifest/) 能為每一個 Gatsby 網站的構建配置一個 `manifest.webmanifest` 文件。

### ✋ 使用 `gatsby-plugin-manifest`

1.  安裝插件:

```shell
npm install --save gatsby-plugin-manifest
```

2. 添加一個網站圖標（favicon） `src/images/icon.png` 到你的應用裡。就本章教程所構建的應用而言，如果你手頭沒有可用的圖標，你可以使用 [這個示例圖標](https://raw.githubusercontent.com/gatsbyjs/gatsby/master/docs/tutorial/part-eight/icon.png)。這個圖標是為清單文件構建所有圖像所必需的。詳情請查這篇文檔 [`gatsby-plugin-manifest`](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-plugin-manifest/README.md)。

3. 在 `gatsby-config.js` 文件裡，把插件添加到 `plugins` 數組中。

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
      },
    },
  ]
}
```

這就是開始向 Gatsby 網站添加網絡清單所需的全部了。給出的示例包含基本的配置——請查看 [插件應用](/packages/gatsby-plugin-manifest/?=gatsby-plugin-manifest#automatic-mode) 來瞭解更多配置選項。

## 添加離線支持

網站要成為 PWA 的另一個要求是使用 [service workder](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)。Service worker 在後臺運行，根據應用與網絡的連通性來決定使用網絡還是內容緩存，從而實現無縫的離線體驗。

[Gatsby 的離線插件](/packages/gatsby-plugin-offline/) 使 Gatsby 站點能夠離線運行，並通過創建一個 service worker 使你的站點在糟糕網絡狀況下受到的影響更小。

### ✋ 使用 `gatsby-plugin-offline`

1.  安裝插件：

```shell
npm install --save gatsby-plugin-offline
```

2.  在 `gatsby-config.js` 文件裡，把插件添加到 `plugins` 數組中。

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
      },
    },
    // highlight-next-line
    `gatsby-plugin-offline`,
  ]
}
```

這就是在 Gatsby 中開始使用 service worker 所需的全部了。

> 💡 離線插件應該放在清單插件 _之後_。以便離線插件可以緩存清單插件創建的 `manifest.webmanifest`。

## 添加頁面元數據（metadata）

為頁面添加元數據（比如 title 和 description），是幫助 Google 之類的搜索引擎理解頁面內容，決定什麼時候出現在搜索結果裡的關鍵。

[React Helmet](https://github.com/nfl/react-helmet) 是一個提供了 React 組件接口的包，幫助你管理 [document head](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/head)。

Gatsby 的 [React Helmet 插件](/packages/gatsby-plugin-react-helmet/) 為 React Helmet 添加的服務端渲染的數據提供了直接支持。使用該插件，你添加到 React Helmet 的屬性將被添加到 Gatsby 構建的靜態 HTML 頁面中。

### ✋ 使用 `React Helmet` 和 `gatsby-plugin-react-helmet`

1.  安裝這兩個插件：

```shell
npm install --save gatsby-plugin-react-helmet react-helmet
```

2.  確保你的 `siteMetadata` 對象中配置好了 `description` 和 `author`。 並在 `gatsby-config.js` 文件裡添加 `gatsby-plugin-react-helmet` 插件到 `plugins` 數組中。

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
    // highlight-start
    description: `A simple description about pandas eating lots...`,
    author: `gatsbyjs`,
    // highlight-end
  },
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
      },
    },
    `gatsby-plugin-offline`,
    // highlight-next-line
    `gatsby-plugin-react-helmet`,
  ],
}
```

3. 在 `src/components` 目錄裡, 創建一個名為 `seo.js` 的文件並添加以下內容：

```jsx:title=src/components/seo.js
import React from "react"
import PropTypes from "prop-types"
import Helmet from "react-helmet"
import { useStaticQuery, graphql } from "gatsby"

function SEO({ description, lang, meta, title }) {
  const { site } = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
            description
            author
          }
        }
      }
    `
  )

  const metaDescription = description || site.siteMetadata.description

  return (
    <Helmet
      htmlAttributes={{
        lang,
      }}
      title={title}
      titleTemplate={`%s | ${site.siteMetadata.title}`}
      meta={[
        {
          name: `description`,
          content: metaDescription,
        },
        {
          property: `og:title`,
          content: title,
        },
        {
          property: `og:description`,
          content: metaDescription,
        },
        {
          property: `og:type`,
          content: `website`,
        },
        {
          name: `twitter:card`,
          content: `summary`,
        },
        {
          name: `twitter:creator`,
          content: site.siteMetadata.author,
        },
        {
          name: `twitter:title`,
          content: title,
        },
        {
          name: `twitter:description`,
          content: metaDescription,
        },
      ].concat(meta)}
    />
  )
}

SEO.defaultProps = {
  lang: `en`,
  meta: [],
  description: ``,
}

SEO.propTypes = {
  description: PropTypes.string,
  lang: PropTypes.string,
  meta: PropTypes.arrayOf(PropTypes.object),
  title: PropTypes.string.isRequired,
}

export default SEO
```

上面的代碼為你最常用的元數據標籤（tag）設置了默認值，並提供了一個 `<SEO>` 組件，可在項目的其餘部分中使用。很棒吧？

4.  現在你可以在模版和頁面中使用 `<SEO>` 組件，並把 props 傳進去。比如像這樣添加到 `blog-post.js` 模版裡：

```jsx:title=src/templates/blog-post.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"
// highlight-next-line
import SEO from "../components/seo"

export default ({ data }) => {
  const post = data.markdownRemark
  return (
    <Layout>
      // highlight-start
      <SEO title={post.frontmatter.title} description={post.excerpt} />
      // highlight-end
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
    </Layout>
  )
}

export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
      // highlight-next-line
      excerpt
    }
  }
`
```

上面的例子基於 [Gatsby Starter 博客](/starters/gatsbyjs/gatsby-starter-blog/)。通過向`<SEO>` 組件傳入 props，你可以實時更改博文的元數據。在這種情況下，將使用博客標題 `title` 和 `excerpt`（如果存在於博客帖子markdown文件中）代替 `gatsby-config.js` `文件中默認 `siteMetadata` 屬性。

現在如果你再運行 Lighthouse 審查，你應該能拿到近乎完美的 100 分！

> 💡 更多文檔和例子：[添加一個 SEO 組件](/docs/add-seo-component/)，以及 [React Helmet 文檔](https://github.com/nfl/react-helmet#example)。

## 不斷改善

在本章教程中，我們向你展示了一些 Gatsby 獨有的工具，這些工具可以改善網站的性能並讓網站準備好上線。

Lighthouse 是一個用於改進和學習網站的非常好的工具——繼續查看它提供的詳細反饋，並不斷改善你的網站！

## 接下來

### 官方文檔

- [官方文檔](https://www.gatsbyjs.org/docs/)：查看我們的官方文檔 _[快速開始](https://www.gatsbyjs.org/docs/quick-start/)_、_[詳細指導](https://www.gatsbyjs.org/docs/preparing-your-environment/)_、_[API 參考](https://www.gatsbyjs.org/docs/gatsby-link/)_、等等。

### 官方插件

- [官方插件](https://github.com/gatsbyjs/gatsby/tree/master/packages): 一個包含了所有 Gatsby 自己維護的官方插件的完整列表。

### 官方 Starters

1.  [Gatsby 的默認 Starter](https://github.com/gatsbyjs/gatsby-starter-default)：使用此默認樣板啟動你的項目。這個入門 Starter 包含了你可能需要的主要 Gatsby 配置文件。_[效果演示](http://gatsbyjs.github.io/gatsby-starter-default/)_
2.  [Gatsby 的博客 Starter](https://github.com/gatsbyjs/gatsby-starter-blog)：能創建一個又好又快的博客的 Gatsby starter。_[效果演示](http://gatsbyjs.github.io/gatsby-starter-blog/)_
3.  [Gatsby 的 Hello-World Starter](https://github.com/gatsbyjs/gatsby-starter-hello-world): 一個最基礎的能讓 Gatsby 網站運行的 Gatsby starter。_[效果演示](https://gatsby-starter-hello-world-demo.netlify.com/)_

## 咱們終於全整完了

呃……並不完全是。只是教程結束了。還有一些 [其他教程](/tutorial/additional-tutorials/)，它們也囊括了一些很有意義的用例。

這只是開始。讓我們繼續探索和使用了不起的 Gatsby！

- 你創建了一個很酷的網站？分享到 Twitter，添加話題 [#buildwithgatsby](https://twitter.com/search?q=%23buildwithgatsby)，並 [艾特我們](https://twitter.com/gatsbyjs)。
- 你為你所學的東西寫了一篇博客？也同樣分享出來吧！
- 你能為 Gatsby 貢獻一份力？逛逛我們 Gatsby 倉庫的 [open issues](https://github.com/gatsbyjs/gatsby/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22)，並 [成為一名貢獻者](/contributing/how-to-contribute/)。

看看 ["如何貢獻"](/contributing/how-to-contribute/) 文檔來獲得更多靈感。

我們迫不及待想看到你做了什麼 😄。
