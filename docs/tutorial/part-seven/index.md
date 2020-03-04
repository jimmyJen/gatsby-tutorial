---
title: 以編程的方式利用數據創建頁面
typora-copy-images-to: ./
disableTableOfContents: true
---

> 本章教程是 Gatsby 數據層系列的一部分。在繼續之前，確保你已經讀過一遍 [第 4 章](/tutorial/part-four/)、[第 5 章](/tutorial/part-five/) 和 [第 6 章](/tutorial/part-six/) 教程。

## 這章教程裡有什麼?

在前一章教程中，你創建了一個漂亮的索引頁面。這個頁面能夠展示出一個博文標題和摘要列表。但你不只是想要看到摘要，你想看到你的 Markdown 文件的真正博文頁面。

你可以通過將 React 組件放置在 `src/pages` 目錄中來繼續創建頁面。但是接下來，你將學習如何通過 _以編程的方式_ 利用 _數據_ 創建頁面。Gatsby _並不_ 僅限於使用文件（例如許多靜態網站生成器）製作頁面。Gatsby 同樣可以在構建時使用 GraphQL 查詢 _數據_ 並將查詢結果 _映射_ 到 _頁面_ 中。這是一個非常好的想法。你將探索這個想法的具體含義和使用方法。

讓我們開始吧。

## 為頁面創建 slug

創建一個新頁面，一共有兩步:

1.  生成頁面的 “路徑”（path）或 slug。
2.  創建頁面。

_**注意**: 通常，數據源會直接為內容提供一個 slug 或路徑名——使用這些系統之一（例如CMS）時，你不需要像創建 Markdown 文件那樣自己創建 slug。_

為了創建你的 Markdown 頁面，你需要學習如何使用這兩個 Gatsby API：[`onCreateNode`](/docs/node-apis/#onCreateNode) 和 [`createPages`](/docs/node-apis/#createPages)。你將在許多站點和插件中看到這兩個主要的API

我們已經盡力使 Gatsby API 易於實現。要實現 API，請在 `gatsby-node.js` 文件中導出（export）以 API 為名稱的函數。

所以你要這麼做：在站點的根目錄下，創建一個名為 `gatsby-node.js` 的文件並添加以下代碼。

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  console.log(node.internal.type)
}
```

每當創建新節點（或更新）時，Gatsby 都會調用 `onCreateNode` 函數。

停止並重啟開發服務器，你會看到終端控制檯顯示出條很多新創建的節點記錄。

在下一部分中，你將會使用這個 API 來把 slug 添加到你的 Markdown頁面。 

修改你的函數使其僅僅記錄 `MarkdownRemark` 節點。

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  // highlight-start
  if (node.internal.type === `MarkdownRemark`) {
    console.log(node.internal.type)
  }
  // highlight-end
}
```

你需要使用每一個 Markdown 文件的名稱來創建頁面 slug。`pandas-and-bananas.md` 就變成 `/pandas-and-bananas/`。但要如何從 `MarkdownRemark` 節點中獲取文件名稱呢？你需要 _遍歷_ 一遍它的 _父節點_ `File` 的 “節點圖”。因為 `File` 節點包含了磁盤中你所需要的文件數據。再次修改函數來實現它：

```javascript:title=gatsby-node.js
// highlight-next-line
exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    // highlight-start
    const fileNode = getNode(node.parent)
    console.log(`\n`, fileNode.relativePath)
    // highlight-end
  }
}
```

在重啟開發服務器之後，你應該能在終端看到你的兩個 Markdown 文件的相對路徑。

![Markdown 相對路徑](markdown-relative-path.png)

現在你需要創建 slug。由於通過文件名創建 slug 的邏輯可能會很棘手，因此 `gatsby-source-filesystem` 插件附帶了創建 slug 的功能。讓我們來使用它。

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`) // highlight-line

exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(createFilePath({ node, getNode, basePath: `pages` })) // highlight-line
  }
}
```

這個函數負責查找父“文件”節點以及創建 slug。再次運行開發服務器，你應該看到終端記錄了兩個 slug，每個 Markdown 文件各一個。

現在，你可以將新的 slug 直接添加到 `MarkdownRemark` 節點裡。這非常有用，因為你添加到節點的任何數據都可以在以後使用 GraphQL 查詢到。因此創建頁面時很容易得到 slug。

