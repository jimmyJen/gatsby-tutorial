---
title: 數據轉換插件
typora-copy-images-to: ./
disableTableOfContents: true
---

> 本章教程是 Gatsby 數據層系列的一部分。在繼續之前，確保你已經讀過一遍 [第 4 章](/tutorial/part-four/) 和 [第 5 章](/tutorial/part-five/) 教程。

## 這章教程裡有什麼?

上一章教程演示了數據源插件是如何將數據 _導入_ 到數據系統中。在這章教程中，你將學習到數據轉換插件如何 _轉換_ 數據源插件給出的原始數據。搭配使用數據源插件和數據轉換插件，所有構建 Gatsby 網站時可能需要的數據源和數據轉換都可以被處理。

## 數據轉換插件

通常，從數據源插件獲取的數據格式，和你要用來構建網站的格式並不一致。文件系統插件使你可以查詢 _關於_ 文件的數據，但是如果要查詢文件 _裡面_ 的數據怎麼辦？

為了實現這一點，Gatsby 通過數據轉換插件提供支持，它從數據源插件中獲取原始內容，並將其內容 _轉換_ 成更有用的內容。

就比如 Markdown 文件：Markdown 的寫作體驗非常棒。但當你需要把它構建成一個網頁的時候，你需要把 Markdown 轉換為 HTML 格式。

添加一個 Markdown 文件，位於 `src/pages/sweet-pandas-eating-sweets.md`（它會成為你的第一篇 Markdown 博客）。你將會學習到如何使用數據轉換插件和 GraphQL 來把它 _轉換_ 成 HTML 的形式。

```markdown:title=src/pages/sweet-pandas-eating-sweets.md
---
title: "Sweet Pandas Eating Sweets"
date: "2017-08-10"
---

Pandas are really sweet.

Here's a video of a panda eating sweets.

<iframe width="560" height="315" src="https://www.youtube.com/embed/4n0xNbfJLR8" frameborder="0" allowfullscreen></iframe>
```

保存文件以後，再次訪問頁面 `/my-files/` ——新的 Markdown 文件便在表格裡了。這是 Gatsby 一個非常強大的功能。和之前的例子 `siteMetadata` 一樣，數據源插件可以實時重載數據。`gatsby-source-filesystem`  一直在掃描新添加的文件，如果掃描到了，就會重新運行查詢。

添加一個可以轉換 Markdown 文件的數據轉換插件：

```shell
npm install --save gatsby-transformer-remark
```

然後像往常一樣把這些代碼添加到 `gatsby-config.js`：

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    `gatsby-transformer-remark`, // highlight-line
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

重啟開發服務器，然後刷新（或者重新打開） GraphiQL，看一看自動完成列表的內容：

![Makrdown 自動完成](markdown-autocomplete.png)

再次選擇 `allMarkdownRemark` 並且運行，和之前對 `allFile` 的操作一樣。你會看到新添加的 Markdown 文件。然後再探索一下 `MarkdownRemark` 這個節點的字段。

![Markdown 查詢](markdown-query.png)

很好！希望你已經掌握了一些基礎知識。數據源插件把數據 _導入_ 到 Gatsby 的數據系統，然後 _數據轉換_ 插件轉換數據源插件導入的原始內容。這個模式能夠處理構建 Gatsby 網站時可能需要的所有數據源和數據轉換。

## 在 `src/pages/index.js` 創建一個站點中所有 Markdown 文件的列表

現在，你必須在首頁上創建 Makrdown 文件列表。像許多博客一樣，你最終需要在首頁上展示一個指向每個博客文章的鏈接列表。使用 GraphQL，你可以 _查詢_ 當前 Markdown 博文的列表，因此無需手動維護列表。

和我們對 `src/pages/my-files.js` 頁面所做的一樣，把 `src/pages/index.js` 替換為以下代碼，來添加一個 GraphQL 查詢語句、一些初步的 HTML 和樣式。

```jsx:title=src/pages/index.js
import React from "react"
import { graphql } from "gatsby"
import { css } from "@emotion/core"
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
  return (
    <Layout>
      <div>
        <h1
          css={css`
            display: inline-block;
            border-bottom: 1px solid;
          `}
        >
          Amazing Pandas Eating Things
        </h1>
        <h4>{data.allMarkdownRemark.totalCount} Posts</h4>
        {data.allMarkdownRemark.edges.map(({ node }) => (
          <div key={node.id}>
            <h3
              css={css`
                margin-bottom: ${rhythm(1 / 4)};
              `}
            >
              {node.frontmatter.title}{" "}
              <span
                css={css`
                  color: #bbb;
                `}
              >
                — {node.frontmatter.date}
              </span>
            </h3>
            <p>{node.excerpt}</p>
          </div>
        ))}
      </div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allMarkdownRemark {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          excerpt
        }
      }
    }
  }
`
```

現在首頁應該是這樣:

![首頁](frontpage.png)

但是隻有一篇博文，形單影隻。所以讓我們在這個地址添加另一篇 `src/pages/pandas-and-bananas.md` 。

```markdown:title=src/pages/pandas-and-bananas.md
---
title: "Pandas and Bananas"
date: "2017-08-21"
---

Do Pandas eat bananas? Check out this short video that shows that yes! pandas do
seem to really enjoy bananas!

<iframe width="560" height="315" src="https://www.youtube.com/embed/4SZl1r2O_bY" frameborder="0" allowfullscreen></iframe>
```

![兩篇博文](two-posts.png)

看起來不錯！除了……博文順序不對。

但是修復起來很簡單。查詢某種類型的連接時，你可以傳入各種參數到 GraphQL 查詢語句中。你可以 `sort`（排序）和 `filter`（過濾）節點，設置要 `skip`（跳過）多少節點，並選擇獲得的節點的 `limit` （數量限制）。通過這些強大的操作符，你可以選擇任意你想要的數據——以你需要的形式。

在你的索引（index）頁面的 GraphQL 查詢語句中，把 `allMarkdownRemark` 替換為 `allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC })`。_注意：在 `frontmatter` 和 `date`之間有三個下劃線符號。_ 保存文件後，順序問題應該修復了。

打開 GraphiQL 然後試試看不同的排序選項。你可以對 `allFile` 和其他各種連接進行排序。

要了解更多查詢語句的操作符，請查看我們的這篇 [GraphQL 查詢選項參考指南.](/docs/graphql-reference/)。

## 一個小挑戰

嘗試再添加一個新博文頁面，看一看主頁的博文列表發生了什麼變化！

## 下一步

你做的很棒！你創建了一個漂亮的索引頁，在這個頁面裡你可以查詢你的 Markdown 文件，並生成一個博文標題與摘要的列表。但你不只是想要看到摘要，你想看到你的 Markdown 文件的真正博文頁面。

你可以通過將 React 組件放置在 `src/pages` 目錄中來繼續創建頁面。但是接下來，你將學習如何通過 _以編程的方式_ 利用 _數據_ 創建頁面。Gatsby _並不_ 僅限於使用文件（例如許多靜態網站生成器）製作頁面。Gatsby 同樣可以在構建時使用 GraphQL 查詢 _數據_ 並將查詢結果 _映射_ 到 _頁面_ 中。這是一個非常好的想法。在下一章教程中，你將探索這個想法的具體含義和使用方法，並且學習如何 [以編程的方式利用數據創建頁面](/tutorial/part-seven/)。