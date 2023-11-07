---
title: "React Hooks Testing Library"
date: 2023-11-07T14:00:00+08:00
draft: false

# post thumb
image: "images/post/react-hooks-testing-library-logo.jpg"

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

{{< target-blank url="https://react-hooks-testing-library.com/" >}}React Hooks Testing Library{{< /target-blank >}}  主要提供三個方法：`renderHook`、`act`、`cleanup`。

##### 什麼時候該用此 Library {{< target-blank url="https://react-hooks-testing-library.com/#the-solution" >}}官方Solution{{< /target-blank >}} 

1. 使用

    - 當寫一個函式庫，且 Hooks 並不是只在某個對應的元件中被使用
    - Hook 非常複雜，很難透過組件互動進行測試

2. 不要使用
       
    - Hook 只給某對應的元件使用時
    - Hook 非常容易測試，只需要直接針對元件進行測試即可
 
---

### renderHook

如果要取得 hook 的回傳值，可透過`result.current`取得當前最新的值。

### act

當 hook 內部 state 改變時（例如，setXXX），需要把這個方法包在 act 後才加以呼叫。

### cleanup

用來清理測試環境，用法`cleanup()`。

### 範例

測試`useUIState`的`初始狀態`和`狀態更新`時，是否都可正常工作。

以下測試三種狀態`pageFlow`、`isLoading`、`isPending`。

```
// useUIState.js

import { useReducer } from 'react'

// 定義操作類型
const ACTIONS = {
  SET_PAGE_FLOW: 'SET_PAGE_FLOW',
  SET_IS_LOADING: 'SET_IS_LOADING',
  SET_IS_PENDING: 'SET_IS_PENDING'
}

// 初始化狀態
const getInitState = props => {
  const {
    match: { params: { phase } }
  } = props
  return {
    pageFlow: phase,
    isLoading: false,
    isPending: false
  }
}

// 自定義 Hook
const useUIState = props => {
  const reducer = (state, action) => {
    const { type, payload } = action
    switch (type) {
      case ACTIONS.SET_PAGE_FLOW:
        return { ...state, pageFlow: payload }
      case ACTIONS.SET_IS_LOADING:
        return { ...state, isLoading: payload }
      case ACTIONS.SET_IS_PENDING:
        return { ...state, isPending: payload }
      default:
        return state
    }
  }

  // 使用 useReducer 管理狀態
  const [state, dispatch] = useReducer(reducer, props, getInitState)

  // dispatch 觸發狀態更新操作
  const setPageFlow = pageFlow => dispatch({
    type: ACTIONS.SET_PAGE_FLOW,
    payload: pageFlow
  })

  const setIsLoading = isLoading => dispatch({
    type: ACTIONS.SET_IS_LOADING,
    payload: isLoading
  })

  const setIsPending = isPending => dispatch({
    type: ACTIONS.SET_IS_PENDING,
    payload: isPending
  })

  const actions = {
    setPageFlow,
    setIsLoading,
    setIsPending
  }

  return { state, actions }
}

export default useUIState
```

可以看到第一個`check initial state`測試，假設`renderHook`取得`props`回傳值，並確認`result.current.state`是跟預期`initialState`相同。

而第二個`test actions works`測試，透過`act`模擬更新狀態，最後檢查當前狀態是否跟`updatedState`相同。

```
// useUIState.test.js

import { renderHook, act } from '@testing-library/react-hooks'
import useUIState from './useUIState'

// flowTypes
export const PAGE_FLOW = {
  INITIAL: 'initial',
  EDIT: 'edit',
  CONFIRM: 'confirm',
  SUCCESS: 'success',
  ERROR: 'error'
}

describe('useUIState', () => {
  const props = {
    match: { params: { phase: PAGE_FLOW.INITIAL } }
  }

  it('check initial state', () => {
    const { result } = renderHook(() => useUIState(props))
    const initialState = {
      pageFlow: PAGE_FLOW.INITIAL,
      isLoading: false,
      isPending: false
    }
    expect(result.current.state).toEqual(initialState)
  })

  it('test actions works', () => {
    const { result } = renderHook(() => useUIState(props))
    const updatedState = {
      pageFlow: PAGE_FLOW.ERROR,
      isLoading: true,
      isPending: true
    }
    act(() => {
      result.current.actions.setIsLoading(updatedState.isLoading)
      result.current.actions.setIsPending(updatedState.isPending)
      result.current.actions.setPageFlow(updatedState.pageFlow)
    })
    expect(result.current.state).toEqual(updatedState)
  })
})
```

### 結語

---

以上是我在工作期間整理`react-hooks-testing-library`幾個基本用法，如果需更深入了解可到{{< target-blank url="https://react-hooks-testing-library.com/usage/basic-hooks" >}}官方文件{{< /target-blank >}}學習。