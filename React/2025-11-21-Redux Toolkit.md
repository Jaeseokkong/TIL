# 🧰 Redux Toolkit (RTK)

Redux Toolkit은 Redux에서 공식으로 제공하는 **Redux의 표준 방식(Standard way)** 으로, 기존 Redux의 불편함(보일러플레이트, 액션/리듀서 분리, 불변성 관리 부담)을 대폭 줄여주는 **현대적인 Redux 개발 방식**입니다.

RTK를 사용하면 Redux 개발이 훨씬 간단해지고, 상태 관리 흐름도 더 명확해집니다.

---

## 1️⃣ 왜 Redux Toolkit을 사용할까?

### ❌ 기존 Redux를 사용할 때 자주 겪는 문제

- Action 타입을 직접 문자열로 작성
- Reducer에서 switch문 반복
- 불변성 유지 때문에 매번 spread(...)
- 파일이 너무 많아짐(actions/reducers/store)

### ✅ Redux Toolkit에서의 해결 방법

- createSlice → action + reducer 자동 생성
- Immer 기반 → 불변성 직접 신경 안 써도 됨
- configureStore → 미들웨어/DevTools 자동 설정
- boilerplate 획기적으로 감소

---

## 2️⃣ Redux Toolkit의 핵심 구조

RTK는 크게 3가지 핵심 기능을 중심으로 구성되어 있습니다.

### 🔹 `createSlice()`

- state + reducer + action을 한 곳에서 관리
- 불변성은 Immer가 자동으로 처리
- action creator 자동 생성

### 🔹 `configureStore()`

- store 생성의 공식 방법
- Redux DevTools / middleware 자동 설정

### 🔹 `createAsyncThunk()`

- 비동기 로직을 자동 패턴으로 관리
- loading/error 상태 처리까지 내장

---

## 3️⃣ Redux Toolkit 기본 예시 (ToDo관리)

### 📁 `todoSlice.js`

```js
import { createSlice } from "@reduxjs/toolkit";

const todoSlice = createSlice({
  name: "todo",
  initialState: {
    items: [],
  },
  reducers: {
    addTodo: (state, action) => {
      state.items.push({
        id: Date.now(),
        text: action.payload,
        done: false,
      });
    },
    toggleTodo: (state, action) => {
      const todo = state.items.find((t) => t.id === action.payload);
      if (todo) todo.done = !todo.done;
    },
    removeTodo: (state, action) => {
      state.items = state.items.filter((t) => t.id !== action.payload);
    },
  },
});

export const { addTodo, toggleTodo, removeTodo } = todoSlice.actions;
export default todoSlice.reducer;
```

#### 🔍 포인트

✔ **slice는 "기능 단위 상태 조각"**

- Redux는 전역 상태를 하나의 큰 store에 담지만, RTK에서는 기능별로 **slice로 분할**하여 관리
- action type을 따로 작성할 필요 없음 (`addTodo()`, `toggleTodo()` 자동 생성)

✔ **Reducer 내부에서 state를 직접 변경하는 코드가 가능한 이유**

- RTK는 내부적으로 **Immer 라이브러리**를 사용
- 직접 변경처럼 보여도 실제로는 새로운 불변 객체를 만듦

---

## 4️⃣ Store 구성 - `configureStore()`

### 📁 `store/index.js`

```js
import { configureStore } from "@reduxjs/toolkit";
import todoReducer from "./todoSlice";

const store = configureStore({
  reducer: {
    todo: todoReducer, // slice.reducer 연결
  },
});

export default store;
```

---

### 🔹 store 구조 이해

RTK의 store는 `slice.reducer`들을 객체 형태로 결합합니다.

```js
{
  todo: {
    items: [...]
  }
}
```


```js
{
  todo: {...},
  user: {...},
  theme: {...}
}
```

slice가 늘어날수록 기능별 상태가 store의 트리 형태로 정리됩니다.

---

### 🔹 configureStore의 기본 기능

RTK의 configureStore는 아래 기능들이 자동으로 내장되어 있습니다.

- **Redux DevTools 자동 연결**

    - 상태 변화 추적 가능

- **Redux Thunk 내장**

    - 비동기 로직을 위한 createAsyncThunk와 호환

- **미들웨어 자동 설정**

    - 불변성 검사, 직렬성 검사 등 기본 안정 장치 포함

- **boilerplate**제거

    - 기존 createStore + applyMiddleware + compose 필요 없음

---

## 5️⃣ React에서 사용 예시

### 📁 `App.jsx`

```jsx
import React, { useState } from "react";
import { Provider, useDispatch, useSelector } from "react-redux";
import store from "./store";
import { addTodo, toggleTodo, removeTodo } from "./todoSlice";

function TodoList() {
  const [text, setText] = useState("");
  const dispatch = useDispatch();
  const todos = useSelector((state) => state.todo.items);

  return (
    <div>
      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="할 일 입력"
      />
      <button onClick={() => dispatch(addTodo(text))}>추가</button>

      <ul>
        {todos.map((t) => (
          <li key={t.id}>
            <span
              onClick={() => dispatch(toggleTodo(t.id))}
              style={{ textDecoration: t.done ? "line-through" : "none" }}
            >
              {t.text}
            </span>
            <button onClick={() => dispatch(removeTodo(t.id))}>삭제</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default function App() {
  return (
    <Provider store={store}>
      <TodoList />
    </Provider>
  );
}
```

---

## 6️⃣ Redux Toolkit의 상태 변화 흐름

RTK도 Redux와 동일하게 **단방향 흐름**을 유지합니다.

```scss
UI → dispatch(action)
  
     ↓

(createSlice가 자동 생성한)
reducer(state, action)

     ↓

새로운 state 반환 (Immer가 불변성 처리)

     ↓

store 변경 & 관련 컴포넌트 렌더링
```

> **UI → dispatch → slice.reducer → 새로운 state → 컴포넌트 리렌더링**

---

## 7️⃣ createSlice 패턴 정리

RTK에서 가장 중요한 건 **slice 기반 설계**입니다.

| 구성           | 설명                                 |
| ------------ | ---------------------------------- |
| name         | slice의 네임스페이스                      |
| initialState | 초기 상태                              |
| reducers     | 상태 변경 함수(불변성 자동 처리)                |
| actions      | reducers로부터 자동 생성되는 action creator |
| reducer      | slice 전체 reducer                   |

### 🔹 slice 설계 패턴

- 기능 단위로 slice 분리 (예: userSlice, authSlice, todoSlice, themeSlice)
- 명확한 action 이름 (예: `addTodo`, `deleteTodo` 같은 도메인 중심 이름)
- 최소한의 state (불필요한 중첩 구조 피하기)

---