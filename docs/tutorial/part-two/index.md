---
title: 在 Gatsby 中使用 CSS 樣式
typora-copy-images-to: ./
disableTableOfContents: true
---

<!-- Idea: Create a glossary to refer to. A lot of these terms get jumbled -->

<!--

  - Global styles
  - Component css
  - CSS-in-JS
  - CSS Modules

-->

歡迎來到 Gatsby 第二部分教程！

## 概述

在這一部分中，你將學習到如何在 Gatsby 中使用 CSS 樣式，並更深入地研究如何使用 React 組件來構建網站。

## 使用全局樣式

每個站點都有某種統一風格。 其中包括網站的排版和背景顏色。 這些樣式用於設置網站的整體風格，就像牆壁的顏色和紋理用於設置房間的整體風格一樣。

### 使用標準 CSS 文件創建全局樣式

向網站添加全局樣式的最直接的方法之一就是使用全局 `.css` 樣式表。

#### ✋ 新建一個Gatsby網站

首先新建一個 Gatsby 網站。 如果你不熟悉命令行工具，最好關閉了用於 [第一部分](/tutorial/part-one/) 的命令行終端窗口，並打開一個新的命令行終端窗口。

打開一個新的終端窗口，新建一個 “hello world” gatsby 站點，並啟動本地開發服務器：

```shell
gatsby new tutorial-part-two https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-two
```

現在，基於 Gatsby 的 “hello world” 啟動模版，你將新建一個具有以下目錄結構的 Gatsby 網站：

```text
├── package.json
├── src
│   └── pages
│       └── index.js
```

#### ✋ 將樣式添加到 CSS 文件

1. 在新項目中創建一個 .css 文件：

```shell
cd src
mkdir styles
cd styles
touch global.css
```

> 注意：如果你願意，你也可以使用代碼編輯器隨意創建這些目錄和文件。

你現在應該具有這樣的文件目錄結構：

```text
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
```

2. 在 global.css 文件中定義一些樣式：

```css:title=src/styles/global.css
html {
  background-color: lavenderblush;
}
```

> 注意：示例中的 css 文件放置在 `/src/styles/` 文件夾中的位置是任意的。

#### ✋ 將樣式表導入 `gatsby-browser.js` 中

1. 創建 `gatsby-browser.js` 文件

```shell
cd ../..
touch gatsby-browser.js
```

你項目的文件目錄結構現在應如下所示：

```text
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
├── gatsby-browser.js
```

> 💡 `gatsby-browser.js` 文件是什麼？ 不必太擔心這一點，現在，只需知道 `gatsby-browser.js` 文件是 Gatsby 會自動掃描並使用的少數特殊文件之一（如果存在）。 在這裡，文件的命名很重要。 如果你確實想現在想了解更多，請查看 [文檔](/docs/browser-apis/)。

2. 將你剛剛創建的樣式表導入 “gatsby-browser.js” 文件中：

```javascript:title=gatsby-browser.js
import "./src/styles/global.css"

// 或者:
// require('./src/styles/global.css')
```

> 注意：CommonJS 格式的（`require`）和 ES Module 格式的（`import`）語法都可以在這裡使用。 如果你不確定選擇哪個，通常最好使用 `import`。 但是，當處理僅在 Node.js 環境中運行的文件時（如 `gatsby-node.js`），則需要使用 `require`。

3. 啟動本地開發服務器：

```shell
gatsby develop
```

在瀏覽器中查看，應該會看到淡紫色背景並已應用了 “hello world” 模板的啟動頁面：

![Lavender Hello World!](global-css.png)

> 提示：本教程這一部分重點介紹了最快和最直接的設置 Gatsby 網站 CSS 樣式的方法 - 使用 `gatsby-browser.js` 直接導入標準 CSS 樣式文件。 在大多數情況下，添加全局樣式的最佳方法是使用公共的佈局組件。[查看文檔](/docs/global-css/) 瞭解有關該方法的更多信息。

## 使用組件範圍內 CSS

到目前為止，我們已經討論了更傳統的使用標準 CSS 樣式表的方法。 現在，我們將討論以面向組件的方式處理樣式的 CSS 樣式模塊化的各種方法。

### CSS 模塊

