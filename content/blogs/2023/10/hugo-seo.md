---
title: "Hugo SEO（搜尋引擎優化）實踐"
date: 2023-10-26
draft: false

# post thumb
image: "images/post/seo-logo.jpg"

# meta description
description: "Hugo 產生靜態網頁的 SEO 最佳化建議"

# seo
keywords:
  - Hugo SEO
  - 靜態網頁SEO
  - SEO最佳化建議
  - 搜尋引擎優化
  - 網站排名

# taxonomies
categories:
  - "Tech"
tags:
  - "Hugo"
  - "SEO"

# post type
type: "post"
---

### 前言

---

- **SEO是什麼？**： SEO 代表搜索引擎優化，它是一系列策略和技術，用於提高網站在搜索引擎中的排名。

- **為什麼它如此重要？**： SEO 是吸引有機流量、提高品牌知名度、增加潛在客戶和改善線上業務成功的關鍵因素。

- **本文的目標**：以下是使用 Hugo 產生靜態網頁的 SEO 最佳化建議。

### 設定頁面關鍵字

---

#### 步驟 1：在該篇文章 Markdown 添加 keywords

```
keywords:
  - Hugo SEO
  - 靜態網頁SEO
  - SEO最佳化建議
  - 搜尋引擎優化
  - 網站排名
```

#### 步驟 2：添加 meta 標籤

這邊我是加在`themes/layouts/partials/head.html`裏面。

```
{{ with .Params.keywords }}
  <meta name="keywords" content="{{ delimit . ", " }}">
{{ end }}
```

> `{{ with .Params.keywords }}`這是一個條件語句，检查文章 Markdown 文件中是否存在`keywords`字段 ，然而`delimit`用逗號和空格分隔成一個字符串。

#### 步驟 3：打包後看 html 結果，這樣就完成頁面關鍵字設定

```
<meta name="keywords" content="Hugo SEO, 靜態網頁SEO, SEO最佳化建議, 搜尋引擎優化, 網站排名">
```

### 設定頁面標題

---

#### 步驟 1：在該篇文章 Markdown 添加 title

```
title: "Hugo SEO（搜尋引擎優化）實踐"
```

#### 步驟 2：添加 title 標籤

這邊我是加在`themes/layouts/partials/head.html`裏面。

- **這段意思是 先檢查是否在該頁面有設定標題，如果沒有會用整個網站的標題參數**

```
<title>{{ with .Title }}{{ . }}{{ else }}{{ .Site.Title }}{{ end }}</title>
```

#### 步驟 3：打包後看 html 結果，這樣就完成頁面標題設定

```
<title>Hugo SEO（搜尋引擎優化）實踐</title>
```

### 設定頁面描述

---

#### 步驟 1：在該篇文章 Markdown 添加 description

```
description: "Hugo 產生靜態網頁的 SEO 最佳化建議"
```

#### 步驟 2：添加 meta 標籤

這邊我是加在`themes/layouts/partials/head.html`裏面。

- **這段意思是同頁面標題**

```
<meta name="description" content="{{ with .Description }}{{ . }}{{ else }}{{ with .Site.Params.description }}{{ . }}{{ end }}{{ end }}">
```

#### 步驟 3：打包後看 html 結果，這樣就完成頁面描述設定

```
<meta name="description" content="Hugo 產生靜態網頁的 SEO 最佳化建議">
```

### 結語

---

正確使用關鍵字可以提高您的文章在搜尋引擎中的排名，但不要忘記提供高質量的內容和有價值的信息。