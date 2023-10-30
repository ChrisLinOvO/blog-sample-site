---
title: "Heroku 轉移至 Fly.io "
date: 2023-10-29
draft: false

# post thumb
image: "images/post/fly.io-logo.jpg"

# meta description
description: "Heroku 轉移至 Fly.io 筆記"

# taxonomies
categories:
  - "Notes"
tags:
  - "Heroku"
  - "Fly.io"

# post type
type: "post"
---

### 前言

---

由於 Heroku 取消了免費的服務...個人作品都壞掉了，因此換成 Fly.io 部署

### 在 Ｆly.io 建立專案

---

#### 步驟 1：安裝 flyctl

這裡教學以 macOS 為例，可參考 {{< target-blank url="https://fly.io/docs/hands-on/install-flyctl/#macos" >}}官網{{< /target-blank >}} 。

```
brew install flyctl
```

#### 步驟 2：註冊並登入

會彈出瀏覽器視窗，可以連結 GitHub 帳號。

```
flyctl auth signup
```

```
flyctl auth login
```

#### 步驟 3：啟動既有專案

cd 到專案後，下`flyctl launch`，輸入專案的名稱。

![image](../../../../images/post/post-8-1.jpg)

選擇 server region，我這邊選擇香港(Hong Kong)。

![image](../../../../images/post/post-8-2.jpg)

> 注意帳號一定要綁定信用卡才行用 ![image](../../../../images/post/post-8-3.jpg)

會問需要使用 DB 嗎?這裡就看需求了(剛好我這個專案需要)，我就選 Yes 。

![image](../../../../images/post/post-8-4.jpg)

![image](../../../../images/post/post-8-5.jpg)

#### 步驟 4：開始部署

```
flyctl deploy
```
這時在終端機看到`succeeded`代表完成部署🎉

![image](../../../../images/post/post-8-6.jpg)

> 可以在終端機下`flyctl logs`除錯

#### 步驟 5：訪問自己網站

可以從`https://fly.io/dashboard`查看所建立的 APP ，裡面會有 Hostname。

![image](../../../../images/post/post-8-7.jpg)

### 結語

---

Heroku 轉移至 Fly.io，讓我的小皮妞Bot復活，但要小心不要亂搞避免產生額外費用{{< target-blank url="https://fly.io/docs/about/pricing/#free-allowances" >}}參考{{< /target-blank >}}。