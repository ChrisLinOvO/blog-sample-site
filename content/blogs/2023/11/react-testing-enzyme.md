---
title: "React Testing Enzyme"
date: 2023-11-07T18:00:00+08:00
draft: false

# post thumb
image: "images/post/react-enzyme-logo.jpg"

# meta description
description: "使用此 Enzyme 重點筆記"

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

{{< target-blank url="https://enzymejs.github.io/enzyme/" >}}React Testing Enzyme{{< /target-blank >}} 主要介紹幾個方法：`shallow`、`render`、`mount`。
 
| 函數       | 渲染範圍         | 子元件渲染        | 事件模擬           |
|------------|-----------------|-----------------|-----------------|
| `shallow`  | 只渲染元件本身  | 不渲染子元件     | 支援事件模擬     |
| `render`   | 產生靜態 HTML 字串 | 不在真實 DOM 渲染 | 不支援事件模擬   |
| `mount`    | 渲染整個 React 樹  | 渲染所有子元件    | 支援事件模擬     |


### 範例

1. 使用`shallow`函數：

情境：測試一個 React 按鈕元件的點擊事件處理，而不需要渲染其子元件。

```
import React from 'react';
import { shallow } from 'enzyme';

const MyButton = ({ onClick }) => (
  <button onClick={onClick}>Click me</button>
);

describe('MyButton', () => {
  it('should handle click event using shallow', () => {
    const onClickMock = jest.fn();
    const wrapper = shallow(<MyButton onClick={onClickMock} />);
    wrapper.find('button').simulate('click');
    expect(onClickMock).toHaveBeenCalled();
  });
});
```

2. 使用`render`函數：

情境：測試一個 React 文字元件的渲染結果，並確保它以特定方式呈現，但不需要模擬事件處理。

```
import React from 'react';
import { render } from 'enzyme';

const Greeting = ({ name }) => (
  <div>Hello, {name}!</div>
);

describe('Greeting', () => {
  it('should render greeting message using render', () => {
    const renderedComponent = render(<Greeting name="John" />);
    expect(renderedComponent.text()).toBe('Hello, John!');
  });
});
```

3. 使用`mount`函數：

情境：測試一個包含多個子元件的 React 表單，包括模擬事件處理和子元件的互動。

```
import React from 'react';
import { mount } from 'enzyme';

const Form = ({ onSubmit }) => (
  <form onSubmit={onSubmit}>
    <input type="text" name="username" />
    <input type="password" name="password" />
    <button type="submit">Submit</button>
  </form>
);

describe('Form', () => {
  it('should handle form submission using mount', () => {
    const onSubmitMock = jest.fn();
    const wrapper = mount(<Form onSubmit={onSubmitMock} />);
    const usernameInput = wrapper.find('input[name="username"]');
    const passwordInput = wrapper.find('input[name="password"]');
    const submitButton = wrapper.find('button');

    usernameInput.simulate('change', { target: { value: 'user123' } });
    passwordInput.simulate('change', { target: { value: 'password123' } });
    submitButton.simulate('submit');

    expect(onSubmitMock).toHaveBeenCalledWith({
      username: 'user123',
      password: 'password123',
    });
  });
});
```

### 結語

---

總結一下：

1. `shallow` 函數：
   - 使用 `shallow` 函數時，它只渲染被測試元件本身，不渲染其子元件。
   - 主要用於測試被測試元件的行為，而不關心子元件的情況。
   - 可以模擬事件處理，用於測試點擊事件、輸入事件等。

2. `render` 函數：
   - `render` 函數將元件渲染成靜態 HTML 字串，但不在真實 DOM 中渲染。
   - 主要用於測試元件的渲染結果，以及進行快照測試。
   - 不支援事件模擬，因為它不在真實 DOM 中渲染。

3. `mount` 函數：
   - 使用 `mount` 函數時，它會渲染整個 React 樹，包括子元件。
   - 主要用於進行完整的測試，包括事件處理、互動和子元件渲染的情況。
   - 可以模擬事件處理，用於測試點擊事件、輸入事件等。

可以根據測試需求選擇適當的函數，以確保測試覆蓋到你的應用程式中不同情境下的需求。

以上是我在工作期間整理`Enzyme`幾個基本用法。