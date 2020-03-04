---
title: 瞭解 Gatsby 的構建模塊
typora-copy-images-to: ./
disableTableOfContents: true
---

在 [**上一章節**](/tutorial/part-zero/) 中，你安裝了必要的軟件，並使用 [**“hello world” starter**](https://github.com/gatsbyjs/gatsby-starter-hello-world) 創建了第一個 Gatsby 網站。你的本地開發環境已經準備就緒了。現在讓我們深入瞭解 starter 生成的代碼。

## 使用 Gatsby starters

在 [**第 0 章教程**](/tutorial/part-zero/) 裡, 你使用了 “hello world” starter 和以下命令創建了一個新的網站：

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

當你新建一個 Gatsby 網站時，你可以用以下命令結構來創建一個網站，基於任何一個現有的 Gatsby starter：

```shell
gatsby new [SITE_DIRECTORY_NAME] [URL_OF_STARTER_GITHUB_REPO]
```

如果在命令的最後沒有 URL，Gatsby 會基於 [**默認 starter**](https://github.com/gatsbyjs/gatsby-starter-default) 來為你創建網站。在這章教程中，我們將專注於你在第 0 章教程中已經創建好的 “Hello World” 網站。你可以在 [修改一個 starter](/docs/modifying-a-starter) 這篇文檔中學習更多 starter 的相關知識。

### ✋ 打開代碼

在你的代碼編輯器裡，打開你生成好的 “Hello World” 網站代碼，看一看 ‘hello-world’ 目錄中的各個子目錄和文件，它們應該看起來像這樣：

![Hello World 項目在 VS Code 中的演示](01-hello-world-vscode.png)

_注意: 再次說明一下, 這裡展示的編輯器是 Visual Studio Code。如果你使用的是其他編輯器，看起來會略有不同。_

讓我們先來看看讓主頁成功顯示的代碼。

> 💡 如果你在上一章運行了 `gatsby develop` 之後停止了開發服務器，現在請重新啟動它——是時候對簡單的 hello-world 網站做出些改變了！

## 熟悉 Gatsby 頁面

在編輯器中打開 `/src` 目錄，它裡面只有一個目錄：`/pages`。

打開 `src/pages/index.js`。這個文件中的代碼生成了一個只包含一個 div 和一些文本的組件——正巧它就是：“Hello world!”

### ✋ 修改 “Hello World” 主頁

1.  把 “Hello World!” 替換成 “Hello Gatsby!” 並保存文件。如果你的窗口是並排顯示的，你在保存文件後幾乎立即就能在瀏覽器中看到內容的改變。

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./02-demo-hot-reloading.mp4"></source>
  <p>Sorry! Your browser doesn't support this video.</p>
</video>

> 💡 Gatsby 使用 **熱重載** 來加快你的開發流程。當你運行 Gatsby 開發服務器的時候，Gatsby 的網站文件在後臺被監聽（watch）。所以每一次你保存文件的時候，瀏覽器就會立即顯示出你所做的改動。你不需要強制刷新頁面或重啟開發服務器，你的改動會自動顯示。

2.  現在你可以做點更顯眼的改動。嘗試把 `src/pages/index.js` 中的代碼替換成下面這段，然後保存。你會看到文本有所變化——文字顏色變成紫色並且字體變大。

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

> 💡 我們會在教程的 [**第 2 章**](/tutorial/part-two/) 介紹更多 Gatsby 中的樣式。

3.  刪除字體大小的樣式（fontSize），把 “Hello Gatsby!” 放到一個一級標題（h1）中，並且在標題下面增加一個段落（p）。

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  {/* highlight-start */}
  <div style={{ color: `purple` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
  {/* highlight-end */}
  </div>
)
```

![更多有熱重載的改動](03-more-hot-reloading.png)

4.  增加一張圖片。 (這裡我們隨意選取了 Unsplash 上的一張圖片).

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div style={{ color: `purple` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
    {/* highlight-next-line */}
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
)
```

![增加圖片](04-add-image.png)

### 等等…… HTML 在 JavaScript 文件裡?

_如果你對 React 和 JSX 有所瞭解，你可以跳過這一段。_ 如果你沒有使用 React 框架的經驗，你可能會想知道 HTML 是怎麼在 JavaScript 函數中起作用的。或者想知道為什麼我們在第一行引入（import）了 `react`，但好像並沒有在哪裡使用它。這個 “HTML-in-JS” 的混合實際上是一種 JavaScript 的語法擴展，在 React 中叫 JSX。即使你沒有接觸過 React，你也可以順著這個教程繼續下去。但如果你很好奇這部分，可以看看這段簡單入門：

這是我們 `src/pages/index.js` 文件的原始內容:

```jsx:title=src/pages/index.js
import React from "react"

export default () => <div>Hello world!</div>
```

如果使用純 JavaScript, 這個文件就變成:

```javascript:title=src/pages/index.js
import React from "react"

export default () => React.createElement("div", null, "Hello world!")
```

現在你能找到在哪裡使用了引入的 `'react'` 了！ 等等，你寫的是 JSX，不是純 HTML 和 JavaScript，瀏覽器是怎麼成功讀取的？答案很簡單：沒必要。Gatsby 有工具已經為你把源代碼轉換成瀏覽器可以解析的代碼了。

## 用組件來構建

你剛剛編輯的主頁是通過定義一個頁面組件創建出來的。那到底什麼是 “組件”（component）？

從廣義上定義，組件是你網站的構建模塊；它是能描述一個 UI（用戶界面）部分的完備的代碼片段。

Gatsby 是以 React 為基礎的。當我們談論使用和定義 **組件** 的時候，我們實際上是在談論 **React 組件**——一個完備的代碼片段（通常是用 JSX 編寫）。它能接收輸入，並返回 React 元素。其中 React 元素描述了一個 UI 部分。

當你開始通過組件構建的時候（如果你已經是一位開發者了），你要做好這樣一個思維轉換：現在 CSS、HTML 和 JavaScript 三者緊緊耦合在一起，並且通常位於同一個文件裡。

有時候一個看似簡單的改動，會對你思考如何構建網站產生深遠的影響。

就拿創建一個自定義按鈕來舉例：在之前，你會創建一個 CSS class（比如 `.primary-button`），為其編寫你自己的樣式，然後就能在任何時候應用這些樣式，比如：

```html
<button class="primary-button">Click me</button>
```

In the world of components, you instead create a `PrimaryButton` component with your button styles and use it throughout your site like:

而在組件的世界裡，你創建的是一個 `PrimaryButton` 組件，併為其加入一些按鈕樣式。在你的網站中要像這樣使用它：

<!-- prettier-ignore -->
```jsx
<PrimaryButton>Click me</PrimaryButton>
```

這時組件就成了你的網站的基礎構建模塊，而不是侷限於使用瀏覽器提供的構建模塊（比如 `<button />`）。現在你可以輕鬆創建一個新的構建模塊並且優雅地滿足你的項目需求。

### ✋ 使用頁面組件

每個定義在 `src/pages/*.js` 中的 React 組件，在瀏覽器裡都是一個頁面。讓我們動手來看看。

你現在已經有了一個 `src/pages/index.js` 文件。它是通過 “Hello World” starter 生成的。讓我們來創建一個 About 頁面。

1.  新建一個文件，路徑是 `src/pages/about.js`。把以下代碼複製到這個新文件裡並保存。

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div style={{ color: `teal` }}>
    <h1>About Gatsby</h1>
    <p>Such wow. Very React.</p>
  </div>
)
```

2.  訪問這個頁面 http://localhost:8000/about/。

![新的 About 頁面](05-about-page.png)

僅僅在 `src/pages/about.js` 文件中放入了一個 React 組件，你就有了一個通過 `/about` 訪問的新頁面。

### ✋ 使用子組件

假設主頁和 About 頁面都很複雜，你寫了很多東西在裡面。你可以使用子組件，把 UI 劃分成可複用的片段。你的兩個頁面裡都有 `<h1>` 標題——那就新建一個描述一個 `Header` 的組件。

1.  新建一個目錄於 `src/components` ，然後新建一個文件在這個目錄裡，命名這個文件為 `header.js`。
2.  把以下代碼添加到新的 `src/components/header.js` 文件裡。

```jsx:title=src/components/header.js
import React from "react"

export default () => <h1>This is a header.</h1>
```

3.  修改 `about.js` 文件，為其引入 `Header` 組件。替換 `h1` 標記為  `<Header />`。

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header" // highlight-line

export default () => (
  <div style={{ color: `teal` }}>
    <Header /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![添加 Header 組件](06-header-component.png)

在瀏覽器裡，“About Gatsby” 這個標題文本現在應該被替換成了 “This is a header.”。但是你不想要 “About” 頁面顯示 “This is a header.” 作為標題，你要的是：“About Gatsby”。

4.  回到 `src/components/header.js` 文件裡，做以下改動：

```jsx:title=src/components/header.js
import React from "react"

export default props => <h1>{props.headerText}</h1> {/* highlight-line */}
```

5.  回到 `src/pages/about.js` 文件裡，做以下改動：

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="About Gatsby" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![傳遞標題數據](07-pass-data-header.png)

你應該重新看到了你想要的 “About Gatsby” 標題！

### 什麼是 “props”?

你剛剛定義了 React 組件作為描述 UI 的可複用片段。要使這些可複用片段動態化，你需要給它們提供不同的數據。你可以通過一種叫做 “props" 的輸入。Props，正如其名，是給 React 組件使用的屬性（property）。

在 `about.js` 裡，你為引入的 `Header` 子組件傳入了一個值為 `"About Gatsby"` 的 `headerText` prop：
```jsx:title=src/pages/about.js
<Header headerText="About Gatsby" />
```

在 `header.js` 中，header 組件期望接收到 `headerText` prop（因為你在 `about.js` 裡已經寫好它是這麼期望的）。所以可以這樣使用這個 prop：

```jsx:title=src/components/header.js
<h1>{props.headerText}</h1>
```

> 💡 在 JSX 中, 你可以嵌入任意 JavaScript 表達式，只要用 `{}` 包裹起來。這就是這段代碼如何從  “props”  對象獲取到 `headerText` 屬性（或 “prop!”）的。

如果你向 `<Header />` 組件傳入另一個 prop，像這樣：

```jsx:title=src/pages/about.js
<Header headerText="About Gatsby" arbitraryPhrase="is arbitrary" />
```

……類似地，你就能使用 `arbitraryPhrase` prop 了：`{props.arbitraryPhrase}`。

6.  為了著重解釋這樣做如何使得你的組件可複用：在 about 頁面中添加一個額外的 `<Header />` 組件，然後把下面這段代碼複製到你的 `src/pages/about.js` 文件裡並保存。

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="About Gatsby" />
    <Header headerText="It's pretty cool" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![複製 header 來展示可複用性](08-duplicate-header.png)

現在你就有了第二個 header。你所做的僅是利用 props 傳入不同的數據，並沒有重寫任何代碼。

### 使用佈局組件

佈局組件的作用，是使得不同頁面能夠共享某些部分。比如大多數 Gatsby 網站都有一個佈局組件來共享頁頭和頁腳。側邊欄和導航菜單欄也是常出現在佈局中的。

你將在 [**第 3 章**](/tutorial/part-three/) 中詳細探索佈局組件。

## 連接不同頁面

你會經常需要把頁面連接到別的頁面——讓我們來看看 Gatsby 網站的路由功能。

### ✋ 使用 `<Link />` 組件

1. 打開 index 頁面組件（`src/pages/index.js`），從 Gatsby 中引入`<Link />` 組件，在 header 上面添加一個 `<Link />` 組件，並且給這個新組件添加一個值為 `"/contact/"` 的 `to` 屬性來指定路徑名：

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby" // highlight-line
import Header from "../components/header"

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contact</Link> {/* highlight-line */}
    <Header headerText="Hello Gatsby!" />
    <p>What a world.</p>
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
)
```

當你點擊主頁上新的 "Contact" 連接，你會看到……

![Gatsby 的 404 開發頁面](09-dev-404.png)

……Gatsby 的 404 開發頁面。為什麼？因為你在嘗試連接到一個並不存在的頁面。

2. 現在你必須為你的新頁面 "Contact" 創建一個頁面組件： `src/pages/contact.js` 。並把它連接回到主頁：

```jsx:title=src/pages/contact.js
import React from "react"
import { Link } from "gatsby"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Link to="/">Home</Link>
    <Header headerText="Contact" />
    <p>Send us a message!</p>
  </div>
)
```

保存這個文件後，你應該能成功看到 contact 頁面，並且能點擊鏈接回到主頁。

<video controls="controls" loop="true">
  <source type="video/mp4" src="./10-linking-between-pages.mp4"></source>
  <p>Sorry! You browser doesn't support this video.</p>
</video>

`<Link />` 這個 Gatsby 組件的作用，是在你的網站裡連接不同頁面。對於那些不是你的 Gatsby 網站所處理的外部鏈接，請使用常規的 HTML `<a>` 標籤。

## 部署 Gatsby 網站

Gatsby.js 是一個 _現代網站生成器_。這意味著部署既不需要設置服務器，也不需要部署複雜的數據庫。相反地，Gatsby 的 `build` 命令僅僅生成了一個包含靜態 HTML 和 JavaScript 文件的目錄。你只需要把這個目錄部署到一個靜態網站託管服務中。

不妨試一試使用 [Surge](http://surge.sh/) 來部署你的第一個 Gatsby 網站。Surge 是眾多能夠部署 Gatsby 網站的 “靜態網站託管” 服務中的一個。

如果你還沒有安裝並設置好 Surge，打開一個終端窗口並安裝他們的命令行工具：

```shell
npm install --global surge

# Then create a (free) account with them
surge login
```

接下來，在終端中定位到你的網站的根目錄，通過運行以下代碼來構建的你的網站文件（提示：確保在你的網站的根目錄裡運行這個命令，對於這篇教程所演示的項目，這個目錄是 hello-world 文件夾。在你運行 `gatsby develop` 的這個窗口內新建一個標籤，即可確保你在網站的根目錄）：

```shell
gatsby build
```

構建的時間大概在 15~30 秒左右。構建完成後，不妨看看 `gatsby build` 為你準備好的將要部署的文件，這些文件很有趣。

要查看生成文件的列表，只需在終端中網站根目錄下輸入這條命令，你會看到 `public` 目錄的文件列表：

```shell
ls public
```

最後，通過發佈生成文件到 surge.sh，部署你的網站。

```shell
surge public/
```

運行結束後，你應該會在終端中看到類似這樣的內容：

![使用 Surge 發佈 Gatsby 網站的截圖](surge-deployment.png)

打開最後一行顯示的網址（在這張圖裡是 `lowly-pain.surge.sh`），你就能看到你新發布的網站了！乾的漂亮！

## ➡️ 下一步

在本節中，你：

- 學習了 Gatsby starters 和如何使用它們來構建新項目
- 學習了 JSX 語法
- 學習了組件
- 學習了 Gatsby 頁面組件和子組件
- 學習了 React “props” 和 React 組件的複用

現在，繼續閱讀如何 [**為你的網站添加樣式**](/tutorial/part-two/)！
