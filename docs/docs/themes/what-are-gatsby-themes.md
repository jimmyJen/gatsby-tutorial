---
title: 什麼是 Gatsby 主題?
---

為了介紹主題，我們先回顧一下歷史。這將有助於我們理解，為什麼要推出主題這一功能。

如果你曾經從頭到尾地創建過一個 Gatsby 站點，你肯定有這種體會，你需要做很多的決定。以博客為例，你需要決定博文數據放在哪裡（是在本地以 Markdown 形式存儲，還是存儲在遠程的 CMS 服務中），怎樣獲取數據（是通過 transformer 轉化 Markdown 文件，還是從 API 中獲取），展示哪些內容，主題如何配色，使用什麼風格的組件等等。

## Gatsby starters

使用 “[Gatsby starters](/docs/starters/)” 可以快速搭建一個與 Starter 功能相同的站點。 Starter 本質上是一些配置好特定功能的 Gatbsy 站點。你下載的是一個完整的 Gatsby 站點，它們通常是為特定場景構建的 (例如：博客、 作品集展示站點等等) 並且是這些都是定製好的。

傳統的 Starter 可以幫助新手快速構建一個 Gatsby 站點，它減少了工作量，使得入門變得容易。但是這存在兩個主要的問題：

- 通過 Starter 創建的站點，已經和 Starter 脫離了關係。即它們已經不再受到 Starter 影響，出現了分叉。如果上游的 Starter 後續有了變更，這些通過 Starter 創建的站點，則沒有很好的辦法與上游 Starter 保持同步。
- 如果你使用同一個 Starter 創建了多個站點，後來你又想對這多個站點進行相同的更改，你將不得不逐個地去修改這些站點。

## Gatsby 主題

言歸正傳，該講講主題了。Gatsby 主題允許你將站點功能作為一個單獨的產品打包，它有助於自己和他人輕鬆地複用這些功能。 使用傳統的 Starter，所有的默認配置都是直接存儲在你的站點中。使用主題，這些默認配置則存儲在 npm 包中。

主題解決了傳統 Starter 面臨的一些問題：

- 使用 Gatbsy 主題創建的站點可以很方便地跟進上游主題的更新 ———— 主題是一個版本化的 npm 包，就跟其它的 npm 包一樣。
- 你可以使用同一個主題創建多個站點. 要同時更新這些站點，你只需要更新主題包，發佈新版。然後修改 `package.json` 文件的主題版本號即可（而不用逐個地修改站點內的功能）。
- 主題是可組合使用的。 你可以將博客主題、筆記主題和電子商務主題（等等）一起安裝使用。

> Gatsby 主題實際上是可組合的 Gatsby 配置。 它們提供了一種高階的方法，可以與 Gatsby 一起使用。通過這種方法將複雜或重複的部分抽象為可重用的 npm 包。

## 哪些情況下我應該使用/構建主題?

**以下情況考慮使用主題：**

- 你有一個 Gatsby 站點了，你想加入其它 Starter 的功能。
- 你希望你站點上的功能保持為最新版本。
- 你希望你站點擁有很多功能，但是沒有一個 Starter 完全擁有你想要的功能 ————  你可以使用多個主題，在一個 Gatsby 站點中組合使用。

**以下情況考慮構建主題：**

- 你打算在多個 Gatsby 站點中，使用相同的功能。
- 你打算和 Gatsby 社區分享你的作品。

## 接下來?

- [使用 Gatsby 主題](/docs/themes/using-a-gatsby-theme)
- [同時使用多個 Gatsby 主題](/docs/themes/using-multiple-gatsby-themes)
- [遮蔽](/docs/themes/shadowing/)
- [構建主題](/docs/themes/building-themes)
- [Starter 轉化為主題](/docs/themes/converting-a-starter/)
- [主題結構](/docs/themes/theme-composition/)
- [約定](/docs/themes/conventions/)

## 相關博客文章

這裡有些附加的內容，它們是主題開發期間發佈的博客文章：

- [Why Themes?](/blog/2019-01-31-why-themes/)
- [Themes Roadmap](/blog/2019-03-11-gatsby-themes-roadmap/)
- [Getting Started with Gatsby Themes and MDX](/blog/2019-02-26-getting-started-with-gatsby-themes/)
- [Watch Us Build a Theme Live](/blog/2019-02-11-gatsby-themes-livestream-and-example/)
- [Introducing Gatsby Themes by Chris Biscardi at Gatsby Days](https://www.gatsbyjs.com/gatsby-days-themes-chris/)
- [See all blog posts on themes](/blog/tags/themes)
