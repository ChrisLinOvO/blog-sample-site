---
title: "使用 Zustand 實作 Todo List"
date: 2024-03-24
draft: false

# post thumb
image: "images/post/react-zustand-todos-logo.png"

# meta description
description: "簡單實作一下React 的 Zustand State Management 套件怎麼使用"

# taxonomies
categories:
  - "Notes"
tags:
  - "React.js"
  - "TypeScript"
  - "Zustand"

# post type
type: "post"
---

### 前言

---

因為工作需要使用到 Zustand，這邊稍微研究一下並實作 Todo List。
 
### Zustand

---

現在 react 的相關套件裡面最使用最廣泛的全域管理工具應該是 Redux，在工作上較大的專案也都是使用 Redux 在做狀態管理，但是我想要介紹另外一個比較輕量狀態管理工具，Zustand。

> npm install zustand

#### 概念

1. **`create`:**
  是 Zustand 提供的一個函數，用於創建狀態容器。它接受一個參數，這個參數是一個稱為 State Creator 的函數。在這個函數中，可以定義初始狀態以及一些操作這個狀態的方法。
2. **`set`:**
  是一個在 State Creator 函數中可以使用的函數。它允許更新狀態容器的數據。更新函數接受一個參數，這個參數是當前的狀態數據，並且返回新的狀態數據。

總之，`create`函數用於創建狀態容器，而`set`函數用於更新狀態。

#### 程式碼範例

這段程式碼是使用 TypeScript 和 Zustand 庫來建立一個簡單的待辦事項應用程序的狀態管理系統。

程式碼結構：

1. **`Interface` 定義：**
    - `Todo`，定義資料結構，包括 ID、內容、完成狀態和編輯狀態等。
    - `TodoState`，定義狀態結構，包括一個待辦事項數組和一些操作這些待辦事項的方法。

2. **`Todo List` 操作函數：**
    - 用於執行各種待辦事項的操作，包括新增、標記完成、刪除和編輯待辦事項等。

3. **`useTodo` 狀態創建函數：**
    - 創建並返回一個包含狀態和操作函數的對象。這些操作函數內部調用了 Zustand 提供的 set 函數，來更新應用程序的狀態。

4. **`create` 導出默認值：**
    - 最後，將使用 create 函數導出一個具有預設狀態的狀態儲存容器。

> `StateCreator<TodoState>`：這裡定義了狀態創建函數（State Creator）的類型。它告訴 TypeScript 狀態創建函數的參數類型以及返回的狀態的類型。在這個例子中，TodoState 定義了狀態的結構和方法，所以我們將其傳遞給 StateCreator 來確保狀態創建函數返回的狀態與 TodoState 相符。

> `devtools((set: SetState<TodoState>) => ({`：這裡使用了 devtools 中間件來對狀態進行調試和監控。devtools 函數接受一個函數作為參數，這個函數內部定義了狀態的創建邏輯。在這個函數中，我們明確地指定了 set 函數的類型為 SetState<TodoState>，這樣做是為了確保 set 函數可以正確地更新 TodoState 類型的狀態。

