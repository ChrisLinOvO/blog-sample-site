---
title: " Hugo 添加評論功能"
date: 2023-10-24
draft: false

# post thumb
image: "images/post/comments-logo.jpg"

# meta description
description: "想要留言給作者嗎？在 Hugo 上使用 Utterances"

# taxonomies
categories:
  - "Tech"
tags:
  - "Hugo"  
  - "Git"

# post type
type: "post"
---

### 前言

---

Utterances 是一種基於 GitHub Issues 的評論系統。它允許在靜態網站中嵌入一個評論框，並將評論存儲在與自己的 GitHub 存儲庫相關聯的 Issues 中。

### 安裝 Utterances 及配置

---

#### 步驟 1：到[安裝頁面](https://github.com/apps/utterances)後點選 Install

![image](../../../../images/post/post-2-1.jpg)

#### 步驟 2：選擇要安裝 Utterances 的 GitHub Repository

![image](../../../../images/post/post-2-2.jpg)

#### 步驟 3：添加配置至程式碼中

可由[官網](https://utteranc.es/)進行配置

##### 設置 repo 名稱

![image](../../../../images/post/post-2-3.jpg)

##### 選擇訪客留言 issue 名稱

![image](../../../../images/post/post-2-4.jpg)

##### 選擇主題

![image](../../../../images/post/post-2-5.jpg)

##### 可複製產出的代碼貼在自己的 repo

這邊我是放在`themes/layouts/partials`資料夾建立一個`comments.html`，把生成的代碼複製過去。

```
<script
  src="https://utteranc.es/client.js"
  repo="ChrisLinOvO/ChrisLinOvO.github.io"
  issue-term="pathname"
  theme="github-light"
  crossorigin="anonymous"
  async
></script>
```

若每篇文章最底下要加評論，只需要在`themes/layouts/_default/single.html`裡加上

```
{{ partial "comments.html" . }}
```

### 結語

---

Utterances 是一個方便的評論系統，可用於 Hugo 靜態網站，它與 GitHub 整合，並通過 Issues 存儲評論，使評論管理變得簡單而有效。添加 Utterances 評論系統可以提升您的網站的互動性和參與度。