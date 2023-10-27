---
title: "添加 Google Search Console"
date: 2023-10-25T10:00:00+08:00
draft: false

# post thumb
image: "images/post/google-search-logo-1.jpg"

# meta description
description: "想要文章出現在 Google，快來加 Google Search Console 吧!!"

# taxonomies
categories:
  - "Tech"
tags:
  - "Google"

# post type
type: "post"
---

### 前言

---

目的想在 Google 被搜尋到我的個人 Blog

### 開始添加 Google Search Console

---

#### 步驟 1：點擊 {{< target-blank url="https://search.google.com/search-console/about" >}}Google Search Console{{< /target-blank >}}，後按下「立即開始」，就可以開始設定

#### 步驟 2：選取資源類型

點擊`網址前置字元`區塊，輸入個人網站網址後，按繼續，這時如果有設定好 GA，他這邊會透過 GA 去驗證你的網站，點擊前往資源。

![image](../../../../images/post/post-4-1.jpg)

#### 步驟 3：新增 Sitemap

點選側邊選單 Sitemap 填寫網址，輸入`sitemap.xml`即可。

![image](../../../../images/post/post-4-2.jpg)

> Sitemap：會把網站所有頁面列出來在這個檔案裡，在把 Sitemap 這個檔案上傳到 Google Search Console上，Google 會將這些頁面存到 Google 搜尋引擎裡面，這樣別人在 Google 搜尋時就能找到你的個人網站了。
實際會在打包後產出`public/sitemap.xml`檔案裡，這些檔案會列出個人網站所有頁面、每個頁面修改時間，到時候會被 Google Search 就能找到。

#### 步驟 4：檢查 Blog 能夠被 Google 搜尋到

方法：在 Google Search 輸入`site:個人網站網址`

```
site:ChrisLinOvO.github.io
```

> 剛建立好可能要等明天才搜尋的到

> 也可以 Google Search Console 手動建立索引，要求指定頁面加到 Google 搜尋引擎裡面

### 結語

---

看完以上內容，趕快加入 Google Search Console，可以輕鬆查看在 Google Search 數據及報表，若要提升搜尋結果排行較前面，可進行 SEO 優化，之後會再做介紹。