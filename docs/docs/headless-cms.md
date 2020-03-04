---
title: 什麼是 Headless CMS 以及如何從同一個地方採集數據
overview: true
---

_Headless CMS_ 是一個內容管理軟件，它允許作者創建和管理內容，以及提供結構化數據給開發者，讓開發者能夠將數據展示在網站或者應用前端的一個獨立系統中。

一個傳統的，完整的 CMS 是同時負責後端的內容管理以及提供內容給最終用戶。但相比之下，一個 headless CMS 將前端分離出來，讓開發者能夠用最好的技術來建立優越的用戶體驗。

如今許多內容管理系統（CMS）都支持 “無頭” 或者 “分離” 模式。這些模式都適用於 Gatsby 網站。

現在通過使用 [數據源插件](/plugins/?=source)，Gatsby 已經可以支持很多 headless CMS 方案。這樣你的內容團隊能夠保持對於管理員界面的熟悉和便利。同時你的開發團隊獲得了更好的開發體驗，以及因使用 Gatsby，GraphQL 和 React 來開發前端而帶來的性能表現的提升。

這一節中的教程將詳細介紹採集一些目前最流行的 headless CMS 數據的設置過程。

<GuideList slug={props.slug} />

<!--
  這一部分的排列順序是根據 Gatsby 插件的下載量 & CMS 的供應商規模/採用程度。
-->

這裡有更多你能連接上的 CMS 系統的教程，插件和 starter。

| CMS                                           | 教程                                                                            | 插件文檔                                             | 官方 Starter                                                        |
| --------------------------------------------- | ------------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------- |
| [Contentful](https://www.contentful.com/)     | [教程](/docs/sourcing-from-contentful/)                                         | [文檔](/packages/gatsby-source-contentful)           | [starter](/starters/contentful-userland/gatsby-contentful-starter/) |
| [NetlifyCMS](https://www.netlifycms.org/)     | [教程](/docs/sourcing-from-netlify-cms/)                                        | [文檔](/packages/gatsby-plugin-netlify-cms)          | [starter](/starters/netlify-templates/gatsby-starter-netlify-cms/)  |
| [WordPress](https://www.wordpress.com/)       | [教程](/docs/sourcing-from-wordpress/)                                          | [文檔](/packages/gatsby-source-wordpress)            |                                                                     |
| [Prismic](https://www.prismic.io/)            | [教程](/docs/sourcing-from-prismic/)                                            | [文檔](/packages/gatsby-source-prismic)              |                                                                     |
| [Strapi](https://strapi.io/)                  | [教程](/blog/2018-1-18-strapi-and-gatsby/)                                      | [文檔](/packages/gatsby-source-strapi)               |
| [DatoCMS](https://www.datocms.com/)           | [教程](https://www.gatsbyjs.com/guides/datocms/)                                | [文檔](/packages/gatsby-source-datocms)              | [starter](/starters/datocms/gatsby-portfolio/)                      |
| [Sanity](https://www.sanity.io/)              | [教程](/docs/sourcing-from-sanity)                                              | [文檔](/packages/gatsby-source-sanity/)              |
| [Drupal](https://www.drupal.com/)             | [教程](/docs/sourcing-from-drupal/)                                             | [文檔](/packages/gatsby-source-drupal)               |                                                                     |
| [Shopify](https://www.shopify.com/)           |                                                                                 | [文檔](/packages/gatsby-source-shopify)              |                                                                     |
| [CosmicJS](https://cosmicjs.com/)             | [教程](/blog/2018-06-07-build-a-gatsby-blog-using-the-cosmic-js-source-plugin/) | [文檔](/packages/gatsby-source-cosmicjs)             | [starters](/starters/?s=cosmicjs&v=2)                               |
| [Contentstack](https://www.contentstack.com/) | [教程](/docs/sourcing-from-contentstack)                                        | [文檔](/packages/gatsby-source-contentstack)         | [starter](/starters/contentstack/gatsby-starter-contentstack/)      |
| [ButterCMS](https://buttercms.com/)           | [教程](/docs/sourcing-from-buttercms/)                                          | [文檔](/packages/gatsby-source-buttercms)            | [starter](/starters/ButterCMS/gatsby-starter-buttercms/)            |
| [Ghost](https://ghost.org/)                   | [教程](/docs/sourcing-from-ghost/)                                              | [文檔](/packages/gatsby-source-ghost/)               | [starter](/starters/TryGhost/gatsby-starter-ghost/)                 |
| [Kentico Cloud](https://kenticocloud.com/)    | [教程](/docs/sourcing-from-kentico-cloud)                                       | [文檔](/packages/gatsby-source-kentico-cloud)        | [starter](/starters/Kentico/gatsby-starter-kentico-cloud/)          |
| [Directus](https://directus.io/)              |                                                                                 | [文檔](/packages/gatsby-source-directus)             |
| [GraphCMS](https://graphcms.com/)             | [教程](/docs/sourcing-from-graphcms)                                            | [文檔](/packages/gatsby-source-graphql)              | [starter](/starters/GraphCMS/gatsby-graphcms-tailwindcss-example/)  |
| [Storyblok](https://www.storyblok.com/)       |                                                                                 | [文檔](/packages/gatsby-source-storyblok)            |
| [Cockpit](https://getcockpit.com/)            |                                                                                 | [文檔](/packages/gatsby-plugin-cockpit)              |
| [CraftCMS](https://craftcms.com/)             |                                                                                 | [文檔](/packages/gatsby-source-craftcms)             |
| [AgilityCMS](https://agilitycms.com/)         | [教程](/docs/sourcing-from-agilitycms/)                                         | [文檔](/packages/@agility/gatsby-source-agilitycms/) | [starter](/starters/agility/agility-gatsby-starter/)                |
| [Gentics Mesh](https://getmesh.io)            | [教程](/docs/sourcing-from-gentics-mesh)                                        |                                                      |                                                                     |

## 如何添加新的教程到這一章節中

如果你在列表中沒有看到你喜愛的 CMS，你可以 [自己寫一份新的教程](/contributing/how-to-contribute/) 或者 [提交一個 issue 來獲取它](https://github.com/gatsbyjs/gatsby/issues/new/choose).

你也可以 [寫自己的數據源插件](/docs/creating-a-source-plugin/) 來結合列表中沒有的 CMS 到 Gatsby。
