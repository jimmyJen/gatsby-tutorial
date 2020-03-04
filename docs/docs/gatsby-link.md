---
title: Gatsby Link API
---

對於內部導航，Gatsby 包含了一個內置的 `<Link>` 組件和一個用於在代碼中導航的 `navigate` 函數。

Gatsby 的 `<Link>` 組件可以鏈接到內部頁面。它有著稱為預加載的顯著提升性能的功能。預加載用於預獲取資源，以便在用戶使用此組件導航時取回資源。當 `Link` 在視圖中時，我們使用 `IntersectionObserver` 來獲取低優先級的請求，然後使用 `onMouseOver` 事件來觸發高優先級的請求來為用戶導航到可能需要的資源。

該組件是 [@reach/router's Link 組件](https://reach.tech/router/api/Link) 的一個包裝，它添加了針對 Gatsby 的功能加強。所有的屬性都傳遞到 @reach/router 的 `Link` 組件中。

## 如何使用 Gatsby Link

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-why-and-how-to-use-gatsby-s-link-component"
  lessonTitle="Why and How to Use Gatsby’s Link Component"
/>

### 為本地鏈接將 `a` 標籤替換為 `Link` 標籤

如果你想在同一站點的頁面之間進行鏈接，請使用 `Link` 組件而不是 `a` 標籤。

```jsx
import React from "react"
// highlight-next-line
import { Link } from "gatsby"

const Page = () => (
  <div>
    <p>
      {/* highlight-next-line */}
      Check out my <Link to="/blog">blog</Link>!
    </p>
    <p>
      {/* Note that external links still use `a` tags. */}
      Follow me on <a href="https://twitter.com/gatsbyjs">Twitter</a>!
    </p>
  </div>
)
```

### 為當前活動鏈接添加自定義樣式

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-add-custom-styles-for-the-active-link-using-gatsby-s-link-component"
  lessonTitle="Add Custom Styles for the Active Link Using Gatsby’s Link Component"
/>

通常，最好從視覺上更改與當前頁面匹配的鏈接，來顯示當前正在查看哪個頁面。

`Link` 提供了兩個向活動鏈接添加樣式的選項：

- `activeStyle`——僅噹噹前項目處於活動狀態時才會應用的樣式對象
- `activeClassName` — 一個僅在當前項目處於活動狀態時才會添加到 `Link` 的類名

如果要將活動鏈接變為紅色，有這樣兩種有效的方法：

```jsx
import React from "react"
import { Link } from "gatsby"

const SiteNavigation = () => (
  <nav>
    <Link
      to="/"
      {/* highlight-start */}
      {/* This assumes the `active` class is defined in your CSS */}
      activeClassName="active"
      {/* highlight-end */}
    >
      Home
    </Link>
    <Link
      to="/about/"
      {/* highlight-next-line */}
      activeStyle={{ color: "red" }}
    >
      About
    </Link>
  </nav>
)
```

### 為高級鏈接樣式使用 `getProps`

Gatsby 的 `<Link>` 組件帶有 `getProps` 這個屬性（prop），這對於高級樣式很有用。它提供了一個具有以下屬性的對象：

- `isCurrent`——當 `location.pathname` 和 `<Link>` 組件的 `to` 屬性相同時，值為 true
- `isPartiallyCurrent` — 當 `location.pathname` 以 `<Link>` 組件的 `to` 屬性開頭時，值為 true
- `href` — `to` 屬性的值
- `location` — 頁面的 `location` 對象

更多信息請參考 [`@reach/router` 的文檔](https://reach.tech/router/api/Link)。

### 為部分匹配的鏈接和父鏈接顯示活動樣式

默認情況下，僅在當前 URL 與其 `to` 屬性（prop）_完全_ 匹配時，才在 `<Link>` 組件上設置 `activeStyle` 和 `activeClassName` 屬性。有時，你可能希望將 `<Link>` 設置為活動樣式，即使它只部分匹配當前 URL。例如：

- 你可能想要將 `/blog/hello-world` 匹配到 `<Link to="/blog">`
- 或者將 `/gatsby-link/#passing-state-through-link-and-navigate` 匹配到 `<Link to="/gatsby-link">`

在這種情況下，只需將 `partiallyActive` 屬性添加到你的 `<Link>` 組件中，即使 `to` 屬性只是部分匹配，樣式也會被應用：

```jsx
import React from "react"
import { Link } from "gatsby"

const Header = <>
  <Link
    to="/articles/"
    activeStyle={{ color: "red" }}
    {/* highlight-next-line */}
    partiallyActive={true}
  >
    Articles
  </Link>
</>;
```

_**注意：**此功能僅在 Gatsby V2.1.3 及以後的版本中可用。如果你遇到問題請檢查版本並更新。_

### 將狀態（state）作為屬性（prop）傳遞到連接的頁面中

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-include-information-about-state-in-navigation-with-gatsby-s-link-component"
  lessonTitle="Include Information About State in Navigation With Gatsby’s Link Component"
/>

有時您會想要將數據從源頁面傳遞到連接的頁面中。您可以通過將 `state` 屬性（prop）傳遞給  `Link` 組件或調用 `navigate` 函數來實現。連接的頁面中將有一個 `location` 屬性，其中包含一個嵌套的 `state` 對象結構，該對象包含傳遞的數據。

```jsx
const PhotoFeedItem = ({ id }) => (
  <div>
    {/* (skip the feed item markup for brevity) */}
    <Link
      to={`/photos/${id}`}
      {/* highlight-next-line */}
      state={{ fromFeed: true }}
    >
      View Photo
    </Link>
  </div>
)

// highlight-start
const Photo = ({ location, photoId }) => {
  if (location.state.fromFeed) {
    // highlight-end
    return <FromFeedPhoto id={photoId} />
  } else {
    return <Photo id={photoId} />
  }
}
```

### 替換歷史記錄來改變 “後退” 按鈕的行為

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-replace-navigation-history-items-with-gatsby-s-link-component"
  lessonTitle="Replace Navigation History Items with Gatsby’s Link Component"
/>

在某些情況下我們需要修改“後退”按鈕的行為。例如，如果你構建了一個頁面，在頁面上選擇了一個東西，然後 “確定嗎？” 頁面出現以確保它是你真正想要的，最後看到成功確認頁面。於是你可能希望在點擊 “後退” 按鈕後跳過 “確定嗎？”頁面。

這時就可以使用 `replace` 屬性將歷史記錄中的當前 URL 替換為 `Link` 的目標。

```jsx
import React from "react"
import { Link } from "gatsby"

const AreYouSureLink = () => (
  <Link
    to="/confirmation/"
    {/* highlight-next-line */}
    replace
  >
    Yes, I’m sure
  </Link>
)
```

## 如何使用 `navigate` 輔助函數

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-navigate-to-a-new-page-programmatically-in-gatsby"
  lessonTitle="Navigate to a New Page Programmatically in Gatsby"
/>

有時，您需要以編程方式導航到頁面，例如在表單提交期間。在這種情況下，`Link` 將不起作用。

_**注意：** `navigate` 之前被命名為 `navigateTo`。`navigateTo` 在 Gatsby v2 中不贊成使用，會在下一個重大更新時被移除。_

作為另一種方案，Gatsby 導出了一個 `navigate` 輔助函數，該函數接受 `to` 和 `options` 參數。

| 參數              |  是否必要 | 描述                                                                                            |
| ----------------- | -------- | ----------------------------------------------------------------------------------------------- |
| `to`              | 是       | 導航到的頁面（例如 `/blog/`）。                                                                   |
| `options.state`   | 否       | 一個對象。傳入的值會在目標頁面屬性的 `location.state` 中可用。                                       |
| `options.replace` | 否       | 一個布爾值。值為 true 時在歷史記錄中替換當前 URL。                                                   |

默認情況下，`navigate` 的操作方式與點擊 `Link` 組件的方式相同。

```jsx
import React from "react"
import { navigate } from "gatsby" // highlight-line

const Form = () => (
  <form
    onSubmit={event => {
      event.preventDefault()

      // TODO: do something with form values
      // highlight-next-line
      navigate("/form-submitted/")
    }}
  >
    {/* (skip form inputs for brevity) */}
  </form>
)
```

### 將狀態添加到代碼式的導航

要包含狀態信息，添加一個 `options` 對象，幷包含具有所需狀態的 `state` 屬性。

```jsx
import React from "react"
import { navigate } from "gatsby"

const Form = () => (
  <form
    onSubmit={event => {
      event.preventDefault()

      // Implementation of this function is an exercise for the reader.
      const formValues = getFormValues()

      navigate(
        "/form-submitted/",
        // highlight-start
        {
          state: { formValues },
        }
        // highlight-end
      )
    }}
  >
    {/* (skip form inputs for brevity) */}
  </form>
)
```

### 在代碼式的導航中替換歷史記錄

如果導航行為應該替換歷史記錄而不是在導航歷史記錄中添加新條目，則將值為 `true` 的 `replace` 屬性添加到 `navigate` 的 `options` 參數中。

```jsx
import React from "react"
import { navigate } from "gatsby"

const Form = () => (
  <form
    onSubmit={event => {
      event.preventDefault()

      // TODO: do something with form values
      navigate(
        "/form-submitted/",
        // highlight-next-line
        { replace: true }
      )
    }}
  >
    {/* (skip form inputs for brevity) */}
  </form>
)
```

## 使用 `withPrefix` 給路徑添加路徑前綴

站點通常被託管在站點的子目錄中。通過 Gatsby，您可以[設置網站的路徑前綴](/docs/path-prefix/)。之後 Gatsby 的 `<Link>` 組件將自動在開發和生產環境中構造正確的 URL。

對於你手動構建的路徑名，一個輔助函數 `withPrefix` 可以幫你在生產環境中添加路徑前綴（在開發環境中不需要路徑前綴）。

```jsx
import { withPrefix } from "gatsby"

const IndexLayout = ({ children, location }) => {
  const isHomepage = location.pathname === withPrefix("/")

  return (
    <div>
      <h1>Welcome {isHomepage ? "home" : "aboard"}!</h1>
      {children}
    </div>
  )
}
```

## 提醒：僅將 `<Link>` 用於內部鏈接！

該組件 _僅_ 用於鏈接到 Gatsby 處理的頁面。對於鏈接到其他域上頁面的鏈接，或同一域上未由 Gatsby 處理的頁面的鏈接，請使用常規的 `<a>` 元素。

有時，您可能無法提前知道鏈接是否在內部，例如數據來自 CMS 的時候。在這些情況下，您可能會發現製作一個組件來檢查鏈接，並使用 Gatsby 的 `<Link>` 或常規的 `<a>` 標籤進行渲染很有用。

由於確定鏈接是否為內部鏈接取決於站點本身，你可能需要針對您的環境自己構思新方案，但是以下內容可能是一個不錯的起點：

```jsx
import { Link as GatsbyLink } from "gatsby"

// Since DOM elements <a> cannot receive activeClassName
// and partiallyActive, destructure the prop here and
// pass it only to GatsbyLink
const Link = ({ children, to, activeClassName, partiallyActive, ...other }) => {
  // Tailor the following test to your environment.
  // This example assumes that any internal link (intended for Gatsby)
  // will start with exactly one slash, and that anything else is external.
  const internal = /^\/(?!\/)/.test(to)

  // Use Gatsby Link for internal links, and <a> for others
  if (internal) {
    return (
      <GatsbyLink
        to={to}
        activeClassName={activeClassName}
        partiallyActive={partiallyActive}
        {...other}
      >
        {children}
      </GatsbyLink>
    )
  }
  return (
    <a href={to} {...other}>
      {children}
    </a>
  )
}

export default Link
```

### 文件下載

您可以類似地檢查文件下載：

```jsx
  const file = /\.[0-9a-z]+$/i.test(to)

  ...

  if (internal) {
    if (file) {
        return (
          <a href={to} {...other}>
            {children}
          </a>
      )
    }
    return (
      <GatsbyLink to={to} {...other}>
        {children}
      </GatsbyLink>
    )
  }
```

## 代碼式的應用內部導航推薦

帶有哈希值或查詢參數的 `<Link>` 和 `navigate` 都不能用於一個路由內部的導航。如果你需要這麼做，則應該使用 `<a>` 標籤或導入 Gatsby 已經依賴的 `@reach/router` 包，以利用其 `navgate` 函數，如下所示：

```jsx
import { navigate } from '@reach/router';

...

onClick = () => {
  navigate('#some-link');
  // OR
  navigate('?foo=bar');
}
```

## 補充資源

- [僅在客戶端中路由的身份驗證教程](/tutorial/authentication-tutorial/)
