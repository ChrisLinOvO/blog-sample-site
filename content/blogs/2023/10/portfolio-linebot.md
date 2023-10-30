---
title: "LineBot 關鍵字儲存查詢"
date: 2023-10-29T23:00:00+08:00
draft: false

# post thumb
image: "images/post/linebot-logo.jpg"

# meta description
description: "用來記錄一些個人資料"

# taxonomies
categories:
  - "Portfolio"
tags:
  - "Render"
  - "LineBot"

# post type
type: "post"
---

### 前言

---

依照個人喜好做出各種風格的機器人像是 AI 聊天，接收到的文字另存至資料庫等

### 建立 Line bot

---

#### 步驟 1：在 {{< target-blank url="https://developers.line.biz/zh-hant/" >}}LINE Developers{{< /target-blank >}} 登入自己Line帳號

#### 步驟 2：建立 line 頻道

選擇 Messaging API

![image](../../../../images/post/post-9-1.jpg)

#### 步驟 3：將帳號加為好友

建立好 channel，可在第二個頁籤`Messaging API`看到`Bot basic ID`，這就是新增好友的 ID，也可以掃QR code。

![image](../../../../images/post/post-9-2.jpg)

#### 步驟 4：設定 Webhook

往下滑會看到`Webhook settings`，把`Use webhook`打開，上方的 URL可以先不輸入，等等再回來設定。

![image](../../../../images/post/post-9-3.jpg)

#### 步驟 5：記下程式連接Bot必要的密鑰

這兩個到時候我是添加到程式中`.env`的設定。

- Basic settings — Channel secret

![image](../../../../images/post/post-9-4.jpg)

- Messaging API — Channel access token

![image](../../../../images/post/post-9-5.jpg)

#### 步驟 6：設定自動回覆及打招呼

在`Messaging API`頁籤拉到下面會看到`LINE Official Account features`，點選`Auto-reply messages`右邊的編輯可以設定。

![image](../../../../images/post/post-9-6.jpg)

回應設定就看個人需求，我這邊都是打開。

![image](../../../../images/post/post-9-7.jpg)

### 建立聊天機器人專案

---

這邊我是用 Node.js + MongoDB Atlas 部署到 Render

### 以下是撰寫 Node.js 程式筆記

#### 步驟 1：安裝`@line/bot-sdk`這個套件，並在專案底下建立一個`app.js`的檔案

```
npm i @line/bot-sdk express dotenv
```

#### 步驟 2：撰寫 LINE Bot

不外乎前面就是引入`@line/bot-sdk`、`express`與`dotenv`這三個套件，Line 官方有提供{{< target-blank url="https://github.com/line/line-bot-sdk-nodejs/blob/master/examples/echo-bot/index.js" >}}範例程式碼{{< /target-blank >}}。

![image](../../../../images/post/post-9-8.jpg)

#### 步驟 3：建立`.env`，並寫下創建 LineBot 的密鑰`channelSecret`及`channelAccessToken`。

![image](../../../../images/post/post-9-9.jpg)

### 部署至 Render

---

#### 步驟 1：到{{< target-blank url="https://render.com/" >}}官網註冊{{< /target-blank >}}，並 Connect GitHub

#### 步驟 2：選擇要部署的專案

點選`Web Services`

![image](../../../../images/post/post-9-10.jpg)

設置`Configure account`，選擇 LineBot 專案

![image](../../../../images/post/post-9-11.jpg)

![image](../../../../images/post/post-9-12.jpg)

#### 步驟 3：Connect 要部署的專案

點擊Connect

![image](../../../../images/post/post-9-13.jpg)

#### 步驟 4：填寫部署資訊

![image](../../../../images/post/post-9-14.jpg)

#### 步驟 5：選擇收費方案

這裡我選Free

![image](../../../../images/post/post-9-15.jpg)

#### 步驟 6：「Advanced」區塊填入環境變數

點一下「Add Environment Variable」，依照欄位填入

![image](../../../../images/post/post-9-16.jpg)

#### 步驟 7：Create Web Service

這時候就會開始進行部署

![image](../../../../images/post/post-9-17.jpg)

### 回到 line 設定 webhook

到{{< target-blank url="https://dashboard.render.com/" >}}dashboard{{< /target-blank >}}選擇想要的`services name`，進去後左上角會看見一串URL，這就是需要設定在 line webhook。

![image](../../../../images/post/post-9-27.jpg)

記得要加上`/callback`。

![image](../../../../images/post/post-9-18.jpg)

### 小皮妞Bot Demo

1. **打開指令介紹**：`**`

![image](../../../../images/post/post-9-19.jpg)

2. **增加紀錄**：`@標題@內容`

![image](../../../../images/post/post-9-20.jpg)

3. **搜詢當下關鍵字**：`@@關鍵字`

![image](../../../../images/post/post-9-21.jpg)

4. **刪除當下第N筆文章**：`@$N`

![image](../../../../images/post/post-9-22.jpg)

5. **輸出當下"文章編號(第N*10筆)**：`$$$N`

![image](../../../../images/post/post-9-23.jpg)

6. **輸出當下第N筆文章**：`$$N`

![image](../../../../images/post/post-9-24.jpg)

7. **輸出自己"文章編號(第N*10筆)**：`$$$myN`

- 可以群聊針對自己文章查詢

![image](../../../../images/post/post-9-25.jpg)

8. **輸出不存在指令**：`可以隨便打測試`

![image](../../../../images/post/post-9-26.jpg)

### 結語

---

Heroku 轉移至 Fly.io，讓我的小皮妞Bot復活，但莫名其妙被多收錢，因此再轉移至 Render。

而 Render 優點：

1. 有免費計畫，且不用先綁信用卡，用起來比較安心

2. 每個月免費 500 分鐘建構，和 450 小時免費使用時數