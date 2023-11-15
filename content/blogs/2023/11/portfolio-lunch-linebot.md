---
title: "午餐機器人"
date: 2023-11-15
draft: false

# post thumb
image: "images/post/lunch-logo.jpg"

# meta description
description: "串接 LineBot 及後台管理訂單介面"

# taxonomies
categories:
  - "Portfolio"
tags:
  - "LineBot"
  - "Line"
  - "Ngrok"
  - "React.js"
  - "Feathers"

# post type
type: "post"
---

### 前言

---

當時在前公司訂便當的時候，都要跑去每個人座位詢問，挺麻煩的!

於是決定與一位後端同仁一起搭建午餐機器人。

##### 架構

這邊我是用 Feathers + MongoDB Atlas 部署到 Render 作品

1. 後台介面：Feathers、React 和 Feathers Socket.IO 創建一個實時應用程序，並實現數據的即時更新。
2. LineBot：設定 Channel ID、Channel Secret...等必要資訊，並 Webhook 路由及處理 LineBot 事件

### 測試環境用 Ngrok 建立一個 LineBot Webhook 的 HTTPS URL

---

#### 步驟 1：在 {{< target-blank url="https://ngrok.com/" >}}Ngrok官網{{< /target-blank >}} 創建一個帳戶

#### 步驟 2：執行 Ngrok

1. 下載並解壓縮後你會獲得一個 ngrok 的執行檔。

2. Mac 環境變數設定：需要將 ngrok 執行檔拖曳到 /usr/local/bin/ 資料夾內。

3. 切換到`/bin`目錄 將 Webhook 轉發到本地端口（例如 3030）。

![image](../../../../images/post/post-11-1.jpg)

4. 啟動服務

可以看到 Forwarding 有一個 https 服務，到時候就可以貼到 LineBot Webhook。

![image](../../../../images/post/post-11-2.jpg)

![image](../../../../images/post/post-11-3.jpg)

> LineBot設定就不再探討，可以去我寫的 {{< target-blank url="/blogs/2023/10/portfolio-qq-linebot/" >}}LineBot 關鍵字儲存查詢{{< /target-blank >}} 文章看

### 午餐機器人及後台管理 Demo

---

1. 可在後台新建菜單

![image](../../../../images/post/post-11-4.jpg)

2. 這時候可以看見LineBot輸入`吃`(這邊是我們自己下的指令)，就會產生訂單

![image](../../../../images/post/post-11-5.jpg)

3. 接著按下開始訂單，就依照機器人回覆輸入相對應文字

![image](../../../../images/post/post-11-6.jpg)

4. 後台介面就可以看到在 LineBot 輸入的資訊了

- 也可以對成員新增、編輯、刪除、查詢

![image](../../../../images/post/post-11-7.jpg)

5. 當同事過來給當天值日生繳錢時，就可以按下`付錢`，後台頁面也會統計誰沒繳方便查詢

![image](../../../../images/post/post-11-8.jpg)

### 結語

---

以上就是我們替同事製作的`午餐 Bot`，大家只要加官方帳號，動手輸入一些指令就能輕鬆訂便當，真的方便許多🎉