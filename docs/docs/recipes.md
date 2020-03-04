---
title: 手冊
tableOfContentsDepth: 2
---

<!-- 創建一個 Gatsby 配方的基本模板:

## 要完成的任務。
用一到兩句話描述。越簡潔越聚焦於主題越好！

### 前置條件
- 系統或版本要求
- 開始任務所需要的一切
- 包括在其它網站上設置賬號，比如 Netlify
- 查看 [文檔模板](/docs/docs-templates/) 以瞭解格式規範

### 操作步驟
一步一步地指導。每個步驟都應該是可重複的並且切中要點。對完成任務來說任何不重要的事物都應省略。

#### 在線演示 (可選)
由於配方的性質，可能無法提供在線演示。這種情況下可以省略。

### 補充資源
- 教程
- 文檔
- 插件的 README 文檔
- 等等。

要獲得更多信息，請查看在 contributing 文檔目錄下的 [文檔模板](/docs/docs-templates/) 。
-->

想要在閱讀 [完整教程](/tutorial/) 和搜索 [文檔](/docs/) 之間找到一個折衷方案? 可以參考這本 Gatsby 風格的指導手冊。我們將告訴你構建各種東西的 “配方”。

## 1. 頁面和佈局

### 項目結構

在 Gatsby 項目中，你會看到這些文件和文件夾，可能是一些，可能是全部：

```
|-- /.cache
|-- /plugins
|-- /public
|-- /src
    |-- /pages
    |-- /templates
    |-- html.js
|-- /static
|-- gatsby-config.js
|-- gatsby-node.js
|-- gatsby-ssr.js
|-- gatsby-browser.js
```

一些值得注意的文件和它們的定義：

- `gatsby-config.js`——配置 Gatsby 站點的選項，比如項目標題和項目描述的元數據，插件等等
- `gatsby-node.js`——實現了 Gatsby 的 Node.js API，來自定義和擴展影響構建過程的設置
- `gatsby-browser.js`——使用 Gatsby 的瀏覽器 API 自定義和擴展影響瀏覽器的的設置
- `gatsby-ssr.js`——使用 Gatsby 的服務端渲染 API 自定義影響服務端渲染的默認設置

#### 補充資源

- 要了解所有常用的文件和文件夾，請查看 [Gatsby 的項目結構](/docs/gatsby-project-structure/)
- 要了解命令，請查看 [Gatsby CLI 文檔](/docs/gatsby-cli)
- 要獲得及時查閱的信息，請查看可下載的 [Gatsby 備忘錄](/docs/cheat-sheet/)

### 自動創建頁面

Gatsby 的核心自動把 `src/pages` 中的 React 組件轉變為頁面和 URL。比如：在 `src/pages/index.js` 和 `src/pages/about.js` 中的組件，會為網站的索引頁（`/`）和 關於頁（`/about`）自動創建基於文件名的頁面。

#### 前置條件

- 一個 [Gatsby 網站](/docs/quick-start)
- 已經安裝好 [Gatsby CLI](/docs/gatsby-cli) 

#### 操作步驟

1. 如果你沒有 `src/pages` 這個目錄的話，創建它。
2. 添加一個組件文件在這個 pages 目錄裡:

```jsx:title=src/pages/about.js
import React from "react"

const AboutPage = () => (
  <main>
    <h1>About the Author</h1>
    <p>Welcome to my Gatsby site.</p>
  </main>
)

export default AboutPage
```

3. 運行 `gatsby develop` 以啟動開發服務器。
4. 在瀏覽器中訪問你的新頁面：<http://localhost:8000/about>

#### 補充資源

- [創建和修改頁面](/docs/creating-and-modifying-pages/)

### 連接不同頁面

Gatsby 中的頁面路由依靠 `<Link />` 這個組件。

#### 前置條件

- 一個 Gatsby 站點，包含這兩個組件：`index.js` 和 `contact.js`
- Gatsby 的 `<Link />` 組件
- [Gatsby CLI](/docs/gatsby-cli/)，用來運行 `gatsby develop`

#### 操作步驟

1. 打開索引頁面（index）組件 (`src/pages/index.js`), 從 Gatsby 中引入 `<Link />` 組件，在標題上面添加一個 `<Link />` 組件， 並給它一個值為 `"/contact/"` 的 `to` 屬性，來指定路徑名：

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby"

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contact</Link>
    <p>What a world.</p>
  </div>
)
```

2. 運行 `gatsby develop` 並導航到索引頁面。你應該看到一個點擊後轉向 contact 頁面的鏈接!

> **注意**：Gatsby 的 `<Link />` 組件是 [`@reach/router` 的 Link 組件](https://reach.tech/router/api/Link) 的一個包裝。更多信息請參考 Gatsby 的 `<Link />` 組件， 查閱 [API 參考中的 `<Link />`](/docs/gatsby-link/)。

### 添加一個佈局組件

用 React 佈局組件來封裝頁面是非常常見的。這使我們可以在不同頁面中分享標記、樣式和功能。

#### 前置條件

- 一個 Gatsby 站點

#### 操作步驟

1. 在 `src/components` 目錄中添加一個佈局組件, 其中子組件作為 props 傳入：

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

2. 在一個頁面中引入並使用這個佈局組件：

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <Link to="/contact/">Contact</Link>
    <p>What a world.</p>
  </Layout>
)
```

#### 補充資源

- 通過閱讀 [第 3 章教程](/tutorial/part-three/#your-first-layout-component) 來創建一個佈局組件
- 為 [佈局組件](/docs/layout-components/) 創建樣式

### 以編程的方式使用 createPage 創建頁面

你可以使用 Gatsby 提供的輔助方法，在 `gatsby-node.js` 文件中以編程方式創建頁面。

#### 前置條件

- 一個 [Gatsby 站點](/docs/quick-start)
- 一個 `gatsby-node.js` 文件

#### 操作步驟

1. 在 `gatsby-node.js` 文件中，為 `createPages` 添加一個輸出（export）

```javascript:title=gatsby-node.js
// highlight-start
exports.createPages = ({ actions }) => {
  // ...
}
// highlight-end
```

2. 從可用操作中解構 `createPage` 操作，以便可以單獨調用它，並添加或獲取數據

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  // highlight-start
  const { createPage } = actions
  // pull in or use whatever data
  const dogData = [
    {
      name: "Fido",
      breed: "Sheltie",
    },
    {
      name: "Sparky",
      breed: "Corgi",
    },
  ]
  // highlight-end
}
```

3. 遍歷 `gatsby-node.js` 中的數據，併為每次調用將路徑，模板和上下文（在 props 的 pageContext 中傳入的數據）提供給 `createPage`

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  const { createPage } = actions

  const dogData = [
    {
      name: "Fido",
      breed: "Sheltie",
    },
    {
      name: "Sparky",
      breed: "Corgi",
    },
  ]
  // highlight-start
  dogData.forEach(dog => {
    createPage({
      path: `/${dog.name}`,
      component: require.resolve(`./src/templates/dog-template.js`),
      context: { dog },
    })
  })
  // highlight-end
}
```

4. 創建一個 React 組件作為你的頁面模板，這個組件在 `createPage` 中已經被使用了

```jsx:title=src/templates/dog-template.js
import React from "react"

export default ({ pageContext: { dog } }) => (
  <section>
    {dog.name} - {dog.breed}
  </section>
)
```

5. 運行 `gatsby develop` 並導航到一個你創建的頁面路徑（例如 <http://localhost:8000/Fido>）來查看傳給它的數據是否成功顯示在頁面上

#### 補充資源

- 教程部分中的 [以編程的方式利用數據創建頁面](/tutorial/part-seven/)
- 參考指導中的 [在不用 GraphQL 的情況下使用 Gatsby](/docs/using-gatsby-without-graphql/)
- 這個配方的 [示例倉庫](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-createPage)

## 2. 使用 CSS 樣式

有很多方法可以為你的網站添加樣式。Gatsby 通過官方和社區插件，能夠支持幾乎所有的選項。

### 在不使用佈局組件的情況下使用全局 CSS 文件

#### 前置條件

- 有一個索引頁面組件的 [Gatsby 網站](/docs/quick-start/)
- 一個 `gatsby-browser.js` 文件

#### 操作步驟

1. 添加一個全局 CSS 文件 `src/styles/global.css` 並輸入以下內容：

```css:title=src/styles/styles/global.css
html {
  background-color: lavenderblush;
}

p {
  color: maroon;
}
```

2. 在 `gatsby-browser.js` 文件中引入這個全局 CSS 文件，像這樣：

```javascript:gatsby-browser.js
import "./src/styles/global.css"
```

> **注意：** 你也可以使用 `require('./src/styles/global.css')` 來向你的 `gatsby-config.js` 文件引入全局 CSS 文件。

3. 運行 `gatsby-develop` 來觀察全局樣式是否應用到你的整個網站了。

> **注意：** 如果你使用 CSS-in-JS 對網站進行樣式設置，這種方法不是最合適的選擇。在這種情況下，應當使用一個包含所有共享組件的佈局頁面。下一個配方將對此進行介紹。

#### 補充資源

