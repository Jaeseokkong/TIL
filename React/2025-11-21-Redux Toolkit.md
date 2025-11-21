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

### 📁 todoSlice.js

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

### 