為此，你將向 API 的實現傳遞一個函數，該函數稱為 [`createNodeField`](/docs/actions/#createNodeField)。此功能允許你在其他插件創建的節點裡創建其他字段。只有節點的原始創建者才能直接修改該節點——所有其他插件（包括你的`gatsby-node.js`）都必須使用此函數來創建額外字段。

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`)
// highlight-next-line
exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions // highlight-line
  if (node.internal.type === `MarkdownRemark`) {
    // highlight-start
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
    // highlight-end
  }
}
```

重啟開發服務器，打開或刷新 GraphiQL。然後運行這條 GraphQL 查詢語句來查看新的 slug。

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        fields {
          slug
        }
      }
    }
  }
}
```

現在 slug 已經創建好了。你可以創建頁面了。

## 創建頁面

在同一個文件  `gatsby-node.js` 中，添加以下代碼。

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

// highlight-start
exports.createPages = async ({ graphql, actions }) => {
  // **Note:** The graphql function call returns a Promise
  // see: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise for more info
  const result = await graphql(`
    query {
      allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  console.log(JSON.stringify(result, null, 4))
}
// highlight-end
```

你添加了 [`createPages`](/docs/node-apis/#createPages) 這個 API 的實現。Gatsby 會調用它，從而使得插件能添加頁面。

如本章教程的導語所述，以編程的方式創建頁面的步驟是：

1.  使用 GraphQL 查詢數據
2.  把查詢結果映射到頁面上

上面的代碼是從 Markdown 創建頁面的第一步。你使用了我們提供的 `graphql` 函數查詢你創建的 Markdown slug。然後你會在終端裡看到如下查詢結果：

![查詢 Markdown slugs](query-markdown-slugs.png)

創建頁面還需要一樣東西：一個頁面模板組件。與 Gatsby 中所有其他東西一樣，用編程生成的頁面也是依靠 React 組件的。創建頁面時，需要指定所使用的組件。

新建一個目錄位於 `src/templates`，然後添加以下代碼到一個新文件 `src/templates/blog-post.js` 中。

```jsx:title=src/templates/blog-post.js
import React from "react"
import Layout from "../components/layout"

export default () => {
  return (
    <Layout>
      <div>Hello blog post</div>
    </Layout>
  )
}
```

然後更新我們的 `gatsby-node.js`：

```javascript:title=gatsby-node.js
const path = require(`path`) // highlight-line
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions // highlight-line
  const result = await graphql(`
    query {
      allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  // highlight-start
  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: path.resolve(`./src/templates/blog-post.js`),
      context: {
        // Data passed to context is available
        // in page queries as GraphQL variables.
        slug: node.fields.slug,
      },
    })
  })
  // highlight-end
}
```

重啟開發服務器後，你會看到頁面已經創建好了！在開發過程中，找到新創建頁面的一種簡單方法是：隨便轉到一個不存在的路徑，在該路徑中 Gatsby 會幫你顯示出站點的頁面列表。比如轉到 <http://localhost:8000/sdf>，就會看到你創建的新頁面。

![新頁面](new-pages.png)

訪問其中一個頁面，你將會看到：

![Hello World 博文](hello-world-blog-post.png)

看起來有點單調無聊，而且並不是你想要看到的。現在你可以從 Markdown 博文中提取數據。把 `src/templates/blog-post.js` 的文件內容替換為：

```jsx:title=src/templates/blog-post.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-start
export default ({ data }) => {
  const post = data.markdownRemark
  // highlight-end
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

// highlight-start
export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
    }
  }
`
// highlight-end
```

見證奇蹟的時刻……

![博文](blog-post.png)

哇哦!

最後一步就是把你的新頁面連接到索引頁。

回到 `src/pages/index.js` 文件中，查詢你的 Markdown slugs，並且創建鏈接。

```jsx:title=src/pages/index.js
import React from "react"
import { css } from "@emotion/core"
import { Link, graphql } from "gatsby" // highlight-line
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default ({ data }) => {
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
            {/* highlight-start */}
            <Link
              to={node.fields.slug}
              css={css`
                text-decoration: none;
                color: inherit;
              `}
            >
              {/* highlight-end */}
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
            </Link> {/* highlight-line */}
          </div>
        ))}
      </div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC }) {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          // highlight-start
          fields {
            slug
          }
          // highlight-end
          excerpt
        }
      }
    }
  }
`
```

走起！一個麻雀雖小但五臟俱全的博客。

## 一個小挑戰

好好把玩一下這個網站。添加更多 Markdown 文件，從 `MarkdownRemark` 節點中查詢其他數據然後添加到主頁或博文頁面中。

在這一系列教程中，你學習到了構建 Gatsby 數據層的基礎知識。你學會了如何使用插件從 _數據源_ 中獲取數據並 _轉換_ 數據，如何使用 GraphQL 將數據 _映射_ 到頁面裡，以及如何構建查詢每個頁面數據的 _頁面模板組件_。

## 下一步

現在你已經構建好了一個 Gatsby 站點。那下面還能做什麼呢？

- 把你的 Gatsby 站點分享到 Twitter。搜索 #gatsbytutorial 看看別人創建的網站是什麼樣的！確保你在推文裡 @gatsbyjs 並添加了話題 #gatsbytutorial :)
- 你可以看一些 [示例網站](https://github.com/gatsbyjs/gatsby/tree/master/examples#gatsby-example-websites)
- 探索更多 [插件](/docs/plugins/)
- 看看 [別人是怎麼把 Gatsby 玩出花的](/showcase/)
- 閱讀文檔 [Gatsby's APIs](/docs/api-specification/)，[節點](/docs/node-interface/) 或是 [GraphQL](/docs/graphql-reference/)
