---
title: 主題組合
---

當你構建主題時，如何與其它主題組合是值得思考的事情。
在某些情況下， 你可能希望將主題分解為模塊化的部分，就像 `gatsby-theme-blog` 和 `gatsby-theme-ecommerce`。

但是在構建主題的早期階段，我們推薦你儘可能構建完整的主題（將功能集合在一個主題內）。 這樣可以快速地開始，使用起來也比較方便。一段時間後，如果你想將主題拆分為獨立的子主題，其實也不會花太多功夫。

## 佈局

在 Gatsby 主題中你可以通過 [`wrapRootElement`](/docs/browser-apis/#wrapRootElement)
或者 [`wrapPageElement`](/docs/browser-apis/#wrapPageElement) 使用全局佈局。 為了主題能更好的與其它主題組合，我們不建議你使用這些 API。 這兩個 API 適合設置一些必要的
[React 上下文](https://reactjs.org/docs/context.html) 配置。通常情況下，你不應該使用任何佈局組件，包括頁眉或者頁腳。因為佈局組件將會在全局範圍內應用，這可能和其它主題造成衝突。

[閱讀更多關於 Gatsby 中佈局和主題相關的知識](https://www.christopherbiscardi.com/post/layouts-in-gatsby-themes)
