---
title: 用 Gatsby 處理圖片
---

優化圖片對於任何站點來說一直都是一個挑戰。在不同設備下都可以提升性能的最佳方法是為每一張圖片提供多種不同的尺寸和分辨率。幸運的是，Gatsby 已經有很多 [插件](/docs/plugins/) 能一起在 [圖片組件](/docs/building-with-components/#page-components) 中處理圖片。

推薦的做法是使用 [GraphQL 查詢](/docs/querying-with-graphql/) 來獲取圖片的最佳尺寸或者分辨率，然後用 [`gatsby-image`](/packages/gatsby-image/) 組件來展示它們。

## 通過 GraphQL 查詢圖片

通過 GraphQL 查詢圖片允許你獲取圖片的數據以及利用 [Sharp](https://github.com/lovell/sharp)，一個高效的圖片處理庫，完成數據轉換。

你需要如下幾個插件：

- [`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/) 插件允許你 [用 GraphQL 查詢文件](/docs/querying-with-graphql/#images)
- [`gatsby-plugin-sharp`](/packages/gatsby-plugin-sharp) 連接 Sharp 和 Gatsby 插件
- [`gatsby-transformer-sharp`](/packages/gatsby-transformer-sharp/) 允許你在一個查詢裡創建多張尺寸和分辨率都合適的圖片

如果最終的圖片尺寸是固定的，那麼優化是通過生成多種不同的分辨率。如果圖片是響應式的，也就是說它會延伸到裝滿整個容器或者頁面，那麼優化是通過生成多種不同的尺寸。查看 [Gatsby 的圖片說明文檔來了解更多](/packages/gatsby-image/#two-types-of-responsive-images)。

你也可以在查詢裡用參數來說明準確的最小和最大尺寸。查看 [Gatsby 的圖片說明文檔來了解所有的選項](/packages/gatsby-image/#two-types-of-responsive-images)。

下面是一個畫廊例子。當頁面改變時，畫廊裡的圖片尺寸會拉伸。它使用了 `fluid` 方法和 fluid 片段來獲取正確的數據，這些數據會在 `gatsby-image` 組件中使用。例子的參數被設置為最大寬度是 400px 以及最大高度是 250px。

```js
export const query = graphql`
  query {
    fileName: file(relativePath: { eq: "images/myimage.jpg" }) {
      childImageSharp {
        fluid(maxWidth: 400, maxHeight: 250) {
          ...GatsbyImageSharpFluid
        }
      }
    }
  }
`
```

## 通過 gatsby-image 優化圖片

[`gatsby-image`](/packages/gatsby-image/) 是一個插件，這個插件能自動創建一些能優化圖片的 React 組件：

> - 為各種設備尺寸和屏幕分辨率加載最好的圖片尺寸
> - 為加載中的圖片佔位，這樣你的頁面內容就不會因為圖片加載而跳動
> - 運用 “模糊化（blur-up）” 效果，例如，當整張圖片在加載時，圖片會先顯示一小部分
> - 或者提供一張圖片的 “跟蹤佔位（traced placeholder）” SVG
> - 延遲加載圖片，這樣可以減少帶寬和加快初始加載的時間
> - 如果瀏覽器也支持 [WebP](https://developers.google.com/speed/webp/) 格式的話就可以使用這種圖片

這裡是一個用了之前查詢例子的圖片組件：

```jsx
<Img fluid={data.fileName.childImageSharp.fluid} alt="" />
```

## 通過片段把格式標準化

假如你有很多的圖片，然後你想讓它們都用同樣的格式，那麼你應該怎麼做呢？

自定義片段是一個很好的方法。這個片段很容易就可以把多張圖片的格式標準化然後重複使用：

```js
export const squareImage = graphql`
  fragment squareImage on File {
    childImageSharp {
      fluid(maxWidth: 200, maxHeight: 200) {
        ...GatsbyImageSharpFluid
      }
    }
  }
`
```

然後可以在圖片查詢中引用這個片段：

```js
export const query = graphql`
  query {
    image1: file(relativePath: { eq: "images/image1.jpg" }) {
      ...squareImage
    }

    image2: file(relativePath: { eq: "images/image2.jpg" }) {
      ...squareImage
    }

    image3: file(relativePath: { eq: "images/image3.jpg" }) {
      ...squareImage
    }
  }
`
```

### 更多資料

- [Gatsby 圖片 API 文檔](/docs/gatsby-image/)
- [在 Gatsby 中使用 gatsby-image](https://egghead.io/playlists/using-gatsby-image-with-gatsby-ea85129e)，egghead.io 的免費播放列表
- [gatsby-image 插件 README 文件](/packages/gatsby-image/)
- [gatsby-plugin-sharp README 文件](/packages/gatsby-plugin-sharp/)
- [gatsby-transformer-sharp README 文件](/packages/gatsby-transformer-sharp/)
