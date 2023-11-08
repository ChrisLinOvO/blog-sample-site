---
title: "Use Jest"
date: 2023-11-07T22:00:00+08:00
draft: false

# post thumb
image: "images/post/react-jest-logo.jpg"

# meta description
description: "紀錄一些 Jest 筆記"

# taxonomies
categories:
  - "Notes"
tags:
  - "React.js"
  - "Jest"

# post type
type: "post"
---

### 前言

---

紀錄一些重點

### 使用 Jest 進行快照測試

---

每當您想要確保 UI 不會意外更改時，快照測試都是一個非常有用的工具。

第一次執行此測試時，Jest 會在`__tests__`目錄內建立`__snapshots__`目錄，裡面建立一個 snapshot 檔案，檔名會是測試檔的名稱再加上`.snap`，當 UI 有變更的時候，Snapshot 就會顯示異動的部分。

執行指令`npm run test -- -u`，覆蓋原本就的，產生新的 Snapshot。

#### 範例

```
// Button.js
import React from 'react';

const Button = ({ label }) => (
  <button>{label}</button>
);

export default Button;
```

```
// __tests__/Button.test.js
import React from 'react';
import renderer from 'react-test-renderer';
import Button from './Button';

test('Button component snapshot', () => {
  const component = renderer.create(<Button label="Click Me" />);
  const tree = component.toJSON();
  expect(tree).toMatchSnapshot();
});
```

示意快照內容如下

```
// 例如：Button.test.js.snap

exports[`Button component snapshot 1`] = `
<button>
  Click Me
</button>
`
```

### 測試覆蓋率報告（Coverage Reporting）

---

執行`npm run test -- --coverage`就可以取得測試覆蓋率的報告。如要修改配置可以參考 {{< target-blank url="https://create-react-app.dev/docs/running-tests/#configuration" >}}configuration{{< /target-blank >}} 的部分。

### jest.config.js 常用配置

---

1. **testRegex**

用來匹配測試文件的路徑和文件名

範例：

```
testRegex: '__tests__/.*\\.?(spec|test)\\.js?$'
```

2. **testRegex**

可以指定處理格式黨

範例：

```
moduleFileExtensions: ['js', 'jsx', 'json']
```

3. **transform**

可指定哪些文件該如何轉換

範例：

```
transform: {
    '\\.js$': [
      'es-jest',
      {
        jsxDev: true,
        jsx: 'automatic',
        loader: 'jsx'
      }
    ]
  }
```

4. **setupFiles**

運行測試初始化配置

範例：

```
setupFiles: ['<rootDir>/__tests__/setup/init.js']
```

5. **testEnvironment**

指定測試運行環境，預設默認`jsdom`

範例：

```
testEnvironment: 'jsdom'
```

6. **coverageThreshold**

用於設定測試覆蓋率值

範例：

```
coverageThreshold: {
  global: {
    statements: 80,
    branches: 80,
    functions: 80,
    lines: 80,
  },
}
```

7. **moduleNameMapper**

可指定模塊路徑該到哪裡

範例：

```
'@storybook/addon-docs/(.*)': '@storybook/addon-docs/dist/cjs/$1',
'@vzmi/ec_file_uploader_sdk': '<rootDir>/__mocks__/file-uploader-sdk.js',
'uuid/v1': '<rootDir>/__mocks__/uuidv1.js'
```

8. **collectCoverage**

啟用覆蓋率報告

範例：

```
collectCoverage: true
```

9. **collectCoverageFrom**

指定哪些文件應包括在測試覆蓋率報告中

範例：

```
collectCoverageFrom: ['src/**/*.{js,jsx}']
```

10. **reporters**

生成測試報告生成測試報告的方式和格式

範例：

```
reporters: ['default', 'jest-junit']
```

11. **coveragePathIgnorePatterns**

配置覆蓋率報告需要忽略的路徑

範例：

```
coveragePathIgnorePatterns: [
  '<rootDir>/conf/',
  '<rootDir>/__tests__/config/',
  '<rootDir>/__tests__/support/'
]
```

12. **testPathIgnorePatterns**

測試需要忽略的路徑

範例：

```
testPathIgnorePatterns: [
  '<rootDir>/node_modules/',
  '<rootDir>/__tests__/config/'
]
```

13. **globals**

全局變量

範例：

```
globals: {
    __DEV__: true
}
```

14. **setupFilesAfterEnv**

測試環境初始化後執行

範例：

```
setupFilesAfterEnv: [
  '<rootDir>/node_modules/raf/polyfill',
  '<rootDir>/node_modules/regenerator-runtime/runtime',
  '<rootDir>/__tests__/config/setup.js',
]
```

15. **testRunner**

指定 Jest 使用的運行器

範例：

```
testRunner: 'jest-jasmine2'
```

16. **maxWorkers**

配置 Jest 啟動後最大工作進程數，提高測試速度

範例：

```
maxWorkers: '50%'
```

### jest.fn()

---

創建一個 mock function，可模擬函數被調用，也可設置返回值

```
// 創建一個模擬函數
const mockFunction = jest.fn();

// 設定模擬函數返回值
mockFunction.mockReturnValue(42);

// 調用模擬函數
const result = mockFunction();

// 測試模擬函數被調用一次
expect(mockFunction).toBeCalledTimes(1);

// 測試模擬函數返回值為 42
expect(result).toBe(42);
```

### 結語

---

關於 Jest 筆記會持續更新～