讓我們一起了解 **CSS 模塊**。 引用自
[the CSS Module homepage](https://github.com/css-modules/css-modules)：

> 一個 **CSS 模塊**是一個 CSS 文件，其中包含所有樣式和 CSS3 動畫名稱
> 默認情況下只適用於本範圍內。

CSS 模塊之所以受歡迎，是因為它們可以讓你正常編寫 CSS 的同時，安全性更高。 該工具會自動生成唯一的樣式和 CSS3 動畫名稱，因此你不必擔心選擇器名稱衝突。

Gatsby 可與 CSS 模塊一起使用。 強烈建議 Gatsby（以及 React）新手使用此方式進行構建。

#### ✋ 使用 CSS 模塊構建新頁面

在本節中，你將創建一個新的頁面組件，並使用 CSS 模塊對該頁面組件進行樣式設置。

首先，新建一個 `Container` 組件。

1. 在 `src/components` 目錄下創建一個新目錄，然後在此目錄中創建一個名為 `container.js` 的文件並粘貼以下內容：

```javascript:title=src/components/container.js
import React from "react"
import containerStyles from "./container.module.css"

export default ({ children }) => (
  <div className={containerStyles.container}>{children}</div>
)
```

你會注意到你導入了一個名為 `container.module.css` 的 CSS 模塊文件。 現在創建該文件。

2. 在同一目錄（`src/components`）中，創建一個 container.module.css 文件並複製 / 粘貼以下內容：

```css:title=src/components/container.module.css
.container {
  margin: 3rem auto;
  max-width: 600px;
}
```

你會注意到，文件名以 .module.css 結尾，而不是通常的 .css 結尾。 這是告訴 Gatsby 該 CSS 文件應作為 CSS 模塊而不是純 CSS 處理的方式。

3. 創建新的頁面組件文件 `src/pages/about-css-modules.js`:

```javascript:title=src/pages/about-css-modules.js
import React from "react"

import Container from "../components/container"

export default () => (
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
  </Container>
)
```

現在，如果你訪問 `http://localhost:8000/about-css-modules/`，則頁面應如下所示：

![使用 CSS 模塊設置頁面](css-modules-basic.png)

#### ✋ 使用 CSS 模塊為組件設置樣式

在本部分中，你將創建一個具有姓名、頭像和拉丁字母組成的簡短人物簡介的人員列表。 你將創建一個 `<User />` 組件，並使用 CSS 模塊對該組件進行樣式設置。

1. 創建 CSS 文件 `src/pages/about-css-modules.module.css`。

2. 將以下內容粘貼到文件中：

```css:title=src/pages/about-css-modules.module.css
.user {
  display: flex;
  align-items: center;
  margin: 0 auto 12px auto;
}

.user:last-child {
  margin-bottom: 0;
}

.avatar {
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
}

.description {
  flex: 1;
  margin-left: 18px;
  padding: 12px;
}

.username {
  margin: 0 0 12px 0;
  padding: 0;
}

.excerpt {
  margin: 0;
}
```

3. 把文件 `src/pages/about-css-modules.module.css` 文件導入到你之前創建 `about-css-modules.js` 文件頁面，編輯開始幾行代碼如下：

```javascript:title=src/pages/about-css-modules.js
import React from "react"
// highlight-next-line
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

// highlight-next-line
console.log(styles)
```

代碼 `console.log(styles)` 將在控制檯打印出已處理的被導入的 `./about-css-modules.module.css` 文件的結果。 如果你在瀏覽器中打開開發者控制檯（使用 Firefox 或 Chrome 的開發者工具），則會看到：

![在控制檯中導入的 CSS 模塊的結果](https://github.com/gatsbyjs/gatsby/raw/master/docs/tutorial/part-two/css-modules-console.png)

如果將其與 CSS 文件進行比較，你會發現每個格式現在都是導入對象中指向長字符串的鍵（key），例如 `avatar` 指向 `src-pages----about-css-modules-module---avatar---2lRF7`。 這些樣式名稱是 CSS 模塊生成的。 保證它們在你的網站上是唯一的。 而且由於必須導入它們才能使用這些類，所以對於在任何地方使用這些 CSS 樣式都沒問題。

4. 在 `about-css-modules.js` 頁面組件中內聯新建一個的 `<User />` 組件。 修改 `about-css-modules.js` 文件如下：

```jsx:title=src/pages/about-css-modules.js
import React from "react"
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

console.log(styles)

// highlight-start
const User = props => (
  <div className={styles.user}>
    <img src={props.avatar} className={styles.avatar} alt="" />
    <div className={styles.description}>
      <h2 className={styles.username}>{props.username}</h2>
      <p className={styles.excerpt}>{props.excerpt}</p>
    </div>
  </div>
)
// highlight-end

export default () => (
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
    {/* highlight-start */}
    <User
      username="Jane Doe"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
      excerpt="I'm Jane Doe. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    <User
      username="Bob Smith"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
      excerpt="I'm Bob Smith, a vertically aligned type of guy. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    {/* highlight-end */}
  </Container>
)
```

> 提示：通常，如果你在站點的多個位置使用組件，則該組件應放置於 `components` 目錄中其自身的模塊文件中。 但是，如果僅在一個文件中使用它，則可以以內聯方式創建它。

現在完成的頁面應如下所示：

![帶有 CSS 模塊的用戶列表頁面](css-modules-userlist.png)

### CSS-in-JS

CSS-in-JS 是一種面向組件的編寫 CSS 樣式方法。 通常，這是一種模式，其中 [CSS 是使用 JavaScript 內聯組成](https://reactjs.org/docs/faq-styling.html#what-is-css-in-js)。

#### 在Gatsby中使用 CSS-in-JS

有許多不同的 CSS-in-JS 庫，其中許多已有Gatsby插件。 在本初始教程中，我們不會介紹 CSS-in-JS 的示例，但是我們鼓勵你 [探索](/docs/styling/) 生態系統必須提供的功能。 有兩個庫的微教程，特別是 [Emotion](/docs/emotion/) 和 [Styled Components](/docs/styled-components/)。

#### CSS-in-JS 的閱讀建議

如果你有興趣進一步閱讀，請查看 [Christopher "vjeux" Chedeau's 2014 presentation that sparked this movement](https://speakerdeck.com/vjeux/react-css-in-js) 和 [Mark Dalgleish's more recent post "A Unified Styling Language"](https://medium.com/seek-blog/a-unified-styling-language-d0c208de2660)。

### 其他 CSS 處理選項

Gatsby 支持幾乎所有可能的 CSS 樣式處理選項（如果你最喜歡的 CSS 選項還沒有 Gatsby 插件，請 [請貢獻一個！](/contributing/how-to-contribute/)）

- [Typography.js](/packages/gatsby-plugin-typography/)
- [Sass](/packages/gatsby-plugin-sass/)
- [JSS](/packages/gatsby-plugin-jss/)
- [Stylus](/packages/gatsby-plugin-stylus/)
- [PostCSS](/packages/gatsby-plugin-postcss/)

和更多！

## 下一步

現在繼續 [教程的第三部分](/tutorial/part-three/)，你將在其中學習 Gatsby 插件和佈局組件。
