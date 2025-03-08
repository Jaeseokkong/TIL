# useReducer
`useReducer`는 `useState`와 비슷한 역할을 하지만, **상태 관리 로직을 더 명확하고 구조적으로 관리할 때** 유용합니다. 특히, **상태가 여러 개의 값으로 이루어져 있고, 복잡한 상태 전환이 필요할 때** 사용됩니다.

## 1️⃣ `useState`와 `useReducer` 차이점
|구분|useState|useReducer|
|---|---|---|
|사용 목적|단순한 상태 관리|복잡한 상태 관리|
|상태 업데이트 방식|`setState`를 사용해 직접 업데이트|`dispatch`를 사용해 액션을 전달|
|상태 변경 로직|컴포넌트 내부에서 직접 작성|`reducer` 함수를 별도로 정의|
|리렌더링 최적화|간단한 변경에 적합|여러 값이 변경될 때 일괄 관리 가능|

✔️ `useState`는 **단순한 상태**에 적합하고, `useReducer`는 
**여러 값이 관련된 복잡한 상태 관리**에 적합합니다.

<br>

- - - 

<br>

## 2️⃣ `useReducer` 기본 문법
```tsx
const [state, dispatch] = useReducer(reducer, initialState);
```
- `state`: 현재 상태 값
- `dispatch(action)`: 상태를 변경하는 함수 (액션을 전달)
- `reducer(state, action)`: 상태를 업데이트하는 함수
- `initialState`: 초기 상태 값

<br>

- - -

<br>

## 3️⃣ `useReducer` 사용 예제
### 🔹 `useState`를 사용하는 경우
```tsx
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
    </div>
  );
}
```
✔️ 간단한 상태 변경에는 `useState`가 적합

<br>

### 🔹 같은 기능을 `useReducer`로 구현
```tsx
import { useReducer } from "react";

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
    </div>
  );
}
```

✔️ `dispatch({ type: "increment" })`를 호출하면 `reducer`가 실행되어 상태를 변경함  
✔️ 상태 변경 로직을 `reducer`에서 관리하여 **가독성 증가 & 유지보수 용이**

