---
title: 將資源直接引入到文件裡
---

把圖片，字體和文件等資源引入到一個 Gatsby 網站主要有兩種方法。常用方法是直接把文件引入到一個 Gatsby 模板，頁面或者組件中。另一種方法是利用 [static 文件夾](/docs/static-folder)，這種方法對於一些邊緣案例是有幫助的。

## 通過 Webpack 引入資源

通過 Webpack，你可以 **把一個文件 `import` 成一個 JavaScript 模塊**。這樣 Webpack 最終就會把那個文件組合到 bundle 裡。與 CSS 的引入不同的是，引入一個文件會給你一個 string 值。這個值其實是一個你可以在代碼中引用的最終路徑，類似於一張圖片的 `src` 屬性或者一條 PDF 連接的 `href` 屬性。

為了降低請求服務器的次數，引入小於 10,000 字節大小的圖片會返回一條
[data URI](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs) 而不是一條路徑。這種情況適用於以下文件後綴：`svg`，`jpg`，`jpeg`，`png`，`gif`，`mp4`，`webm`，`wav`，`mp3`，`m4a`，`aac` 和 `oga`。

請看下面的例子：

```js
import React from "react"
import logo from "./logo.png" // 這裡告訴 Webpack 這個 JS 文件用了這張圖片

console.log(logo) // /logo.84287d09.png

function Header() {
  // 引入得到的是你的圖片 URL
  return <img src={logo} alt="Logo" />
}

export default Header
```

當 Webpack 打包的時候，Webpack 會準確地把這些圖片放入 public 文件夾裡，然後幫我們提供正確的路徑。

你甚至可以在 CSS 裡引用文件來引入它們：

```css
.Logo {
  background-image: url(./logo.png);
}
```

Webpack 會在 CSS 裡查找所有的相對模塊引用（它們都是以 `./` 開頭），然後把它們替換成在已經編譯好的 bundle 裡的最終路徑。如果你拼寫錯誤或者無意地刪除了一個重要的文件，你會看到一個類似於引入不存在的 JavaScript 模塊的編譯錯誤。在 Webpack 已編譯好的 bundle 裡，這些最終文件名是由 Webpack 通過 content hashes 生成的。如果日後文件的內容有改動，Webpack 會在 production 模式裡重命名文件。所以這樣你不需要擔心資源的持久化緩存。

如果你用的是 SCSS，引入的是一個入口 SCSS 文件。

但請注意，這事實上是一個可以自行定製的 Webpack 功能。

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-import-a-local-image-into-a-gatsby-component-with-webpack"
  lessonTitle="Import a Local Image into a Gatsby Component with webpack"
/>

### 更多資料

- 更多關於 [使用引入的字體](https://www.gatsbyjs.org/docs/recipes/#adding-a-local-font)。

## 通過 gatsby-source-filesystem 用 GraphQL 查詢 `File`

你也可以在 GraphQL 的數據層裡查詢來引入文件，GraphQL 會複製那些文件到 public 目錄裡。通過查詢 `File` 節點（node）的 `publicURL` 字段（field），你可以獲得能用在 Javascript 組件，頁面和模板的 URLs。

### 例子

1. 複製你所有在數據層裡的 `.pdf` 文件到你的 build 目錄然後返回它們的 URLs：

```graphql
{
  allFile(filter: { extension: { eq: "pdf" } }) {
    edges {
      node {
        publicURL
      }
    }
  }
}
```

在頁面裡用 useStaticQuery 來引用這些 PDF 文件：

```jsx
import React from "react"
import { useStaticQuery, graphql } from "gatsby"

import Layout from "../components/layout"

const DownloadsPage = () => {
  const data = useStaticQuery(graphql`
    {
      allFile(filter: { extension: { eq: "pdf" } }) {
        edges {
          node {
            publicURL
            name
          }
        }
      }
    }
  `)
  return (
    <Layout>
      <h1>All PDF Downloads</h1>
      <ul>
        {data.allFile.edges.map((file, index) => {
          return (
            <li key={`pdf-${index}`}>
              <a href={file.node.publicURL} download>
                {file.node.name}
              </a>
            </li>
          )
        })}
      </ul>
    </Layout>
  )
}
export default DownloadsPage
```

2. 在 Markdown 的頭（frontmatter）處連接附件：

```markdown
---
title: "Title of article"
attachments:
  - "./assets.zip"
  - "./presentation.pdf"
---

Hi, this is a great article.
```

接著在一個文章模板組件裡，你可以查詢那些附件：

```graphql
query($slug: String!) {
  markdownRemark(fields: { slug: { eq: $slug } }) {
    html
    frontmatter {
      title
      attachments {
        publicURL
      }
    }
  }
}
```

[`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/) 文件可以閱讀到更多的相關信息。