- 更多信息請參考 [在不使用佈局組件的情況下添加全局樣式](/docs/global-css/#adding-global-styles-without-a-layout-component)

### 在一個佈局組件中使用全局樣式

#### 前置條件

- 有一個索引頁面組件的 [Gatsby 網站](/docs/quick-start/)

#### 操作步驟

你可以添加全局樣式到一個 [共享的佈局組件](/tutorial/part-three/#your-first-layout-component)。這個組件用於整個站點通用的內容，例如頁眉或頁腳。

1. 如果你沒有這個目錄 `/src/components` 的話，創建它。

2. 在組件目錄中創建兩個文件：`layout.css` 和 `layout.js`。

3. 添加以下內容到 `layout.css`：

```css:title=/src/components/layout.css
body {
  background: red;
}
```

4. 編輯 `layout.js` 文件使其引入 CSS 文件並輸出 layout 標記：

```jsx:title=/src/components/layout.js
import React from "react"
import "./layout.css"

export default ({ children }) => <div>{children}</div>
```

5. 現在編輯你的站點主頁 `/src/pages/index.js` 並使用新的佈局組件：

```jsx:title=/src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => <Layout>Hello world!</Layout>
```

#### 補充資源

- [使用全局 CSS 文件的標準](/docs/global-css/)
- [更多佈局組件的信息](/tutorial/part-three)

### 使用樣式化組件（Styled Components）

#### 前置條件

- 有一個索引頁面組件的 [Gatsby 網站](/docs/quick-start/)
- 在 `package.json` 安裝好 [gatsby-plugin-styled-components、styled-components 和 babel-plugin-styled-components](/packages/gatsby-plugin-styled-components/)

#### 操作步驟

1. 在你的 `gatsby-config.js` 文件中添加 `gatsby-plugin-styled-components`

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-styled-components`],
}
```

2. 打開索引頁面組件 (`src/pages/index.js`) 並引入 `styled-components` 包

3. 為每種元素添加樣式代碼塊來給組件添加樣式

4. 通過在 JSX 中添加樣式組件來應用到頁面

```jsx:title=src/pages/index.js
import React from "react"
import styled from "styled-components" //highlight-line

const Container = styled.div`
  margin: 3rem auto;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
`

const Avatar = styled.img`
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
`

const Username = styled.h2`
  margin: 0 0 12px 0;
  padding: 0;
`

const User = props => (
  <>
    <Avatar src={props.avatar} alt={props.username} />
    <Username>{props.username}</Username>
  </>
)

export default () => (
  <Container>
    <h1>About Styled Components</h1>
    <p>Styled Components is cool</p>
    <User
      username="Jane Doe"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
    />
    <User
      username="Bob Smith"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
    />
  </Container>
)
```

4. 運行 `gatsby develop`，看看有什麼改變

#### 補充資源

- [更多關於使用樣式化組件的信息](/docs/styled-components/)
- [Egghead 的教學](https://egghead.io/lessons/gatsby-style-gatsby-sites-with-styled-components)

### 使用 CSS 模塊

#### 前置條件

- 有一個索引頁面組件的 [Gatsby 網站](/docs/quick-start/)

#### 操作步驟

1. 創建一個 CSS 模塊 `src/pages/index.module.css` 並粘貼以下代碼到模塊中：

```css:title=src/components/index.module.css
.feature {
  margin: 2rem auto;
  max-width: 500px;
}
```

2. 修改 `index.js` 文件內容如下，以 JSX 對象 `style` 的形式引入 CSS 模塊：

```jsx:title=src/pages/index.js
import React from "react"

// highlight-start
import style from "./index.module.css"

export default () => (
  <section className={style.feature}>
    <h1>Using CSS Modules</h1>
  </section>
)
// highlight-end
```

3. 運行 `gatsby develop` 命令以查看變化。

**注意：**
請注意文件擴展名是 `.module.css` 而不是 `.css`。它告訴 Gatsby 這是一個 CSS 模塊。

#### 補充資源

- 更多 [使用 CSS 模塊](/tutorial/part-two/#css-modules) 的信息
- [使用 CSS 模塊的在線演示](https://github.com/gatsbyjs/gatsby/blob/master/examples/using-css-modules)

### 使用 Sass 或 SCSS

Sass 是一個 CSS 的擴展。它提供了更多諸如嵌套規則，變量和 mixin 等等高級功能。

Sass具有兩種語法。最常用的語法是 “SCSS”，它是 CSS 的超集。這意味著所有有效的 CSS 語法都是有效的 SCSS 語法。SCSS 文件使用擴展名.scss。

Sass 會為你把 .scss 和 .sass 文件編譯成 .css 文件。因此你可以為樣式表編寫更多高級功能。

#### 前置條件

- 一個 [Gatsby 站點](/docs/quick-start/)。

#### 操作步驟

1. 安裝 Gatsby 插件 [gatsby-plugin-sass](https://www.gatsbyjs.org/packages/gatsby-plugin-sass/) 和 `node-sass`。

`npm install --save node-sass gatsby-plugin-sass`

2. 把插件添加到你的 `gatsby-config.js` 文件中。

```javascript:title=gatsby-config.js
plugins: [`gatsby-plugin-sass`],
```

3.  在 `.sass` 或 `.scss` 文件中編寫你的樣式表並引入到 JavaScript 文件中。如果你不知道如何引入樣式，請看這部分 [使用 CSS 編寫樣式](/docs/recipes/#2-styling-with-css)

```css:title=styles.scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

```css:title=styles.sass
$font-stack:    Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

```javascript
import "./styles.scss"
import "./styles.sass"
```

_注意: 你也可以用模塊的方式使用 Sass/SCSS 文件。像前一個關於 CSS 模塊的配方所提到的一樣。唯一的區別就是 .css 文件擴展名要變成 .scss 或 .sass_

#### 補充資源

- [.sass 和 .scss 之間的區別](https://responsivedesign.is/articles/difference-between-sass-and-scss/)
- [Sass 官網指導 ](https://sass-lang.com/guide)
- [一個更全面的 Sass 安裝教程。包含更多解釋和資源](https://www.gatsbyjs.org/docs/sass/)

### 添加一個本地字體

#### 前置條件

- 一個 [Gatsby 網站](/docs/quick-start/)
- 一個字體文件: `.woff2`、`.ttf` 等文件類型。

#### 操作步驟

1. 把一個字體文件複製到你的項目當中，例如 `src/fonts/fontname.woff2`。

```
src/fonts/fontname.woff2
```

2. 把這個字體資源引入到一個 CSS 文件中，來把它集成到你的 Gatsby 網站當中：

```css:title=src/css/typography.css
@font-face {
  font-family: "Font Name";
  src: url("../fonts/fontname.woff2");
}
```

**注意：** 確保你的字體名稱在相關 CSS 文件中被引用，例如：

```css:title=src/components/layout.css
body {
  font-family: "Font Name", sans-serif;
}
```

通過為 HTML 的 `body` 元素設置字體，你的字體就會引用到大多數頁面文本中。你需要一些其他 CSS 來設置其他元素，例如 `button` 或 `textarea`。

如果在這些操作以後字體依然沒有更新，請確保你在相關的 CSS 中替換了已有的 font-family。

#### 補充資源

- 更多信息請參考 [向文件引入資源](/docs/importing-assets-into-files/)

### 使用 Emotion

[Emotion](https://emotion.sh) 是一個強大的 CSS-in-JS 庫。它同時支持內聯 CSS 樣式和樣式組件。你可以單獨使用每一個樣式功能或在同一個文件中一起用。

#### 前置條件

- 一個 [Gatsby 網站](/docs/quick-start/)

#### 操作步驟

1. 安裝 [Gatsby Emotion 插件](/packages/gatsby-plugin-emotion/) 和 Emotion 的相關包。

```shell
npm install --save gatsby-plugin-emotion @emotion/core @emotion/styled
```

2. 添加 `gatsby-plugin-emotion` 插件到你的 `gatsby-config.js` 文件中：

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-emotion`],
}
```

3. 在你的 Gatsby 站點中創建一個頁面 `src/pages/emotion-sample.js`，如果你還沒有它的話。

引入 Emotion 的 `css` 核心包。之後你就可以使用  `css` prop 來添加 [Emotion 對象樣式](https://emotion.sh/docs/object-styles) 到任何一個組件中的元素中了：

```jsx:title=src/pages/emotion-sample.js
import React from "react"
import { css } from "@emotion/core"

export default () => (
  <div>
    <p
      css={{
        background: "pink",
        color: "blue",
      }}
    >
      This page is using Emotion.
    </p>
  </div>
)
```

4. 要使用 Emotion 的 [樣式化組件](https://emotion.sh/docs/styled)，引入這個包並用  `styled` 函數定義組件。

```jsx:title=src/pages/emotion-sample.js
import React from "react"
import styled from "@emotion/styled"

const Content = styled.div`
  text-align: center;
  margin-top: 10px;
  p {
    font-weight: bold;
  }
`

export default () => (
  <Content>
    <p>This page is using Emotion.</p>
  </Content>
)
```

#### 補充資源

- [在 Gatsby 中使用 Emotion](/docs/emotion/)
- [Emotion 網站](https://emotion.sh)
- [開始使用 Emotion 和 Gatsby](https://egghead.io/lessons/gatsby-getting-started-with-emotion-and-gatsby)

### 使用 Google Fonts

在項目中本地託管你自己的 [Google 字體](https://fonts.google.com/)，使你在網站加載時不必通過網絡獲取它們，從而使網站的速度指數在電腦上提高約 300 毫秒 ，在 3G 下提高約為 1 秒多。我們還建議你限制自定義字體的使用，只應用於重要的部分，這樣能提高網站性能。

#### 前置條件

- 一個 [Gatsby 網站](/docs/quick-start/)
- 安裝好 [Gatsby CLI](/docs/gatsby-cli/)
- 從 [Typefaces 項目](https://github.com/KyleAMathews/typefaces) 中挑選字體包

#### 操作步驟

1. 運行 `npm install --save typeface-your-chosen-font`，替換 `your-chosen-font` 為你在 [Typefaces 項目](https://github.com/KyleAMathews/typefaces) 中想要的字體。

比如要加載現在很流行的 “Source Sans Pro” 字體，就運行 `npm install --save typeface-source-sans-pro`。

2. 添加 `import "typeface-your-chosen-font"` 這一行代碼到一個佈局模板，頁面組件，或 `gatsby-browser.js`中。

```jsx:title=src/components/layout.js
import "typeface-your-chosen-font"
```

3. 引入之後，你就可以在一個 CSS 樣式表，CSS 模塊或者 CSS-in-JS 中引用字體名字了。

```css:title=src/components/layout.css
body {
  font-family: "Your Chosen Font";
}
```

_注意： 對於上面這個例子，CSS 字體引用應該是 `font-family: 'Source Sans Pro';`_

#### 補充資源

- [Typography.js](/docs/typography-js/)——另一個在 Gatsby 站點中使用 Google 字體的選項
- [Typefaces 項目文檔](https://github.com/KyleAMathews/typefaces/blob/master/README.md)
- [Kyle Mathews 博客上的在線演示](https://www.bricolage.io/typefaces-easiest-way-to-self-host-fonts/)

## 3. 使用 starters

[Starters](/docs/starters/) 是官方或社區維護的 Gatsby 站點樣板。

### 使用一個 starter

#### 前置條件

- 安裝好 [Gatsby CLI](/docs/gatsby-cli)

#### 操作步驟

1. 找到你想使用的 starter (_[Starter 庫](/starters/?v=2)裡應有盡有！_)

2. 基於 starter 生成一個新站點。在終端中運行：

```shell
gatsby new {your-project-name} {link-to-starter}
```

> _不要一字不改地運行上面的命令——記得把 {your-project-name} 和 {link-to-starter} 替換成你自己的！_

3. 運行你的新站點：

```shell
cd {your-project-name}
gatsby develop
```

#### 補充資源

- 跟著 [更詳細的指導](/docs/starters/) 來學習使用 Gatsby starters
- 學習如何使用 [Gatsby CLI](/docs/gatsby-cli) 工具，在 [教程第 1 章](/tutorial/part-one/#using-gatsby-starters) 中使用 starter
- 瀏覽 [Starter 庫](/starters/?v=2)
- 看看我們 Gatsby 的 [官方默認 starter](https://github.com/gatsbyjs/gatsby-starter-default)

## 4. 處理主題

Gatsby 主題將 Gatsby 的配置（共享功能，數據源，設計）抽象為可安裝的程序包。這意味著配置和功能不是直接寫到你的項目中，而是通過版本化，集中管理等手段，並作為依賴項安裝。你可以無縫更新主題、將主題組合在一起，甚至可以將一個主題換成另一個兼容的主題。

- 更多內容請參考 什麼是 Gatsby 主題？](/docs/themes/what-are-gatsby-themes)

### 使用主題 starter 創建新站點

基於有主題配置的 starter 創建網站的過程，與基於**沒有主題配置**的啟動器創建網站的過程相同。在此示例中，你可以使用 [使用官方 Gatsby 博客主題的新站點 starter](https://github.com/gatsbyjs/gatsby-starter-blog-theme)。

#### 前置條件

- 安裝好 [Gatsby CLI](/docs/gatsby-cli)

#### 操作步驟

1. 基於博客主題 starter 生成一個新站點：

```shell
gatsby new {your-project-name} https://github.com/gatsbyjs/gatsby-starter-blog-theme
```

2. 運行你的新站點：

```shell
cd {your-project-name}
gatsby develop
```

#### 補充資源

- 在 [簡短的概念指南](/docs/themes/using-a-gatsby-theme) 中學習如何使用一個已有的 Gatsby 主題，或參考更詳細的 [分步教程](/tutorial/using-a-theme)。

### 構建一個新主題

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-the-gatsby-theme-workspace-starter-to-begin-building-a-new-theme"
  lessonTitle="Use the Gatsby Theme Workspace Starter to Begin Building a New Theme"
/>

#### 前置條件

- 安裝好 [Gatsby CLI](/docs/gatsby-cli)

* 安裝好 [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable)

#### 操作步驟

1. 使用 [Gatsby 主題工作區 starter](https://github.com/gatsbyjs/gatsby-starter-theme-workspace) 生成一個新的主題工作區：

```shell
gatsby new {your-project-name} https://github.com/gatsbyjs/gatsby-starter-theme-workspace
```

2. 在工作區中運行示例站點：

```shell
yarn workspace example develop
```

#### 補充資源

- 閱讀關於如何使用 Gatsby 主題工作區 starter 的 [更詳細的指南](/docs/themes/building-themes/)。
- 通過 [Egghead 上的 Gatsby Theme Authoring 視頻](https://egghead.io/courses/gatsby-theme-authoring) 學習如何構建你自己的主題，或者通過 [視頻課程的補充書面教程](/tutorial/building-a-theme)。

## 5. 處理數據源

在 Gatsby 中處理數據源是基於插件的：數據源插件從數據源中提取數據（例如 `gatsby-source-filesystem` 插件從文件系統中提取數據，`gatsby-source-wordpress` 插件從 WordPress API 提取數據，等等）。你也可以自行處理數據源。

### 向 GraphQL 添加數據

Gatsby 的 [GraphQL 數據層](/docs/querying-with-graphql/) 使用節點來描述數據塊。Gatsby 的數據源插件添加了你可以查詢到的數據源節點，你也可以自己添加數據源節點。要自己添加自定義的數據到 GraphQL 數據層，Gatsby 提供了你可以使用的方法。

這個配方將展示如何用 `createNode()` 添加自定義數據。

#### 操作步驟

1. 在 `gatsby-node.js` 中，使用 `sourceNodes()` 和 `actions.createNode()` 創建並導出可供數據查詢的節點。

```javascript:title=gatsby-node.js
exports.sourceNodes = ({ actions, createNodeId, createContentDigest }) => {
  const pokemons = [
    { name: "Pikachu", type: "electric" },
    { name: "Squirtle", type: "water" },
  ]

  pokemons.forEach(pokemon => {
    const node = {
      name: pokemon.name,
      type: pokemon.type,
      id: createNodeId(`Pokemon-${pokemon.name}`),
      internal: {
        type: "Pokemon",
        contentDigest: createContentDigest(pokemon),
      },
    }
    actions.createNode(node)
  })
}
```

2. 運行 `gatsby develop`。

   > _注意：在你修改了 `gatsby-node.js` 之後，你需要重新運行 `gatsby develop` 使改變生效。_

3. 查詢數據（在 GraphiQL 中或在組件中）。

```graphql
query MyPokemonQuery {
  allPokemon {
    nodes {
      name
      type
      id
    }
  }
}
```

#### 補充資源

- 使用 `gatsby-source-filesystem` 插件過一遍 [第 5 章教程](/tutorial/part-five/#source-plugins) 中的例子
- 在 [Gatsby 插件庫](/plugins/?=source) 中搜索可用的數據源插件
- 在 [Pixabay 數據源教程](/docs/pixabay-source-plugin-tutorial/) 中通過構建一個數據源插件來理解數據源插件
- createNode 方法的 [文檔](/docs/actions/#createNode)

### 使用 GraphQL 為博文和其他頁面獲取 Markdown 數據

你可以獲取 Markdown 數據並使用 Gatsby 的 [`createPages` API](/docs/actions/#createPage) 來動態生成頁面。

這個配方告訴你如何在你本地文件系統中，通過 Gatsby 的 GraphQL 數據層使用 Markdown 文件創建頁面。

#### 前置條件

- 一個擁有 `gatsby-config.js` 文件的 [Gatsby 站點](/docs/quick-start)
- 安裝好 [Gatsby CLI](/docs/gatsby-cli)
- 安裝好 [gatsby-source-filesystem 插件](/packages/gatsby-source-filesystem)
- 安裝好 [gatsby-transformer-remark 插件](/packages/gatsby-transformer-remark)
- 一個 `gatsby-node.js` 文件

#### 操作步驟

1. 在 `gatsby-config.js` 文件中, 配置 `gatsby-transformer-remark` 和 `gatsby-source-filesystem` 文件，讓它們從一個源文件夾中獲取 Markdown 文件。如果你已經有其他 `gatsby-source-filesystem` 項目，比如圖片，你應該添加到它們一起:

```js:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-transformer-remark`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `content`,
        path: `${__dirname}/src/content`,
      },
    },
  ]
```

2. 在 `src/content` 添加一篇 Markdown 博文，包含 title，date 和 path 信息作為 frontmatter，和一些文章主體的初步內容：

```markdown:title=src/content/my-first-post.md
---
title: My First Post
date: 2019-07-10
path: /my-first-post
---

This is my first Gatsby post written in Markdown!
```

3. 通過運行 `gatsby develop` 啟動開發服務器，導航到 GraphiQL 瀏覽器 <http://localhost:8000/___graphql>，使用下面的查詢語句來獲取所有 Makrdown 數據：

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          path
        }
      }
    }
  }
}
```

<iframe
  title="Query for all markdown"
  src="https://q4xpb.sse.codesandbox.io/___graphql?explorerIsOpen=false&query=%7B%0A%20%20allMarkdownRemark%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D"
  width="600"
  height="300"
/>

4. 添加 JavaScript 代碼以在構建時為 Makrdown 博文生成頁面，通過複製 GraphQL 查詢命令到 `gatsby-node.js` 中，並在循環內使用結果：

```js:title=gatsby-node.js
const path = require(`path`)

exports.createPages = async ({ actions, graphql }) => {
  const { createPage } = actions

  const result = await graphql(`
    {
      allMarkdownRemark {
        edges {
          node {
            frontmatter {
              path
            }
          }
        }
      }
    }
  `)
  if (result.errors) {
    console.error(result.errors)
  }

  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.frontmatter.path,
      component: path.resolve(`src/templates/post.js`),
    })
  })
}
```

5. 添加一個博文模版到 `src/templates`。這個模版要包含一個 GraphQL 查詢語句，在構建時為 Markdown 內容動態生成頁面：

```jsx:title=src/templates/post.js
import React from "react"
import { graphql } from "gatsby"

export default function Template({ data }) {
  const { markdownRemark } = data // data.markdownRemark holds your post data
  const { frontmatter, html } = markdownRemark
  return (
    <div className="blog-post">
      <h1>{frontmatter.title}</h1>
      <h2>{frontmatter.date}</h2>
      <div
        className="blog-post-content"
        dangerouslySetInnerHTML={{ __html: html }}
      />
    </div>
  )
}

export const pageQuery = graphql`
  query($path: String!) {
    markdownRemark(frontmatter: { path: { eq: $path } }) {
      html
      frontmatter {
        date(formatString: "MMMM DD, YYYY")
        path
        title
      }
    }
  }
`
```

6. 運行 `gatsby develop` 來重啟開發服務器。在你的瀏覽器裡查看博文：<http://localhost:8000/my-first-post>

#### 補充資源

- [教程：以編程的方式通過數據創建頁面](/tutorial/part-seven/)
- [創建和修改頁面](/docs/creating-and-modifying-pages/)
- [添加 Markdown 頁面](/docs/adding-markdown-pages/)
- [以編程的方式通過數據創建頁面的指南](/docs/programmatically-create-pages-from-data/)
- [示例倉庫](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-markdown) for this recipe

### 從 WordPress 中獲取數據

#### 前置條件

- 一個擁有 `gatsby-config.js` 和 `gatsby-node.js` 文件的 [Gatsby 站點](/docs/quick-start/)。
- 一個 WordPress 實例。可以是自己託管的，也可以是 Wordpress.com 託管的

#### 操作步驟

1. 通過運行以下命令安裝 `gatsby-source-wordpress` 插件：

```shell
npm install gatsby-source-wordpress --save
```

2. 通過修改 `gatsby-config.js` 文件來配置插件，修改內容如下：

```javascript:title=gatsby-config.js
module.exports = {
  ...
  plugins: [
    {
      resolve: `gatsby-source-wordpress`,
      options: {
        // baseUrl will need to be updated with your WordPress source
        baseUrl: `wpexample.com`,
        protocol: `https`,
        // is it hosted on wordpress.com, or self-hosted?
        hostingWPCOM: false,
        // does your site use the Advanced Custom Fields Plugin?
        useACF: false
      }
    },
  ]
}
```

> **注意：** 請參考 [`gatsby-source-wordpress` 插件文檔](/packages/gatsby-source-wordpress/?=wordpre#how-to-use) 來了解更多配置插件的信息。

3. 創建一個模版組件，例如包含以下代碼的 `src/templates/post.js`：

```javascript:title=post.js
import React, { Component } from "react"
import { graphql } from "gatsby"
import PropTypes from "prop-types"

class Post extends Component {
  render() {
    const post = this.props.data.wordpressPost

    return (
      <>
        <h1>{post.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.content }} />
      </>
    )
  }
}

Post.propTypes = {
  data: PropTypes.object.isRequired,
  edges: PropTypes.array,
}

export default Post

export const pageQuery = graphql`
  query($id: String!) {
    wordpressPost(id: { eq: $id }) {
      title
      content
    }
  }
`
```

4. 為你的 WordPress 博文創建動態頁面，通過粘貼以下樣本代碼到 `gatsby-node.js`：

```javascript:title=gatsby-node.js
const path = require(`path`)
const slash = require(`slash`)

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  // query content for WordPress posts
  const result = await graphql(`
    query {
      allWordpressPost {
        edges {
          node {
            id
            slug
          }
        }
      }
    }
  `)

  const postTemplate = path.resolve(`./src/templates/post.js`)
  result.data.allWordpressPost.edges.forEach(edge => {
    createPage({
      // `path` will be the url for the page
      path: edge.node.slug,
      // specify the component template of your choice
      component: slash(postTemplate),
      // In the ^template's GraphQL query, 'id' will be available
      // as a GraphQL variable to query for this posts's data.
      context: {
        id: edge.node.id,
      },
    })
  })
}
```

5. 運行 `gatsby-develop` ，導航到新生成的頁面查看它們。

6. 通過 `localhost:8000/__graphql` 打開 `GraphiQL IDE` 並且打開 Docs 或 Explorer 來為 `allWordpressPosts` 查看可查詢的字段。

之前在 `gatsby-node.js` 裡創建的動態頁面，有導航到具體博文的唯一路徑，生成博文的模版組件，和一個示例 GraphQL 命令來獲取 WordPress 博文內容。

#### 補充資源

- [開始使用 WordPress 和 Gatsby](/blog/2019-04-26-how-to-build-a-blog-with-wordpress-and-gatsby-part-1/)
- 更多 [從 WordPress 獲取數據](/docs/sourcing-from-wordpress/) 的信息
- [從 WordPress 獲取數據的在線演示](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-wordpress)

### 從 Contentful 中獲取數據

#### 前置條件

- 一個 [Gatsby 站點](/docs/quick-start/)
- 一個 [Contentful 賬號](https://www.contentful.com/)
- 安裝好 [Contentful CLI](https://www.npmjs.com/package/contentful-cli)

#### 操作步驟

1. 使用 CLI 和以下步驟登錄到 Contentful。如果你還沒有賬號的話，它會為你創建一個賬號。

```shell
contentful login
```

2. 創建一個新 space 如果你還沒創建的話。確保你保存了命令最後給出的 space ID。如果你已經有一個 Contentful space 和 space ID，你可以跳過第 2 步和第 3 步。

注意：對於新賬戶，你可以覆蓋默認的引導 space。詳情請查看 [賬戶中的 space](https://app.contentful.com/account/profile/space_memberships)。

```shell
contentful space create --name 'Gatsby example'
```

3. 新建一個種子 space 來包含示例博文內容。使用在上一個命令中返回的新 space ID 替換掉 `<space ID>`。

```shell
contentful space seed -s '<space ID>' -t blog
```

這是一個包含真實 space ID 的例子：`contentful space seed -s '22fzx88spbp7' -t blog`

4. 為你的 space 創建一個新訪問令牌（access token）。記住這個令牌，你會在第 6 步使用它。

```shell
contentful space accesstoken create -s '<space ID>' --name 'Example token'
```

5. 在你的 Gatsby 站點中安裝 `gatsby-source-contentful` 插件：

```shell
npm install --save gatsby-source-contentful
```

6. 編輯文件 `gatsby-config.js` ，添加 `gatsby-source-contentful` 到 `plugins` 數組中來啟用插件。出於安全考慮，我們強烈建議你使用 [環境變量](/docs/environment-variables/) 來存儲你的 space ID 和令牌。

```javascript:title=gatsby-config.js
plugins: [
   // add to array along with any other installed plugins
   // highlight-start
   {


    resolve: `gatsby-source-contentful`,
    options: {
      spaceId: `<space ID>`, // or process.env.CONTENTFUL_SPACE_ID
      accessToken: `<access token>`, // or process.env.CONTENTFUL_TOKEN
    },
  },
  // highlight-end
],
```

7. 運行 `gatsby develop` 並確保站點成功編譯。

8. 在 <https://localhost:8000/___graphql> 使用 [GraphiQL editor](/docs/introducing-graphiql/) 查詢數據。Contentful 插件在你的站點中添加了一些新節點類型，包括你的的 Contentful 網站的每一個內容類型。你的示例 space包含了一個 “Blog Post” 內容類型，它為你在 GraphQL 中生成了一個 `allContentfulBlogPost` 節點類型。

![使用了下述查詢的 GraphQL 界面](./images/recipe-sourcing-contentful-graphql.png)

要從 Contentful 中查詢博文標題，請使用以下 GraphQL 命令：

```graphql
{
  allContentfulBlogPost {
    edges {
      node {
        title
      }
    }
  }
}
```

Contentful 節點還包括了一些元數據字段，比如 `createdAt` 和 `node_locale`。

9. 要顯示一個博文鏈接列表，添加文件 `/src/pages/blog.js`。這個頁面會顯示所有博文，按照更新日期排序。

```jsx:title=src/pages/blog.js
import React from "react"
import { graphql, Link } from "gatsby"

const BlogPage = ({ data }) => (
  <div>
    <h1>Blog</h1>
    <ul>
      {data.allContentfulBlogPost.edges.map(({ node, index }) => (
        <li key={index}>
          <Link to={`/blog/${node.slug}`}>{node.title}</Link>
        </li>
      ))}
    </ul>
  </div>
)

export default BlogPage

export const query = graphql`
  {
    allContentfulBlogPost(sort: { fields: [updatedAt] }) {
      edges {
        node {
          title
          slug
        }
      }
    }
  }
`
```

要繼續構建你的 Contentful 站點使其包含詳情頁面，請查看餘下的 [Gatsby 文檔](/docs/sourcing-from-contentful/) 和下面列出的補充資源。

#### 補充資源

- [使用 React 和 Contentful 構建網站](/blog/2018-1-25-building-a-site-with-react-and-contentful/)
- [更多從 Contentful 中獲取數據的信息](/docs/sourcing-from-contentful/)
- [Contentful 數據源插件](/packages/gatsby-source-contentful/)
- [長文本字段類型作為對象返回](/packages/gatsby-source-contentful/#a-note-about-longtext-fields)
- [這個配方的示例倉庫](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-contentful)

### 從外部數據源中獲取數據並且不使用 GraphQL 創建頁面

雖然 [一些理由告訴你應該考慮使用 GraphQL](/docs/why-gatsby-uses-graphql/)，但是你不是一定要使用 GraphQL 數據層來為頁面引入數據。你可以使用節點 `createPages` API 來直接把非結構化數據導入到你的 Gatsby 站點中，以替代 GraphQL 加數據源插件的方案。

在這個配方中，你將會使用從 [PokéAPI 的 REST 端點](https://www.pokeapi.co/) 獲取的數據創建動態頁面。[完整示例](https://github.com/jlengstorf/gatsby-with-unstructured-data/) 在 GitHub 上。

#### 前置條件

- 有一個 `gatsby-node.js` 文件的 Gatsby 站點。
- 安裝好 [Gatsby CLI](/docs/gatsby-cli)
- 通過 npm 安裝好 [axios](https://www.npmjs.com/package/axios) 包

#### 操作步驟

1. 在 `gatsby-node.js` 中添加 JavaScript 代碼來從 PokéAPI 獲取數據，並以編程的方式創建一個索引頁面：

```js:title=gatsby-node.js
const axios = require("axios")

const get = endpoint => axios.get(`https://pokeapi.co/api/v2${endpoint}`)

const getPokemonData = names =>
  Promise.all(
    names.map(async name => {
      const { data: pokemon } = await get(`/pokemon/${name}`)
      return { ...pokemon }
    })
  )
exports.createPages = async ({ actions: { createPage } }) => {
  const allPokemon = await getPokemonData(["pikachu", "charizard", "squirtle"])

  // Create a page that lists Pokémon.
  createPage({
    path: `/`,
    component: require.resolve("./src/templates/all-pokemon.js"),
    context: { allPokemon },
  })
}
```

2. 創建一個模版來在主頁顯示 Pokémon：

```js:title=src/templates/all-pokemon.js
import React from "react"

export default ({ pageContext: { allPokemon } }) => (
  <div>
    <h1>Behold, the Pokémon!</h1>
    <ul>
      {allPokemon.map(pokemon => (
        <li key={pokemon.id}>
          <img src={pokemon.sprites.front_default} alt={pokemon.name} />
          <p>{pokemon.name}</p>
        </li>
      ))}
    </ul>
  </div>
)
```

3. 運行 `gatsby develop` 來獲取數據，構建頁面，並且啟動開發服務器。

4. 在你的瀏覽器中查看主頁：<http://localhost:8000>

#### 補充資源

- [完整的 Pokemon 數據倉庫](https://github.com/jlengstorf/gatsby-with-unstructured-data/)
- 更多使用非結構化數據的信息 [在不使用 GraphQL 的情況下使用 Gatsby](/docs/using-gatsby-without-graphql/)
- 對更復雜的 Gatsby 站點，何時以及如何 [使用 GraphQL 查詢數據](/docs/querying-with-graphql/)

### 從 Drupal 中獲取內容

#### 前置條件

- 一個 [Gatsby 站點](/docs/quick-start)
- 一個 [Drupal](http://drupal.org) 站點
- 在 Drupal 站點中安裝並啟用了 [JSON:API 模塊](https://www.drupal.org/project/jsonapi)

#### 操作步驟

1. 安裝 `gatsby-source-drupal` 插件。

```
npm install --save gatsby-source-drupal
```

2. 編輯你的 `gatsby-config.js` 文件，啟用並配置插件：

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-source-drupal`,
      options: {
        baseUrl: `https://your-website/`,
        apiBase: `api`, // optional, defaults to `jsonapi`
      },
    },
  ],
}
```

3. 運行 `gatsby develop` 以啟動開發服務器。在 <http://localhost:8000/___graphql> 中打開 GraphiQL 瀏覽器。在 Explorer 標籤下，你應該看到新節點類型，例如顯示 Drupal block 信息的 `allBlockBlock` ，和一個顯示 Drupal 站點中每一個內容類型的節點。例如假設你有一個 “Page” 內容類型，它能在 `allNodePage` 中顯示。要查詢所有 “Page” 節點的標題和內容，使用如下查詢命令：

```graphql
{
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

4. 要使用你的 Drupal 數據，在 Gatsby 中創建一個新頁面 `src/pages/drupal.js`。這個頁面會列出所有 Drupal "Page" 節點。

_**注意：** 確切的 GraphQL 模式將取決於你的 Drupal 實例的結構。_

```jsx:title=src/pages/drupal.js
import React from "react"
import { graphql } from "gatsby"

const DrupalPage = ({ data }) => (
  <div>
    <h1>Drupal pages</h1>
    <ul>
    {data.allNodePage.edges.map(({ node, index }) => (
      <li key={index}>
        <h2>{node.title}</h2>
        <div>
          {node.body.value}
        </div>
      </li>
    ))}
   </ul>
  </div>
)

export default DrupalPage

export const query = graphql`
  {
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

5. 運行開發服務器後，你可以通過訪問以下頁面來查看新頁面 <http://localhost:8000/drupal>。

#### 補充資源

- [在 Gatsby 中使用 Decoupled Drupal ](/blog/2018-08-13-using-decoupled-drupal-with-gatsby/)
- [更多從 Drupal 中獲取數據的信息](/docs/sourcing-from-drupal)
- [教程第 7 章：以編程的方式通過數據創建頁面](/tutorial/part-seven/)

## 6. 查詢數據

### 使用頁面查詢獲取數據

你可以使用 `graphql` 標籤在你的 Gatsby 站點頁面中查詢數據。這樣你就可以訪問 Gatsby 數據層中包含的所有內容，例如網站元數據，數據源插件，圖像等。

#### 操作步驟

1. 從 `gatsby` 中引入 `graphql`。

2. 導出一個名為 `query` 的變量，並設置它的值為 `graphql` 模版加上用兩個反引號括起來的查詢命令。

3. 把 `data` 作為 prop 傳入到組件中。

4. `data` 變量保存了被查詢的數據，並且可以被 JSX 引用來生成 HTML。

```jsx:title=src/pages/index.js
import React from "react"
// highlight-next-line
import { graphql } from "gatsby"

import Layout from "../components/layout"

// highlight-start
export const query = graphql`
  query HomePageQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`
// highlight-end

// highlight-next-line
const IndexPage = ({ data }) => (
  <Layout>
    // highlight-next-line
    <h1>{data.site.siteMetadata.title}</h1>
  </Layout>
)

export default IndexPage
```

#### 補充資源

- [GraphQL 和 Gatsby](/docs/graphql/)：瞭解數據的預期格式
- [更多使用 GraphQL 獲取頁面數據的信息](/docs/page-query/)
- [模板字符串的 MDN 頁面](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) 就像我們在 GraphQL 中使用的那樣

### 使用 StaticQuery 組件查詢數據

`StaticQuery` 是一個從 Gatsby 數據層獲取 [非頁面組件](/docs/static-query/) 數據的組件。非頁面組件包括頁眉，導航欄，或者其他子組件。

#### 操作步驟

1. `StaticQuery` 組件需要兩個用於渲染的 props: `query` 和 `render`。

```jsx:title=src/components/NonPageComponent.js
import React from "react"
import { StaticQuery, graphql } from "gatsby"

const NonPageComponent = () => (
  <StaticQuery
    query={graphql` // highlight-line
      query NonPageQuery {
        site {
          siteMetadata {
            title
          }
        }
      }
    `}
    render={(
      data // highlight-line
    ) => (
      <h1>
        Querying title from NonPageComponent with StaticQuery:
        {data.site.siteMetadata.title}
      </h1>
    )}
  />
)

export default NonPageComponent
```

2. 你現在可以在 [任何其他組件](/docs/building-with-components#non-page-components) 中使用這個組件，通過把它引入到一個更大的包含 JSX 組件和 HTML 標記的頁面。

### 使用 useStaticQuery hook 查詢數據

從 Gatsby v2.1.0 版本起，你可以使用 `useStaticQuery` hook 來查詢數據。它是一個 JavaScript 函數而不是一個組件。這個函數語法不再需要一個 `<StaticQuery>` 來包含所有東西。對於一些人來說更容易編寫。

`useStaticQuery` hook 接收一個 GraphQL 查詢命令並返回所查詢的數據。它可以被存儲到一個變量裡，在之後的 JSX 模版中使用。

#### 前置條件

- 你將需要 React 和 ReactDOM 16.8.0 或更高版本 (持續更新 Gatsby 的話就沒問題了)
- 推薦閱讀：[React Hooks 的使用規則](https://reactjs.org/docs/hooks-rules.html)

#### 操作步驟

1. 從 `gatsby` 中引入 `useStaticQuery` 和 `graphql` 以使用 hook 來查詢數據。

2. 在一個無狀態功能性組件的一開始，使用 `useStaticQuery` 和 `graphql` 查詢命令作為其參數，給一個變量賦值。

3. 在組件返回的 JSX 代碼中，你可以引用這個變量來處理返回的數據。

```jsx:title=src/components/NonPageComponent.js
import React from "react"
import { useStaticQuery, graphql } from "gatsby" //highlight-line

const NonPageComponent = () => {
  // highlight-start
  const data = useStaticQuery(graphql`
    query NonPageQuery {
      site {
        siteMetadata {
          title
        }
      }
    }
  `)
  // highlight-end
  return (
    <h1>
      Querying title from NonPageComponent: {data.site.siteMetadata.title}{" "}
      //highlight-line
    </h1>
  )
}

export default NonPageComponent
```

#### 補充資源

- [更多在組件中使用 Static Query 查詢數據的信息](/docs/static-query/)
- [Static query 和 page query 的區別](/docs/static-query/#how-staticquery-differs-from-page-query)
- [更多 useStaticQuery hook 的信息](/docs/use-static-query/)
- [使用 GraphiQL 可視化數據](/docs/introducing-graphiql/)

### 給 GraphQL 添加限制

使用 GraphQL 查詢數據時，可以使用具體數字限制返回的結果數。如果你只需要少量數據或需要 [給數據分頁](/docs/adding-pagination/)，這將很有幫助。

為了限制數據，你需要一個Gatsby站點，該站點在 GraphQL 數據層中有一些節點。所有站點都有一些自動創建的節點，例如  `allSitePage` 和 `sitePage`：可以通過在 `gatsby-config.js` 中安裝數據源插件（如 `gatsby-source-filesystem`）來添加更多節點。

#### 前置條件

- 一個 [Gatsby 站點](/docs/quick-start/)

#### 操作步驟

1. 運行 `gatsby develop` 來啟動開發服務器。
2. 在你的瀏覽器中打開頁面：<http://localhost:8000/___graphql>。
3. 在編輯器中添加一個具有以下字段的 `allSitePage` 查詢命令作為開始：

```graphql
{
  allSitePage {
    edges {
      node {
        id
        path
      }
    }
  }
}
```

4. 在 `allSitePage` 字段中添加一個 `limit` 參數並給它一個整數值 `3`。

```graphql
{
  allSitePage(limit: 3) { // highlight-line
    edges {
      node {
        id
        path
      }
    }
  }
}
```

5. 單擊 GraphiQL 頁面中的播放按鈕，`edges` 字段中的數據將限制為指定的數量。

#### 補充資源

- 學習 [Gatsby 的 GraphQL 數據 API 中的節點](/docs/node-interface/)
- [Gatsby GraphQL 添加限制的參考](/docs/graphql-reference/#limit)
- 在線演示：

<iframe
  title="Limiting returned data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(limit%3A%203)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### 使用 GraphQL 的結果排序

結果的順序可以用 GraphQL 的 `sort` 參數來控制。你可以指定使用哪個字段來排序，也可以設置升序還是降序。

對於這個配方，你將需要一個 Gatsby 站點和一系列節點以在 GraphQL 數據層中排序。所有的站點都有一些自動生成的節點，例如 `allSitePage`。安裝數據源插件以後會有更多節點。

#### 前置條件

- 一個 [Gatsby 站點](/docs/quick-start)
- 以 `all` 為前綴的可查詢字段，例如 `allSitePage`

#### 操作步驟

1. 運行 `gatsby develop` 以啟動開發服務器
2. 在瀏覽器標籤中打開 GraphiQL 瀏覽器：<http://localhost:8000/___graphql>
3. 在編輯器中添加一個具有以下字段的 `allSitePage` 查詢命令作為開始：

```graphql
{
  allSitePage {
    edges {
      node {
        id
        path
      }
    }
  }
}
```

4. 在 `allSitePage` 字段中添加一個 `sort` 參數並且給它一個包含 `fields` 和 `order` 屬性的對象。`fields` 的值可以是用於排序的一個字段或一個字段數組（這個例子中我們使用了 `path` 字段），`order` 可以是遞增 `ASC` 或遞減 `DESC`。

```graphql
{
  allSitePage(sort: {fields: path, order: ASC}) { // highlight-line
    edges {
      node {
        id
        path
      }
    }
  }
}

```

5. 點擊 GraphiQL 頁面中的播放按鈕，返回的數據就會以 `path` 字段遞增的順序顯示出來了。

#### 補充資源

- [Gatsby GraphQL 排序參考文檔](/docs/graphql-reference/#sort)
- 學習 [Gatsby's GraphQL 數據 API 中的節點](/docs/node-interface/)
- 在線演示：

<iframe
  title="Sorting data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(sort%3A%20%7Bfields%3A%20path%2C%20order%3A%20ASC%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### 使用 GraphQL 的結果過濾

被查詢到的數據可以將 `eq`（等於）、`ne`（不等於）、`in`（位於）和 `regex`（正則匹配）等操作符引用到指定字段上來進行過濾。

對於這個配方，你將需要一個擁有一系列節點的 Gatsby 站點來在 GraphQL 數據層中進行過濾。所有的站點都有一些自動生成的節點，例如 `allSitePage`。安裝數據轉換插件以後會有更多節點，例如可以在 `gatsby-config.js` 中安裝 `gatsby-source-filesystem` 和 `gatsby-transformer-remark` 來生成 `allMarkdownRemark`。

#### 前置條件

- 一個 [Gatsby 站點](/docs/quick-start)
- 以 `all` 為前綴的可查詢字段，例如 `allSitePage` 和 `allMarkdownRemark`

#### 操作步驟

1. 運行 `gatsby develop` 以啟動開發服務器
2. 在瀏覽器標籤中打開 GraphiQL 瀏覽器：<http://localhost:8000/___graphql>
3. 在編輯器中添加一個以 “all” 為前綴的字段，例如 `allMarkdownRemark`（意味著會返回一個節點列表）

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          title
          categories
        }
      }
    }
  }
}
```

4. 添加一個 `filter` 參數到  `allMarkdownRemark` 字段中，給它一個包含你想要過濾的字段的對象。在這個例子中，Markdown 內容被在 frontmatter 元數據中的 `categories` 屬性過濾了。下一個值是操作符：這個例子中是 `eq`，或者 equals，值為 “magical creatures”。

```graphql
{
  allMarkdownRemark(filter: {frontmatter: {categories: {eq: "magical creatures"}}}) { // highlight-line
    edges {
      node {
        frontmatter {
          title
          categories
        }
      }
    }
  }
}
```

5. 點擊 GraphiQL 頁面中的播放按鈕，只有和過濾器參數匹配的數據會返回。在這個例子中，只有類別為 “magical creatures” 的能成功獲取的 Markdown 文件會被返回出來。

#### 補充資源

- [Gatsby GraphQL 過濾的參考文檔](/docs/graphql-reference/#filter)
- [所有操作符的完整列表](/docs/graphql-reference/#complete-list-of-possible-operators)
- 學習 [Gatsby's GraphQL 數據 API 中的節點](/docs/node-interface/)
- 在線演示：

<iframe
  title="Filtering data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allMarkdownRemark(filter%3A%20%7Bfrontmatter%3A%20%7Bcategories%3A%20%7Beq%3A%20%22magical%20creatures%22%7D%7D%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20title%0A%20%20%20%20%20%20%20%20%20%20categories%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### GraphQL 查詢命令別名（Query Alias）

你可以重命名 GraphQL 查詢命令中的任意字段為一個別名（alias）。

如果你想要在同一個數據源中運行兩條查詢命令，你可以使用一個別名來避免兩條查詢命令中的命名衝突。

#### 操作步驟

1. 運行 `gatsby develop` 以啟動開發服務器。
2. 在瀏覽器標籤中打開 GraphiQL 瀏覽器：<http://localhost:8000/___graphql>
3. 在編輯器中添一個有兩個相同名字 `allFile` 字段的查詢指令

```graphql
{
  allFile {
    totalCount
  }
  allFile {
    pageInfo {
      currentPage
    }
  }
}
```

4. 在你的 GraphQL 模式中，在任何你想要設置別名的字段前面添加一個名字，用冒號分隔（例如 `[alias-name]: [original-name]`）

```graphql
{
  fileCount: allFile { // highlight-line
    totalCount
  }
  filePageInfo: allFile { // highlight-line
    pageInfo {
      currentPage
    }
  }
}
```

5. 在 GraphiQL 頁面中點擊播放按鈕，兩個帶有你提供的別名的對象就會顯示出來。

#### 補充資源

- [Gatsby GraphQL 別名參考文檔](/docs/graphql-reference/#aliasing)
- 在線演示：

<iframe
  title="Using aliases"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20fileCount%3A%20allFile%20%7B%20%0A%20%20%20%20totalCount%0A%20%20%7D%0A%20%20filePageInfo%3A%20allFile%20%7B%0A%20%20%20%20pageInfo%20%7B%0A%20%20%20%20%20%20currentPage%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### GraphQL 查詢指令片段（Fragments）

GraphQL 片段（fragments）是可分享的可重用的一段查詢指令。

你可能想要使用它來在查詢語句之間分享多個字段，或者在一個組件中使數據共存。

#### 操作步驟

1. 聲明一個 `graphql` 模版字符串，包含一個片段。這個片應該由關鍵字 `fragment`、一個名字、它所使用的 GraphQL 類型（在這個例子中是 `Site`，用 `on Site` 來表示）、和組成片段的字段：

```jsx
export const query = graphql`
  // highlight-start
  fragment SiteInformation on Site {
    title
    description
  }
  // highlight-end
`
```

2. 現在，在查詢語句內和片段指定的類型相同的字段中，使用這個片段。這樣就能包含那些字段，而不用去逐一聲明它們：

```diff
export const pageQuery = graphql`
  query SiteQuery {
    site {
-     title
-     description
+   ...SiteInformation
    }
  }
`
```

**注意**：片段不需要在 Gatsby 中被引入。導出一個包含片段的查詢命令，片段就能在項目裡 _所有_ 查詢命令中被使用。

片段可以被嵌套在其他片段中。一個查詢指令中可以使用片段多次。

#### 補充資源

- [使用片段的簡單示例倉庫](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-fragments)
- [Gatsby GraphQL 片段的參考文檔](/docs/graphql-reference/#fragments)
- [Gatsby 圖像片段](/docs/gatsby-image/#image-query-fragments)
- [使數據共存的示例倉庫](https://github.com/gatsbyjs/gatsby/tree/master/examples/gatsbygram)

## 7. 使用圖片

### 使用 webpack 將一個圖片引入到組件中

Webpack 可以將圖片引入到一個 JavaScript 模塊中。這個過程中會自動最小化壓縮圖片並複製圖片到你的站點的 `public` 目錄中，並提供一個動態的圖片 URL，使你可以像平常一樣，將其傳入到一個 HTML `<img>` 元素的文件路徑中。

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-import-a-local-image-into-a-gatsby-component-with-webpack"
  lessonTitle="Import a Local Image into a Gatsby Component with webpack"
/>

#### 前置條件

- 一個 [Gatsby 站點](/docs/quick-start)，其中包含一個導出 React 組件的 `.js` 文件
- `src` 文件夾中有一個圖片（`.jpg`、`.png`、`.gif`、`.svg` 等文件類型）

#### 操作步驟

1. 從文件的 `src` 文件夾的路徑引入文件

```jsx:title=src/pages/index.js
import React from "react"
// Tell webpack this JS file uses this image
import FiestaImg from "../assets/fiesta.jpg" // highlight-line
```

2. 在 `index.js` 中，添加一個 `<img>` 標籤，它的  `src` 屬性設置為你從 webpack 中引入的名字（這個例子中是 `FiestaImg`），再添加一個 `alt` 屬性來 [描述圖片](https://webaim.org/techniques/alttext/)：

```jsx:title=src/pages/index.js
import React from "react"
import FiestaImg from "../assets/fiesta.jpg"

export default () => (
  // The import result is the URL of your image
  <img src={FiestaImg} alt="A dog smiling in a party hat" /> // highlight-line
)
```

3. 運行 `gatsby develop` 以啟動開發服務器。

4. 在瀏覽器中查看圖片：<http://localhost:8000/>

#### 補充資源

- [使用 webpack 引入圖片的示例倉庫](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-webpack-image)
- [Gatsby 中的所有圖片技巧](/docs/images-and-files/)

### 從 `static` 文件夾中引用圖片

作為一個使用 webpack 引入資源的替代選項，`static` 文件夾能在構建時自動複製到 `public` 文件夾，使圖片能夠訪問。

這是一個在 [特定情況](/docs/static-folder/#when-to-use-the-static-folder) 下的 **迂迴方案**。我們更推薦能夠利用 Gatsby 優化的其他方案，例如 [使用 webpack 引入](#import-an-image-into-a-component-with-webpack)。

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-a-local-image-from-the-static-folder-in-a-gatsby-component"
  lessonTitle="Use a local image from the static folder in a Gatsby component"
/>

#### 前置條件

- 一個 [Gatsby 站點](/docs/quick-start)，其中包含一個導出 React 組件的 `.js` 文件
- `static` 文件夾中有一個圖片（`.jpg`、`.png`、`.gif`、`.svg` 等文件類型）

#### 操作步驟

1. 確保項目根目錄下的 `static` 文件夾中有你所需要的圖片。你的項目結構看起來應該是這樣：

```
├── gatsby-config.js
├── src
│   └── pages
│       └── index.js
├── static
│       └── fiesta.jpg
```

2. 在 `index.js` 中，添加一個 `<img>` 標籤，它的  `src` 屬性設置為 `static` 文件夾的相對路徑，再添加一個 `alt` 屬性來 [描述圖片](https://webaim.org/techniques/alttext/)：

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <img src={`fiesta.jpg`} alt="A dog smiling in a party hat" /> // highlight-line
)
```

3. 運行 `gatsby develop` 以啟動開發服務器。

4. 在瀏覽器中查看圖片：<http://localhost:8000/>

#### 補充資源

- [從 static 文件夾中引用圖片的示例倉庫](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-static-image)
- [使用 static 文件夾](/docs/static-folder/)
- [Gatsby 中的所有圖片技巧](/docs/images-and-files/)

### 使用 gatsby-image 優化和查詢本地圖片

`gatsby-image` 插件可以減輕站點圖片優化的許多痛苦。

Gatsby 會生成優化好的資源，可以在 GraphQL 中被查詢，並傳入到 Gatsby 的圖片組件中。這可以解決繁重的工作，包括創建多個圖像尺寸並在正確的時間加載它們。

#### 前置條件

- 安裝好 `gatsby-image`、`gatsby-transformer-sharp`，和 `gatsby-plugin-sharp` 包並添加到 `gatsby-config` 裡的插件數組中
- 在 `gatsby-config` 中 [引用圖片文件](/packages/gatsby-image/#install)，使用諸如 `gatsby-source-filesystem` 之類的插件

#### 操作步驟

1. 首先從 `gatsby-image` 中引入 `Img`，從 `gatsby` 中引入 `graphql` 和 `useStaticQuery`

```jsx
import { useStaticQuery, graphql } from "gatsby" // to query for image data
import Img from "gatsby-image" // to take image data and render it
```

2. 編寫一個查詢命令來獲取圖片數據，並傳入數據到 `<Img />` 組件當中：

選擇以下任意一個選項或者它們的組合：

a. 一個通過文件 [路徑](/docs/content-and-data/) 查詢的單一圖片文件（例如：`images/corgi.jpg`）

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) { // highlight-line
      childImageSharp {
        fluid {
          base64
          aspectRatio
          src
          srcSet
          sizes
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

b. 使用一個 [GraphQL 片段](/docs/using-graphql-fragments/) 來更簡潔地查詢所需要的字段

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fluid {
          ...GatsbyImageSharpFluid // highlight-line
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

c. 使用 `extension` 和 `relativeDirectory` 字段 [過濾](/docs/graphql-reference/#filter) 後的一個目錄中的多個圖片文件（例如 `images/dogs`），然後映射到 `Img` 組件中

```jsx
const data = useStaticQuery(graphql`
  query {
    allFile(
      // highlight-start
      filter: {
        extension: { regex: "/(jpg)|(png)|(jpeg)/" }
        relativeDirectory: { eq: "dogs" }
      }
      // highlight-end
    ) {
      edges {
        node {
          base
          childImageSharp {
            fluid {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`)

return (
  <div>
    // highlight-start
    {data.allFile.edges.map(image => (
      <Img
        fluid={image.node.childImageSharp.fluid}
        alt={image.node.base.split(".")[0]} // only use section of the file extension with the filename
      />
    ))}
    // highlight-end
  </div>
)
```

**注意**：這種方法可能會使匹配帶有 `alt` 文本的圖像變得困難。這個例子使用文件名中包含 `alt` 文本的圖像，例如 `dog in a party hat.jpg`。

d. 一個固定大小的圖片，使用 `fixed` 字段而不是 `fluid` 字段

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fixed(width: 250, height: 250) { // highlight-line
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`)
return (
  <Img fixed={data.file.childImageSharp.fixed} alt="A corgi smiling happily" />
)
```

e. 一個有 `maxWidth` 的固定大小圖片

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fixed(maxWidth: 250) { // highlight-line
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`)
return (
  <Img fixed={data.file.childImageSharp.fixed} alt="A corgi smiling happily" /> // highlight-line
)
```

f. 一個使用最大寬度（以像素 pixel 為單位）或更高質量（默認值為 50 也就是 50%）填滿 fluid 容器的圖片

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fluid(maxWidth: 800, quality: 75) { // highlight-line
          ...GatsbyImageSharpFluid
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

3. （可選）將內聯樣式添加到 `<Img />` 中，就像添加到其他組件一樣

```jsx
<Img
  fluid={data.file.childImageSharp.fluid}
  alt="A corgi smiling happily"
  style={{ border: "2px solid rebeccapurple", borderRadius: 5, height: 250 }} // highlight-line
/>
```

4.（可選）在傳遞給 `<Img />` 組件之前，通過覆蓋 GraphQL 查詢返回的 `aspectRatio` 字段，將圖像強制設置為所需的寬高比

```jsx
<Img
  fluid={{
    ...data.file.childImageSharp.fluid,
    aspectRatio: 1.6, // 1280 / 800 = 1.6
  }}
  alt="A corgi smiling happily"
/>
```

5. 運行 `gatsby develop`，從文件系統中的文件生成圖像（如果尚未完成）並緩存它們

#### 補充資源

- [說明這些示例的示例倉庫](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Gatsby Image API](/docs/gatsby-image/)
- [使用 Gatsby Image](/docs/using-gatsby-image)
- [有關在 Gatsby 中處理圖像的更多信息](/docs/working-with-images/)
- [解釋這些步驟的 egghead.io 免費視頻](https://egghead.io/playlists/using-gatsby-image-with-gatsby-ea85129e)

### 使用 gatsby-image 在博文 frontmatter 中優化和查詢圖像

對於博文中的特色圖片這類情況，你 _依舊_ 可以使用 `gatsby-image`。`Img` 組件需要處理後的圖像數據，這些圖像數據可以來自本地（或遠程）文件，包括來自 `.md` 或 `.mdx` 的 frontmatter 中的 URL。

要在 Makrdown 文中使用圖片（使用 `![]()` 語法），請考慮使用諸如 [`gatsby-remark-images`](/packages/gatsby-remark-images/) 之類的插件

#### 前置條件

- 安裝好 `gatsby-image`、`gatsby-transformer-sharp`，和 `gatsby-plugin-sharp` 包並添加到 `gatsby-config` 裡的插件數組中
- 在 `gatsby-config` 中 [引用圖片文件](/packages/gatsby-image/#install)，使用諸如 `gatsby-source-filesystem` 之類的插件
- 在 `gatsby-config` 中引用 Markdown 文件，其 frontmatter 中包含圖片 URL
- 使用 [`createPages`](https://www.gatsbyjs.org/docs/node-apis/#createPages) 從 Markdown 文件 [創建的頁面](/docs/creating-and-modifying-pages/)

#### 操作步驟

1. 驗證 Markdown 文件包含了一個指向有效圖片文件路徑的圖片 URL

```mdx:title=post.mdx
---
title: My First Post
featuredImage: ./corgi.png // highlight-line
---

Post content...
```

2. 驗證在 `gatsby-node.js` 中調用 `createPages` 時，是否在上下文中傳遞了唯一標識符（在本示例中為 slug），該標識符隨後將傳遞到佈局組件中的 GraphQL 查詢語句中

```js:title=gatsby-node.js
exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  // query for all markdown

  result.data.allMdx.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: path.resolve(`./src/components/markdown-layout.js`),
      // highlight-start
      context: {
        slug: node.fields.slug,
      },
      // highlight-end
    })
  })
}
```

3. 現在，從 `gatsby-image` 中引入 `Img`，從 `gatsby` 中引入 `graphql` 到模版組件中。編寫一個 [pageQuery](/docs/page-query/) 來根據 `slug` 中傳入的數據來獲取圖片數據，並將數據傳入到 `<Img />` 組件當中：

```jsx:title=markdown-layout.jsx
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Img from "gatsby-image" // highlight-line

export default ({ children, data }) => (
  <main>
    // highlight-start
    <Img
      fluid={data.markdown.frontmatter.image.childImageSharp.fluid}
      alt="A corgi smiling happily"
    />
    // highlight-end
    {children}
  </main>
)

// highlight-start
export const pageQuery = graphql`
  query PostQuery($slug: String) {
    markdown: mdx(fields: { slug: { eq: $slug } }) {
      id
      frontmatter {
        image {
          childImageSharp {
            fluid {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`
// highlight-end
```

4. 運行 `gatsby develop`，會依據文件系統中的文件生成圖像

#### 補充資源

- [使用這個配方的示例倉庫](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [frontmatter 中的特色圖片](/docs/working-with-images-in-markdown/#featured-images-with-frontmatter-metadata)
- [Gatsby Image API](/docs/gatsby-image/)
- [使用 Gatsby Image](/docs/using-gatsby-image)
- [有關在 Gatsby 中處理圖像的更多信息](/docs/working-with-images/)

## 8. 轉換數據

Gatsby中的數據轉換是由插件驅動的。數據轉換插件會使用數據源插件獲取的數據，並將其處理為更有用的內容（例如，將 JSON 轉換為 JavaScript 對象等）。

### 將 Markdown 轉換為 HTML

`gatsby-transformer-remark` 插件可以轉換 Markdown 文件為 HTML。

#### 前置條件

- 一個有 `gatsby-config.js` 和一個 `index.js` 頁面的 Gatsby 站點
- 一個保存在你 Gatsby 站點 `src` 目錄的 Markdown 文件 
- 安裝好一個數據源插件，例如 `gatsby-source-filesystem`
- 安裝好 `gatsby-transformer-remark` 插件

#### 操作步驟

1. 在你的 `gatsby-config.js` 文件中添加數據轉換插件：

```js:title=gatsby-config.js
plugins: [
  // not shown: gatsby-source-filesystem for creating nodes to transform
  `gatsby-transformer-remark`
],
```

2. 添加一個 GraphQL 查詢指令到你的 Gatsby 站點的 `index.js` 文件中，以獲取 `MarkdownRemark` 節點：

```jsx:title=src/pages/index.js
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

3. 重啟開發服務器並打開 GraphiQL <http://localhost:8000/___graphql>。探索 `MarkdownRemark` 節點上的可用字段。

#### 補充資源

- 使用 `gatsby-transformer-remark` [轉換 Markdown 為 HTML 的教程](/tutorial/part-six/#transformer-plugins)
- 在 [Gatsby 插件庫](/plugins/?=transformer) 中瀏覽可用的數據轉換插件

## 9. 部署你的站點

請開始你的表演！當你對你的站點滿意的時候，你可以準備將它部署上線了！

### 準備部署

#### 前置條件

- 一個 [Gatsby 站點](/docs/quick-start)
- 安裝好 [Gatsby CLI](/docs/gatsby-cli)

#### 操作步驟

1. 停止你的開發服務器，如果它正在運行的話（大多數情況下在命令行中按下 `Ctrl + C` 停止）

2. 在標準的站點路徑根目錄（`/`）下，在命令行中使用 Gatsby CLI 運行 `gatsby build`。構件好的文件就會在 `public` 文件夾中了。

```shell
gatsby build
```

3. 要包含一個非 `/` 的站點路徑（例如 `/site-name/`），通過添加以下內容到 `gatsby-config.js` 來設置一個路徑前綴，替換 `yourpathprefix` 為你想要的路徑前綴：

```js:title=gatsby-config.js
module.exports = {
  pathPrefix: `/yourpathprefix`,
}
```

有一些原因導致你需要這麼做——例如在一個域上託管一個由 Gatsby 構建的博客，而另一個域不是基於 Gatsby 構建的。主站點將指向 `example.com`，而具有路徑前綴的 Gatsby 網站可以位於 `example.com/blog`。

4. 在 `gatsby-config.js` 中設置好路徑前綴後，運行 `gatsby build` 並添加 `--prefix-paths` 標記來自動為所有 Gatsby 站點 URL 和 `<Link>` 標籤添加前綴。

```shell
gatsby build --prefix-paths
```

5. 確保你的站點在運行 `gatsby build` 後和運行 `gatsby develop` 時看起來一樣。通過構建時運行 `gatsby serve`，你可以在部署上線前測試（並在必要時調試）成品。

```shell
gatsby build && gatsby serve
```

#### 補充資源

- 在 [教程第 1 章](/tutorial/part-one/#deploying-a-gatsby-site) 中學習一遍構建和部署示例站點
- 學習 [性能優化](/docs/performance/)
- 閱讀 [其他部署相關的內容](/docs/preparing-for-deployment/)
- 查看 [部署文檔](/docs/deploying-and-hosting/) 來了解特定部署平臺和如何部署到它們上

### 部署到 Netlify

使用 [`netlify-cli`](https://www.netlify.com/docs/cli/) 來部署你的 Gatsby 應用，而無需離開命令行界面。

#### 前置條件

- 有唯一一個 `index.js` 組件的 [Gatsby 站點](/docs/quick-start)
- 安裝好 [netlify-cli](https://www.npmjs.com/package/netlify-cli) 包
- 安裝好 [Gatsby CLI](/docs/gatsby-cli)

#### 操作步驟

1. 使用 `gatsby build` 構建你的 Gatsby 應用

2. 使用 `netlify login` 登錄到 Netlify 

3. 運行命令 `netlify init`。選擇 “Create & configure a new site” 選項

4. 選擇一個自定義網站名稱，或者按下回車來使用一個隨機名稱

5. 選擇你的 [Team](https://www.netlify.com/docs/teams/)

6. 修改部署路徑為 `public/`

7. 在使用 `netlify deploy --prod` 部署上線前，確保整個站點看起來沒問題

#### 補充資源

- [使用 Netlify 託管](/docs/hosting-on-netlify)
- [gatsby-plugin-netlify](/packages/gatsby-plugin-netlify)

### 部署到 ZEIT Now

使用 [Now CLI](https://zeit.co/download) 來部署你的 Gatsby 應用，而無需離開命令行界面。

#### 前置條件

- 一個 [ZEIT Now](https://zeit.co/signup) 賬號
- 有唯一一個 `index.js` 組件的 [Gatsby 站點](/docs/quick-start)
- 安裝好 [Now CLI](https://zeit.co/download) 包
- 安裝好 [Gatsby CLI](/docs/gatsby-cli)

#### 操作步驟

1. 使用 `now login` 登錄到 Now CLI 

2. 在終端裡切換到 Gatsby.js 應用的路徑，如果不在的話

3. 運行 `now` 來部署應用

#### 補充資源

- [部署到 ZEIT Now](/docs/deploying-to-zeit-now/)