```
// state/useTodo.tsx

import { create, State, StateCreator, SetState } from "zustand";
import { devtools } from "zustand/middleware";
import { v4 as uuidv4 } from "uuid";

export interface Todo {
  id: string;
  todoContent: string;
  complete: boolean;
  edit: boolean;
}

interface TodoState extends State {
  todos: Array<Todo>;
  addNewTodo: (todoContent: string) => void;
  toggleCompleteTodo: (todoId: string) => void;
  deleteTodo: (todoId: string) => void;
  toggleEditTodo: (todoId: string) => void;
  updateEditingTodoContent: (todoId: string, todoContent: string) => void;
}

const todoObj = (todoContent: string): Todo => {
  return {
    id: uuidv4(),
    todoContent,
    complete: false,
    edit: false,
  };
};

const addNewTodo = (todos: Array<Todo>, todoContent: string): Array<Todo> => {
  const todo = todoObj(todoContent);
  const newTodo = todos.concat(todo);

  return newTodo;
};

const toggleCompleteTodo = (todos: Array<Todo>, todoId: string): Array<Todo> => {
  const newTodo = todos.map((todo) =>
    todo.id === todoId ? { ...todo, complete: !todo.complete } : todo
  );

  return newTodo;
};

const deleteTodo = (todos: Array<Todo>, todoId: string): Array<Todo> => {
  const newTodo = todos.filter((todo) => todo.id !== todoId);

  return newTodo;
};

const toggleEditTodo = (todos: Array<Todo>, todoId: string): Array<Todo> => {
  const newTodo = todos.map((todo) =>
    todo.id === todoId ? { ...todo, edit: !todo.edit } : todo
  );

  return newTodo;
};

const updateEditingTodoContent = (
  todos: Array<Todo>,
  todoId: string,
  todoContent: string
): Array<Todo> => {
  const newTodo = todos.map((todo) =>
    todo.id === todoId ? { ...todo, todoContent } : todo
  );

  return newTodo;
};

const useTodo: StateCreator<TodoState> = devtools((set: SetState<TodoState>) => ({
  todos: [],
  addNewTodo: (todoContent: string) => {
    set((state: TodoState) => ({ // 明確指定 state 參數的型別為 TodoState
      ...state,
      todos: addNewTodo(state.todos, todoContent),
    }));
  },
  toggleCompleteTodo: (todoId: string) => {
    set((state: TodoState) => ({
      ...state,
      todos: toggleCompleteTodo(state.todos, todoId),
    }));
  },
  deleteTodo: (todoId: string) => {
    set((state: TodoState) => ({
      ...state,
      todos: deleteTodo(state.todos, todoId),
    }));
  },
  toggleEditTodo: (todoId: string) => {
    set((state: TodoState) => ({
      ...state,
      todos: toggleEditTodo(state.todos, todoId),
    }));
  },
  updateEditingTodoContent: (todoId: string, todoContent: string) => {
    set((state: TodoState) => ({
      ...state,
      todos: updateEditingTodoContent(state.todos, todoId, todoContent),
    }));
  },
}));

export default create<TodoState>(useTodo);
```

這時候 component 就可以去接收 golbal 狀態，以下舉例 Form Input 及渲染項目和操作 UI。

```
// TodoForm.tsx

import * as React from "react";
import { useRef, FormEvent } from "react";
import useTodo from "./state/useTodo";

const TodoForm: React.FC = () => {
  const { addNewTodo } = useTodo();
  const inputRef = useRef<HTMLInputElement>(null);

  const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    if (!inputRef.current || !inputRef.current.value.trim()) return;

    const value = inputRef.current.value.trim();
    addNewTodo(value);

    inputRef.current.value = "";
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" placeholder="Type in Something ..." ref={inputRef} />
    </form>
  );
};

export default TodoForm;
```

```
// Todos.tsx

import * as React from "react";
import useTodo, { Todo } from "./state/useTodo";

const Todos: React.FC = () => {
  const {
    todos,
    toggleCompleteTodo,
    deleteTodo,
    toggleEditTodo,
    updateEditingTodoContent,
  } = useTodo();
  console.log(todos);
  return (
    <div>
      {todos.map((todo: Todo) => (
        <div className="todo" key={todo.id}>
          {todo.edit ? (
            <div>
              <input
                value={todo.todoContent}
                onChange={(e) =>
                  updateEditingTodoContent(todo.id, e.target.value)
                }
              />
              <button className="save" onClick={() => toggleEditTodo(todo.id)}>
                Save
              </button>
            </div>
          ) : (
              <div>
                <span
                  style={{
                    textDecoration: todo.complete ? "line-through" : undefined,
                  }}
                >
                  {todo.todoContent}
                </span>
                <button
                  className="toggle"
                  onClick={() => toggleCompleteTodo(todo.id)}
                >
                  Complete
              </button>

                <button className="edit" onClick={() => toggleEditTodo(todo.id)}>
                  Edit
              </button>
                <button className="delete" onClick={() => deleteTodo(todo.id)}>
                  Delete
              </button>
              </div>
            )}
        </div>
      ))}
    </div>
  );
};

export default Todos;
```

#### Demo

示範在 Redux Devtools上 監控 Todo List 操作。

{{< video src="/images/post/zustamd-todos.mp4" >}}

### 結語

---

透過 Zustand 就可以做到把狀態跟元件分開，而且不需要使用 React 的 Context 或 Provider component 是還蠻方便的。
