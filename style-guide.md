# Gatsby 簡中文檔翻譯風格指南

嘗試翻譯的小夥伴，請先閱讀此文檔，以保證翻譯風格一致。

## 規則

### 注意事項

1. 提交前檢查增加和刪除的行數是否一致。
2. 遵循以下關於空格的排版規範：
   + 英文或數字前後都是中文時，前後各加一個空格。
   + 英文或數字若緊鄰中文全角標點，則其與標點之間不加空格。
   + 行內代碼的兩端各添加一個空格。若行內代碼緊鄰標點符號，則其與標點之間不加空格。


### 代碼塊中的文案

除了註釋以外儘量不要翻譯代碼塊中的文本。 你可以選擇性的翻譯一些字符串，但是一定給要注意，不要將代碼中（影響代碼運行）的字符翻譯。

如果示例代碼在演示圖片或視頻裡出現，則儘可能不翻譯任何字符串，保持原樣，以免實際運行效果與演示結果不一致，對讀者造成困擾。（最簡單的做法就是不翻譯代碼塊）

例如

```js
// Example
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

✅ 正確:

```js
// 案例
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

✅ 這樣也行:

```js
// 案例
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>你好 Gatsby</div>
)
```

❌ 千萬不要這樣:

```js
// 案例
import React from "react"
export default () => (
  // 'purple' 是 CSS 中的關鍵字
  <div style={{ color: `紫色`, fontSize: `72px` }}>¡Hola Gatsby!</div>
)
```

❌ 絕對不要這樣:

```js
導入 "反應" 從 "反應"
導出默認（）=>（
   <div style = {{color：`purple`，fontSize：`72px`}}>你好蓋茨比</ div>
）
```

### 擴展鏈接

指向 [MDN] 或者 [Wikipedia] 的擴展鏈接，如果其中文版本的翻譯質量不錯，則可以鏈接到中文版本的地址，否則還是保留原來的英文鏈接。

[mdn]: https://developer.mozilla.org/en-US/
[wikipedia]: https://en.wikipedia.org/wiki/Main_Page

例如:

```md
React elements are [immutable](https://en.wikipedia.org/wiki/Immutable_object).
```

✅ OK:

```md
React 元素是 [不可變的](https://zh.wikipedia.org/zh-cn/%E4%B8%8D%E5%8F%AF%E8%AE%8A%E7%89%A9%E4%BB%B6)。
```

對於那些沒有對應翻譯內容的鏈接(Stack Overflow, YouTube 等)，保留英文鏈接即可。

## 詞彙表

文檔中提到下表中出現的詞彙時，應該按照表中規則翻譯。對於不知道如何翻譯的詞彙，可以在 Discord 中提出並商討，然後更新到下列表格中。

| 詞彙   | 翻譯 | 註釋 |
| ------ | ----------- | ----------------- |
| Plugin | 插件        | |
| Source plugin | 數據源插件 | |
| Transformer plugin | 數據轉換插件 | |
| Theme  | 主題        ||
| Query  | 查詢         |作動詞時譯為 `查詢`，作名詞時譯為 `查詢語句`，具體視語境而定。|
| Site   | 網站/站點     |二者均可|
| Starter  | ??     |暫不翻譯|
| tutorial/part-x | 第 <數字> 章 | 為了與 `section` 區分，這裡譯為 `章` 而不是 `部分` |
