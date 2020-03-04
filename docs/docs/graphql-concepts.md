---
title: GraphQL 概念
---

import LayerModel from "../../www/src/components/layer-model"

把數據加載到 React 組件裡有許多方式。[GraphQL](http://graphql.org/) 是一個非常受歡迎且強大的技術。

GraphQL 是 Facebook 發明的，它能幫助項目工程師 _提取_ 所需數據到組件裡。

GraphQL 是一種 **q**uery **l**anguage（查詢語言）（名字中 _QL_ 的由來）。如果你熟悉 SQL，它們的使用方式很像。通過一種特殊的語法，你就能描述你想要的組件中的數據，然後它就能把數據傳給你。

Gatsby 利用 GraphQL 來讓 [page 和 StaticQuery 組件](/docs/building-with-components/) 能夠聲明它們和它們的子組件所需的數據。然後，Gatsby 在瀏覽器裡讓這些數據隨時能被需要的組件使用。

數據從任何數量的數據源被查詢到了一個聚合層，這個聚合層是 Gatsby 構建過程中的一個重要組成部分：

<LayerModel initialLayer="Data" />

## 為什麼 GraphQL 這麼棒？

想深入瞭解更多，請閱讀 [為什麼 Gatsby 使用 GraphQL](/docs/why-gatsby-uses-graphql/)。

- 淘汰前端數據模板 — 不用擔心請求和等待數據。只需要在一個 Gatsby 查詢裡說明你想要的數據，數據就會在你需要的時候出現
- 把前端複雜的東西都放入查詢裡 — 你的 GraphQL 在 _構建時_ 就可以完成很多數據轉換
- 對於經常要依賴複雜/嵌套數據的現代應用，它是一種完美的數據查詢語言
- 通過防止數據過度膨脹來提升性能 — GraphQL 允許你只選擇需要的數據，無論 API 返回的是什麼

## 一個 GraphQL 查詢是什麼樣的？

GraphQL 允許你請求必要的數據。查詢格式看起來像 JSON：

```graphql
{
  site {
    siteMetadata {
      title
    }
  }
}
```

然後返回：

```json
{
  "site": {
    "siteMetadata": {
      "title": "A Gatsby site!"
    }
  }
}
```

一個帶有 GraphQL 查詢的基本頁面組件可以是這樣子的：

```jsx
import React from "react"
import { graphql } from "gatsby"

export default ({ data }) => (
  <div>
    <h1>About {data.site.siteMetadata.title}</h1>
    <p>We're a very cool website you should return to often.</p>
  </div>
)

export const query = graphql`
  query {
    site {
      siteMetadata {
        title
      }
    }
  }
`
```

查詢的結果會被自動放進 React 組件的 `data` prop 裡。GraphQL 和 Gatsby 能讓你請求數據然後立馬就能用上。

**注意：** 如果你要在一個非頁面組件裡運行 GraphQL 查詢，你需要使用 [Gatsby 的靜態查詢功能](/docs/static-query/)。

### 瞭解查詢的各個部分

下圖展示了在一個 GraphQL 查詢裡，每個單詞用顏色標出的圖例名稱：

![GraphQL 查詢圖解](./images/basic-query.png)

#### 查詢操作類型（Query operation type）

圖中標記了 `query` 為 “操作類型”。因為 Gatsby 唯一使用的操作類型是 `query`，所以這個操作類型是可以省略的（就像上面的例子一樣）。

#### 操作名稱（Operation name）

被標記為 “操作名稱” 的 `SiteInformation` 是一個你賦予給這個查詢的獨有名稱。這個名稱等同於你命名一個方程或者一個變量。如果你希望這個查詢是匿名的，它也可以像方程一樣被省略。

#### 查詢字段（Query fields）

`site`，`id`，`siteMetadata` 和 `title` 都被標記為 “字段”。任何上層字段 -- 如圖中的 `site` -- 有時候被稱為 **根級字段（root level fields）**。事實上，這個名字並沒有暗示任何功能上的特殊性，因為在 GraphQL 查詢裡所有的字段都在執行同樣的事請。

## 如何學習 GraphQL

可能用 Gatsby 開發是讓你第一次遇上了 GraphQL！我們希望你和我們一樣喜愛它，也希望你能發現它可以幫助你所有的項目。

我們推薦下面的兩個教程來學習 GraphQL：

- https://www.howtographql.com/
- http://graphql.org/learn/

[官方的 Gatsby 教程](/tutorial/part-four/) 也介紹瞭如何在 Gatsby 裡使用 GraphQL。

## GraphQL 和 Gatsby 如何一起工作？

Gatsby 的一大優點是靈活性。人們可以在 [很多不同的編程語言中](http://graphql.org/code/) 以及在網頁和原生應用中使用 GraphQL。

多數人在服務器裡運行 GraphQL 來響應客戶端的數據請求。你定義一個模式（schema，一個描述數據形狀的正規方法）給你的 GraphQL 服務器，然後你的 GraphQL 解析器會向一些數據庫和/或者其它 API 檢索數據。

Gatsby 是在 _構建時_ 使用 GraphQL _而不是_ 在運行中的網站使用。這種方式是獨一無二的，而且你不需要再在 production 模式的網站裡運行額外的服務（例如一個數據庫和 node.js 服務）來使用 GraphQL。

用 Gatsby 這個框架來開發應用是非常實用的，所以結合 Gatsby 的原生構建 GraphQL 和 GraphQL 查詢一起比在瀏覽器裡運行 GraphQL 服務器更好。

## Gatsby 的 GraphQL 模式是怎麼來的？

GraphQL 的常見用法需要人工創建一個 GraphQL 模式。

Gatsby 運用了可以從不同數據源提取數據的插件。數據會自動 _推斷_ 出一個 GraphQL 模式。

如果你的 Gatsby 數據是這樣的：

```json
{
  "title": "A long long time ago"
}
```

Gatsby 會生成如下模式：

```
title: String
```

這樣從任何地方提取數據都會變得很容易，而且你可以馬上開始編寫 GraphQL 查詢。

有時候一些數據源會允許你定義一個數據還沒有被添加的模式，這樣是 _會_ 引起混亂的，但 Gatsby 可能會跳過生成那部分的模式。

## 強大的數據轉換

GraphQL 也為 Gatsby 帶來了其它獨一無二的特色 — 它允許你在查詢裡用參數控制數據轉換。請看下面的例子。

### 格式化日期

人們通常喜歡保存像 “2018-01-05” 的日期格式，但希望用其它格式，例如 “January 5th, 2018”，來顯示。一種方法是加載一個格式化日期的 JavaScript 庫到瀏覽器裡。或者，利用 Gatsby 的 GraphQL 層，你就可以在查詢時就做格式化，就像這樣：

```graphql
{
  date(formatString: "MMMM Do, YYYY")
}
```

請在我們的 [GraphQL 參考頁](/docs/graphql-reference/#dates) 裡查看全部的格式化選項。

### Markdown

Gatsby 有能把數據從一個格式變成另一個格式的 _數據轉換插件_。一個常見的例子是 markdown。如果你安裝了 [`gatsby-transformer-remark`](/packages/gatsby-transformer-remark/)， 那麼在你的查詢裡，你可以指定你想要的是經轉換的 HTML 版本而不是 markdown：

```graphql
markdownRemark {
  html
}
```

### 圖片

Gatsby 對於圖片處理有豐富的支持。響應式圖片是現代網站的一大部分，每張圖片通常需要創建 5 種以上不同大小的縮略圖。利用 Gatsby 的 [`gatsby-transformer-sharp`](/packages/gatsby-transformer-sharp/)，你可以 _查詢_ 圖片的響應式版本。你的查詢會自動生成全部所需的響應式縮略圖，然後返回 `src` 和 `srcSet` 字段來讓你添加你的圖片元素。

結合 [gatsby-image](/packages/gatsby-image/) 這個特別的 Gatsby 圖片組件，你在建立網站時就擁有了一個非常強大的圖片處理基礎。

這是一個用了 `gatsby-image` 的組件：

```jsx
import React from "react"
import Img from "gatsby-image"
import { graphql } from "gatsby"

export default ({ data }) => (
  <div>
    <h1>Hello gatsby-image</h1>
    <Img fixed={data.file.childImageSharp.fixed} />
  </div>
)

export const query = graphql`
  query {
    file(relativePath: { eq: "blog/avatars/kyle-mathews.jpeg" }) {
      childImageSharp {
        # 在查詢裡就可以指定圖片處理的參數。
        # 這樣即使你的頁面有設計變化也不會對圖片本身造成影響
        fixed(width: 125, height: 125) {
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`
```

你也可以查看下面的博文：

- [讓建立網站變得有趣](/blog/2017-10-16-making-website-building-fun/)
- [Gatsby.js 讓優化圖片變得容易](https://medium.com/@kyle.robert.gill/ridiculously-easy-image-optimization-with-gatsby-js-59d48e15db6e)

## 高級

### 片段

在上面的 [圖片查詢](#images) 例子裡，我們用了一個 Gatsby 片段 `...GatsbyImageSharpFixed`，它是一個可以重複使用的字段組合。你可以在 [這裡](http://graphql.org/learn/queries/#fragments) 瞭解更多。

如果你希望自定義片段，你可以用一些指定的 export 來把它們導出到任何 JavaScript 文件裡，然後 Gatsby 會自動在你的 GraphQL 查詢裡處理它們。

例如，如果我在一個 helper 組件裡放一個片段，那麼我就可以在任何其它查詢裡使用這個片段：

```jsx:title=src/components/PostItem.js
export const markdownFrontmatterFragment = graphql`
  fragment MarkdownFrontmatter on MarkdownRemark {
    frontmatter {
      path
      title
      date(formatString: "MMMM DD, YYYY")
    }
  }
`
```

然後它們能在任何 GraphQL 查詢裡被使用！

```graphql
query($path: String!) {
  markdownRemark(frontmatter: { path: { eq: $path } }) {
    ...MarkdownFrontmatter
  }
}
```

你的 helper 組件應當定義和導出一個數據所需的片段。例如，在你的 index 頁面裡，你可能想遍歷所有的文章然後顯示在一個列表裡。

```jsx:title=src/pages/index.jsx
import React from "react"
import { graphql } from "gatsby"

export default ({ data }) => {
  return (
    <div>
      <h1>Index page</h1>
      <h4>{data.allMarkdownRemark.totalCount} Posts</h4>
      {data.allMarkdownRemark.edges.map(({ node }) => (
        <div key={node.id}>
          <h3>
            {node.frontmatter.title} <span>— {node.frontmatter.date}</span>
          </h3>
        </div>
      ))}
    </div>
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
        }
      }
    }
  }
`
```

如果 index 組件變得非常大，你可能需要把它重構成一些小組件。

```jsx:title=src/components/IndexPost.jsx
import React from "react"
import { graphql } from "gatsby"

export default ({ frontmatter: { title, date } }) => (
  <div>
    <h3>
      {title} <span>— {date}</span>
    </h3>
  </div>
)

export const query = graphql`
  fragment IndexPostFragment on MarkdownRemark {
    frontmatter {
      title
      date(formatString: "MMMM DD, YYYY")
    }
  }
`
```

現在，你可以在 index 頁面裡與導出的片段一起使用。

```jsx:title=src/pages/index.jsx
import React from "react"
import IndexPost from "../components/IndexPost"
import { graphql } from "gatsby"

export default ({ data }) => {
  return (
    <div>
      <h1>Index page</h1>
      <h4>{data.allMarkdownRemark.totalCount} Posts</h4>
      {data.allMarkdownRemark.edges.map(({ node }) => (
        <div key={node.id}>
          <IndexPost frontmatter={node.frontmatter} />
        </div>
      ))}
    </div>
  )
}

export const query = graphql`
  query {
    allMarkdownRemark {
      totalCount
      edges {
        node {
          ...IndexPostFragment
        }
      }
    }
  }
`
```

## 閱讀更多

- [為什麼 Gatsby 使用 GraphQL](/docs/why-gatsby-uses-graphql/)
- [GraphQL 查詢分析](https://blog.apollographql.com/the-anatomy-of-a-graphql-query-6dffa9e9e747)

### 上手 GraphQL

- http://graphql.org/learn/
- https://www.howtographql.com/
- https://reactjs.org/blog/2015/05/01/graphql-introduction.html
- https://services.github.com/on-demand/graphql/

### GraphQL 高級閱讀

- [GraphQL 規範](https://facebook.github.io/graphql/October2016/)
- [接口和聯合](https://medium.com/the-graphqlhub/graphql-tour-interfaces-and-unions-7dd5be35de0d)
- [Relay 編譯器（Gatsby 用它來處理查詢）](https://facebook.github.io/relay/docs/en/compiler-architecture.html)
