---
title: "前端優化- Throttle & Debounce"
date: 2023-11-27
draft: false

# post thumb
image: "images/post/throttle-debounce-logo.jpg"

# meta description
description: "簡單實作 throttle & debounce"

# taxonomies
categories:
  - "Notes"
tags:
  - "React.js"

# post type
type: "post"
---

### 前言

---

以下是我針對 throttle & debounce 兩種方法在React上實作。
 
### Throttle (節流) 

---

以下範例用`throttle`實現`Infinite Scroll`效果。

#### Demo

影片中可以看到當滾動頁面到90%位置，就會載入新的元素進來。

{{< video src="/images/post/throttle.mp4" >}}

#### 程式碼範例

這段程式碼使用了閉包（closure）的概念，主要是為了維護 `timer` 變數的狀態。閉包是指函數內部可以訪問其外部作用域的變數，即使在外部函數已經執行完畢的情況下。

在這裡，`throttle` 函數返回了一個匿名函數，而這個匿名函數引用了 `timer` 變數，形成了閉包。這樣可以確保 `timer` 的狀態在多次呼叫 `throttle` 函數時保持一致。

具體來說：

1. **`timer` 的保存：**
    - 在 `throttle` 函數內，`timer` 是一個在外部作用域（`throttle` 函數）中定義的變數。每次呼叫 `throttle` 函數，都會創建一個新的 `timer` 變數，但由於閉包的存在，每個返回的匿名函數都保留對自己的 `timer` 的引用。

2. **檢查 `timer`：**
    - 在返回的匿名函數內部，首先檢查 `timer` 是否存在。如果存在，表示上一次的執行還在範圍內，則直接返回，不執行後續的操作。

3. **執行 `setTimeout`：**
    - 如果 `timer` 不存在，則使用 `setTimeout` 設置一個計時器。這個計時器在指定的時間後會執行提供的 `callback` 函數。同時，將 `timer` 設為 `null`，表示現在已經可以再次觸發。

這種方式保證了在一個特定時間內，只有第一次呼叫 `throttle` 函數的 `callback` 函數被執行，後續的呼叫都被忽略。這在一些需要限制執行頻率的情境下是非常有用的。閉包確保了 `timer` 的狀態在整個過程中的一致性。

```
// helper/throttle.js

export const throttle = (callback, time) => {
  let timer;
  return () => {
    // 如果 timer 存在（表示上一次的執行還在範圍內），則直接返回，不執行後續的操作。
    if (timer) {
      return;
    }
    // 如果 timer 不存在，則使用 setTimeout 設置一個計時器，
    // 當計時器過期時，執行 callback 函數，然後將 timer 設為 null，表示現在已經可以再次觸發。
    timer = setTimeout(() => {
      callback();
      timer = null;
    }, time);
  };
};
```
這段程式碼實現一個無窮捲動效果，值得注意的是`組件掛載時添加了滾動事件監聽器，而在組件卸載時清理這個監聽器，以防止內存洩漏和不必要的事件處理`。

```
// components/InfiniteScrollDemo/InfiniteScroll.js

import React, { useState, useEffect } from "react";
import { throttle } from "../../helper/throttle";

const InfiniteScroll = () => {
  // 初始化 paragraphs 的初始資料
  const initialParagraphs = Array.from({ length: 30 }, (_, index) => (
    <p key={`origin-${index}`}>123</p>
  ));

  const [paragraphs, setParagraphs] = useState(initialParagraphs);

  const handleScrollThrottled = throttle(() => {
    // 檢查是否滾動到頁面底部
    let clientHeight = document.documentElement.clientHeight;
    let scrollTop = document.documentElement.scrollTop;
    let scrollHeight = document.documentElement.scrollHeight;

    if ((scrollTop + clientHeight) / scrollHeight >= 0.9) {
      // 生成 "456" 字串的 p 標籤節點
      const newParagraphs = Array.from({ length: 9 }, (_, index) => (
        <p key={`new-${index + paragraphs.length}`}>456</p>
      ));

      // 更新 paragraphs 狀態
      setParagraphs((prevParagraphs) => [...prevParagraphs, ...newParagraphs]);
    }
  }, 1000);

  useEffect(() => {
    // 將 handleScrollThrottled 函數添加到 scroll 事件監聽器
    window.addEventListener("scroll", handleScrollThrottled);

    // 在組件卸載時清理滾動事件監聽器
    return () => {
      window.removeEventListener("scroll", handleScrollThrottled);
    };
  }, [handleScrollThrottled]);

  return <div>{paragraphs}</div>;
};

export default InfiniteScroll;
```

### Debounce (防抖)

---

以下範例用`debounce`實現`延遲對輸入值的處理`。

#### Demo1

影片中可以確保只有在使用者輸入停頓 1.5 秒後才會觸發對輸入值的處理 (可參照右邊log)。

{{< video src="/images/post/debounce1.mp4" >}}

