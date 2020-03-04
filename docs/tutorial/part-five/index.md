---
title: 數據源插件
typora-copy-images-to: ./
disableTableOfContents: true
---

> 本章教程是 Gatsby 數據層系列的一部分。在繼續之前，確保你已經讀過一遍 [第 4 章](/tutorial/part-four/) 教程。

## 這章教程裡有什麼?

在本章教程中，你將學習如何使用 GraphQL 和數據源插件將數據提取到 Gatsby 站點中。但是在學習這些插件之前，你需要了解如何使用一個叫做 GraphiQL 的工具，該工具可幫助你正確構建查詢語句。

## 介紹 GraphiQL

GraphiQL 是 GraphQL 的集成開發環境（IDE）。它功能強大，且各方面都很棒，在你構建 Gatsby 網站時會經常使用。

你可以在站點的開發服務器正在運行的時候訪問它——地址通常是：
<http://localhost:8000/___graphql>。

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="/graphiql-explore.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

檢查一下內置的 `Site` "類型"，看看裡面有哪些字段可用——它包括了你先前查詢過的 `siteMetadata` 對象。嘗試打開 GraphiQL 並玩玩看這些數據！按下 <kbd>Ctrl + Space</kbd>（或 <kbd>Shift + Space</kbd>）來調出自動完成窗口，然後按下 <kbd>Ctrl + Enter</kbd> 來運行 GraphQL 的查詢。在本教程的其餘部分中，你將使用到 GraphiQL 的更多功能。

## 使用 GraphiQL Explorer

GraphiQL Explorer 使你可以通過點擊可用的字段和在輸入框裡輸入的方式，來交互式地構建完整的查詢，而無需重複手工輸入的過程來完成查詢。

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-build-a-graphql-query-using-gatsby-s-graphiql-explorer"
  lessonTitle="Build a GraphQL Query using Gatsby’s GraphiQL Explorer"
/>

## 數據源插件

Gatsby 網站中的數據可以來自任何地方：API、數據庫、CMS、本地文件等等。

數據源插件可以從其數據源中獲取數據。例如文件系統源插件知道如何從文件系統中獲取數據。 WordPress 插件知道如何從 WordPress API 獲取數據。

添加 [`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/) 這個插件並探索一下它是如何工作的。

首先，將插件安裝在項目的根目錄：

```shell
npm install --save gatsby-source-filesystem
```

然後把這些代碼添加到 `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    // highlight-start
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    // highlight-end
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

保存這個文件然後重啟 Gatsby 開發服務器。然後重新打開 GraphiQL。

在 explorer 面板裡，你會看見 `allFile` 和 `file` 這兩個可選項：

![GraphiQL 文件系統](graphiql-filesystem.png)

點擊 `allFile` 下拉按鈕，在 query 輸入框中，把光標放在 `allFile` 之後，然後按下 <kbd>Ctrl + Enter</kbd>。它會為你自動填好查詢所有文件的語句。按下 “Play” 按鈕來運行查詢語句：

![文件系統查詢](filesystem-query.png)

在 explorer 面板中，`id` 字段已經被自動選擇了。通過選中字段的相應複選框來選擇更多字段。按下 “Play” 按鈕以使用新字段再次運行查詢：

![文件系統的 explorer 選項](filesystem-explorer-options.png)

或者，你可以使用自動完成的快捷鍵（<kbd>Ctrl + Space</kbd>）。這將在 `File` 節點上顯示可查詢的字段。

![文件系統的自動完成](filesystem-autocomplete.png)

嘗試在查詢中添加許多字段，每次要重新運行查詢的時候按下 <kbd>Ctrl + Enter</kbd>。你會看到更新後的查詢結果：

![所有文件的查詢](allfile-query.png)

結果是一個 `File` “節點” 的數組（節點（node）是圖（graph）中一個對象的特殊稱呼）。每個 `File` 節點對象都包含了你要查詢的字段。

## 通過 GraphQL 查詢語句來構建一個頁面

用 Gastby 構建新頁面的過程通常開始於 GraphiQL。你先在 GraphiQL 測試出數據查詢語句，然後把 query 輸入框裡的語句複製到你的 React 頁面組件裡，接下來才開始構建 UI。

讓我們試試看這個。

用你剛剛創建好的 `allFile` GraphQL 查詢語句來創建一個新文件 `src/pages/my-files.js`

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data) // highlight-line
  return (
    <Layout>
      <div>Hello world</div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

`console.log(data)` 這一行在上面高亮顯示出來了。當要創建一個新組件時，在控制檯打印出從 GraphQL 查詢到的數據通常會很有幫助。你在構建 UI 時就可以在瀏覽器控制檯中瀏覽數據。

如果你訪問了一個新頁面 `/my-files/` 並打開瀏覽器控制檯，你會看到類似於：

![控制檯中顯示的數據](data-in-console.png)

圖片裡的數據和 GraphQL 查詢語句是相匹配的。

讓我們繼續添加一些代碼來顯示出文件數據。

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>My Site's Files</h1>
        <table>
          <thead>
            <tr>
              <th>relativePath</th>
              <th>prettySize</th>
              <th>extension</th>
              <th>birthTime</th>
            </tr>
          </thead>
          <tbody>
            {data.allFile.edges.map(({ node }, index) => (
              <tr key={index}>
                <td>{node.relativePath}</td>
                <td>{node.prettySize}</td>
                <td>{node.extension}</td>
                <td>{node.birthTime}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

然後訪問 [http://localhost:8000/my-files](http://localhost:8000/my-files)… 😲

![我的文件頁面](my-files-page.png)

## 接下來？

現在，你已經瞭解了數據源插件如何將數據 _引入_ Gatsby 的數據系統。在下一章教程中，你將學習數據轉換插件如何 _轉換_ 數據源插件給出的原始數據。通過數據源插件和數據轉換插件兩者的協同合作，所有在構建 Gatsby 網站時可能需要的數據源和數據轉換都能夠處理了。讓我們在  [第 6 章教程](/tutorial/part-six/) 中瞭解有關數據轉換插件的信息。