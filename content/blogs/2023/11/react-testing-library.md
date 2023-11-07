---
title: "React Testing Library"
date: 2023-11-07T16:00:00+08:00
draft: false

# post thumb
image: "images/post/react-testing-library-logo.png"

# meta description
description: "使用此 library 重點筆記"

# taxonomies
categories:
  - "Notes"
tags:
  - "React.js"
  - "Jest"
  - "Testing Library"

# post type
type: "post"
---

### 前言

---

{{< target-blank url="https://testing-library.com/docs/" >}}React Testing Library{{< /target-blank >}}  主要提供幾個方法：`render`、`fireEvent`、`screen`、`waitFor`。

前置設定需搭配 `@testing-library/jest-dom`。

##### 什麼時候該用此 Library 可參考{{< target-blank url="https://testing-library.com/docs/react-testing-library/intro#this-solution" >}}官方Solution{{< /target-blank >}} 

---
 
### render

用於渲染 React 組件到虛擬 DOM 中，並返回一個包含渲染結果的容器對象。

### fireEvent

用於模擬 DOM 事件，例如點擊、輸入文字、提交表單等。這可用於觸發元素上的事件，以模擬使用者操作。

### screen

提供了一組方法來查找 DOM 元素，以進行測試操作。

### waitFor

用於等待異步操作完成，通常用於等待數據載入、渲染完成或狀態更新。

### 範例

以下測試`測試按鈕點擊後是否正確顯示文字`流程。

```
// ButtonComponent.js

import React, { useState } from 'react';

function ButtonComponent() {
  const [isClicked, setIsClicked] = useState(false);

  return (
    <div>
      <button onClick={() => setIsClicked(true)}>Click Me</button>
      {isClicked && <p>Button was clicked!</p>}
    </div>
  );
}

export default ButtonComponent;
```

```
import React from 'react';
import { render, fireEvent, screen, waitFor } from '@testing-library/react';
import ButtonComponent from './ButtonComponent';

test('測試按鈕點擊後是否正確顯示文字', async () => {
  // 渲染組件
  render(<ButtonComponent />);

  // 首先確保文字元素不可見
  expect(screen.queryByText('Button was clicked!')).toBeNull();

  // 找到按鈕元素
  const button = screen.getByText('Click Me');

  // 模擬點擊按鈕
  fireEvent.click(button);

  // 使用 waitFor 等待直到文字元素出現
  await waitFor(() => {
    expect(screen.getByText('Button was clicked!')).toBeInTheDocument();
  });
});

```

### 結語

---

以上是我在工作期間整理`react-testing-library`幾個基本用法，如果需更深入了解可到{{< target-blank url="https://testing-library.com/docs/react-testing-library/example-intro" >}}官方文件{{< /target-blank >}}學習。