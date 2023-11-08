---
title: "End-to-End (E2E) Testing"
date: 2023-11-08
draft: false

# post thumb
image: "images/post/end-to-end-testing-logo.jpg"

# meta description
description: "紀錄一些 E2E Testing 筆記"

# taxonomies
categories:
  - "Notes"
tags:
  - "React.js"
  - "CodeceptJS"
  - "Puppeteer"
  - "webdriver"

# post type
type: "post"
---

### 前言

---

E2E 測試在模擬真實用戶在應用程式中的實際使用情境，驗證各個組件之間功能是否正常。

主要測試目標如下：

1. **驗證應用功能**：測試將模擬用戶與應用程式的互動情境，例如填寫表單、點擊按鈕、導航到不同頁面等，以確保所有功能正常運作。

2. **測試使用者界面（UI）**：測試使用者界面，以確保 UI 元素的正確性和可用性。這包括驗證頁面佈局、樣式、互動和使用者體驗。

3. **跨瀏覽器和跨平台兼容性**：測試不同的瀏覽器和操作系統上運行，以確保應用程式在各種環境中都能正常運作。這有助於發現和解決跨瀏覽器和跨平台兼容性問題。

4. **性能測試**：測試應用程式的性能，例如加載時間、響應時間和並發用戶支援。這有助於發現性能瓶頸和優化機會。

5. **視覺測試**：驗證應用程式的用戶界面（UI）外觀和布局是否正確的測試方法。它的主要目標是檢測潛在的 UI 問題，例如樣式錯誤、布局差異、文本溢出等，以確保應用程式的 UI 在不同環境和設備上都能夠正確呈現。前公司為例是用`頁面截圖比對`。

以下 E2E 測試會使用自動化測試框架`CodeceptJS`來模擬用戶行為。

### 管理（Page Objects）模組化

---

可建立`pageObjects.js`，裡面 exports`pageObject`函數用來簡化頁面對象的管理和使用，提高腳本可讀性。

```
// pageObjects.js

// 可針對選擇器命名
const pageObjects = {
topNav: ['.top-nav', 'Top Navigation'],
logoutBtn: ['#logoutBtn', 'Logout Button']
infoBlock: ['[data-role="info-block"]', 'info block']
}

const pageObject = (name) => {
  const selectedPageObject = pageObjects[name]

  if (!selectedPageObject) {
    const msg = `❌ PAGE OBJECT ERROR: "${name}" is not defined!`
    throw new Error(msg)
  }

  if (!selectedPageObject[2]) {
    console.log(`✅ pageObject('${name}') => '${selectedPageObject[0]}'  // ${selectedPageObject[1]}`)
    selectedPageObject[2] = 1
  }

  return selectedPageObject[0]
}

module.exports = {
  pageObject
}
```

##### 如何應用 pageObject

以下範例示範用`pageObject('topNav')`，就可取得`pageObjects`定義的元素。

```
Feature('Navigation @pc @noie @nosafari @nofirefox')
const pageObject = require('./pageObjects').pageObject

Scenario('open dashboard, see navigation sections', ({ I }) => {
  I.amOnPage('/dashboard')
  I.seeElement(pageObject('topNav'))
})
```

### codecept.conf.js 配置

---

以下是常見的`WebDriver`和`Puppeteer`配置，可參考 {{< target-blank url="https://codecept.io/configuration/" >}}configuration{{< /target-blank >}}。

```
exports.config = {
  tests: './tests/*.js',
  output: './output',
  helpers: {
    WebDriver: {
      url: 'https://example.com', // 設定要測試的網站 URL
      browser: 'chrome', // 選擇瀏覽器（可以是 chrome、firefox、edge 等）
      windowSize: '1920x1080', // 設定瀏覽器視窗大小
    },
    Puppeteer: {
      url: 'https://example.com', // 設定要測試的網站 URL
      show: false, // 是否顯示瀏覽器視窗
      windowSize: '1920x1080', // 設定瀏覽器視窗大小
    },
  },
  include: {
    I: './steps_file.js',
  },
  bootstrap: null,
  mocha: {},
  name: '我的測試專案',
  plugins: {
    wdio: {
      enabled: true,
      services: ['selenium-standalone'],
    },
  },
  multiple: {
    parallel: {
      chunks: 2,
    },
  },
  screenshotPath: './output/screenshots'
}
```

### Visual Testing

---

CodeceptJS 使用`WebDriver`幫助程式和Selenium來自動化瀏覽器。它還能夠截取應用程式的螢幕截圖，這可用於視覺測試，可參考{{< target-blank url="https://codecept.io/visual/#visual-testing" >}}官方文件{{< /target-blank >}}。

方法：在`codecept.conf.js`新增配置，指定截圖目錄路徑。

```
screenshotPath: './output/screenshots'
```

使用：

```
Feature('視覺測試');

Scenario('檢查頁面外觀', (I) => {
  I.amOnPage('https://example.com');
  // 執行一些操作...

  // 生成並截圖至 screenshots 資料夾
  I.saveScreenshot('my-screenshot.png');
});
```

### 自定義 Helper 配置

---

可以在`codecept.conf.js`，自定 Helper。

範例：`CustomHelper`

```
exports.config = {
  helpers: {
    WebDriver: {
      // 配置 WebDriver Helper
      url: 'http://example.com',
      browser: 'chrome',
    },
    CustomHelper: {
      // 配置 CustomHelper
      apiKey: 'your_api_key_here',
    },
  },
  // ...
};
```

如何使用

```
class CustomHelper {
  constructor(config) {
    this.options = config; // 訪問自定義 Helper 的配置選項
  }

  async performCustomTask() {
    const apiKey = this.options.apiKey;
    // 使用 apiKey 
  }
}

module.exports = CustomHelper;
```

### 結語

---

關於 E2E Testing 筆記會持續更新～

