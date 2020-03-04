---
title: 使用主題
---

在這篇指南中，你將學會如何使用 Gatsby 主題。我們將使用官方的博客主題創建一個新的站點。

## 使用博客主題 starter 創建新站點

使用主題 starter 創建新站點和使用常規 Gatsby starter 的方法一樣。

```shell
gatsby new my-blog https://github.com/gatsbyjs/gatsby-starter-blog-theme
```

## 運行站點

通過上面的方式創建新站點 ，博客主題的依賴已經全部安裝好了。 接下來運行站點，看看它長什麼樣子吧。

```shell
cd my-blog
gatsby develop
```

![Default screen when starting a project using gatsby blog starter](./images/starter-blog-theme-default.png)

## 替換你的頭像

這個博客主題的 starter 使用一張純灰度圖像作為頭像。選一張你自己的頭像圖片，覆蓋 `/content/assets/avatar.png` 這個文件。

## 更新站點的 metadata

修改 `gatsby-config.js` 文件中的 `siteMetadata`，定製化自己的站點信息。

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: "gatsby-theme-blog",
      options: {},
    },
  ],
  // Customize your site metadata:
  {/* highlight-start */}
  siteMetadata: {
    title: "My Blog",
    author: "Amberley Romo",
    description: "A collection of my thoughts and writings.",
    siteUrl: "https://amberley.blog/",
    social: [
      {
        name: "twitter",
        url: "https://twitter.com/amber1ey",
      },
      {
        name: "github",
        url: "https://github.com/amberleyromo",
      },
    ],
  },
  {/* highlight-end */}
}
```

## 替換掉個人簡介內容

使用 Gatsby 主題時，你可以利用稱為組件遮蔽（Gatsby 會優先使用自定義的同名組件渲染）的功能。 這使你可以使用已創建的自定義組件覆蓋主題中包含的默認組件。

Gatsby 博客主題包中包含一個組件，其內容為網站作者的傳記內容。 該組件（在博客主題包中，而不是你的站點中）的文件路徑是 `gatsby-theme-blog/src/components/bio-content.js`。 通過瀏覽 `node_modules/gatsby-theme-blog` 這個目錄下的內容，你可以找到此路徑

如果你看一下站點的文件結構，你將會看到下面這樣的內容:

```
my-blog
├── content
│   ├── assets
│   │   └── avatar.png
│   └── posts
│       ├── hello-world.mdx
│       └── my-second-post.mdx
├── src
│   └── gatsby-theme-blog
│       ├── components
│       │   └── bio-content.js
│       └── gatsby-plugin-theme-ui
│           └── colors.js
├── gatsby-config.js
└── package.json
```

`src` 目錄下有一個名為 `gatsby-theme-blog` 的目錄，該目錄下符合名稱匹配規則（與主題內的文件同名）的文件，將會遮蔽主題原有的組件。

> 💡 這個目錄的名稱 (`gatsby-theme-blog`) 必須與主題發佈包的名稱完全相同，本例中名稱為：[`gatsby-theme-blog`](https://www.npmjs.com/package/gatsby-theme-blog)。

打開 `bio-content.js` 文件，然後做一些編輯：

```jsx:title=bio-content.js
export default () => (
  {/* highlight-start */}
  <Fragment>
    這裡是我的個人介紹
    <br />
    這樣將會覆蓋原有主題中的個人介紹
  </Fragment>
  {/* highlight-end */}
)
```

進行到這裡，你應該已經更新了頭像、站點詳情、個人簡介。

![Screenshot of project with current tutorial edits](./images/starter-blog-theme-edited.png)

## 添加自己的博客內容

現在你可以添加自己的博客文章了，和 starter 中的演示內容說再見吧。

### 創建一篇新文章

在 `my-blog/content/posts` 目錄下創建文件。 取一個自己想要的名字(以 `.md` 或者 `.mdx` 文件擴展名結尾)，添加一些內容，下面是例子。

```mdx:title=my-blog/content/posts/my-first-post.mdx
---
title: 我的第一篇文章
date: 2019-07-03
---

這裡是文章內容
```

### 刪除演示用的文章

刪除 `/content/posts` 目錄下的2篇演示文章

- `my-blog/content/posts/hello-world.mdx`
- `my-blog/content/posts/my-second-post.mdx`

重啟開發服務器，你將看到新的文章內容。

![Screenshot of project with updated post content](./images/starter-blog-theme-updated-content.png)

## 修改主題顏色

這個博客主題附帶默認的 Gatsby 紫色主題 ，但是你可以覆蓋修改為自己喜歡的顏色。在這個指南中你會修改一些顏色。

打開 `/src/gatsby-theme-blog/gatsby-plugin-theme-ui/colors.js`，取消代碼的註釋。

```javascript:title=colors.js
{/* highlight-start */}
const darkBlue = `#007acc`
const lightBlue = `#66E0FF`
const blueGray = `#282c35`
{/* highlight-end */}

export default merge(defaultThemeColors, {
  {/* highlight-start */}
  text: blueGray,
  primary: darkBlue,
  heading: blueGray,
  modes: {
    dark: {
      background: blueGray,
      primary: lightBlue,
      highlight: lightBlue,
    },
  },
  {/* highlight-end */}
})
```

現在主題色從紫色變成了藍色。

![Screenshot of project with updated color theme](./images/starter-blog-theme-updated-colors.png)

在這個文件中，你會拉取默認的主題 (`defaultThemeColors`)，合併覆蓋部分顏色。

想看看哪些顏色你可以修改，查看官方博客主題中的 `colors.js` 文件  (`node_modules/gatsby-theme-blog/src/gatsby-plugin-theme-ui/colors.js`)

## 總結

通過展示這些例子，我們一步一步的介紹瞭如何使用 Gatsby 主題。值得注意的是，不同的主題會有不同的配置選項。想深入瞭解，可以查看 [Gatsby 主題文檔](/docs/themes/)。