#### 程式碼範例

這段程式碼使用自訂的 useDebounce hook，透過 debounce 技術來降低對輸入值的處理頻率。

```
// helper/debounce.js

import { useEffect } from 'react'

// useEffect with debounce
const useDebounce = (fn, delay, deps = []) => {
  useEffect(
    () => {
      // 設置一個定時器，延遲 delay 毫秒後執行 
      const timer = setTimeout(fn, delay)
      // 在每次 useEffect 執行前，清除之前的定時器，確保防彈跳邏輯的正確性
      return () => clearTimeout(timer)
    },
    // eslint-disable-next-line
    deps
  )
}
export default useDebounce
```

```
// components/SearchBarDemo/SearchBar.js

import React, { useState } from "react";
import useDebounce from "../../helper/debounce";

const SearchBar = () => {
  const [inputValue, setInputValue] = useState("");
  const [debouncedValue, setDebouncedValue] = useState("");

  const handleInputChange = (value) => {
    setInputValue(value);
    console.log("Input Value:", value);
  };

  useDebounce(
    () => {
      console.log("Debounced Input Value:", inputValue);
      setDebouncedValue(inputValue);
    },
    1500,
    [inputValue]
  );

  return (
    <div>
      <input
        type="text"
        value={inputValue}
        onChange={(e) => handleInputChange(e.target.value)}
        placeholder="Type something..."
      />
      <p>Input Value: {inputValue}</p>
      <p>Debounced Value: {debouncedValue}</p>
    </div>
  );
};

export default SearchBar;
```

#### Demo2

影片中用戶實際輸入後停頓後 1.5 秒後，才會觸發 API 請求。

{{< video src="/images/post/debounce2.mp4" >}}

#### 程式碼範例

在這邊我是拿到全部資料過濾`userId === 1`，印出每筆的 title。

```
// helper/debounce.js

import { useEffect } from "react";

// useEffect with debounce
const useDebounce = (fn, delay, deps = []) => {
  useEffect(
    () => {
      if (deps.length > 0) {
        // 設置一個定時器，延遲 delay 毫秒後執行
        const timer = setTimeout(fn, delay);
        // 在每次 useEffect 執行前，清除之前的定時器，確保防彈跳邏輯的正確性
        return () => clearTimeout(timer);
      }
    },
    // eslint-disable-next-line
    deps
  );
};
export default useDebounce;
```

```
// components/SearchBarDemo/SearchBarAPI.js

import React, { useState } from "react";
import useDebounce from "../../helper/debounce";

const SearchBarAPI = () => {
  const [inputValue, setInputValue] = useState("");
  const [filteredPosts, setFilteredPosts] = useState([]);

  const fetchData = async () => {
    try {
      const response = await fetch(
        `https://jsonplaceholder.typicode.com/posts`
      );
      const data = await response.json();
      const filteredData = data.filter((post) => post.userId === 1);
      console.log(filteredData);
      setFilteredPosts(filteredData);
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  };

  // 使用 useDebounce 包裹 fetchData，只有在有輸入值的情況下才會觸發資料請求
  useDebounce(
    () => {
      if (inputValue) {
        fetchData();
      }
    },
    1500,
    [inputValue]
  );

  const handleChange = (event) => {
    setInputValue(event.target.value);
  };

  return (
    <div>
      <input
        type="text"
        value={inputValue}
        onChange={handleChange}
        placeholder="Type something..."
      />
      <p>Filtered Posts:</p>
      <ul>
        {filteredPosts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
};

export default SearchBarAPI;
```

### 結語

---

`Throttle（節流）` 和 `Debounce（防抖）`是用於控制某些操作執行頻率的兩種不同的技術。這兩種技術主要應用在處理用戶輸入、監聽事件或執行一些高頻率操作的情境中。以下是它們的主要區別：

1. **Throttle（節流）：**
   - **定義：** 當一個操作在固定的時間間隔內只執行一次，不論這段時間內有多少次觸發事件，都只會執行一次。
   - **使用場景：** 當你希望限制某個操作的執行頻率，特別是在高頻率事件觸發的情況下，例如滾動事件或鍵盤輸入。

2. **Debounce（防抖）：**
   - **定義：** 當一個操作在最後一次觸發事件後，等待一段時間，只有在這段時間內沒有新的觸發事件發生時，才執行該操作。
   - **使用場景：** 當你希望等待一段時間，確保用戶已經停止輸入或停止觸發事件，然後再執行相應的操作。這對於節省資源和減少不必要的操作很有用，例如在搜索欄輸入時實時搜索。

簡單來說，節流主要用於控制一個操作的執行頻率，確保在一段時間內只執行一次；而防抖則確保在最後一次觸發事件後的等待時間內沒有新的觸發事件，才執行相應的操作。這兩種技術都有助於提高性能和優化用戶體驗，特別是在處理大量事件或高頻率觸發的情況下。
