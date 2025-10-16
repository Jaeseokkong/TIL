# useReducer 구조와 동작 원리
`useReducer`는 React의 상태 관리 훅 중 하나로,  
**상태(state) 와 상태 변경 로직(reducer)을 명확히 분리**할 수 있습니다.

![useReducer](../images/useReducer.png)

---

## 1️⃣ 구조

```tsx
const [state, dispatch] = useReducer(reducer, initialState);
```

### 🔹 반환값 구조

|이름|타입|설명|
|:---|:---|:---|
|`state`|any|현재 상태 값|
|`dispatch`|function|상태 변경을 요청하는 함수 (액션 전달)|

### 🔹 매개변수 구조


|이름|타입|
|:---|:---|
|`reducer(state, action)`|상태 변경 로직을 담은 함수|
|`initialState`|초기 상태 값|

---
