---
title: 使用多個 Gatsby 主題
---

Gatsby 主題的宗旨就是可組合。這意味著你可以同時安裝多個主題。

`gatsby-starter-theme` 由兩個主題組合而成：`gatsby-theme-blog` 和 `gatsby-theme-notes`

```shell
gatsby new my-notes-blog https://github.com/gatsbyjs/gatsby-starter-theme
```

這個 Starter 包含了2個主題包（`gatsby-theme-blog` 和 `gatsby-theme-notes`），在 Starter 的 `gatsby-config.js` 文件中你可以找到它們。

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-theme-notes`,
      options: {
        mdx: true,
        basePath: `/notes`,
      },
    },
    // with gatsby-plugin-theme-ui, the last theme in the config
    // will override the theme-ui context from other themes
    { resolve: `gatsby-theme-blog` },
  ],
  siteMetadata: {
    title: `Shadowed Site Title`,
  },
}
```

默認配置中，博客內容將會存在於根目錄下(`/`)，筆記內容將會存在於 `/notes` 路徑下。

運行 `gatsby develop` 啟動開發服務器，然後便可查看自己的站點：

![The homepage of the site created by gatsby-theme-starter](../images/gatsby-theme-starter-home.png)

![The `notes` route of a site created by gatsby-theme starter](../images/gatsby-theme-starter-notes.png)

## 教程

關於更加詳細的分步教程，你可以在[教程：“同時使用多個主題"](/tutorial/using-multiple-themes-together)中查閱。
