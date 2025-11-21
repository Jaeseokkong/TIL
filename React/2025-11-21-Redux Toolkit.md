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
